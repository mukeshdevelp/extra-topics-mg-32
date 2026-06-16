# Third Week Session — Developer Tools & Practices

A reference guide covering code quality tools, Git workflows, Maven plugins, and DevOps utilities.

---

## Table of Contents

1. [PMD](#1-pmd)
2. [FindBugs](#2-findbugs)
3. [Checkstyle](#3-checkstyle)
4. [Cobertura](#4-cobertura)
5. [Git Diff](#5-git-diff)
6. [Git Pull vs Git Fetch and Git Merge](#6-git-pull-vs-git-fetch-and-git-merge)
7. [Git Stash](#7-git-stash)
8. [popd & pushd](#8-popd--pushd)
9. [Git Merge — Ours/Theirs Method](#9-git-merge--ourstheirs-method)
10. [Log](#10-log)
11. [Config](#11-config)
12. [Fetch](#12-fetch)
13. [Submodule](#13-submodule)
14. [Compiler](#14-compiler)
15. [Surefire](#15-surefire)
16. [JaCoCo](#16-jacoco)
17. [OWASP Dependency Check](#17-owasp-dependency-check)
18. [SonarQube](#18-sonarqube)
19. [Deploy](#19-deploy)
20. [SpotBugs](#20-spotbugs)

---

## 1. PMD

**PMD** is a static source code analyzer that finds common programming flaws such as unused variables, empty catch blocks, unnecessary object creation, and more.

### Key Features
- Detects dead code, suboptimal code, overcomplicated expressions, and duplicate code (via CPD — Copy-Paste Detector)
- Supports Java, JavaScript, Apex, PLSQL, XML, and more
- Integrates with Maven, Gradle, Ant, and IDEs

### Maven Integration

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-pmd-plugin</artifactId>
  <version>3.21.0</version>
</plugin>
```

### Common Commands

```bash
# Run PMD check
mvn pmd:check

# Generate PMD report
mvn pmd:pmd

# Run Copy-Paste Detector
mvn pmd:cpd
```

### Example Rule Violation

```java
// PMD will flag this — empty catch block
try {
    doSomething();
} catch (Exception e) {
    // swallowing the exception
}
```

---

## 2. FindBugs

**FindBugs** is a static analysis tool that inspects Java bytecode to detect bug patterns. It identifies real bugs rather than style issues.

> ⚠️ FindBugs is no longer actively maintained. Its successor is [SpotBugs](#20-spotbugs).

### Bug Categories
- **Correctness** — definite bugs (e.g., null dereference)
- **Performance** — inefficient code patterns
- **Security** — potential security vulnerabilities
- **Dodgy Code** — confusing or error-prone constructs

### Maven Integration

```xml
<plugin>
  <groupId>org.codehaus.mojo</groupId>
  <artifactId>findbugs-maven-plugin</artifactId>
  <version>3.0.5</version>
</plugin>
```

### Common Commands

```bash
# Run FindBugs analysis
mvn findbugs:check

# Generate HTML report
mvn findbugs:findbugs
```

---

## 3. Checkstyle

**Checkstyle** enforces coding standards and style conventions in Java code. It ensures consistency across a codebase.

### What It Checks
- Naming conventions (classes, methods, variables)
- Whitespace and indentation
- Javadoc comments
- Block structure (braces, blank lines)
- Import organization

### Maven Integration

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-checkstyle-plugin</artifactId>
  <version>3.3.0</version>
  <configuration>
    <configLocation>google_checks.xml</configLocation>
  </configuration>
</plugin>
```

### Common Commands

```bash
# Fail build on violations
mvn checkstyle:check

# Generate report only
mvn checkstyle:checkstyle
```

### Popular Rule Sets
- `sun_checks.xml` — Sun/Oracle Java conventions
- `google_checks.xml` — Google Java Style Guide

---

## 4. Cobertura

**Cobertura** is a code coverage tool for Java that measures how much of your source code is exercised by tests.

### Metrics Provided
- **Line coverage** — percentage of executable lines hit
- **Branch coverage** — percentage of branches (if/else, switch) taken
- **Cyclomatic complexity** — measure of code complexity

### Maven Integration

```xml
<plugin>
  <groupId>org.codehaus.mojo</groupId>
  <artifactId>cobertura-maven-plugin</artifactId>
  <version>2.7</version>
</plugin>
```

### Common Commands

```bash
# Run tests and generate coverage report
mvn cobertura:cobertura

# Fail build if coverage drops below threshold
mvn cobertura:check
```

> ℹ️ Cobertura is largely superseded by [JaCoCo](#16-jacoco) in modern projects.

---

## 5. Git Diff

`git diff` shows the differences between commits, branches, files, or the working tree and index.

### Common Usages

```bash
# Show unstaged changes in working directory
git diff

# Show staged (indexed) changes ready to commit
git diff --staged

# Compare two branches
git diff main feature-branch

# Compare two commits
git diff abc1234 def5678

# Diff a specific file
git diff HEAD~1 -- src/Main.java

# Show a summary (stats only)
git diff --stat
```

### Understanding the Output

```diff
- old line removed
+ new line added
@@ -10,6 +10,7 @@ context line   ← hunk header (line numbers)
```

---

## 6. Git Pull vs Git Fetch and Git Merge

Understanding the difference between `git pull` and the two-step `git fetch` + `git merge` is crucial for controlling how remote changes are integrated.

### `git pull`

A convenience command that fetches and merges in one step.

```bash
git pull origin main
# Equivalent to:
git fetch origin
git merge origin/main
```

### `git fetch`

Downloads remote changes **without** modifying your working directory or current branch.

```bash
# Fetch all remote branches
git fetch origin

# Fetch a specific branch
git fetch origin main
```

### `git merge`

Integrates fetched changes into your current branch.

```bash
git merge origin/main
```

### Why Prefer Fetch + Merge?

| Aspect | `git pull` | `git fetch` + `git merge` |
|---|---|---|
| Control | Low — merges immediately | High — review before merging |
| Safety | Can cause unexpected conflicts | Allows inspection first |
| Use case | Simple, trusted branches | Complex or shared branches |

```bash
# Best practice workflow
git fetch origin
git log HEAD..origin/main   # review incoming commits
git merge origin/main       # merge when ready
```

---

## 7. Git Stash

`git stash` temporarily shelves (stashes) changes in your working directory so you can switch context without committing incomplete work.

### Common Commands

```bash
# Stash current changes
git stash

# Stash with a descriptive message
git stash push -m "WIP: login feature"

# List all stashes
git stash list

# Apply the most recent stash (keeps stash)
git stash apply

# Apply and remove the most recent stash
git stash pop

# Apply a specific stash
git stash apply stash@{2}

# Drop a specific stash
git stash drop stash@{1}

# Clear all stashes
git stash clear

# Stash including untracked files
git stash push --include-untracked
```

### Typical Workflow

```bash
# You're mid-feature and need to fix a hotfix
git stash push -m "WIP: user profile"
git checkout main
# ... fix the bug, commit ...
git checkout feature/user-profile
git stash pop
```

---

## 8. popd & pushd

`pushd` and `popd` are shell built-ins that manage a **directory stack**, allowing quick navigation between directories.

### Commands

```bash
# Push current directory onto stack and cd into new dir
pushd /path/to/directory

# Pop the top directory off the stack and return to it
popd

# View the current directory stack
dirs

# Push without changing directory
pushd -n /some/path

# Rotate the stack (go to 2nd entry)
pushd +1
```

### Example Session

```bash
$ pwd
/home/user/projects

$ pushd /etc
/etc /home/user/projects    ← stack shows current, then previous

$ pushd /var/log
/var/log /etc /home/user/projects

$ popd
/etc /home/user/projects    ← back to /etc

$ popd
/home/user/projects         ← back to original
```

### When to Use
- Navigating deep directory structures during scripting
- Temporarily switching directories and returning cleanly

---

## 9. Git Merge — Ours/Theirs Method

When resolving merge conflicts, Git provides **merge strategies** and **options** to automatically favor one side.

### Ours vs Theirs

| Term | Refers To |
|---|---|
| **Ours** | The current branch (the one you're merging into) |
| **Theirs** | The branch being merged in |

### Using `-X` Strategy Options

```bash
# Favor our branch on all conflicts
git merge -X ours feature-branch

# Favor their branch on all conflicts
git merge -X theirs feature-branch
```

### Resolving Individual Files

```bash
# Keep our version of a specific file
git checkout --ours src/config.java
git add src/config.java

# Keep their version of a specific file
git checkout --theirs src/config.java
git add src/config.java
```

### The `ours` Merge Strategy (Different!)

```bash
# This strategy keeps ALL of our content and discards theirs entirely
git merge -s ours old-branch
```

> ⚠️ `-s ours` (strategy) is different from `-X ours` (strategy option). The former discards the other branch entirely; the latter only resolves conflict hunks in favor of ours.

---

## 10. Log

`git log` displays the commit history of a repository.

### Common Commands

```bash
# Full commit history
git log

# One line per commit
git log --oneline

# Visual branch graph
git log --oneline --graph --all

# Last N commits
git log -5

# Commits by a specific author
git log --author="Jane Doe"

# Commits in a date range
git log --after="2024-01-01" --before="2024-06-01"

# Commits that changed a specific file
git log -- src/Main.java

# Search commit messages
git log --grep="fix"

# Show diff for each commit
git log -p

# Show stats (files changed, insertions, deletions)
git log --stat
```

### Formatting Output

```bash
# Custom format
git log --pretty=format:"%h - %an, %ar : %s"
# Output: abc1234 - Jane Doe, 2 days ago : Fix login bug
```

---

## 11. Config

`git config` manages Git configuration settings at local, global, or system level.

### Configuration Levels

| Level | Scope | File Location |
|---|---|---|
| `--local` | Current repo only | `.git/config` |
| `--global` | Current user | `~/.gitconfig` |
| `--system` | All users on machine | `/etc/gitconfig` |

### Common Commands

```bash
# Set your identity
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Set default editor
git config --global core.editor "vim"

# Set default branch name
git config --global init.defaultBranch main

# View all config settings
git config --list

# View a specific value
git config user.email

# Set default merge strategy
git config --global pull.rebase false

# Enable colored output
git config --global color.ui auto

# Create a shortcut alias
git config --global alias.st status
git config --global alias.lg "log --oneline --graph --all"
```

---

## 12. Fetch

`git fetch` downloads objects and refs from a remote repository **without** merging them into your working branch.

### Common Commands

```bash
# Fetch all branches from origin
git fetch origin

# Fetch a specific branch
git fetch origin main

# Fetch from all remotes
git fetch --all

# Fetch and prune deleted remote branches
git fetch --prune
# or shorthand:
git fetch -p

# Fetch tags
git fetch --tags
```

### After Fetching

```bash
# See what was fetched
git log HEAD..origin/main --oneline

# Compare local branch with remote
git diff main origin/main

# Merge after reviewing
git merge origin/main
```

### Fetch vs Pull

`fetch` is safe — it never modifies your local working files. Always prefer `fetch` when working on shared branches so you can inspect incoming changes before integrating them.

---

## 13. Submodule

Git submodules allow you to embed one Git repository inside another, keeping them as separate projects with independent histories.

### Common Commands

```bash
# Add a submodule
git submodule add https://github.com/org/repo libs/repo

# Initialize submodules after cloning
git submodule init

# Fetch and checkout submodule content
git submodule update

# Clone a repo and initialize all submodules in one step
git clone --recurse-submodules https://github.com/org/main-repo

# Update all submodules to their latest remote commit
git submodule update --remote

# Run a command in each submodule
git submodule foreach git pull origin main

# Remove a submodule
git submodule deinit libs/repo
git rm libs/repo
```

### Key File: `.gitmodules`

```ini
[submodule "libs/repo"]
    path = libs/repo
    url = https://github.com/org/repo
    branch = main
```

---

## 14. Compiler

The **Maven Compiler Plugin** compiles Java source files. It wraps the `javac` compiler and is one of the most essential Maven plugins.

### Maven Integration

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-compiler-plugin</artifactId>
  <version>3.11.0</version>
  <configuration>
    <source>17</source>
    <target>17</target>
    <encoding>UTF-8</encoding>
  </configuration>
</plugin>
```

### Common Commands

```bash
# Compile main sources
mvn compile

# Compile test sources
mvn test-compile

# Clean and recompile
mvn clean compile
```

### Key Configuration Options

| Parameter | Description |
|---|---|
| `source` | Java source compatibility version |
| `target` | Bytecode target version |
| `encoding` | Source file encoding |
| `compilerArgs` | Additional `javac` arguments |
| `showWarnings` | Enable compiler warnings |

---

## 15. Surefire

The **Maven Surefire Plugin** runs unit tests during the `test` phase of the Maven lifecycle.

### Maven Integration

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-surefire-plugin</artifactId>
  <version>3.1.2</version>
</plugin>
```

### Common Commands

```bash
# Run all tests
mvn test

# Skip tests
mvn install -DskipTests

# Run a specific test class
mvn test -Dtest=MyServiceTest

# Run a specific test method
mvn test -Dtest=MyServiceTest#testLogin

# Run tests matching a pattern
mvn test -Dtest="*ServiceTest"
```

### Configuration Options

```xml
<configuration>
  <!-- Run tests in parallel -->
  <parallel>methods</parallel>
  <threadCount>4</threadCount>

  <!-- Exclude certain tests -->
  <excludes>
    <exclude>**/SlowTest.java</exclude>
  </excludes>

  <!-- Fail build if no tests found -->
  <failIfNoTests>true</failIfNoTests>
</configuration>
```

### Test Reports
Reports are generated in `target/surefire-reports/` as XML and plain-text files.

---

## 16. JaCoCo

**JaCoCo** (Java Code Coverage) is the standard code coverage library for Java, widely used with Maven and Gradle.

### Maven Integration

```xml
<plugin>
  <groupId>org.jacoco</groupId>
  <artifactId>jacoco-maven-plugin</artifactId>
  <version>0.8.10</version>
  <executions>
    <execution>
      <goals><goal>prepare-agent</goal></goals>
    </execution>
    <execution>
      <id>report</id>
      <phase>test</phase>
      <goals><goal>report</goal></goals>
    </execution>
  </executions>
</plugin>
```

### Common Commands

```bash
# Run tests and generate coverage report
mvn test jacoco:report

# Enforce minimum coverage thresholds
mvn verify
```

### Coverage Report
HTML report generated at: `target/site/jacoco/index.html`

### Coverage Thresholds

```xml
<execution>
  <id>check</id>
  <goals><goal>check</goal></goals>
  <configuration>
    <rules>
      <rule>
        <limits>
          <limit>
            <counter>LINE</counter>
            <value>COVEREDRATIO</value>
            <minimum>0.80</minimum> <!-- 80% line coverage required -->
          </limit>
        </limits>
      </rule>
    </rules>
  </configuration>
</execution>
```

---

## 17. OWASP Dependency Check

The **OWASP Dependency-Check** plugin scans project dependencies for known security vulnerabilities using the National Vulnerability Database (NVD).

### Maven Integration

```xml
<plugin>
  <groupId>org.owasp</groupId>
  <artifactId>dependency-check-maven</artifactId>
  <version>8.4.0</version>
</plugin>
```

### Common Commands

```bash
# Run vulnerability scan
mvn dependency-check:check

# Generate report without failing the build
mvn dependency-check:aggregate

# Update the NVD database
mvn dependency-check:update-only
```

### Configuration

```xml
<configuration>
  <!-- Fail build if CVSS score is 7 or higher (High/Critical) -->
  <failBuildOnCVSS>7</failBuildOnCVSS>

  <!-- Suppress false positives -->
  <suppressionFile>suppressions.xml</suppressionFile>

  <!-- Report formats -->
  <formats>
    <format>HTML</format>
    <format>JSON</format>
  </formats>
</configuration>
```

### Reports
Generated at `target/dependency-check-report.html`.

---

## 18. SonarQube

**SonarQube** is a continuous code quality and security inspection platform. It analyzes code for bugs, vulnerabilities, code smells, and coverage.

### Key Concepts

| Term | Description |
|---|---|
| **Bug** | Code that is clearly wrong or likely to cause errors |
| **Vulnerability** | Security issue that could be exploited |
| **Code Smell** | Maintainability issue that may cause future problems |
| **Technical Debt** | Estimated time to fix all code smells |
| **Quality Gate** | Pass/fail threshold for code quality metrics |

### Maven Integration

```xml
<plugin>
  <groupId>org.sonarsource.scanner.maven</groupId>
  <artifactId>sonar-maven-plugin</artifactId>
  <version>3.10.0.2594</version>
</plugin>
```

### Common Commands

```bash
# Run analysis (requires SonarQube server)
mvn sonar:sonar \
  -Dsonar.projectKey=my-project \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=your-token

# Run with coverage (JaCoCo must run first)
mvn clean verify sonar:sonar
```

### SonarCloud (Cloud-hosted)

```bash
mvn sonar:sonar \
  -Dsonar.organization=my-org \
  -Dsonar.host.url=https://sonarcloud.io \
  -Dsonar.login=$SONAR_TOKEN
```

---

## 19. Deploy

The **Maven Deploy Plugin** uploads built artifacts (JARs, WARs, POMs) to a remote Maven repository for sharing with team members or CI/CD pipelines.

### Maven Integration

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-deploy-plugin</artifactId>
  <version>3.1.1</version>
</plugin>
```

### Common Commands

```bash
# Build and deploy to remote repository
mvn deploy

# Skip tests when deploying
mvn deploy -DskipTests

# Deploy to a specific repository URL
mvn deploy -DaltDeploymentRepository=myRepo::default::https://repo.example.com/releases
```

### Repository Configuration in `pom.xml`

```xml
<distributionManagement>
  <repository>
    <id>releases</id>
    <url>https://nexus.example.com/repository/maven-releases/</url>
  </repository>
  <snapshotRepository>
    <id>snapshots</id>
    <url>https://nexus.example.com/repository/maven-snapshots/</url>
  </snapshotRepository>
</distributionManagement>
```

### Credentials in `settings.xml`

```xml
<servers>
  <server>
    <id>releases</id>
    <username>deploy-user</username>
    <password>secret</password>
  </server>
</servers>
```

---

## 20. SpotBugs

**SpotBugs** is the spiritual successor to [FindBugs](#2-findbugs), actively maintained and compatible with modern Java versions. It analyzes Java bytecode for bug patterns.

### Improvements Over FindBugs
- Supports Java 8+ (including lambdas and streams)
- Active community maintenance
- Better IDE and CI integration
- Compatible with FindBugs plugins (e.g., `fb-contrib`, `find-sec-bugs`)

### Maven Integration

```xml
<plugin>
  <groupId>com.github.spotbugs</groupId>
  <artifactId>spotbugs-maven-plugin</artifactId>
  <version>4.8.2.0</version>
  <configuration>
    <effort>Max</effort>
    <threshold>Low</threshold>
    <xmlOutput>true</xmlOutput>
  </configuration>
</plugin>
```

### Common Commands

```bash
# Fail build on bugs found
mvn spotbugs:check

# Generate HTML/XML report without failing
mvn spotbugs:spotbugs

# Launch the SpotBugs GUI
mvn spotbugs:gui
```

### Adding Security Plugin (`find-sec-bugs`)

```xml
<configuration>
  <plugins>
    <plugin>
      <groupId>com.h3xstream.findsecbugs</groupId>
      <artifactId>findsecbugs-plugin</artifactId>
      <version>1.12.0</version>
    </plugin>
  </plugins>
</configuration>
```

---

*End of Third Week Session Reference Guide*
