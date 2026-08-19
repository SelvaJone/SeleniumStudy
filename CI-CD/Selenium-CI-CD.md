# Selenium CI/CD

## Table of Contents

1. [What is CI/CD?](#1-what-is-cicd)
2. [Why CI/CD is Important for Selenium](#2-why-cicd-is-important-for-selenium)
3. [CI vs CD](#3-ci-vs-cd)
4. [Selenium in CI/CD](#4-selenium-in-cicd)
5. [Typical Selenium CI/CD Architecture](#5-typical-selenium-cicd-architecture)
6. [Tools Used](#6-tools-used)
7. [Git and GitHub](#7-git-and-github)
8. [Maven](#8-maven)
9. [Running Selenium Tests with Maven](#9-running-selenium-tests-with-maven)
10. [TestNG XML in CI/CD](#10-testng-xml-in-cicd)
11. [Jenkins](#11-jenkins)
12. [Jenkins Installation](#12-jenkins-installation)
13. [Create a Jenkins Job](#13-create-a-jenkins-job)
14. [Jenkins Freestyle Job](#14-jenkins-freestyle-job)
15. [Jenkins Pipeline](#15-jenkins-pipeline)
16. [Jenkinsfile](#16-jenkinsfile)
17. [Maven in Jenkins](#17-maven-in-jenkins)
18. [Jenkins Parameters](#18-jenkins-parameters)
19. [Browser Parameter](#19-browser-parameter)
20. [Environment Parameter](#20-environment-parameter)
21. [Running TestNG from Jenkins](#21-running-testng-from-jenkins)
22. [Scheduled Execution](#22-scheduled-execution)
23. [GitHub Integration](#23-github-integration)
24. [Webhook](#24-webhook)
25. [Jenkins Pipeline Stages](#25-jenkins-pipeline-stages)
26. [Pipeline with Selenium Tests](#26-pipeline-with-selenium-tests)
27. [Headless Selenium](#27-headless-selenium)
28. [Selenium Grid + Jenkins](#28-selenium-grid--jenkins)
29. [Parallel Execution](#29-parallel-execution)
30. [TestNG Reports in Jenkins](#30-testng-reports-in-jenkins)
31. [Extent Reports in Jenkins](#31-extent-reports-in-jenkins)
32. [Allure Reports in Jenkins](#32-allure-reports-in-jenkins)
33. [Screenshots and Artifacts](#33-screenshots-and-artifacts)
34. [Environment Variables](#34-environment-variables)
35. [Credentials](#35-credentials)
36. [Secrets Management](#36-secrets-management)
37. [Docker and Selenium](#37-docker-and-selenium)
38. [CI/CD with Selenium Grid](#38-cicd-with-selenium-grid)
39. [Complete Jenkins Pipeline](#39-complete-jenkins-pipeline)
40. [Complete Project Structure](#40-complete-project-structure)
41. [CI/CD Best Practices](#41-cicd-best-practices)
42. [Common CI/CD Problems](#42-common-cicd-problems)
43. [Interview Questions](#43-interview-questions)
44. [Quick Revision](#44-quick-revision)

---

# 1. What is CI/CD?

CI/CD stands for:

```text
CI = Continuous Integration
CD = Continuous Delivery / Continuous Deployment
```

CI/CD automates the process of:

```text
Code
 ↓
Build
 ↓
Test
 ↓
Report
 ↓
Deploy
```

For Selenium automation:

```text
Developer pushes code
        ↓
GitHub
        ↓
Jenkins
        ↓
Maven
        ↓
TestNG
        ↓
Selenium
        ↓
Browser
        ↓
Test Results
        ↓
Reports
```

---

# 2. Why CI/CD is Important for Selenium

Without CI/CD, testers may manually execute:

```bash
mvn test
```

every time.

With CI/CD:

```text
Git Push
   ↓
Jenkins
   ↓
Automated Tests
   ↓
Reports
```

Tests can execute automatically.

Benefits:

* Early defect detection
* Faster feedback
* Automated regression
* Consistent execution
* Scheduled testing
* Parallel testing
* Continuous validation
* Centralized reports
* Integration with development workflows

---

# 3. CI vs CD

## Continuous Integration

Developers frequently merge code into a shared repository.

Pipeline:

```text
Code Commit
    ↓
Build
    ↓
Unit Tests
    ↓
Automation Tests
    ↓
Report
```

---

## Continuous Delivery

Code is automatically built and tested and is kept ready for deployment.

```text
Code
 ↓
Build
 ↓
Test
 ↓
Package
 ↓
Ready for Deployment
```

---

## Continuous Deployment

Code that passes the pipeline can be automatically deployed.

```text
Code
 ↓
Build
 ↓
Test
 ↓
Deploy
 ↓
Production
```

---

# 4. Selenium in CI/CD

Selenium usually belongs in the testing stage.

Example:

```text
                CI/CD
                  |
        +---------+---------+
        |         |         |
      Build      Test     Deploy
                  |
              Selenium
                  |
              TestNG
                  |
              Browsers
```

A typical QA pipeline:

```text
GitHub
  ↓
Jenkins
  ↓
Maven Build
  ↓
Unit Tests
  ↓
Selenium Tests
  ↓
TestNG
  ↓
Extent/Allure
  ↓
Jenkins Report
```

---

# 5. Typical Selenium CI/CD Architecture

```text
Developer
    |
    | git push
    v
GitHub Repository
    |
    | webhook
    v
Jenkins
    |
    +------------------+
    |                  |
    v                  v
Maven Build        TestNG Suite
                       |
                       v
                 Selenium WebDriver
                       |
              +--------+--------+
              |        |        |
            Chrome   Firefox   Edge
              |        |        |
              +--------+--------+
                       |
                       v
                  Test Results
                       |
              +--------+--------+
              |                 |
              v                 v
         Extent Report     Allure Report
              |                 |
              +--------+--------+
                       |
                       v
                    Jenkins
```

---

# 6. Tools Used

Common tools in a Selenium CI/CD framework:

```text
Java
Selenium WebDriver
TestNG
Maven
Git
GitHub
Jenkins
Selenium Grid
Docker
ExtentReports
Allure
```

Optional:

```text
AWS
Azure
GitLab
Bitbucket
Kubernetes
BrowserStack
Sauce Labs
```

---

# 7. Git and GitHub

Git is a distributed version control system.

GitHub is a platform for hosting Git repositories and collaborating around source code.

Typical workflow:

```bash
git clone <repository>
cd SeleniumFramework

git status

git add .

git commit -m "Add Selenium tests"

git push origin main
```

A push can trigger Jenkins automatically.

---

# 8. Maven

Maven is a build and dependency management tool for Java.

Maven can:

* Download dependencies
* Compile code
* Run tests
* Generate reports
* Package applications
* Execute plugins

Important file:

```text
pom.xml
```

Example:

```xml
<project>

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>

    <artifactId>selenium-framework</artifactId>

    <version>1.0</version>

    <dependencies>

        <dependency>
            <groupId>
                org.seleniumhq.selenium
            </groupId>

            <artifactId>
                selenium-java
            </artifactId>

            <version>4.x.x</version>
        </dependency>

        <dependency>
            <groupId>
                org.testng
            </groupId>

            <artifactId>
                testng
            </artifactId>

            <version>7.x.x</version>

            <scope>test</scope>
        </dependency>

    </dependencies>

</project>
```

Use versions compatible with the rest of the project.

---

# 9. Running Selenium Tests with Maven

From the project directory:

```bash
mvn test
```

Clean and test:

```bash
mvn clean test
```

Skip tests:

```bash
mvn clean package -DskipTests
```

Run a specific TestNG suite through Surefire configuration:

```bash
mvn clean test
```

The suite selection can be configured in `pom.xml`.

---

# 10. TestNG XML in CI/CD

Example:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE suite SYSTEM
    "https://testng.org/testng-1.0.dtd">

<suite name="Regression Suite">

    <test name="Regression Tests">

        <classes>

            <class name="tests.LoginTest"/>
            <class name="tests.SearchTest"/>
            <class name="tests.CheckoutTest"/>

        </classes>

    </test>

</suite>
```

Maven Surefire can execute this suite.

Example:

```xml
<build>

    <plugins>

        <plugin>

            <groupId>
                org.apache.maven.plugins
            </groupId>

            <artifactId>
                maven-surefire-plugin
            </artifactId>

            <version>3.5.3</version>

            <configuration>

                <suiteXmlFiles>

                    <suiteXmlFile>
                        testng.xml
                    </suiteXmlFile>

                </suiteXmlFiles>

            </configuration>

        </plugin>

    </plugins>

</build>
```

---

# 11. Jenkins

Jenkins is an automation server widely used for CI/CD.

Jenkins can:

```text
Pull source code
Build projects
Run tests
Run Selenium
Publish reports
Archive artifacts
Send notifications
Schedule jobs
Deploy applications
```

Typical Selenium workflow:

```text
GitHub
 ↓
Jenkins
 ↓
Maven
 ↓
TestNG
 ↓
Selenium
 ↓
Reports
```

---

# 12. Jenkins Installation

General process:

```text
1. Install Java
2. Install Jenkins
3. Start Jenkins
4. Open Jenkins in browser
5. Unlock Jenkins
6. Install recommended plugins
7. Create admin user
8. Configure tools
```

Jenkins commonly runs on:

```text
http://localhost:8080
```

The actual port can be changed during configuration.

---

# 13. Create a Jenkins Job

Basic steps:

```text
Jenkins
 ↓
New Item
 ↓
Enter job name
 ↓
Pipeline / Freestyle Project
 ↓
Configure
 ↓
Save
 ↓
Build Now
```

For modern projects, Pipeline jobs are generally preferred.

---

# 14. Jenkins Freestyle Job

A Freestyle job can execute shell commands.

Example build command:

```bash
mvn clean test
```

Windows:

```bat
mvn clean test
```

Linux:

```bash
mvn clean test
```

However, Pipeline jobs provide better version-controlled configuration.

---

# 15. Jenkins Pipeline

A Jenkins Pipeline defines CI/CD stages as code.

Example:

```groovy
pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

    }
}
```

For Windows agents:

```groovy
bat 'mvn clean test'
```

---

# 16. Jenkinsfile

A `Jenkinsfile` contains the pipeline definition.

Recommended location:

```text
SeleniumFramework/
│
├── Jenkinsfile
├── pom.xml
├── testng.xml
└── src/
```

Example:

```groovy
pipeline {

    agent any

    stages {

        stage('Checkout') {

            steps {
                checkout scm
            }
        }

        stage('Build') {

            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {

            steps {
                sh 'mvn test'
            }
        }

    }

    post {

        always {
            echo 'Pipeline completed'
        }

        success {
            echo 'Tests passed'
        }

        failure {
            echo 'Tests failed'
        }
    }
}
```

---

# 17. Maven in Jenkins

Jenkins should have access to:

```text
Java
Maven
Git
Browser
Selenium/Grid
```

Depending on the agent setup.

Example:

```groovy
stage('Run Tests') {

    steps {

        sh '''
            mvn clean test
        '''
    }
}
```

Windows:

```groovy
stage('Run Tests') {

    steps {

        bat '''
            mvn clean test
        '''
    }
}
```

---

# 18. Jenkins Parameters

Parameters allow users to select test configurations.

Example:

```groovy
pipeline {

    agent any

    parameters {

        choice(
            name: 'BROWSER',
            choices: [
                'chrome',
                'firefox',
                'edge'
            ],
            description:
                'Select browser'
        )

        choice(
            name: 'ENVIRONMENT',
            choices: [
                'qa',
                'stage',
                'prod'
            ],
            description:
                'Select environment'
        )
    }

    stages {

        stage('Test') {

            steps {

                sh """
                    mvn clean test
                    -Dbrowser=${BROWSER}
                    -Denvironment=${ENVIRONMENT}
                """
            }
        }
    }
}
```

---

# 19. Browser Parameter

Java can retrieve a system property:

```java
String browser =
    System.getProperty(
        "browser",
        "chrome"
    );
```

Run:

```bash
mvn clean test -Dbrowser=chrome
```

Or:

```bash
mvn clean test -Dbrowser=firefox
```

Example factory:

```java
public static WebDriver createDriver(
        String browser) {

    if (browser.equalsIgnoreCase("chrome")) {

        return new ChromeDriver();

    } else if (
        browser.equalsIgnoreCase("firefox")) {

        return new FirefoxDriver();

    } else if (
        browser.equalsIgnoreCase("edge")) {

        return new EdgeDriver();

    }

    throw new IllegalArgumentException(
        "Unsupported browser: " + browser
    );
}
```

---

# 20. Environment Parameter

Java:

```java
String environment =
    System.getProperty(
        "environment",
        "qa"
    );
```

Run:

```bash
mvn clean test -Denvironment=qa
```

or:

```bash
mvn clean test -Denvironment=stage
```

Configuration:

```java
switch (environment.toLowerCase()) {

    case "qa":
        url = "https://qa.example.com";
        break;

    case "stage":
        url = "https://stage.example.com";
        break;

    default:
        throw new IllegalArgumentException(
            "Invalid environment"
        );
}
```

Never hard-code sensitive credentials into the source code.

---

# 21. Running TestNG from Jenkins

Simple pipeline:

```groovy
pipeline {

    agent any

    stages {

        stage('Run Selenium Tests') {

            steps {

                sh 'mvn clean test'

            }
        }
    }
}
```

Maven reads the configured TestNG suite from `pom.xml`.

---

# 22. Scheduled Execution

Jenkins can execute regression tests automatically.

Example cron:

```text
H 2 * * *
```

This represents a daily run around the configured 2 AM hour, with Jenkins distributing the exact minute.

Other examples:

```text
H * * *
```

Approximately hourly.

```text
H H * * 1-5
```

Once on each weekday at a distributed time.

Scheduling syntax should be configured according to the team's desired execution window.

---

# 23. GitHub Integration

Typical workflow:

```text
Developer
   |
   | git push
   v
GitHub
   |
   | webhook
   v
Jenkins
   |
   v
Pipeline
   |
   v
Selenium Tests
```

Jenkins can check out the repository using Git credentials or an appropriate GitHub integration.

---

# 24. Webhook

A webhook allows GitHub to notify Jenkins when repository activity occurs.

Example:

```text
GitHub Push
    ↓
Webhook
    ↓
Jenkins
    ↓
Pipeline
```

This avoids waiting for a scheduled polling interval.

Typical configuration:

```text
GitHub Repository
    ↓
Settings
    ↓
Webhooks
    ↓
Jenkins URL
```

For production systems, webhook security and authentication should be configured appropriately.

---

# 25. Jenkins Pipeline Stages

A mature Selenium pipeline can contain:

```text
Checkout
   ↓
Install Dependencies
   ↓
Compile
   ↓
Unit Tests
   ↓
Smoke Tests
   ↓
Regression Tests
   ↓
Reports
   ↓
Archive Artifacts
```

Example:

```groovy
stages {

    stage('Checkout') {
        steps {
            checkout scm
        }
    }

    stage('Build') {
        steps {
            sh 'mvn clean compile'
        }
    }

    stage('Smoke Tests') {
        steps {
            sh 'mvn test -Dgroups=smoke'
        }
    }

    stage('Regression Tests') {
        steps {
            sh 'mvn test -Dgroups=regression'
        }
    }
}
```

---

# 26. Pipeline with Selenium Tests

Complete basic example:

```groovy
pipeline {

    agent any

    parameters {

        choice(
            name: 'BROWSER',
            choices: [
                'chrome',
                'firefox'
            ],
            description: 'Browser'
        )

        choice(
            name: 'ENVIRONMENT',
            choices: [
                'qa',
                'stage'
            ],
            description: 'Environment'
        )
    }

    stages {

        stage('Checkout') {

            steps {
                checkout scm
            }
        }

        stage('Build') {

            steps {

                sh '''
                    mvn clean compile
                '''
            }
        }

        stage('Automation Tests') {

            steps {

                sh """
                    mvn test
                    -Dbrowser=${BROWSER}
                    -Denvironment=${ENVIRONMENT}
                """
            }
        }
    }

    post {

        always {

            archiveArtifacts(
                artifacts:
                    'reports/**/*',
                allowEmptyArchive: true
            )
        }

        success {
            echo 'Automation passed'
        }

        failure {
            echo 'Automation failed'
        }
    }
}
```

For Windows agents, replace `sh` with `bat`.

---

# 27. Headless Selenium

CI servers may not have a graphical desktop.

Chrome can run headlessly:

```java
ChromeOptions options =
    new ChromeOptions();

options.addArguments(
    "--headless=new"
);

WebDriver driver =
    new ChromeDriver(options);
```

Other useful options may include:

```java
options.addArguments(
    "--window-size=1920,1080"
);
```

Headless mode is commonly used in CI environments.

---

# 28. Selenium Grid + Jenkins

Jenkins can trigger Selenium tests that execute on Grid.

Architecture:

```text
                 Jenkins
                    |
                    v
                 Maven
                    |
                    v
                 TestNG
                    |
                    v
             RemoteWebDriver
                    |
                    v
              Selenium Grid
             /      |      \
            /       |       \
       Chrome    Firefox    Edge
        Node       Node      Node
```

Example:

```java
URL gridUrl =
    new URL("http://grid-host:4444");

WebDriver driver =
    new RemoteWebDriver(
        gridUrl,
        new ChromeOptions()
    );
```

The Grid URL should normally come from configuration rather than being hard-coded.

---

# 29. Parallel Execution

TestNG:

```xml
<suite
    name="Regression"
    parallel="tests"
    thread-count="4">
```

Jenkins:

```groovy
stage('Regression') {

    steps {

        sh 'mvn clean test'
    }
}
```

For parallel Selenium tests:

```text
Thread 1 → Driver 1
Thread 2 → Driver 2
Thread 3 → Driver 3
Thread 4 → Driver 4
```

Use `ThreadLocal<WebDriver>` when multiple test threads share the same driver-management framework.

---

# 30. TestNG Reports in Jenkins

TestNG commonly creates:

```text
test-output/
```

Jenkins can archive or publish generated result files depending on the configured reporting plugins.

Maven Surefire reports:

```text
target/surefire-reports/
```

These can be used by Jenkins for test-result processing.

---

# 31. Extent Reports in Jenkins

If ExtentReports generates:

```text
reports/ExtentReport.html
```

Jenkins can archive it:

```groovy
post {

    always {

        archiveArtifacts(
            artifacts:
                'reports/**/*.html',
            allowEmptyArchive: true
        )
    }
}
```

Screenshots:

```groovy
archiveArtifacts(
    artifacts:
        'reports/screenshots/**/*',
    allowEmptyArchive: true
)
```

This makes the report and screenshots available from the Jenkins build.

---

# 32. Allure Reports in Jenkins

Typical flow:

```text
Selenium Tests
      ↓
Allure Results
      ↓
Jenkins Allure Integration
      ↓
Allure HTML Report
```

Allure results are typically generated under:

```text
allure-results/
```

The report can then be generated and published through the configured Allure tooling.

---

# 33. Screenshots and Artifacts

Useful CI artifacts:

```text
ExtentReport.html
Allure results
Screenshots
Videos
Logs
TestNG XML
Surefire XML
Application logs
```

Example Jenkins pipeline:

```groovy
post {

    always {

        archiveArtifacts(
            artifacts:
                'reports/**/*',
            allowEmptyArchive: true
        )

        archiveArtifacts(
            artifacts:
                'target/surefire-reports/**/*',
            allowEmptyArchive: true
        )
    }
}
```

---

# 34. Environment Variables

Environment variables are useful for configuration.

Example:

```groovy
environment {

    TEST_ENV = 'qa'
    BROWSER = 'chrome'
}
```

Access them:

```groovy
echo "${TEST_ENV}"
echo "${BROWSER}"
```

Java can read operating-system environment variables:

```java
String environment =
    System.getenv("TEST_ENV");
```

System properties are different:

```java
String environment =
    System.getProperty(
        "environment"
    );
```

Command-line example:

```bash
mvn test -Denvironment=qa
```

---

# 35. Credentials

Never hard-code:

```text
Username
Password
API Token
Access Token
Private Key
```

Bad:

```java
String password = "MyPassword123";
```

Better:

```text
Jenkins Credentials
        ↓
Environment / Secret
        ↓
Test
```

Example Jenkins:

```groovy
environment {

    API_TOKEN = credentials(
        'my-api-token'
    )
}
```

Credentials should be managed through Jenkins Credentials or an enterprise secrets-management solution.

---

# 36. Secrets Management

Good practice:

```text
Source Code
    ❌ Password
    ❌ API Token
    ❌ Private Key

Jenkins Credentials
    ✅ Username
    ✅ Password
    ✅ Token
    ✅ SSH Key
```

Also avoid printing secrets in logs:

```groovy
echo "Password=${PASSWORD}"
```

Never intentionally expose credentials in pipeline logs.

---

# 37. Docker and Selenium

Docker can provide consistent execution environments.

Example architecture:

```text
Jenkins
   |
   v
Docker
   |
   +---- Selenium Hub
   |
   +---- Chrome Node
   |
   +---- Firefox Node
```

Advantages:

* Consistent environment
* Easy setup
* Isolation
* Reproducibility
* Scalable Grid infrastructure

---

# 38. CI/CD with Selenium Grid

Enterprise architecture:

```text
                     GitHub
                        |
                        v
                     Jenkins
                        |
                        v
                      Maven
                        |
                        v
                     TestNG
                        |
                        v
                Selenium Grid
                /      |      \
               /       |       \
          Chrome    Firefox    Edge
             |         |         |
             +---------+---------+
                       |
                       v
                 Test Results
                       |
          +------------+------------+
          |                         |
          v                         v
    Extent / Allure          Screenshots
          |                         |
          +------------+------------+
                       |
                       v
                    Jenkins
```

---

# 39. Complete Jenkins Pipeline

A more complete example:

```groovy
pipeline {

    agent any

    parameters {

        choice(
            name: 'BROWSER',
            choices: [
                'chrome',
                'firefox',
                'edge'
            ],
            description:
                'Browser for automation'
        )

        choice(
            name: 'ENVIRONMENT',
            choices: [
                'qa',
                'stage'
            ],
            description:
                'Application environment'
        )

        choice(
            name: 'TEST_GROUP',
            choices: [
                'smoke',
                'regression'
            ],
            description:
                'Test group'
        )
    }

    environment {

        MAVEN_OPTS =
            '-Dmaven.test.failure.ignore=false'
    }

    stages {

        stage('Checkout') {

            steps {

                checkout scm
            }
        }

        stage('Build') {

            steps {

                echo 'Building project'

                sh '''
                    mvn clean compile
                '''
            }
        }

        stage('Run Tests') {

            steps {

                echo "Browser: ${BROWSER}"

                echo "Environment: ${ENVIRONMENT}"

                echo "Test Group: ${TEST_GROUP}"

                sh """
                    mvn test \
                    -Dbrowser=${BROWSER} \
                    -Denvironment=${ENVIRONMENT} \
                    -Dgroups=${TEST_GROUP}
                """
            }
        }
    }

    post {

        always {

            echo 'Collecting test artifacts'

            archiveArtifacts(
                artifacts:
                    'reports/**/*',
                allowEmptyArchive: true
            )

            archiveArtifacts(
                artifacts:
                    'target/surefire-reports/**/*',
                allowEmptyArchive: true
            )
        }

        success {

            echo 'Pipeline completed successfully'
        }

        failure {

            echo 'Pipeline failed'
        }
    }
}
```

---

# 40. Complete Project Structure

Recommended Selenium CI/CD framework:

```text
SeleniumFramework
│
├── .gitignore
├── README.md
├── pom.xml
├── testng.xml
├── Jenkinsfile
│
├── src
│   │
│   ├── main
│   │   └── java
│   │       │
│   │       ├── base
│   │       │   ├── DriverFactory.java
│   │       │   └── BasePage.java
│   │       │
│   │       ├── pages
│   │       │   ├── LoginPage.java
│   │       │   └── HomePage.java
│   │       │
│   │       ├── utilities
│   │       │   ├── ConfigReader.java
│   │       │   ├── ScreenshotUtil.java
│   │       │   └── ExtentManager.java
│   │       │
│   │       └── listeners
│   │           └── ExtentListener.java
│   │
│   └── test
│       └── java
│           │
│           ├── tests
│           │   ├── LoginTest.java
│           │   └── SearchTest.java
│           │
│           └── data
│               └── TestData.java
│
├── reports
│   └── screenshots
│
└── target
    └── surefire-reports
```

---

# 41. CI/CD Best Practices

## 1. Store Jenkinsfile in Git

Keep:

```text
Jenkinsfile
```

inside the repository.

This makes the pipeline version-controlled.

---

## 2. Do not hard-code environments

Avoid:

```java
driver.get(
    "https://qa.example.com"
);
```

Prefer configuration:

```java
String url =
    ConfigReader.getUrl(
        environment
    );
```

---

## 3. Do not hard-code credentials

Use:

```text
Jenkins Credentials
```

or another approved secrets manager.

---

## 4. Keep smoke tests fast

Smoke tests should provide quick feedback.

Example:

```text
Smoke
 ↓
Login
 ↓
Search
 ↓
Critical workflow
```

Then execute the full regression suite separately.

---

## 5. Run regression on schedule

Example:

```text
Every night
      ↓
Regression Suite
      ↓
Selenium Grid
      ↓
Reports
```

---

## 6. Use parallel execution

Parallel execution can reduce overall execution time.

Example:

```text
Sequential:

Test 1 → Test 2 → Test 3 → Test 4

Parallel:

Test 1 ─┐
Test 2 ─┼→ Results
Test 3 ─┤
Test 4 ─┘
```

---

## 7. Archive important artifacts

Keep:

```text
Reports
Screenshots
Logs
Test results
```

for failed builds and important releases according to team retention policies.

---

## 8. Fail the pipeline when appropriate

If critical automation fails, the pipeline should normally report failure.

Avoid:

```bash
mvn test || true
```

unless there is a deliberate reason to allow failures.

---

## 9. Separate test environments

Use:

```text
QA
Stage
Production
```

through configuration.

Be especially careful when running destructive tests against production.

---

## 10. Keep CI tests stable

Avoid excessive:

```java
Thread.sleep()
```

Use explicit waits and robust synchronization.

---

# 42. Common CI/CD Problems

## Problem 1: Chrome does not start

Possible causes:

```text
Browser not installed
Driver/browser incompatibility
Missing Linux dependencies
Incorrect headless configuration
```

---

## Problem 2: Tests pass locally but fail in Jenkins

Possible causes:

```text
Different Java version
Different browser version
Different OS
Missing environment variables
Different screen resolution
Timing differences
Missing dependencies
Network restrictions
```

Always compare:

```text
Local Environment
vs
Jenkins Agent Environment
```

---

## Problem 3: Jenkins cannot find Maven

Check:

```text
Maven installation
PATH
Jenkins Tool Configuration
JAVA_HOME
MAVEN_HOME
```

---

## Problem 4: Jenkins cannot find Java

Verify:

```bash
java -version
```

and:

```bash
echo $JAVA_HOME
```

Windows:

```bat
java -version
echo %JAVA_HOME%
```

---

## Problem 5: Selenium tests fail in headless mode

Check:

```text
Window size
Browser options
Downloads
File paths
Popups
Permissions
Timing
```

---

## Problem 6: Screenshot path does not exist

Create the directory before writing:

```java
Files.createDirectories(
    destination.getParent()
);
```

---

## Problem 7: Parallel tests interfere with each other

Common causes:

```text
Shared WebDriver
Shared test data
Shared files
Static mutable variables
Non-thread-safe reporting
```

Use:

```text
ThreadLocal<WebDriver>
ThreadLocal<ExtentTest>
```

and isolate test data where necessary.

---

## Problem 8: Jenkins cannot access GitHub

Check:

```text
Repository URL
Credentials
SSH key / token
Network access
Repository permissions
Webhook configuration
```

---

# 43. Interview Questions

## Beginner

### 1. What is CI/CD?

CI/CD automates software integration, testing, delivery, and deployment processes.

---

### 2. Why use Jenkins with Selenium?

Jenkins can automatically trigger Selenium tests after code changes or on a schedule and publish the resulting reports.

---

### 3. What command runs Selenium tests with Maven?

```bash
mvn clean test
```

---

### 4. What is a Jenkinsfile?

A Jenkinsfile contains the Jenkins Pipeline definition as code.

---

### 5. What is a Jenkins Pipeline?

A Pipeline defines automated stages such as:

```text
Checkout
Build
Test
Report
Deploy
```

---

### 6. What is a webhook?

A webhook allows an external system such as GitHub to notify Jenkins about repository events.

---

## Intermediate

### 7. How do you pass a browser from Jenkins to Selenium?

Jenkins passes a system property:

```bash
mvn test -Dbrowser=chrome
```

Java reads it:

```java
String browser =
    System.getProperty("browser");
```

---

### 8. How do you run tests on different environments?

Pass an environment property:

```bash
mvn test -Denvironment=qa
```

or:

```bash
mvn test -Denvironment=stage
```

---

### 9. How do you run only smoke tests?

```bash
mvn test -Dgroups=smoke
```

The exact command depends on the Surefire/TestNG configuration.

---

### 10. How do you run regression tests?

```bash
mvn test -Dgroups=regression
```

---

### 11. How do you execute Selenium tests in Jenkins without a GUI?

Use headless browser execution.

Example:

```java
ChromeOptions options =
    new ChromeOptions();

options.addArguments(
    "--headless=new"
);
```

---

### 12. How do you run Selenium tests on multiple browsers?

Use:

```text
Jenkins Parameters
+
TestNG Parameters/DataProvider
+
WebDriver Factory
```

---

### 13. How do you execute tests in parallel?

Use TestNG:

```xml
<suite
    parallel="tests"
    thread-count="4">
```

and ensure WebDriver/reporting objects are thread-safe.

---

### 14. How do you integrate Selenium Grid with Jenkins?

```text
Jenkins
 ↓
Maven
 ↓
TestNG
 ↓
RemoteWebDriver
 ↓
Selenium Grid
 ↓
Browser Nodes
```

---

## Advanced

### 15. How would you design a Selenium CI/CD pipeline?

A strong answer:

```text
GitHub
 ↓
Jenkins
 ↓
Checkout
 ↓
Compile
 ↓
Smoke Tests
 ↓
Regression Tests
 ↓
Selenium Grid
 ↓
Extent/Allure
 ↓
Archive Results
 ↓
Notification
```

---

### 16. How do you handle secrets in Jenkins?

Store secrets in Jenkins Credentials or an approved secrets-management system and inject them into the pipeline when needed.

Never hard-code passwords or tokens in:

```text
Java
Jenkinsfile
pom.xml
Git repository
```

---

### 17. What happens when Selenium tests fail in Jenkins?

A good pipeline should:

```text
1. Mark the build appropriately
2. Collect test results
3. Capture screenshots
4. Collect logs
5. Publish reports
6. Notify the team when configured
```

---

### 18. How do you handle tests that pass locally but fail in Jenkins?

Investigate differences in:

```text
OS
Java version
Browser version
Driver
Environment variables
Network
Screen size
Headless mode
Test data
Timing
Dependencies
```

---

### 19. How do you reduce Selenium execution time?

Use:

```text
Parallel execution
Selenium Grid
Headless browsers
TestNG groups
Efficient waits
Independent tests
Smoke/regression separation
```

---

### 20. How do you make a Selenium framework CI/CD ready?

A strong framework should have:

```text
Page Object Model
Driver Factory
ThreadLocal WebDriver
TestNG
DataProvider
Listeners
Retry Analyzer
Reporting
Configuration management
Environment selection
Maven
Git
Jenkins
Selenium Grid
```

---

# 44. Quick Revision

## CI/CD

```text
CI
=
Continuous Integration

CD
=
Continuous Delivery / Continuous Deployment
```

---

## Selenium CI/CD Flow

```text
GitHub
  ↓
Jenkins
  ↓
Maven
  ↓
TestNG
  ↓
Selenium
  ↓
Browser / Grid
  ↓
Test Results
  ↓
Reports
```

---

## Important Commands

```bash
git clone <repository>

git status

git add .

git commit -m "message"

git push origin main
```

Maven:

```bash
mvn clean test
```

Browser:

```bash
mvn test -Dbrowser=chrome
```

Environment:

```bash
mvn test -Denvironment=qa
```

Smoke:

```bash
mvn test -Dgroups=smoke
```

Regression:

```bash
mvn test -Dgroups=regression
```

---

## Important Jenkins Concepts

```text
Jenkins Job
Jenkins Pipeline
Jenkinsfile
Agent
Stage
Step
Parameter
Credential
Environment Variable
Webhook
Build
Artifact
```

---

## Jenkins Pipeline Structure

```groovy
pipeline {

    agent any

    stages {

        stage('Checkout') {
        }

        stage('Build') {
        }

        stage('Test') {
        }

        stage('Report') {
        }
    }

    post {

        always {
        }

        success {
        }

        failure {
        }
    }
}
```

---

## Selenium CI/CD Architecture

```text
                    GitHub
                       |
                       v
                    Jenkins
                       |
                       v
                     Maven
                       |
                       v
                    TestNG
                       |
              +--------+--------+
              |                 |
              v                 v
        Local Browser      Selenium Grid
                                |
                    +-----------+-----------+
                    |           |           |
                  Chrome      Firefox      Edge
                    |           |           |
                    +-----------+-----------+
                                |
                                v
                         Test Execution
                                |
                   +------------+------------+
                   |                         |
                   v                         v
             Extent Report             Allure Report
                   |                         |
                   +------------+------------+
                                |
                                v
                          Jenkins Results
```

---

# Final Interview Summary

Remember these points:

```text
1. CI = Continuous Integration.

2. CD = Continuous Delivery or Continuous Deployment.

3. Jenkins is commonly used to automate CI/CD pipelines.

4. Maven can build and execute Selenium/TestNG projects.

5. Jenkins can trigger tests after Git commits.

6. GitHub webhooks can trigger Jenkins pipelines.

7. Jenkinsfile stores Pipeline-as-Code.

8. Browser and environment can be passed as parameters.

9. Selenium can run headlessly in CI environments.

10. Selenium Grid enables distributed browser execution.

11. TestNG supports parallel execution.

12. ThreadLocal<WebDriver> is useful for parallel execution.

13. ExtentReports and Allure can provide detailed reports.

14. Screenshots and logs should be archived for failures.

15. Credentials should never be hard-coded.

16. CI environments may differ from developer machines.

17. Smoke tests provide quick feedback.

18. Regression suites can run in parallel or on a schedule.

19. Jenkins can archive reports and test artifacts.

20. A production Selenium framework should be designed
    for repeatable, configurable, and unattended execution.
```

---

# Complete Selenium CI/CD Stack

```text
Java
   ↓
Selenium WebDriver
   ↓
TestNG
   ↓
Page Object Model
   ↓
DataProvider
   ↓
Listeners
   ↓
Retry Analyzer
   ↓
ThreadLocal
   ↓
Selenium Grid
   ↓
Maven
   ↓
Git / GitHub
   ↓
Jenkins
   ↓
ExtentReports / Allure
   ↓
CI/CD
```

This stack represents a common enterprise-level Selenium automation architecture.
