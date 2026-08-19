# Selenium Parallel Execution

## 1. Introduction

Parallel execution allows multiple Selenium tests to run at the same time instead of running one after another.

This significantly reduces overall test execution time and is especially useful for:

* Large regression suites
* Cross-browser testing
* CI/CD pipelines
* Selenium Grid
* Docker-based test execution
* Distributed test execution

### Sequential execution

```text
Test 1 → Test 2 → Test 3 → Test 4

Total Time = T1 + T2 + T3 + T4
```

### Parallel execution

```text
             ┌── Test 1
             ├── Test 2
Start ───────┼── Test 3
             └── Test 4

Tests execute simultaneously
```

---

# 2. Why Parallel Execution?

Suppose you have 100 tests.

If each test takes 30 seconds:

```text
100 × 30 seconds = 3000 seconds
                   = 50 minutes
```

If 5 tests can run simultaneously:

```text
Approximately 50 / 5 = 10 minutes
```

Actual execution time depends on:

* Test dependencies
* Machine resources
* Browser startup time
* Network speed
* Selenium Grid configuration
* Application performance

---

# 3. TestNG Parallel Execution

TestNG provides built-in parallel execution support through `testng.xml`.

The most commonly used options are:

```xml
parallel="methods"
parallel="classes"
parallel="tests"
```

---

# 4. Parallel Methods

With:

```xml
parallel="methods"
```

TestNG can execute test methods in parallel.

## Example

```java
import org.testng.annotations.Test;

public class LoginTest {

    @Test
    public void validLogin() throws InterruptedException {
        System.out.println("Valid Login - " +
                Thread.currentThread().getId());

        Thread.sleep(3000);
    }

    @Test
    public void invalidLogin() throws InterruptedException {
        System.out.println("Invalid Login - " +
                Thread.currentThread().getId());

        Thread.sleep(3000);
    }
}
```

### testng.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE suite SYSTEM
        "https://testng.org/testng-1.0.dtd">

<suite name="ParallelSuite" parallel="methods" thread-count="2">

    <test name="Login Tests">
        <classes>
            <class name="LoginTest"/>
        </classes>
    </test>

</suite>
```

Both test methods can run simultaneously.

---

# 5. Parallel Classes

Use:

```xml
parallel="classes"
```

when you want different test classes to execute in parallel.

## Example

### LoginTest.java

```java
import org.testng.annotations.Test;

public class LoginTest {

    @Test
    public void loginTest() throws InterruptedException {

        System.out.println(
                "Login Test Thread: " +
                Thread.currentThread().getId());

        Thread.sleep(5000);
    }
}
```

### SearchTest.java

```java
import org.testng.annotations.Test;

public class SearchTest {

    @Test
    public void searchTest() throws InterruptedException {

        System.out.println(
                "Search Test Thread: " +
                Thread.currentThread().getId());

        Thread.sleep(5000);
    }
}
```

### testng.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE suite SYSTEM
        "https://testng.org/testng-1.0.dtd">

<suite name="ParallelClasses"
       parallel="classes"
       thread-count="2">

    <test name="Regression Tests">

        <classes>

            <class name="LoginTest"/>
            <class name="SearchTest"/>

        </classes>

    </test>

</suite>
```

Both classes can run simultaneously.

---

# 6. Parallel Tests

TestNG also supports:

```xml
parallel="tests"
```

Example:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE suite SYSTEM
        "https://testng.org/testng-1.0.dtd">

<suite name="Parallel Tests"
       parallel="tests"
       thread-count="2">

    <test name="Chrome Tests">

        <parameter name="browser" value="chrome"/>

        <classes>
            <class name="LoginTest"/>
        </classes>

    </test>

    <test name="Firefox Tests">

        <parameter name="browser" value="firefox"/>

        <classes>
            <class name="LoginTest"/>
        </classes>

    </test>

</suite>
```

This is useful for cross-browser testing.

---

# 7. Thread Count

`thread-count` controls the maximum number of threads TestNG can use.

Example:

```xml
<suite name="Regression"
       parallel="methods"
       thread-count="4">
```

This allows up to four parallel threads.

### Example

```text
Thread 1 → Test 1
Thread 2 → Test 2
Thread 3 → Test 3
Thread 4 → Test 4
```

If there are more tests:

```text
Thread 1 → Test 5
Thread 2 → Test 6
...
```

TestNG manages the available threads.

---

# 8. The Biggest Selenium Parallel Execution Problem

A WebDriver instance should generally **not be shared between parallel tests**.

Bad approach:

```java
public class BaseTest {

    protected static WebDriver driver;

}
```

If multiple threads use the same static driver:

```text
Thread 1 → Chrome
Thread 2 → Chrome
Thread 3 → Chrome
```

all threads may interact with the same browser.

This can cause:

* Wrong page interactions
* Tests affecting each other
* Stale elements
* Unexpected navigation
* Session conflicts
* Random failures

---

# 9. ThreadLocal WebDriver

`ThreadLocal` provides a separate WebDriver instance for each thread.

Conceptually:

```text
Thread 1 → WebDriver 1
Thread 2 → WebDriver 2
Thread 3 → WebDriver 3
Thread 4 → WebDriver 4
```

This is one of the most important techniques for parallel Selenium frameworks.

---

# 10. ThreadLocal WebDriver Manager

## DriverManager.java

```java
import org.openqa.selenium.WebDriver;

public class DriverManager {

    private static ThreadLocal<WebDriver> driver =
            new ThreadLocal<>();

    public static void setDriver(WebDriver webDriver) {
        driver.set(webDriver);
    }

    public static WebDriver getDriver() {
        return driver.get();
    }

    public static void unload() {
        driver.remove();
    }
}
```

---

# 11. BaseTest with ThreadLocal

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;

public class BaseTest {

    @BeforeMethod
    public void setUp() {

        WebDriver driver = new ChromeDriver();

        DriverManager.setDriver(driver);

        DriverManager.getDriver()
                .manage()
                .window()
                .maximize();

        DriverManager.getDriver()
                .get("https://example.com");
    }

    @AfterMethod
    public void tearDown() {

        if (DriverManager.getDriver() != null) {

            DriverManager.getDriver().quit();

            DriverManager.unload();
        }
    }
}
```

---

# 12. Using ThreadLocal Driver in Tests

```java
import org.testng.annotations.Test;

public class LoginTest extends BaseTest {

    @Test
    public void loginTest() {

        System.out.println(
                "Thread ID: " +
                Thread.currentThread().getId());

        DriverManager.getDriver()
                .get("https://example.com/login");

        System.out.println(
                DriverManager.getDriver().getTitle());
    }
}
```

---

# 13. Complete Parallel Example

## DriverManager.java

```java
import org.openqa.selenium.WebDriver;

public class DriverManager {

    private static ThreadLocal<WebDriver> driver =
            new ThreadLocal<>();

    public static void setDriver(WebDriver webDriver) {
        driver.set(webDriver);
    }

    public static WebDriver getDriver() {
        return driver.get();
    }

    public static void unload() {
        driver.remove();
    }
}
```

## BaseTest.java

```java
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;

public class BaseTest {

    @BeforeMethod
    public void setUp() {

        DriverManager.setDriver(new ChromeDriver());

        DriverManager.getDriver()
                .manage()
                .window()
                .maximize();

        DriverManager.getDriver()
                .get("https://example.com");
    }

    @AfterMethod
    public void tearDown() {

        if (DriverManager.getDriver() != null) {

            DriverManager.getDriver().quit();

            DriverManager.unload();
        }
    }
}
```

## TestClass1.java

```java
import org.testng.annotations.Test;

public class TestClass1 extends BaseTest {

    @Test
    public void testOne() throws InterruptedException {

        System.out.println(
                "Test One - Thread: " +
                Thread.currentThread().getId());

        Thread.sleep(3000);
    }

    @Test
    public void testTwo() throws InterruptedException {

        System.out.println(
                "Test Two - Thread: " +
                Thread.currentThread().getId());

        Thread.sleep(3000);
    }
}
```

## TestClass2.java

```java
import org.testng.annotations.Test;

public class TestClass2 extends BaseTest {

    @Test
    public void testThree() throws InterruptedException {

        System.out.println(
                "Test Three - Thread: " +
                Thread.currentThread().getId());

        Thread.sleep(3000);
    }

    @Test
    public void testFour() throws InterruptedException {

        System.out.println(
                "Test Four - Thread: " +
                Thread.currentThread().getId());

        Thread.sleep(3000);
    }
}
```

## testng.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE suite SYSTEM
        "https://testng.org/testng-1.0.dtd">

<suite name="Selenium Parallel Suite"
       parallel="methods"
       thread-count="4">

    <test name="Regression Tests">

        <classes>

            <class name="TestClass1"/>
            <class name="TestClass2"/>

        </classes>

    </test>

</suite>
```

---

# 14. Parallel Execution with Multiple Browsers

A real framework often needs:

```text
Chrome
Firefox
Edge
```

The browser can be supplied through TestNG parameters.

## testng.xml

```xml
<suite name="Cross Browser Suite"
       parallel="tests"
       thread-count="3">

    <test name="Chrome Test">

        <parameter name="browser" value="chrome"/>

        <classes>
            <class name="LoginTest"/>
        </classes>

    </test>

    <test name="Firefox Test">

        <parameter name="browser" value="firefox"/>

        <classes>
            <class name="LoginTest"/>
        </classes>

    </test>

    <test name="Edge Test">

        <parameter name="browser" value="edge"/>

        <classes>
            <class name="LoginTest"/>
        </classes>

    </test>

</suite>
```

---

# 15. Browser Factory

Create a browser factory to centralize WebDriver creation.

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.edge.EdgeDriver;
import org.openqa.selenium.firefox.FirefoxDriver;

public class DriverFactory {

    public static WebDriver createDriver(String browser) {

        switch (browser.toLowerCase()) {

            case "chrome":
                return new ChromeDriver();

            case "firefox":
                return new FirefoxDriver();

            case "edge":
                return new EdgeDriver();

            default:
                throw new IllegalArgumentException(
                        "Unsupported browser: " + browser);
        }
    }
}
```

---

# 16. BaseTest with Browser Parameter

```java
import org.openqa.selenium.WebDriver;
import org.testng.annotations.*;

public class BaseTest {

    @BeforeMethod
    @Parameters("browser")
    public void setUp(String browser) {

        WebDriver driver =
                DriverFactory.createDriver(browser);

        DriverManager.setDriver(driver);

        DriverManager.getDriver()
                .manage()
                .window()
                .maximize();
    }

    @AfterMethod
    public void tearDown() {

        if (DriverManager.getDriver() != null) {

            DriverManager.getDriver().quit();

            DriverManager.unload();
        }
    }
}
```

---

# 17. Parallel Execution with Selenium Grid

Parallel execution becomes even more powerful with Selenium Grid.

Instead of running browsers only on one machine:

```text
TestNG
   |
   +---- Thread 1 → Chrome
   |
   +---- Thread 2 → Firefox
   |
   +---- Thread 3 → Edge
   |
   +---- Thread 4 → Chrome
```

The Grid distributes WebDriver sessions across available nodes.

Example:

```java
WebDriver driver =
        new RemoteWebDriver(
                new URL("http://localhost:4444"),
                new ChromeOptions());
```

Each parallel thread should have its own RemoteWebDriver session.

---

# 18. Parallel Execution with DataProvider

TestNG `DataProvider` can also execute data sets in parallel.

```java
import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;

public class LoginTest {

    @DataProvider(
            name = "loginData",
            parallel = true
    )
    public Object[][] loginData() {

        return new Object[][] {
                {"user1", "password1"},
                {"user2", "password2"},
                {"user3", "password3"},
                {"user4", "password4"}
        };
    }

    @Test(dataProvider = "loginData")
    public void loginTest(
            String username,
            String password) {

        System.out.println(
                username + " - Thread: " +
                Thread.currentThread().getId());
    }
}
```

The important setting is:

```java
parallel = true
```

---

# 19. DataProvider + Selenium

When Selenium is used with a parallel DataProvider, every thread must have an independent WebDriver.

Example:

```java
@DataProvider(
        name = "users",
        parallel = true
)
public Object[][] users() {

    return new Object[][] {
            {"user1", "password1"},
            {"user2", "password2"},
            {"user3", "password3"}
    };
}
```

Then:

```java
@Test(dataProvider = "users")
public void loginTest(
        String username,
        String password) {

    DriverManager.getDriver()
            .get("https://example.com/login");

    // Perform login
}
```

---

# 20. Thread Safety

Parallel execution requires thread-safe framework components.

Avoid shared mutable variables such as:

```java
static WebDriver driver;
```

Prefer:

```java
private static ThreadLocal<WebDriver> driver =
        new ThreadLocal<>();
```

Also be careful with:

* Static page objects
* Static test data
* Shared collections
* Shared files
* Shared database records
* Global variables
* Test-generated screenshots
* Reports

---

# 21. Thread-Safe Page Objects

Do not create one global Page Object instance and share it across threads.

Avoid:

```java
public static LoginPage loginPage;
```

Instead create Page Objects using the current thread's driver:

```java
LoginPage loginPage =
        new LoginPage(DriverManager.getDriver());
```

Example:

```java
public class LoginPage {

    private WebDriver driver;

    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }
}
```

---

# 22. Thread-Safe Test Data

Bad:

```java
static String username;
```

If multiple tests modify the variable, one test can overwrite another test's value.

Better approaches include:

* Local variables
* DataProvider
* Immutable objects
* ThreadLocal where appropriate
* Separate test data records

---

# 23. Thread-Safe Screenshots

Parallel tests may overwrite the same screenshot file.

Bad:

```java
screenshots/failure.png
```

Instead include:

* Test name
* Thread ID
* Timestamp

Example:

```java
String fileName =
        testName + "_" +
        Thread.currentThread().getId() + "_" +
        System.currentTimeMillis() +
        ".png";
```

Example output:

```text
loginTest_15_1755601234567.png
loginTest_16_1755601234599.png
```

---

# 24. Parallel Execution and Listeners

Listeners must also be designed carefully.

Example:

```java
@Override
public void onTestFailure(ITestResult result) {

    String testName =
            result.getName();

    long threadId =
            Thread.currentThread().getId();

    System.out.println(
            testName +
            " failed on thread " +
            threadId);
}
```

For screenshots:

```java
WebDriver driver =
        DriverManager.getDriver();
```

The listener retrieves the WebDriver belonging to the current thread.

---

# 25. Parallel Execution and Reports

Reports should contain:

* Test name
* Browser
* Thread ID
* Start time
* End time
* Status
* Screenshot
* Error message

Example:

```text
Test: LoginTest
Browser: Chrome
Thread: 12
Status: PASS
```

This makes debugging parallel failures much easier.

---

# 26. Parallel Execution with Jenkins

A typical CI/CD flow can look like:

```text
GitHub
   ↓
Jenkins
   ↓
Maven
   ↓
TestNG
   ↓
Parallel Selenium Tests
   ↓
Selenium Grid
   ↓
Browsers
   ↓
Test Reports
```

Example Maven command:

```bash
mvn clean test
```

TestNG reads the parallel configuration from:

```text
testng.xml
```

---

# 27. Maven Configuration

Example `pom.xml` dependency:

```xml
<dependency>
    <groupId>org.testng</groupId>
    <artifactId>testng</artifactId>
    <version>7.11.0</version>
    <scope>test</scope>
</dependency>
```

Example Surefire configuration:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.5.3</version>

    <configuration>

        <suiteXmlFiles>
            <suiteXmlFile>
                testng.xml
            </suiteXmlFile>
        </suiteXmlFiles>

    </configuration>
</plugin>
```

---

# 28. Parallel Execution Best Practices

### 1. Use ThreadLocal WebDriver

```java
ThreadLocal<WebDriver>
```

### 2. Never share a WebDriver between threads

Each thread should have its own browser session.

### 3. Avoid static mutable test data

Use local or thread-safe data.

### 4. Use unique screenshot names

Include test name and thread ID.

### 5. Keep tests independent

One test should not depend on another test's execution order.

### 6. Avoid hard-coded waits

Prefer:

```java
WebDriverWait
```

instead of:

```java
Thread.sleep()
```

### 7. Clean up every driver

Use:

```java
driver.quit();
```

### 8. Remove ThreadLocal values

Use:

```java
driver.remove();
```

after the test finishes.

### 9. Choose thread count carefully

More threads do not always mean faster execution.

Consider:

* CPU
* RAM
* Browser memory
* Grid capacity
* Application capacity

### 10. Make test data independent

Parallel tests should not modify the same records simultaneously.

---

# 29. Common Problems

## Problem 1: Tests randomly fail

Possible cause:

```text
Shared WebDriver
```

Solution:

```text
ThreadLocal<WebDriver>
```

---

## Problem 2: One test navigates another test's browser

Cause:

```java
static WebDriver driver;
```

Solution:

```java
ThreadLocal<WebDriver>
```

---

## Problem 3: Screenshots are overwritten

Cause:

```text
failure.png
```

Solution:

```text
testName + threadId + timestamp
```

---

## Problem 4: Tests become slower with more threads

Possible causes:

* Insufficient CPU
* Insufficient memory
* Too many browser sessions
* Selenium Grid bottleneck
* Network bottleneck
* Application server limitations

---

# 30. `parallel="methods"` vs `classes` vs `tests`

| Option                       | What Runs in Parallel |
| ---------------------------- | --------------------- |
| `methods`                    | Test methods          |
| `classes`                    | Test classes          |
| `tests`                      | `<test>` sections     |
| DataProvider `parallel=true` | Data sets             |

Example:

```xml
parallel="methods"
```

```xml
parallel="classes"
```

```xml
parallel="tests"
```

---

# 31. Recommended Framework Architecture

A production Selenium framework can use:

```text
Selenium Framework
│
├── DriverManager
│   └── ThreadLocal<WebDriver>
│
├── DriverFactory
│   ├── Chrome
│   ├── Firefox
│   └── Edge
│
├── BaseTest
│
├── Pages
│   ├── LoginPage
│   ├── HomePage
│   └── SearchPage
│
├── Tests
│
├── Utilities
│
├── Listeners
│
├── Reports
│
└── testng.xml
       │
       └── Parallel Execution
```

---

# 32. Interview Questions

### Q1. What is parallel execution?

Parallel execution means running multiple tests simultaneously using multiple threads.

### Q2. How do you execute Selenium tests in parallel?

Using TestNG:

```xml
parallel="methods"
```

or:

```xml
parallel="classes"
```

or:

```xml
parallel="tests"
```

### Q3. Why should WebDriver not be shared between threads?

Because WebDriver sessions are not intended to be concurrently controlled by multiple test threads.

### Q4. How do you make WebDriver thread-safe?

Use:

```java
ThreadLocal<WebDriver>
```

### Q5. What is ThreadLocal?

`ThreadLocal` provides a separate variable value for each thread.

### Q6. What does `thread-count` do?

It controls the maximum number of parallel threads TestNG can use.

Example:

```xml
thread-count="5"
```

### Q7. Can DataProvider execute tests in parallel?

Yes.

```java
@DataProvider(
    name = "data",
    parallel = true
)
```

### Q8. Can Selenium Grid be used with parallel execution?

Yes. TestNG can create parallel WebDriver sessions that are distributed through Selenium Grid.

### Q9. What happens if you use static WebDriver in parallel execution?

Multiple threads may share the same browser session, resulting in unpredictable behavior and test failures.

### Q10. How do you handle screenshots in parallel execution?

Generate unique filenames using the test name, thread ID, and timestamp.

---

# 33. Important Interview Code

A commonly expected implementation is:

```java
public class DriverManager {

    private static ThreadLocal<WebDriver> driver =
            new ThreadLocal<>();

    public static void setDriver(WebDriver webDriver) {
        driver.set(webDriver);
    }

    public static WebDriver getDriver() {
        return driver.get();
    }

    public static void unload() {
        driver.remove();
    }
}
```

TestNG:

```xml
<suite name="Parallel Suite"
       parallel="methods"
       thread-count="4">
```

Cleanup:

```java
@AfterMethod
public void tearDown() {

    if (DriverManager.getDriver() != null) {

        DriverManager.getDriver().quit();

        DriverManager.unload();
    }
}
```

---

# 34. Key Takeaways

```text
Parallel Selenium Execution
        ↓
TestNG
        ↓
Multiple Threads
        ↓
ThreadLocal<WebDriver>
        ↓
Independent Browser Sessions
        ↓
Selenium Grid / Docker
        ↓
Faster Regression Execution
```

The most important concepts to remember are:

```text
TestNG parallel execution
Thread count
ThreadLocal<WebDriver>
Thread-safe framework
Independent test data
Unique screenshots
Selenium Grid
CI/CD integration
```

A strong Selenium automation framework should be designed for **parallel execution from the beginning**, rather than adding parallel execution after the framework has already been built.
