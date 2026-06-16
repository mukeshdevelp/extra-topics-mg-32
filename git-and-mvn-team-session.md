# Git, Maven & OWASP – Complete Learning Guide

## Table of Contents

1. [Git Introduction & Basic Commands](#1-git-introduction--basic-commands)
2. [Git Reset vs Checkout vs Revert](#2-git-reset-vs-checkout-vs-revert)
3. [Git Merge vs Rebase](#3-git-merge-vs-rebase)
4. [Git Submodules](#4-git-submodules)
5. [Maven Lifecycle](#5-maven-lifecycle)
6. [OWASP Dependency Check](#6-owasp-dependency-check)

---

# 1. Git Introduction & Basic Commands

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Git is a distributed version control system used to track source code changes.

Architecture:

```text
Working Directory
        ↓
Staging Area
        ↓
Local Repository
        ↓
Remote Repository
```

Flow:

```text
Code
↓
git add
↓
git commit
↓
git push
```

---

## Basic Commands

Initialize repository:

```bash
git init
```

Clone:

```bash
git clone REPO_URL
```

Check status:

```bash
git status
```

Add files:

```bash
git add .
```

Commit:

```bash
git commit -m "Initial commit"
```

Push:

```bash
git push origin main
```

Pull:

```bash
git pull
```

Create branch:

```bash
git checkout -b feature
```

Switch branch:

```bash
git switch main
```

View logs:

```bash
git log --oneline
```

View branches:

```bash
git branch
```

---

## Layman Explanation

Git = Google Docs version history for developers.

---

## Best Practices

✔ Commit frequently
✔ Use meaningful commit messages
✔ Pull before push
✔ Create feature branches

---

# 2. Git Reset vs Checkout vs Revert

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

These commands modify project state differently.

---

## Git Reset

Moves HEAD backward.

Soft:

```bash
git reset --soft HEAD~1
```

Keep changes.

Mixed:

```bash
git reset HEAD~1
```

Remove staging.

Hard:

```bash
git reset --hard HEAD~1
```

Delete changes.

---

## Git Checkout

Move between commits/branches.

Switch branch:

```bash
git checkout develop
```

Restore file:

```bash
git checkout file.txt
```

---

## Git Revert

Create opposite commit.

```bash
git revert COMMIT_ID
```

Safe for shared branches.

---

## Comparison

| Command  | Deletes History | Safe |
| -------- | --------------- | ---- |
| reset    | Yes             | No   |
| checkout | No              | Yes  |
| revert   | No              | Yes  |

---

## Layman Explanation

Reset → Time travel and erase

Checkout → Visit another place

Revert → Undo mistake safely

---

# 3. Git Merge vs Rebase

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Combine changes differently.

---

## Merge

Preserves history.

```bash
git checkout main

git merge feature
```

Result:

```text
A---B---C
 \     /
  D---E
```

---

## Rebase

Moves commits.

```bash
git checkout feature

git rebase main
```

Result:

```text
A-B-C-D-E
```

---

## Comparison

| Feature    | Merge     | Rebase   |
| ---------- | --------- | -------- |
| History    | Preserved | Linear   |
| Safe       | High      | Moderate |
| Complexity | Easy      | Medium   |

---

## Conflict Resolution

```bash
git status

git add .

git rebase --continue
```

Abort:

```bash
git rebase --abort
```

---

## Layman Explanation

Merge → Combine roads

Rebase → Move house to new road

---

## Best Practices

✔ Merge shared branches
✔ Rebase local feature branches

---

# 4. Git Submodules

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Submodule = Git repository inside another repository.

Use Cases:

* Shared libraries
* Common infrastructure
* Multiple repositories

Structure:

```text
parent-repo

app/

common-lib/
```

---

## Add Submodule

```bash
git submodule add URL
```

Clone with submodules:

```bash
git clone --recurse-submodules URL
```

Initialize:

```bash
git submodule update --init
```

Update:

```bash
git submodule update --remote
```

Remove:

```bash
git rm MODULE
```

---

## Layman Explanation

Main project borrowing another project.

---

## Best Practices

✔ Keep versions fixed
✔ Update intentionally

---

# 5. Maven Lifecycle

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

Maven automates Java project builds.

Lifecycle:

```text
Validate
↓
Compile
↓
Test
↓
Package
↓
Verify
↓
Install
↓
Deploy
```

---

## Lifecycle Commands

Validate:

```bash
mvn validate
```

Compile:

```bash
mvn compile
```

Test:

```bash
mvn test
```

Package:

```bash
mvn package
```

Install:

```bash
mvn install
```

Deploy:

```bash
mvn deploy
```

Clean:

```bash
mvn clean
```

---

## Example

```bash
mvn clean package
```

Output:

```text
target/
app.jar
```

---

## Layman Explanation

Maven = Factory assembly line.

---

## Best Practices

✔ Keep dependencies minimal
✔ Use profiles
✔ Avoid unnecessary plugins

---

# 6. OWASP Dependency Check

[⬆ Back to Table of Contents](#table-of-contents)

## Technical Explanation

OWASP Dependency Check scans project dependencies for vulnerabilities.

Checks:

* CVEs
* Dependency risk
* Vulnerable packages

Flow:

```text
Source
↓
Dependencies
↓
Scan
↓
Report
```

---

## Maven Plugin

Add:

```xml
<plugin>

<groupId>
org.owasp
</groupId>

<artifactId>
dependency-check-maven
</artifactId>

</plugin>
```

Run:

```bash
mvn dependency-check:check
```

Report:

```text
target/site/
dependency-check-report.html
```

---

## Jenkins Example

```groovy
stage('OWASP'){

steps{

sh 'mvn dependency-check:check'

}

}
```

---

## Common Severity

| Severity | Meaning          |
| -------- | ---------------- |
| LOW      | Minor            |
| MEDIUM   | Attention        |
| HIGH     | Critical         |
| CRITICAL | Immediate action |

---

## Layman Explanation

OWASP Dependency Check = Security scanner for third-party libraries.

---

## Best Practices

✔ Scan every build
✔ Fail on high severity
✔ Update dependencies regularly

---

\
