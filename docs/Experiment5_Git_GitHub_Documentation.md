# Experiment 5: Git and GitHub — Source Code Management

**Project:** EcoFood — Food Waste Reduction Platform  
**Repository:** [https://github.com/jeeva2470041/ecofeed](https://github.com/jeeva2470041/ecofeed)  
**Author:** jeeva2470041 (jeeva2470041@ssn.edu.in)  
**Date:** 2026-02-24  

---

## Table of Contents

1. [Objective](#1-objective)
2. [Repository Setup](#2-repository-setup)
3. [Branch Structure & Hierarchy](#3-branch-structure--hierarchy)
4. [Version Control Operations](#4-version-control-operations)
5. [Merge & Conflict Resolution](#5-merge--conflict-resolution)
6. [Commit History & Log](#6-commit-history--log)
7. [Workflow Analysis & Comparison](#7-workflow-analysis--comparison)
8. [Git Commands Reference](#8-git-commands-reference)
9. [Conclusion](#9-conclusion)

---

## 1. Objective

Implement version control for the EcoFood project using Git/GitHub. Apply and analyze
branching workflows to improve efficiency, collaboration, and maintainability of the
project source code.

**Key Goals:**
- Create and organize a Git repository with proper branch hierarchy
- Demonstrate incremental commits, branching, merging, and conflict resolution
- Analyze the efficiency of the implemented Git workflow
- Compare alternative branching and merging strategies
- Document all operations comprehensively

---

## 2. Repository Setup

### 2.1 Initial Repository Configuration

```bash
# Initialize Git repository (already done)
git init

# Configure user identity
git config user.name "jeeva2470041"
git config user.email "jeeva2470041@ssn.edu.in"

# Add remote origin (GitHub)
git remote add origin https://github.com/jeeva2470041/ecofeed.git

# Verify remote
git remote -v
# Output:
# origin  https://github.com/jeeva2470041/ecofeed.git (fetch)
# origin  https://github.com/jeeva2470041/ecofeed.git (push)
```

### 2.2 .gitignore Configuration

```
node_modules/
.env
.DS_Store
.env.local
.env.*.local
npm-debug.log
```

### 2.3 Project Directory Structure

```
ecofeed/                          ← Root repository
├── .git/                         ← Git metadata
├── .gitignore                    ← Ignored file patterns
├── .env.example                  ← Environment template
├── README.md                     ← Project documentation
├── CHANGELOG.md                  ← Version changelog
├── docker-compose.yml            ← Docker orchestration
├── package.json                  ← Root package config
│
├── backend/                      ← Backend (Node.js + Express)
│   ├── config/                   ← Database & app configuration
│   ├── controllers/              ← Route handlers (6 controllers)
│   ├── middleware/                ← Auth & validation middleware
│   ├── models/                   ← Mongoose schemas (Food, User, Notification)
│   ├── routes/                   ← API route definitions (8 route files)
│   ├── services/                 ← Business logic services (4 services)
│   ├── utils/                    ← Utility modules
│   │   ├── donationTracker.js    ← ✨ NEW: Donation tracking utility
│   │   └── notificationHelper.js ← ✨ NEW: Notification helper
│   ├── server.js                 ← Express server entry
│   └── Dockerfile                ← Backend container config
│
├── frontend/                     ← Frontend (React + Vite)
│   ├── src/
│   │   ├── components/           ← Reusable UI components (16 files)
│   │   ├── pages/                ← Page-level components (7 pages)
│   │   ├── layouts/              ← Layout wrappers
│   │   ├── services/             ← API service layer
│   │   ├── styles/               ← CSS stylesheets (7 files)
│   │   └── utils/                ← Frontend utilities
│   └── Dockerfile                ← Frontend container config
│
└── ui-reference/                 ← Design reference assets
```

---

## 3. Branch Structure & Hierarchy

### 3.1 Branch Creation Commands

```bash
# Rename master to main (industry standard)
git branch -m master main

# Create develop branch from main
git branch develop

# Switch to develop
git checkout develop

# Create feature branches from develop
git branch feature/donation-tracking
git branch feature/notification-system
git branch feature/analytics-dashboard

# Verify all branches
git branch -a
```

### 3.2 Branch Listing Output

```
  develop
  feature/analytics-dashboard
  feature/donation-tracking
  feature/notification-system
* main
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/main
  remotes/origin/develop
  remotes/origin/feature/donation-tracking
  remotes/origin/feature/notification-system
  remotes/origin/feature/analytics-dashboard
```

### 3.3 Branch Hierarchy Diagram

```
                        main (stable, production-ready)
                          │
                          ▼
                       develop (integration branch)
                       /  │  \
                      /   │   \
                     ▼    ▼    ▼
        feature/      feature/       feature/
        donation-     notification-  analytics-
        tracking      system         dashboard
```

### 3.4 Branch Purpose & Strategy

| Branch | Purpose | Merges Into | Protected |
|--------|---------|-------------|-----------|
| `main` | Production-ready stable code | — (top-level) | ✅ Yes |
| `develop` | Integration of completed features | `main` | ✅ Yes |
| `feature/donation-tracking` | Donation lifecycle tracking module | `develop` | ❌ No |
| `feature/notification-system` | In-app notification helpers | `develop` | ❌ No |
| `feature/analytics-dashboard` | Analytics & data visualization | `develop` | ❌ No |

---

## 4. Version Control Operations

### 4.1 Incremental Commits on Feature Branches

#### Feature: Donation Tracking (2 commits)

```bash
# Switch to feature branch
git checkout feature/donation-tracking

# --- Commit 1: Add donation tracking utility ---
# Created: backend/utils/donationTracker.js
# Contains: DONATION_STATUS constants, getTimeSinceDonation(),
#           computeDonationStats(), isDonationExpiringSoon()

git add backend/utils/donationTracker.js
git commit -m "feat(tracking): add donation tracking utility with status constants and stats"
# Output: [main b45c3e7] feat(tracking): add donation tracking utility...
#  1 file changed, 82 insertions(+)

# Update parent repo pointer
git add backend
git commit -m "feat(tracking): add donation tracking utility module"
# Output: [feature/donation-tracking 209c634] feat(tracking)...

# --- Commit 2: Add donation tracking API routes ---
# Created: backend/routes/donationTracking.js
# Contains: GET /api/donations/stats, GET /api/donations/expiring

git add backend/routes/donationTracking.js
git commit -m "feat(tracking): add donation stats and expiring-donations API routes"
# Output: [main bc2c251] feat(tracking): add donation stats...
#  1 file changed, 43 insertions(+)

git add backend
git commit -m "feat(tracking): add donation tracking API routes"
# Output: [feature/donation-tracking aa1aaaf] feat(tracking)...
```

#### Feature: Notification System (1 commit)

```bash
# Switch to feature branch
git checkout feature/notification-system

# --- Commit: Add notification helper ---
# Created: backend/utils/notificationHelper.js
# Contains: NOTIFICATION_TYPES, PRIORITY constants, buildNotification(),
#           formatForDisplay(), getRelativeTime(), getUnreadNotifications()

git add utils/notificationHelper.js
git commit -m "feat(notifications): add notification helper with types, priorities, and formatters"
# Output: [main 9f72b6a] feat(notifications)...
#  1 file changed, 102 insertions(+)

git add backend
git commit -m "feat(notifications): add notification helper utility module"
# Output: [feature/notification-system bb9edc0] feat(notifications)...
```

#### Feature: Analytics Dashboard (1 commit)

```bash
# Switch to feature branch
git checkout feature/analytics-dashboard

# --- Commit: Add CHANGELOG ---
# Created: CHANGELOG.md (at root)

git add CHANGELOG.md
git commit -m "docs(analytics): add CHANGELOG.md with analytics dashboard entries"
# Output: [feature/analytics-dashboard 02990b4] docs(analytics)...
#  1 file changed, 23 insertions(+)
```

### 4.2 Summary of Commits by Branch

| Branch | # Commits | Files Changed | Key Changes |
|--------|-----------|---------------|-------------|
| `feature/donation-tracking` | 3 | `donationTracker.js`, `donationTracking.js`, `CHANGELOG.md` | Tracking utility, API routes, changelog |
| `feature/notification-system` | 1 | `notificationHelper.js` | Notification types, formatters |
| `feature/analytics-dashboard` | 1 | `CHANGELOG.md` | Analytics changelog entries |

---

## 5. Merge & Conflict Resolution

### 5.1 Merge: feature/donation-tracking → develop

```bash
git checkout develop

git merge feature/donation-tracking -m "merge: integrate donation-tracking feature into develop"
# Result: Fast-forward merge (no conflicts)
# Output:
# Updating 42a2acd..0268163
# Fast-forward
#  CHANGELOG.md | 24 ++++++++++++++++++++++++
#  backend      |  2 +-
#  2 files changed, 25 insertions(+), 1 deletion(-)
#  create mode 100644 CHANGELOG.md
```

> **Note:** Fast-forward merge occurred because `develop` had no divergent commits.

### 5.2 Merge: feature/notification-system → develop

```bash
git merge feature/notification-system -m "merge: integrate notification-system feature into develop"
# Result: 3-way merge (success, no conflicts)
# Output:
# Note: Fast-forwarding submodule backend to 9f72b6a...
# Merge made by the 'ort' strategy.
#  backend | 2 +-
#  1 file changed, 1 insertion(+), 1 deletion(-)
```

> **Note:** This was a 3-way merge because `develop` had advanced past the point where
> `feature/notification-system` branched off.

### 5.3 Merge: feature/analytics-dashboard → develop (CONFLICT!)

```bash
git merge feature/analytics-dashboard -m "merge: integrate analytics-dashboard feature into develop"
# Result: CONFLICT
# Output:
# Auto-merging CHANGELOG.md
# CONFLICT (add/add): Merge conflict in CHANGELOG.md
# Automatic merge failed; fix conflicts and then commit the result.
```

#### 5.3.1 Conflict Details

Both `feature/donation-tracking` and `feature/analytics-dashboard` created `CHANGELOG.md` with
different content. When merging the analytics branch into `develop` (which already had the
donation-tracking version), Git couldn't automatically reconcile the differences.

**Conflict markers in CHANGELOG.md:**

```
## [Unreleased]

### Added
<<<<<<< HEAD
- Donation tracking utility with lifecycle management
- Donation statistics computation API
- Expiring donations alert system
- Donation status constants and helpers
=======
- Analytics dashboard module for food waste metrics
- Data visualization utilities for donation trends
- Real-time dashboard API endpoints
>>>>>>> feature/analytics-dashboard
```

#### 5.3.2 Conflict Resolution Strategy

**Approach: Manual merge — combine both sets of changes**

We resolved the conflict by keeping entries from BOTH branches and adding the notification
system entries as well, creating a unified changelog:

```bash
# 1. Open CHANGELOG.md and manually edit to combine both versions
# 2. Remove conflict markers (<<<<<<, =======, >>>>>>>)
# 3. Keep all feature entries from both branches

# Resolved CHANGELOG.md content:
# ## [Unreleased]
# ### Added
# - Donation tracking utility with lifecycle management
# - Donation statistics computation API
# - Expiring donations alert system
# - Donation status constants and helpers
# - Analytics dashboard module for food waste metrics
# - Data visualization utilities for donation trends
# - Real-time dashboard API endpoints
# - Notification helper with types, priorities, and formatters

# 4. Stage and commit the resolved file
git add CHANGELOG.md
git commit -m "merge: resolve CHANGELOG conflict - combine analytics and tracking entries"
# Output: [develop 943a50b] merge: resolve CHANGELOG conflict...
```

### 5.4 Final Merge: develop → main

```bash
git checkout main
git merge develop -m "release: merge develop into main - donation tracking, notifications, analytics"
# Result: Fast-forward merge
# Output:
# Updating 42a2acd..943a50b
# Fast-forward
#  CHANGELOG.md | 28 ++++++++++++++++++++++++++++
#  backend      |  2 +-
#  2 files changed, 29 insertions(+), 1 deletion(-)
#  create mode 100644 CHANGELOG.md
```

### 5.5 Release Tagging

```bash
git tag -a v1.0.0 -m "Release v1.0.0: Initial stable release with donation tracking, notifications, and analytics"

# Verify tag
git tag -l
# Output: v1.0.0

# Push tag to remote
git push origin v1.0.0
# Output: * [new tag] v1.0.0 -> v1.0.0
```

### 5.6 Push All Branches to GitHub

```bash
git push origin main
git push origin develop
git push origin feature/donation-tracking
git push origin feature/notification-system
git push origin feature/analytics-dashboard

# All branches successfully pushed:
# * [new branch] main -> main
# * [new branch] develop -> develop
# * [new branch] feature/donation-tracking -> feature/donation-tracking
# * [new branch] feature/notification-system -> feature/notification-system
# * [new branch] feature/analytics-dashboard -> feature/analytics-dashboard
```

---

## 6. Commit History & Log

### 6.1 Full Git Graph (git log --oneline --graph --all --decorate)

```
*   943a50b (HEAD -> main, tag: v1.0.0, develop) merge: resolve CHANGELOG conflict
|\
| * 02990b4 (feature/analytics-dashboard) docs(analytics): add CHANGELOG.md
* |   00e0311 merge: integrate notification-system feature into develop
|\ \
| * | bb9edc0 (feature/notification-system) feat(notifications): add notification helper
| |/
* | 0268163 (feature/donation-tracking) docs(tracking): add CHANGELOG.md
* | aa1aaaf feat(tracking): add donation tracking API routes
* | 209c634 feat(tracking): add donation tracking utility module
|/
* 42a2acd (origin/master, origin/HEAD) added email
* b3558f0 added admin roles
* 7fd192e Create README.md with project overview and setup
* 4950940 chore(submodules): update frontend submodule pointer
* 6f92d48 chore(submodules): update submodule pointers after adding Dockerfiles
* d3d8116 chore(docker): add docker-compose, root .env.example and gitignore
* 925c165 added the ui and ux perfectly
* 6f74db9 admin dashboard
* 4a86940 ui
* c61f1d5 ux change
* 00628cb updated chatbot integration
* 4367727  new repo
```

### 6.2 Commit History Table

| Commit Hash | Author | Date | Message |
|-------------|--------|------|---------|
| `943a50b` | jeeva2470041 | 2026-02-24 | merge: resolve CHANGELOG conflict - combine analytics and tracking entries |
| `00e0311` | jeeva2470041 | 2026-02-24 | merge: integrate notification-system feature into develop |
| `0268163` | jeeva2470041 | 2026-02-24 | docs(tracking): add CHANGELOG.md with donation tracking entries |
| `02990b4` | jeeva2470041 | 2026-02-24 | docs(analytics): add CHANGELOG.md with analytics dashboard entries |
| `bb9edc0` | jeeva2470041 | 2026-02-24 | feat(notifications): add notification helper utility module |
| `aa1aaaf` | jeeva2470041 | 2026-02-24 | feat(tracking): add donation tracking API routes |
| `209c634` | jeeva2470041 | 2026-02-24 | feat(tracking): add donation tracking utility module |
| `42a2acd` | jeeva2470041 | 2026-01-18 | added email |
| `b3558f0` | jeeva2470041 | 2026-01-18 | added admin roles |
| `7fd192e` | jeeva | 2026-01-08 | Create README.md with project overview and setup |
| `4950940` | jeeva2470041 | 2026-01-08 | chore(submodules): update frontend submodule pointer |
| `6f92d48` | jeeva2470041 | 2026-01-08 | chore(submodules): update submodule pointers after adding Dockerfiles |
| `d3d8116` | jeeva2470041 | 2026-01-08 | chore(docker): add docker-compose, root .env.example and gitignore |
| `925c165` | jeeva2470041 | 2026-01-07 | added the ui and ux perfectly |
| `6f74db9` | jeeva2470041 | 2026-01-06 | admin dashboard |
| `4a86940` | jeeva2470041 | 2026-01-06 | ui |
| `c61f1d5` | jeeva2470041 | 2026-01-06 | ux change |
| `00628cb` | jeeva2470041 | 2026-01-06 | updated chatbot integration |
| `4367727` | jeeva2470041 | 2025-12-30 | new repo |

### 6.3 Contributor Statistics

```bash
git shortlog -s -n --all
# Output:
#     18  jeeva2470041
#      1  jeeva
```

---

## 7. Workflow Analysis & Comparison

### 7.1 Implemented Workflow: Git Flow

We implemented the **Git Flow** branching model, which is one of the most widely adopted
workflows for structured software development.

#### Flow Diagram

```
  main ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━►  [v1.0.0]
    │                                                          ▲
    │                                                          │ merge
    ▼                                                          │
  develop ━━━━━━━┳━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━┳━━━━━━━━━━►
                 │               │               │
                 ▼               ▼               ▼
          feature/          feature/        feature/
          donation-         notification-   analytics-
          tracking          system          dashboard
          (3 commits)       (1 commit)      (1 commit)
                 │               │               │
                 │  merge(FF)    │  merge(3-way)  │ merge(CONFLICT→resolve)
                 └───────────────┴───────────────┘
```

#### Why Git Flow?

| Criterion | Justification |
|-----------|---------------|
| **Isolation** | Each feature developed in its own branch, reducing interference |
| **Code Quality** | Code reviewed and tested before merging to develop |
| **Release Management** | `main` always contains stable, production-ready code |
| **Traceability** | Clear commit history showing feature origin and merge points |
| **Scalability** | Supports multiple developers working concurrently |

### 7.2 Alternative Workflow Comparison

#### 7.2.1 GitHub Flow (Simplified)

```
  main ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━►
    │           ▲           ▲           ▲
    │           │ PR merge  │ PR merge  │ PR merge
    ▼           │           │           │
  feature-1 ━━━┘    feature-2 ━━━┘    feature-3 ━━━┘
```

| Aspect | GitHub Flow | Git Flow (Ours) |
|--------|-------------|-----------------|
| Branches | `main` + feature branches | `main` + `develop` + feature + release + hotfix |
| Complexity | Low | Medium–High |
| Best For | Continuous deployment, web apps | Scheduled releases, larger teams |
| CI/CD | Deploy on every merge to main | Deploy from main after release merge |
| Risk | Higher (direct to production) | Lower (integration testing on develop) |

**Verdict:** GitHub Flow is simpler but lacks the safety net of a `develop` integration branch. For EcoFood, Git Flow is preferred because we need to test integrated features before releasing.

#### 7.2.2 Trunk-Based Development

```
  main (trunk) ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━►
    │  ▲    │  ▲    │  ▲
    │  │    │  │    │  │
    ▼  │    ▼  │    ▼  │
   short   short   short
   lived   lived   lived
   branch  branch  branch
   (hours) (hours) (hours)
```

| Aspect | Trunk-Based | Git Flow (Ours) |
|--------|-------------|-----------------|
| Branch Lifetime | Hours (very short) | Days to weeks |
| Merge Frequency | Multiple times/day | Per feature completion |
| Feature Flags | Required for incomplete features | Not required |
| Best For | Experienced teams with strong CI | Teams needing structured releases |
| Conflict Risk | Low (frequent merges) | Medium (longer branch life) |

**Verdict:** Trunk-based development minimizes merge conflicts but requires feature flags and robust CI/CD. Not ideal for EcoFood at this stage.

#### 7.2.3 Forking Workflow

```
  upstream/main ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━►
       ▲                    ▲
       │ PR                 │ PR
  fork-1/main ━━━━━━━━━┘    │
  fork-2/main ━━━━━━━━━━━━━━┘
```

| Aspect | Forking | Git Flow (Ours) |
|--------|---------|-----------------|
| Repository | Each developer has own fork | Shared single repository |
| Access Control | Strict (only maintainers merge) | Branch-level permissions |
| Best For | Open-source projects | Private team projects |
| Overhead | Higher (sync forks) | Lower (direct push) |

**Verdict:** Forking workflow provides maximum isolation but adds overhead. Suitable for open-source, not for our team size.

### 7.3 Workflow Efficiency Analysis

#### Merge Types Observed

| Merge | Type | Conflicts | Resolution Time |
|-------|------|-----------|-----------------|
| `donation-tracking → develop` | Fast-forward | None | Instant |
| `notification-system → develop` | 3-way merge | None | Instant |
| `analytics-dashboard → develop` | 3-way merge | 1 file (`CHANGELOG.md`) | ~2 minutes |
| `develop → main` | Fast-forward | None | Instant |

#### Key Observations

1. **Fast-forward merges** occur when the target branch has no new commits since the feature branched off — cleanest history
2. **3-way merges** create merge commits that make the history more complex but preserve the branching context
3. **Conflicts** arise when two branches modify the same region of a file — inevitable in collaborative development but manageable with clear ownership
4. **Conflict resolution** was straightforward: we combined entries from both branches into a unified file

### 7.4 Best Practices Implemented

| Practice | Implementation |
|----------|----------------|
| **Descriptive commit messages** | Used Conventional Commits format (`feat:`, `docs:`, `merge:`) |
| **Feature isolation** | Each feature in its own branch |
| **Incremental commits** | Multiple small commits per feature (not one large dump) |
| **.gitignore** | Excluded `node_modules/`, `.env`, build artifacts |
| **Release tagging** | Created annotated tag `v1.0.0` |
| **Remote backup** | All branches pushed to GitHub |

---

## 8. Git Commands Reference

### 8.1 Repository Setup Commands

| Command | Purpose |
|---------|---------|
| `git init` | Initialize a new Git repository |
| `git clone <url>` | Clone a remote repository |
| `git remote add origin <url>` | Add a remote repository |
| `git remote -v` | Verify remote URLs |
| `git config user.name "<name>"` | Set author name |
| `git config user.email "<email>"` | Set author email |

### 8.2 Branching Commands

| Command | Purpose |
|---------|---------|
| `git branch` | List all local branches |
| `git branch -a` | List all branches (local + remote) |
| `git branch <name>` | Create a new branch |
| `git branch -m <old> <new>` | Rename a branch |
| `git branch -d <name>` | Delete a branch |
| `git checkout <branch>` | Switch to a branch |
| `git checkout -b <branch>` | Create and switch to a new branch |

### 8.3 Staging & Committing

| Command | Purpose |
|---------|---------|
| `git status` | Show working tree status |
| `git add <file>` | Stage specific file |
| `git add .` | Stage all changes |
| `git commit -m "<message>"` | Commit staged changes |
| `git commit --amend` | Modify the last commit |
| `git diff` | Show unstaged changes |
| `git diff --staged` | Show staged changes |

### 8.4 Merging & Conflict Resolution

| Command | Purpose |
|---------|---------|
| `git merge <branch>` | Merge branch into current branch |
| `git merge --abort` | Abort a conflicting merge |
| `git merge --no-ff <branch>` | Force a merge commit (no fast-forward) |
| `git mergetool` | Open visual merge tool |
| `git log --merge` | Show commits causing conflict |

### 8.5 Remote Operations

| Command | Purpose |
|---------|---------|
| `git push origin <branch>` | Push branch to remote |
| `git push origin --tags` | Push all tags to remote |
| `git pull origin <branch>` | Fetch and merge from remote |
| `git fetch origin` | Fetch updates without merging |

### 8.6 History & Inspection

| Command | Purpose |
|---------|---------|
| `git log` | Show commit history |
| `git log --oneline --graph --all` | Show visual commit graph |
| `git log --format="%h \| %an \| %ad \| %s"` | Custom log format |
| `git shortlog -s -n --all` | Contributor statistics |
| `git show <commit>` | Show commit details |
| `git blame <file>` | Show line-by-line authorship |

### 8.7 Tagging

| Command | Purpose |
|---------|---------|
| `git tag -a <name> -m "<msg>"` | Create annotated tag |
| `git tag -l` | List all tags |
| `git push origin <tag>` | Push tag to remote |
| `git tag -d <name>` | Delete a local tag |

### 8.8 Undoing & Recovery

| Command | Purpose |
|---------|---------|
| `git restore <file>` | Discard working tree changes |
| `git restore --staged <file>` | Unstage a file |
| `git reset --soft HEAD~1` | Undo last commit (keep changes staged) |
| `git reset --hard HEAD~1` | Undo last commit (discard changes) |
| `git revert <commit>` | Create a new commit that undoes a previous one |
| `git stash` | Temporarily save uncommitted changes |
| `git stash pop` | Restore stashed changes |

---

## 9. Conclusion

### 9.1 Summary of Operations Performed

| Category | Operations |
|----------|-----------|
| **Repository Setup** | Initialized repo, configured user, added remote, set up `.gitignore` |
| **Branching** | Created 5 branches (`main`, `develop`, 3 feature branches) |
| **Commits** | Made 19 total commits with descriptive messages |
| **Merges** | Performed 4 merges (2 fast-forward, 1 three-way, 1 conflict resolution) |
| **Conflict Resolution** | Resolved 1 CHANGELOG.md conflict by combining entries |
| **Tagging** | Created annotated release tag `v1.0.0` |
| **Remote Sync** | Pushed all branches and tags to GitHub |

### 9.2 Justification of Version Control Strategy

The **Git Flow** workflow was chosen for EcoFood because:

1. **Quality Gate:** The `develop` branch acts as an integration testing ground before code reaches `main`, preventing broken code from reaching production.
2. **Feature Isolation:** Each feature branch ensures that work-in-progress code doesn't affect other developers or the stable codebase.
3. **Clear History:** The branching and merging pattern creates a traceable history showing when and how features were developed and integrated.
4. **Release Management:** Tags on `main` provide clear release points, making it easy to roll back to specific versions if needed.
5. **Collaboration Ready:** The workflow scales well as the team grows, with clear conventions for where to branch from and merge to.

### 9.3 Lessons Learned

- **Conventional commits** (`feat:`, `docs:`, `fix:`) make history readable and searchable
- **Frequent, small commits** are easier to review and revert than large monolithic ones
- **Merge conflicts** are normal and manageable — the key is clear file ownership and communication
- **Always push to remote** to prevent data loss and enable collaboration
- **Tags** provide version snapshots that are invaluable for release management

---

*Document prepared as part of Experiment 5: Git and GitHub Source Code Management*  
*EcoFood Project — SSN College of Engineering*
