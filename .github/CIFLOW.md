# CI/CD Flow — Development Versioning and Branch Handling

This document describes the branching strategy, versioning policy, and GitHub Actions workflows used by the karaf-features project. The workflow follows [GitFlow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow).

## Branches

The project uses five types of branches, each with a specific role in the development and release lifecycle:

| Branch Pattern | Base | Purpose |
|----------------|------|---------|
| `develop` | — | Main development branch; contains the latest development sources |
| `feature/JNG-xxx_description` | `develop` | New features targeting the next release |
| `release/x.y.z` | `develop` | Stabilization branch for a specific release version |
| `bugfix/JNG-xxx_description` | `release/*` | Bug fixes applied during release testing (before merge to master) |
| `support/JNG-xxx_description` | `release/*` | Minor changes to a previous release; merged back to the release branch |
| `hotfix/JNG-xxx_description` | `master` | Emergency fixes applied to both master and release branches |
| `master` | — | Latest released production sources |

### Branch Flow

```mermaid
gitGraph
    commit id: "initial"
    branch develop order: 1
    commit id: "dev-1"
    branch feature/JNG-1 order: 2
    commit id: "feat-1a"
    commit id: "feat-1b"
    checkout develop
    merge feature/JNG-1 id: "merge-feat-1"
    branch feature/JNG-2 order: 3
    commit id: "feat-2a"
    checkout develop
    merge feature/JNG-2 id: "merge-feat-2"
    branch release/1.0 order: 4
    commit id: "rc-1"
    branch bugfix/JNG-3 order: 5
    commit id: "bugfix-3"
    checkout release/1.0
    merge bugfix/JNG-3 id: "merge-bugfix"
    checkout master
    merge release/1.0 id: "v1.0"
    checkout develop
    merge release/1.0 id: "back-merge"
```

## Version Numbers

Versions follow semantic versioning with specific rules per branch type:

| Event | Version Change | Example |
|-------|---------------|---------|
| Start feature branch | No change | stays `2.0.2-SNAPSHOT` |
| Start release branch | 2nd number incremented on develop | develop → `2.1.0-SNAPSHOT` |
| Start bugfix branch | No change | applied on release branch |
| Start support branch | 3rd number incremented | `1.0.1-SNAPSHOT` |
| Start hotfix branch | 4th number incremented | `1.0.0.1` |

### Build Version Format

| Branch Type | Version Format | Example |
|-------------|---------------|---------|
| `master`, `release/*` | `major.minor.qualifier` | `2.0.1` |
| `develop`, `increment/*` | `major.minor.qualifier.date_commitId_branchName` | `2.0.2.20240115_abc1234_develop` |

## GitHub Actions Workflows

The project uses several interconnected GitHub Actions workflows. The following diagram shows how they trigger each other:

```mermaid
flowchart TD
    subgraph Triggers
        push_develop[Push to develop]
        pr[PR to develop/master/release/increment]
        push_master[Push to master]
        manual[Manual trigger with version]
        merge_tag[Push merge-pr/* tag]
    end

    subgraph Workflows
        build[build.yml]
        merge_pr[merge-pr-tagged.yml]
        create_release_master[create-release-on-master.yml]
        release[release.yml]
    end

    push_develop --> build
    pr --> build

    build -->|"increment/*, release/* branches"| merge_tag
    merge_tag --> merge_pr

    merge_pr -->|"major.minor.qualifier version"| push_master
    merge_pr -->|"other version format"| push_develop
    push_master --> create_release_master

    manual --> release
    release -->|"PR to master"| build
    release -->|"PR to develop"| build
```

### build.yml

The main build workflow that runs on every push to `develop` and on pull requests to key branches.

```mermaid
flowchart TD
    start([Push/PR event])
    check{Branch type?}
    ver_release[Set version from pom.xml<br/>without -SNAPSHOT]
    ver_dev[Set version as<br/>major.minor.qualifier.date_commitId_branch]
    build_deploy[Build and deploy to Nexus]
    tag[Create git tag v-version-]
    is_release{increment/* or release/*?}
    create_merge_tag[Create merge-pr/version tag]
    trigger_merge[Triggers merge-pr-tagged.yml]
    is_develop{develop?}
    changelog[Build changelog]
    gh_release[Create GitHub pre-release]
    done([End])

    start --> check
    check -->|master, release/*| ver_release
    check -->|develop, increment/*| ver_dev
    ver_release --> build_deploy
    ver_dev --> build_deploy
    build_deploy --> tag
    tag --> is_release
    is_release -->|Yes| create_merge_tag --> trigger_merge --> is_develop
    is_release -->|No| is_develop
    is_develop -->|Yes| changelog --> gh_release --> done
    is_develop -->|No| done
```

### merge-pr-tagged.yml

Handles automatic merging of pull requests after a successful build.

```mermaid
flowchart TD
    start([merge-pr/* tag pushed])
    extract[Extract version from tag]
    check{Version format?}
    merge_master[Merge PR to master]
    squash_develop[Squash PR to develop]
    trigger_master([Triggers create-release-on-master.yml])
    trigger_build([Triggers build.yml])
    cleanup[Delete merge-pr/* tag]
    done([End])

    start --> extract --> check
    check -->|major.minor.qualifier| merge_master --> trigger_master --> cleanup
    check -->|other| squash_develop --> trigger_build --> cleanup
    cleanup --> done
```

### create-release-on-master.yml

Creates an official GitHub release when code lands on master.

- Triggered by: push to `master`
- Extracts version from tag
- Builds changelog
- Creates a GitHub release (marked as latest)

### release.yml

Manually triggered workflow to initiate a release.

```mermaid
flowchart TD
    start([Manual trigger])
    check{Version input?}
    auto[Read version from pom.xml<br/>remove -SNAPSHOT]
    manual[Use provided version]
    next[Calculate next version<br/>qualifier + 1]
    pr_master[Create PR to master<br/>with release version]
    pr_develop[Create PR to develop<br/>with next version]
    trigger_build([Triggers build.yml for both PRs])

    start --> check
    check -->|auto| auto --> next
    check -->|specific version| manual --> next
    next --> pr_master --> trigger_build
    next --> pr_develop --> trigger_build
```

## Development Rules

> **Important:** There is no commit without a ticket number. Every commit and pull request must reference a JIRA ticket: `JNG-xxx`.

Issue tracking is managed in [JIRA](https://blackbelt.atlassian.net/jira/dashboards).
