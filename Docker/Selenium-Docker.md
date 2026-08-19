# Selenium Docker

## Table of Contents

1. [What is Docker?](#1-what-is-docker)
2. [Why Use Docker with Selenium?](#2-why-use-docker-with-selenium)
3. [Docker vs Virtual Machine](#3-docker-vs-virtual-machine)
4. [Important Docker Concepts](#4-important-docker-concepts)
5. [Docker Architecture](#5-docker-architecture)
6. [Installing Docker](#6-installing-docker)
7. [Verify Docker Installation](#7-verify-docker-installation)
8. [Important Docker Commands](#8-important-docker-commands)
9. [Docker Image](#9-docker-image)
10. [Docker Container](#10-docker-container)
11. [Dockerfile](#11-dockerfile)
12. [Docker Hub](#12-docker-hub)
13. [Selenium Docker Images](#13-selenium-docker-images)
14. [Selenium Standalone Container](#14-selenium-standalone-container)
15. [Selenium Grid with Docker](#15-selenium-grid-with-docker)
16. [Selenium Hub and Nodes](#16-selenium-hub-and-nodes)
17. [RemoteWebDriver with Docker](#17-remotewebdriver-with-docker)
18. [Chrome with Selenium Docker](#18-chrome-with-selenium-docker)
19. [Firefox with Selenium Docker](#19-firefox-with-selenium-docker)
20. [Edge with Selenium Docker](#20-edge-with-selenium-docker)
21. [Docker Compose](#21-docker-compose)
22. [docker-compose.yml](#22-docker-composeyml)
23. [Selenium Grid with Docker Compose](#23-selenium-grid-with-docker-compose)
24. [Running Tests Against Grid](#24-running-tests-against-grid)
25. [Docker Networks](#25-docker-networks)
26. [Docker Volumes](#26-docker-volumes)
27. [Environment Variables](#27-environment-variables)
28. [Headless vs Docker Browser](#28-headless-vs-docker-browser)
29. [Parallel Selenium Execution](#29-parallel-selenium-execution)
30. [Docker + Maven + Selenium](#30-docker--maven--selenium)
31. [Docker + Jenkins + Selenium](#31-docker--jenkins--selenium)
32. [Complete Selenium Docker Project](#32-complete-selenium-docker-project)
33. [Complete Dockerfile](#33-complete-dockerfile)
34. [Complete Docker Compose Example](#34-complete-docker-compose-example)
35. [Useful Docker Commands for Selenium](#35-useful-docker-commands-for-selenium)
36. [Common Docker Problems](#36-common-docker-problems)
37. [Docker Best Practices](#37-docker-best-practices)
38. [Docker Interview Questions](#38-docker-interview-questions)
39. [Quick Revision](#39-quick-revision)

---

# 1. What is Docker?

Docker is a platform used to package and run applications inside lightweight, isolated environments called containers.

A container contains everything required to run an application:

```text
Application
Libraries
Dependencies
Configuration
Runtime
For Selenium, Docker can provide a consistent browser execution environment.

Example:

Selenium Test
     |
     v
Docker Container
     |
     v
Chrome Browser
2. Why Use Docker with Selenium?

A Selenium test may work on a developer's machine but fail on another machine because of:

Different OS
Different Browser Version
Different Java Version
Different Driver Version
Different Dependencies
Different Configuration

Docker helps create a consistent environment.

Without Docker:

Developer Machine
      |
      v
Selenium
      |
   Chrome

With Docker:

Developer
    |
    v
Docker
    |
    v
Selenium
    |
    v
Chrome

Benefits:

Consistent test environment
Easy setup
Easy cleanup
Isolation
Scalability
Parallel execution
CI/CD integration
Selenium Grid support
Reproducible execution
3. Docker vs Virtual Machine
Virtual Machine
Physical Machine
      |
      +-------------------+
      |                   |
   VM 1                 VM 2
   OS                    OS
   App                   App

Each VM generally has its own operating system.

Docker Container
Physical Machine
      |
      v
 Docker Engine
      |
 +----+----+----+
 |    |    |    |
 C1   C2   C3   C4

Containers share the host operating system kernel while remaining isolated from one another.

Comparison
Feature	Docker	Virtual Machine
Startup	Fast	Slower
Resource usage	Lower	Higher
Isolation	Process-level/container isolation	Strong VM isolation
OS	Shares host kernel	Full guest OS
Size	Usually smaller	Usually larger
Scalability	Excellent	More resource intensive
4. Important Docker Concepts

The most important Docker terms are:

Docker Engine
Docker Image
Docker Container
Dockerfile
Docker Hub
Docker Network
Docker Volume
Docker Compose
Docker Registry
5. Docker Architecture

Basic architecture:

User
 |
 v
Docker CLI
 |
 v
Docker Engine
 |
 +----------------+
 |                |
 v                v
Images         Containers

Example:

docker run
    |
    v
Docker Engine
    |
    v
Selenium Image
    |
    v
Selenium Container
6. Installing Docker

For Windows, Docker Desktop is commonly used.

General process:

1. Install Docker Desktop
2. Start Docker Desktop
3. Wait until Docker is running
4. Open Terminal
5. Verify installation

Docker Desktop uses a Linux environment/virtualization layer to run Linux containers on Windows.

7. Verify Docker Installation

Run:

docker --version

Example:

Docker version 28.x.x

Check Docker information:

docker info

Run a test container:

docker run hello-world

If Docker is working, Docker downloads the image and runs the container.

8. Important Docker Commands
Show Docker version
docker --version
Show Docker information
docker info
Download an image
docker pull image-name

Example:

docker pull selenium/standalone-chrome
List images
docker images

or:

docker image ls
Run a container
docker run image-name

Example:

docker run selenium/standalone-chrome
List running containers
docker ps
List all containers
docker ps -a
Stop a container
docker stop container-name
Start a stopped container
docker start container-name
Remove a container
docker rm container-name
Remove an image
docker rmi image-name
View container logs
docker logs container-name
Execute command inside container
docker exec -it container-name bash

The exact shell available depends on the image.

9. Docker Image

A Docker image is a packaged, immutable template used to create containers.

Example:

Selenium Chrome Image
        |
        +---- Browser
        +---- Selenium Server
        +---- Required dependencies

An image can create multiple containers:

             Selenium Image
              /     |     \
             /      |      \
            v       v       v
        Container Container Container
10. Docker Container

A container is a running instance of an image.

Example:

Image
  |
  | docker run
  v
Container

One image can create many containers:

selenium-chrome-image
        |
   +----+----+----+
   |    |    |    |
   v    v    v    v
  C1   C2   C3   C4
11. Dockerfile

A Dockerfile contains instructions for building a Docker image.

Example:

FROM maven:3.9-eclipse-temurin-17


WORKDIR /app


COPY pom.xml .


RUN mvn dependency:go-offline


COPY . .


CMD ["mvn", "clean", "test"]

Explanation:

FROM
    Base image


WORKDIR
    Working directory


COPY
    Copy files


RUN
    Execute command while building image


CMD
    Default command when container starts
12. Docker Hub

Docker Hub is a public container image registry.

You can:

Search images
Pull images
Push images
Share images

Example:

docker pull nginx

For Selenium, official Selenium images are commonly published through the Selenium project container registry ecosystem.

Always verify the image name and version you intend to use.

13. Selenium Docker Images

Selenium provides container images for browser automation.

Common image patterns include:

selenium/standalone-chrome
selenium/standalone-firefox
selenium/standalone-edge

For production projects, prefer a specific supported version tag instead of relying on an unspecified latest tag.

Example:

docker pull selenium/standalone-chrome:<version>
14. Selenium Standalone Container

A standalone Selenium container contains Selenium Server and a browser.

Architecture:

Selenium Container
      |
      +---- Selenium Server
      |
      +---- Chrome

Run:

docker run -d \
  --name selenium-chrome \
  -p 4444:4444 \
  selenium/standalone-chrome:<version>

Windows PowerShell can use:

docker run -d `
  --name selenium-chrome `
  -p 4444:4444 `
  selenium/standalone-chrome:<version>

Check:

docker ps
15. Selenium Grid with Docker

Docker can be used to create a Selenium Grid.

Example:

                 Selenium Grid
                      |
          +-----------+-----------+
          |           |           |
          v           v           v
       Chrome      Firefox       Edge
        Node         Node        Node

This allows tests to execute on different browsers.

16. Selenium Hub and Nodes

Traditional Grid architecture:

              Hub
               |
       +-------+-------+
       |       |       |
       v       v       v
    Chrome  Firefox   Edge
     Node     Node    Node

The hub/router receives WebDriver requests and routes them to available browser sessions.

Modern Selenium Grid can also operate in different deployment modes, including distributed and standalone configurations.

17. RemoteWebDriver with Docker

Instead of:

WebDriver driver =
    new ChromeDriver();

use:

WebDriver driver =
    new RemoteWebDriver(
        gridUrl,
        options
    );

Example:

import java.net.MalformedURLException;
import java.net.URI;


import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.remote.RemoteWebDriver;


public class DriverFactory {


    public static WebDriver createDriver()
            throws MalformedURLException {


        ChromeOptions options =
            new ChromeOptions();


        return new RemoteWebDriver(
            URI.create(
                "http://localhost:4444"
            ).toURL(),
            options
        );
    }
}

The Grid URL should normally be configurable.

18. Chrome with Selenium Docker

Run Chrome container:

docker run -d \
  --name chrome \
  -p 4444:4444 \
  selenium/standalone-chrome:<version>

Then Java connects to:

http://localhost:4444

Example:

ChromeOptions options =
    new ChromeOptions();


WebDriver driver =
    new RemoteWebDriver(
        gridUrl,
        options
    );
19. Firefox with Selenium Docker

Run:

docker run -d \
  --name firefox \
  -p 4444:4444 \
  selenium/standalone-firefox:<version>

Java:

FirefoxOptions options =
    new FirefoxOptions();


WebDriver driver =
    new RemoteWebDriver(
        gridUrl,
        options
    );
20. Edge with Selenium Docker

Run:

docker run -d \
  --name edge \
  -p 4444:4444 \
  selenium/standalone-edge:<version>

Java:

EdgeOptions options =
    new EdgeOptions();


WebDriver driver =
    new RemoteWebDriver(
        gridUrl,
        options
    );
21. Docker Compose

Docker Compose is used to define and run multiple related containers.

Instead of executing:

docker run ...
docker run ...
docker run ...

you can define services in:

docker-compose.yml

Then use:

docker compose up

and:

docker compose down

Modern Docker uses:

docker compose

The older:

docker-compose

command may still exist on some systems.

22. docker-compose.yml

Basic example:

services:


  selenium-chrome:
    image: selenium/standalone-chrome:<version>
    ports:
      - "4444:4444"


  selenium-firefox:
    image: selenium/standalone-firefox:<version>
    ports:
      - "4445:4444"

Start:

docker compose up -d

Stop:

docker compose down

Check:

docker compose ps
23. Selenium Grid with Docker Compose

A simple Selenium Grid setup can be represented as:

services:


  selenium:
    image: selenium/standalone-chrome:<version>
    shm_size: 2gb
    ports:
      - "4444:4444"
      - "7900:7900"

Start:

docker compose up -d

Check:

docker compose ps

The shm_size setting can help browser stability for workloads that consume substantial shared memory.

24. Running Tests Against Grid

Start Selenium:

docker compose up -d

Then run:

mvn clean test

Java:

URL gridUrl =
    URI.create(
        "http://localhost:4444"
    ).toURL();


ChromeOptions options =
    new ChromeOptions();


WebDriver driver =
    new RemoteWebDriver(
        gridUrl,
        options
    );

Flow:

Maven
  |
  v
TestNG
  |
  v
RemoteWebDriver
  |
  v
Docker Selenium
  |
  v
Chrome
25. Docker Networks

Containers can communicate using Docker networks.

Create network:

docker network create selenium-network

Run container:

docker run -d \
  --network selenium-network \
  --name selenium \
  selenium/standalone-chrome:<version>

Containers on the same Docker network can communicate using container/service names where supported by the network configuration.

Example concept:

Test Container
      |
      | selenium:4444
      v
Selenium Container

This is useful when both the test runner and Selenium Grid run inside Docker.

26. Docker Volumes

Volumes persist data outside the container's writable layer.

Example:

docker volume create selenium-data

Use:

docker run \
  -v selenium-data:/data \
  image-name

For Selenium testing, volumes can be useful for:

Reports
Screenshots
Downloads
Logs
Test artifacts

Example:

Container
   |
   v
/data/reports
   |
   v
Docker Volume
27. Environment Variables

Docker Compose:

services:


  selenium:
    image: selenium/standalone-chrome:<version>


    environment:
      SE_NODE_MAX_SESSIONS: "2"

Test container:

services:


  tests:
    environment:
      BROWSER: chrome
      TEST_ENV: qa

Java:

String browser =
    System.getenv("BROWSER");


String environment =
    System.getenv("TEST_ENV");
28. Headless vs Docker Browser

There are two common approaches.

Local Headless
Jenkins
  |
  v
Chrome
  |
  +-- Headless

Example:

ChromeOptions options =
    new ChromeOptions();


options.addArguments(
    "--headless=new"
);
Docker Browser
Jenkins
  |
  v
Docker
  |
  v
Selenium Container
  |
  v
Chrome

Docker Selenium images can run browsers in a containerized environment.

29. Parallel Selenium Execution

Docker makes it easier to scale browser sessions.

Example:

                 Selenium Grid
                      |
       +--------------+--------------+
       |              |              |
       v              v              v
    Chrome         Firefox          Edge
    Session 1      Session 2       Session 3

TestNG:

<suite
    name="Parallel"
    parallel="tests"
    thread-count="3">

Each thread should have its own WebDriver instance.

A common design is:

private static ThreadLocal<WebDriver>
    driver = new ThreadLocal<>();

Example:

public static void setDriver(
        WebDriver webDriver) {


    driver.set(webDriver);
}


public static WebDriver getDriver() {


    return driver.get();
}


public static void quitDriver() {


    if (driver.get() != null) {


        driver.get().quit();
        driver.remove();
    }
}
30. Docker + Maven + Selenium

A test container can contain:

Java
Maven
Selenium Tests

Example Dockerfile:

FROM maven:3.9-eclipse-temurin-17


WORKDIR /app


COPY pom.xml .


RUN mvn dependency:go-offline


COPY . .


CMD ["mvn", "clean", "test"]

Build:

docker build -t selenium-tests .

Run:

docker run --rm selenium-tests

If the tests need a Selenium Grid in another container, configure the Grid URL through an environment variable.

31. Docker + Jenkins + Selenium

Enterprise flow:

GitHub
   |
   v
Jenkins
   |
   v
Docker
   |
   +----------------+
   |                |
   v                v
Test Container   Selenium Grid
                    |
          +---------+---------+
          |         |         |
        Chrome    Firefox    Edge

Jenkins can:

1. Checkout code
2. Build Docker image
3. Start Selenium containers
4. Run Maven tests
5. Collect reports
6. Archive screenshots
7. Stop containers
32. Complete Selenium Docker Project

Recommended structure:

SeleniumDockerProject
│
├── Dockerfile
├── docker-compose.yml
├── pom.xml
├── testng.xml
├── Jenkinsfile
│
├── src
│   ├── main
│   │   └── java
│   │       ├── base
│   │       │   └── DriverFactory.java
│   │       │
│   │       ├── pages
│   │       │   ├── LoginPage.java
│   │       │   └── HomePage.java
│   │       │
│   │       └── utilities
│   │           └── ConfigReader.java
│   │
│   └── test
│       └── java
│           └── tests
│               └── LoginTest.java
│
├── reports
│   └── screenshots
│
└── target
33. Complete Dockerfile

Example:

FROM maven:3.9-eclipse-temurin-17


WORKDIR /app


COPY pom.xml .


RUN mvn dependency:go-offline


COPY src ./src
COPY testng.xml .
COPY pom.xml .


CMD ["mvn", "clean", "test"]

Build:

docker build -t selenium-tests .

Run:

docker run --rm selenium-tests
34. Complete Docker Compose Example

A simple setup with Selenium and a test container:

services:


  selenium:
    image: selenium/standalone-chrome:<version>


    shm_size: 2gb


    ports:
      - "4444:4444"


  tests:
    build: .


    depends_on:
      - selenium


    environment:
      SELENIUM_URL: http://selenium:4444
      BROWSER: chrome
      TEST_ENV: qa


    volumes:
      - ./reports:/app/reports

Java:

String seleniumUrl =
    System.getenv(
        "SELENIUM_URL"
    );


URL gridUrl =
    URI.create(
        seleniumUrl
    ).toURL();


ChromeOptions options =
    new ChromeOptions();


WebDriver driver =
    new RemoteWebDriver(
        gridUrl,
        options
    );

Run:

docker compose up --build

Stop:

docker compose down
35. Useful Docker Commands for Selenium
List containers
docker ps

All containers:

docker ps -a
View logs
docker logs selenium

Follow logs:

docker logs -f selenium
Stop container
docker stop selenium
Remove container
docker rm selenium
Force remove
docker rm -f selenium
List images
docker images
Remove image
docker rmi selenium-image
Build image
docker build -t selenium-tests .
Run image
docker run --rm selenium-tests
Compose start
docker compose up -d
Compose build and start
docker compose up -d --build
Compose stop and remove
docker compose down
Compose logs
docker compose logs
Compose logs for one service
docker compose logs selenium
36. Common Docker Problems
Problem 1: Docker command not found

Error:

'docker' is not recognized...

Possible causes:

Docker Desktop not installed
Docker Desktop not running
PATH not configured
Terminal needs restarting

Check:

docker --version
Problem 2: Cannot connect to Docker daemon

Possible causes:

Docker Desktop is not running
Docker Engine has not started

Start Docker Desktop and retry.

Problem 3: Port already in use

Example:

Bind for 0.0.0.0:4444 failed:
port is already allocated

Check:

docker ps

Another application may already use port 4444.

Use another host port:

docker run -p 4445:4444 ...

Then connect to:

http://localhost:4445
Problem 4: Container exits immediately

Check:

docker ps -a

Then:

docker logs container-name

The logs normally reveal the startup failure.

Problem 5: Selenium cannot connect

Check:

1. Container is running
2. Correct Grid URL
3. Correct port
4. Network connectivity
5. Docker service name

Inside Compose:

http://selenium:4444

From the host:

http://localhost:4444

These are different network contexts.

Problem 6: Browser crashes

Possible causes:

Insufficient shared memory
Resource limits
Browser startup arguments
Container resource constraints

For Chromium-based browser workloads, increasing shared memory may help:

shm_size: 2gb
Problem 7: Tests cannot find application URL

Inside a container:

localhost

means the current container itself.

It does NOT automatically mean:

Your Windows/Mac host

This is a very important Docker networking concept.

Problem 8: Tests pass locally but fail in Docker

Check:

Browser version
Java version
Environment variables
File paths
Network access
Application accessibility
Time zone
Screen size
Downloads
Permissions
Problem 9: Reports disappear after container exits

Container filesystem data can be lost when the container is removed.

Use a volume:

volumes:
  - ./reports:/app/reports

This maps:

Host reports/
       |
       v
Container /app/reports
37. Docker Best Practices
1. Use specific image versions

Prefer:

selenium/standalone-chrome:<specific-version>

rather than depending on an unspecified moving tag.

This improves reproducibility.

2. Keep Docker images small

Use an appropriate base image and avoid unnecessary packages.

3. Do not store secrets in Dockerfiles

Bad:

ENV PASSWORD=secret123

Use:

Jenkins Credentials
Environment Variables
Secrets Manager

instead.

4. Use .dockerignore

Example:

.git
.gitignore
target
reports
*.log
.idea
.vscode

This reduces Docker build context.

5. Keep tests independent

A test should not depend on another test's execution order.

6. Use volumes for reports

Persist:

Screenshots
Reports
Logs
Downloads

when needed.

7. Use Docker Compose for multiple services

Instead of manually starting:

Selenium
Test Runner
Database
Application

define them in:

docker-compose.yml
8. Use environment variables

Do not hard-code:

QA URL
Stage URL
Grid URL
Browser
Credentials
9. Clean unused resources

Periodically inspect:

docker ps -a
docker images
docker volume ls
docker network ls

Clean only resources that are safe to remove.

10. Pin compatible versions

Keep compatible versions of:

Java
Selenium
Browser
Selenium image
Maven
TestNG

especially in CI.

38. Docker Interview Questions
Beginner Questions
1. What is Docker?

Docker is a containerization platform used to package and run applications in isolated environments.

2. What is a Docker image?

An image is an immutable template used to create containers.

3. What is a Docker container?

A container is a running instance of a Docker image.

4. What is a Dockerfile?

A Dockerfile contains instructions used to build a Docker image.

5. What is Docker Compose?

Docker Compose is used to define and run multi-container applications.

6. What command lists running containers?
docker ps
7. What command lists all containers?
docker ps -a
8. How do you run a container?
docker run image-name
9. How do you stop a container?
docker stop container-name
10. How do you see container logs?
docker logs container-name
Intermediate Questions
11. Why use Docker with Selenium?

Docker provides consistent, isolated, reproducible browser environments and makes Selenium Grid scaling easier.

12. What is Selenium Grid?

Selenium Grid allows WebDriver tests to execute remotely on different browser environments.

13. How does Selenium connect to a Docker container?

Using RemoteWebDriver:

WebDriver driver =
    new RemoteWebDriver(
        gridUrl,
        options
    );
14. What is the difference between localhost:4444 and selenium:4444?

From the host:

localhost:4444

can refer to a published Docker port.

From another container on the same Compose network:

selenium:4444

uses the Compose service name.

15. How do you run Selenium Chrome in Docker?

Example:

docker run -d \
  --name selenium-chrome \
  -p 4444:4444 \
  selenium/standalone-chrome:<version>
16. How do you run Firefox?
docker run -d \
  --name selenium-firefox \
  -p 4444:4444 \
  selenium/standalone-firefox:<version>
17. How do you run Selenium tests in Docker?

Create a test image:

FROM maven:3.9-eclipse-temurin-17


WORKDIR /app


COPY . .


CMD ["mvn", "clean", "test"]

Build:

docker build -t selenium-tests .

Run:

docker run --rm selenium-tests
18. How do you preserve Selenium reports?

Use Docker volumes:

volumes:
  - ./reports:/app/reports
19. How do you execute Selenium tests in parallel using Docker?

Use:

TestNG parallel execution
+
Multiple Selenium sessions
+
Selenium Grid
+
Multiple browser nodes/containers
20. What is Docker networking?

Docker networking allows containers to communicate with each other and with external systems according to configured networks and port mappings.

Advanced Questions
21. How would you integrate Docker with Jenkins?

Example:

GitHub
   ↓
Jenkins
   ↓
Docker Build
   ↓
Selenium Container
   ↓
Maven
   ↓
TestNG
   ↓
Selenium Grid
   ↓
Reports
22. Why might Selenium tests fail in Docker but pass locally?

Investigate:

OS
Browser
Java
Selenium version
Environment variables
Network
File paths
Shared memory
Permissions
Application accessibility
23. Why use RemoteWebDriver with Selenium Docker?

Because the browser runs remotely inside the Selenium container rather than directly on the test runner machine.

24. What is the purpose of shm_size?

Browser processes can consume shared memory. Increasing the container's shared memory allocation can improve stability for browser-heavy workloads.

Example:

shm_size: 2gb
25. How do you pass environment information to a Dockerized Selenium test?

Use environment variables:

environment:
  TEST_ENV: qa
  BROWSER: chrome

Java:

String environment =
    System.getenv("TEST_ENV");
26. How do you handle credentials in Docker?

Do not put credentials in:

Dockerfile
Git
Source Code
docker-compose.yml

Use an approved secrets mechanism such as:

Jenkins Credentials
Docker/Kubernetes Secrets
Cloud Secrets Manager
27. How do you scale Selenium tests using Docker?

Use multiple browser containers or Selenium Grid nodes:

              Grid
                |
      +---------+---------+
      |         |         |
   Chrome    Firefox     Edge
      |         |         |
   Node 1     Node 2    Node 3

Then execute TestNG tests in parallel.

28. How would you design a production Selenium Docker architecture?

A good answer:

GitHub
   ↓
Jenkins
   ↓
Maven Build
   ↓
Docker
   ↓
Selenium Grid
   ↓
Chrome / Firefox / Edge
   ↓
Parallel TestNG Execution
   ↓
Extent / Allure
   ↓
Screenshots + Logs
   ↓
Jenkins Artifacts
39. Quick Revision
Docker
Docker
 ↓
Containerization
 ↓
Consistent Environment
Image vs Container
Image
  ↓
Template


Container
  ↓
Running Instance
Dockerfile
Dockerfile
   ↓
docker build
   ↓
Docker Image
   ↓
docker run
   ↓
Container
Selenium Docker
Selenium Test
      |
      v
RemoteWebDriver
      |
      v
Selenium Container
      |
      v
Browser
Selenium Grid
                 Selenium Grid
                      |
        +-------------+-------------+
        |             |             |
        v             v             v
     Chrome        Firefox         Edge
Docker Compose
docker-compose.yml
        |
        v
docker compose up
        |
        v
Multiple Containers
CI/CD + Docker + Selenium
Developer
    |
    v
GitHub
    |
    v
Jenkins
    |
    v
Maven
    |
    v
Docker
    |
    v
Selenium Grid
    |
    +---------+---------+
    |         |         |
 Chrome    Firefox     Edge
    |         |         |
    +---------+---------+
              |
              v
         TestNG Results
              |
       +------+------+
       |             |
       v             v
   Extent         Allure
   Report         Report
       |             |
       +------+------+
              |
              v
          Jenkins
Important Commands
# Docker version
docker --version


# Docker information
docker info


# Test Docker
docker run hello-world


# Pull image
docker pull <image>


# List images
docker images


# List running containers
docker ps


# List all containers
docker ps -a


# Run container
docker run <image>


# Run in background
docker run -d <image>


# Stop container
docker stop <container>


# Start container
docker start <container>


# Remove container
docker rm <container>


# Remove image
docker rmi <image>


# View logs
docker logs <container>


# Build image
docker build -t selenium-tests .


# Run built image
docker run --rm selenium-tests


# Start Compose
docker compose up -d


# Build and start Compose
docker compose up -d --build


# Stop Compose
docker compose down


# View Compose services
docker compose ps


# View Compose logs
docker compose logs
Key Selenium Docker Code
ChromeOptions options =
    new ChromeOptions();


String seleniumUrl =
    System.getenv(
        "SELENIUM_URL"
    );


URL gridUrl =
    URI.create(
        seleniumUrl
    ).toURL();


WebDriver driver =
    new RemoteWebDriver(
        gridUrl,
        options
    );


driver.get(
    "https://example.com"
);


System.out.println(
    driver.getTitle()
);


driver.quit();
Final Interview Summary

Remember:

1. Docker provides containerized environments.


2. A Docker image is a template.


3. A container is a running instance of an image.


4. Dockerfile is used to build images.


5. Docker Compose manages multiple related containers.


6. Selenium can run inside Docker containers.


7. RemoteWebDriver is used to connect to remote Selenium.


8. Selenium Grid can distribute tests across browsers.


9. Docker helps provide consistent test environments.


10. Docker makes browser execution easier to scale.


11. TestNG can execute tests in parallel.


12. ThreadLocal<WebDriver> is useful for thread-safe parallel execution.


13. Docker volumes can preserve reports and screenshots.


14. Docker networks allow containers to communicate.


15. Environment variables can configure browser and environment.


16. Credentials should never be hard-coded.


17. Jenkins can build Docker images and execute Selenium tests.


18. Docker Compose can simplify local Selenium Grid setup.


19. Specific image versions improve reproducibility.


20. Docker + Selenium + TestNG + Maven + Jenkins +
    Selenium Grid is a powerful enterprise automation stack.
Selenium Automation Learning Path

After completing Docker, the recommended remaining advanced topics are:

Docker
   ↓
Cloud Selenium
   ↓
BrowserStack / Sauce Labs
   ↓
API + UI Automation
   ↓
Rest Assured Integration
   ↓
Cucumber / BDD
   ↓
Advanced Framework Design
   ↓
Performance Testing Basics
   ↓
Security Testing Basics
   ↓
Advanced Selenium Interview Preparation

