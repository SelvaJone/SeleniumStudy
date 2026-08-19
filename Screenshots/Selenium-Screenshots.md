# Selenium Screenshots

## 1. Introduction

Screenshots are an important part of Selenium automation.

They help with:

* Debugging failed tests
* Reporting
* Evidence collection
* Regression testing
* CI/CD execution
* Test failure analysis
* Visual verification

Selenium provides screenshot functionality through the:

```java
TakesScreenshot
```

interface.

---

# 2. TakesScreenshot Interface

Import:

```java
import org.openqa.selenium.TakesScreenshot;
```

A WebDriver can be cast to `TakesScreenshot`:

```java
TakesScreenshot screenshot =
        (TakesScreenshot) driver;
```

Then use:

```java
getScreenshotAs()
```

---

# 3. Basic Screenshot

```java
TakesScreenshot screenshot =
        (TakesScreenshot) driver;

File source =
        screenshot.getScreenshotAs(
            OutputType.FILE
        );
```

Import:

```java
import org.openqa.selenium.OutputType;
import java.io.File;
```

---

# 4. Save Screenshot to a File

Using Java NIO:

```java
import java.nio.file.Files;
import java.nio.file.Path;

TakesScreenshot screenshot =
        (TakesScreenshot) driver;

File source =
        screenshot.getScreenshotAs(
            OutputType.FILE
        );

Files.copy(
    source.toPath(),
    Path.of("screenshots/homepage.png")
);
```

---

# 5. Complete Screenshot Example

```java
import java.io.File;
import java.nio.file.Files;
import java.nio.file.Path;

import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class ScreenshotExample {

    public static void main(String[] args)
            throws Exception {

        WebDriver driver =
                new ChromeDriver();

        driver.get("https://example.com");

        TakesScreenshot screenshot =
                (TakesScreenshot) driver;

        File source =
                screenshot.getScreenshotAs(
                    OutputType.FILE
                );

        Files.copy(
            source.toPath(),
            Path.of("screenshots/homepage.png")
        );

        driver.quit();
    }
}
```

---

# 6. OutputType

Selenium supports different screenshot output formats.

Common options include:

```java
OutputType.FILE
OutputType.BYTES
OutputType.BASE64
```

---

# 7. OutputType.FILE

Returns the screenshot as a temporary file.

```java
File source =
    screenshot.getScreenshotAs(
        OutputType.FILE
    );
```

Useful when you want to save the screenshot to disk.

---

# 8. OutputType.BYTES

Returns screenshot data as a byte array.

```java
byte[] screenshotBytes =
    screenshot.getScreenshotAs(
        OutputType.BYTES
    );
```

Import:

```java
import java.util.Arrays;
```

Bytes are useful for:

* Test reports
* Attachments
* APIs
* Custom reporting systems

---

# 9. OutputType.BASE64

Returns the screenshot as a Base64 encoded string.

```java
String base64 =
    screenshot.getScreenshotAs(
        OutputType.BASE64
    );
```

Base64 screenshots are useful for:

* HTML reports
* Extent Reports
* Allure attachments
* Custom reporting

---

# 10. Screenshot Using FileUtils

Apache Commons IO is commonly used in Selenium projects.

Example:

```java
File source =
    ((TakesScreenshot) driver)
        .getScreenshotAs(OutputType.FILE);

FileUtils.copyFile(
    source,
    new File("screenshots/homepage.png")
);
```

Import:

```java
import org.apache.commons.io.FileUtils;
```

If the project uses Maven, the appropriate Commons IO dependency must be available.

---

# 11. Screenshot Using Files.copy()

Java NIO is another good approach.

```java
File source =
    ((TakesScreenshot) driver)
        .getScreenshotAs(OutputType.FILE);

Files.copy(
    source.toPath(),
    Path.of("screenshots/homepage.png")
);
```

Imports:

```java
import java.io.File;
import java.nio.file.Files;
import java.nio.file.Path;
```

---

# 12. Create Screenshot Folder

Before saving screenshots, make sure the directory exists.

```java
Path screenshotFolder =
    Path.of("screenshots");

Files.createDirectories(
    screenshotFolder
);
```

Then:

```java
Path screenshotPath =
    screenshotFolder.resolve(
        "homepage.png"
    );
```

---

# 13. Screenshot With Timestamp

Timestamped screenshots prevent files from being overwritten.

```java
String timestamp =
    LocalDateTime.now()
        .format(
            DateTimeFormatter.ofPattern(
                "yyyyMMdd_HHmmss"
            )
        );

String fileName =
    "homepage_" + timestamp + ".png";
```

Imports:

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
```

---

# 14. Complete Timestamp Screenshot

```java
Path folder =
    Path.of("screenshots");

Files.createDirectories(folder);

String timestamp =
    LocalDateTime.now()
        .format(
            DateTimeFormatter.ofPattern(
                "yyyyMMdd_HHmmss"
            )
        );

Path destination =
    folder.resolve(
        "homepage_" +
        timestamp +
        ".png"
    );

File source =
    ((TakesScreenshot) driver)
        .getScreenshotAs(OutputType.FILE);

Files.copy(
    source.toPath(),
    destination
);
```

---

# 15. Element Screenshot

Modern Selenium versions allow screenshots of individual elements.

Example:

```java
WebElement logo =
    driver.findElement(
        By.id("logo")
    );

File source =
    logo.getScreenshotAs(
        OutputType.FILE
    );
```

Save it:

```java
Files.copy(
    source.toPath(),
    Path.of("screenshots/logo.png")
);
```

---

# 16. Complete Element Screenshot

```java
WebElement logo =
    driver.findElement(
        By.id("logo")
    );

File source =
    logo.getScreenshotAs(
        OutputType.FILE
    );

Files.copy(
    source.toPath(),
    Path.of(
        "screenshots/logo.png"
    )
);
```

---

# 17. Element Screenshot vs Page Screenshot

Page screenshot:

```java
((TakesScreenshot) driver)
    .getScreenshotAs(
        OutputType.FILE
    );
```

Element screenshot:

```java
element.getScreenshotAs(
    OutputType.FILE
);
```

### Page Screenshot

Captures the browser viewport.

### Element Screenshot

Captures the specified element.

---

# 18. Screenshot Utility Class

A reusable screenshot utility is recommended for Selenium frameworks.

```java
import java.io.File;
import java.nio.file.Files;
import java.nio.file.Path;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;

public class ScreenshotUtils {

    public static String captureScreenshot(
            WebDriver driver,
            String testName)
            throws Exception {

        Path folder =
            Path.of("screenshots");

        Files.createDirectories(folder);

        String timestamp =
            LocalDateTime.now()
                .format(
                    DateTimeFormatter.ofPattern(
                        "yyyyMMdd_HHmmss"
                    )
                );

        String fileName =
            testName +
            "_" +
            timestamp +
            ".png";

        Path destination =
            folder.resolve(fileName);

        File source =
            ((TakesScreenshot) driver)
                .getScreenshotAs(
                    OutputType.FILE
                );

        Files.copy(
            source.toPath(),
            destination
        );

        return destination.toString();
    }

    public static String captureElementScreenshot(
            WebElement element,
            String elementName)
            throws Exception {

        Path folder =
            Path.of("screenshots");

        Files.createDirectories(folder);

        String timestamp =
            LocalDateTime.now()
                .format(
                    DateTimeFormatter.ofPattern(
                        "yyyyMMdd_HHmmss"
                    )
                );

        String fileName =
            elementName +
            "_" +
            timestamp +
            ".png";

        Path destination =
            folder.resolve(fileName);

        File source =
            element.getScreenshotAs(
                OutputType.FILE
            );

        Files.copy(
            source.toPath(),
            destination
        );

        return destination.toString();
    }
}
```

---

# 19. Using Screenshot Utility

```java
String path =
    ScreenshotUtils.captureScreenshot(
        driver,
        "LoginTest"
    );

System.out.println(
    "Screenshot saved: " + path
);
```

---

# 20. Element Screenshot Utility

```java
WebElement logo =
    driver.findElement(
        By.id("logo")
    );

String path =
    ScreenshotUtils.captureElementScreenshot(
        logo,
        "CompanyLogo"
    );

System.out.println(
    "Screenshot saved: " + path
);
```

---

# 21. Screenshot on TestNG Failure

One of the most important uses of screenshots is automatically capturing a screenshot when a test fails.

TestNG provides:

```java
ITestResult
```

which can be used to determine whether a test failed.

---

# 22. Basic TestNG Listener

Create:

```java
TestListener.java
```

Example:

```java
import org.testng.ITestListener;
import org.testng.ITestResult;

public class TestListener
        implements ITestListener {

    @Override
    public void onTestFailure(
            ITestResult result) {

        System.out.println(
            "Test failed: "
            + result.getName()
        );
    }
}
```

---

# 23. Screenshot From TestNG Listener

To capture the browser screenshot, the listener needs access to the active WebDriver.

A simple approach is to store the driver in a shared/base-test mechanism.

Example:

```java
public class DriverManager {

    private static ThreadLocal<WebDriver>
            driver = new ThreadLocal<>();

    public static void setDriver(
            WebDriver webDriver) {

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

This approach also works well for parallel execution.

---

# 24. Listener Screenshot Example

```java
import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import org.testng.ITestListener;
import org.testng.ITestResult;

public class TestListener
        implements ITestListener {

    @Override
    public void onTestFailure(
            ITestResult result) {

        WebDriver driver =
            DriverManager.getDriver();

        if (driver != null) {

            String testName =
                result.getName();

            try {

                ScreenshotUtils
                    .captureScreenshot(
                        driver,
                        testName
                    );

            } catch (Exception e) {

                e.printStackTrace();
            }
        }
    }
}
```

---

# 25. Register TestNG Listener

You can register a listener using:

```java
@Listeners(TestListener.class)
```

Example:

```java
import org.testng.annotations.Listeners;

@Listeners(TestListener.class)
public class LoginTest {

    @Test
    public void loginTest() {

        // Test code
    }
}
```

---

# 26. Listener at Suite Level

You can also configure listeners through:

```text
testng.xml
```

Example:

```xml
<listeners>
    <listener class-name="listeners.TestListener"/>
</listeners>
```

This allows the listener to apply to the test suite.

---

# 27. Screenshot on Assertion Failure

Example:

```java
Assert.assertEquals(
    actualTitle,
    expectedTitle
);
```

If the assertion fails, TestNG invokes:

```java
onTestFailure()
```

The listener can then capture the screenshot.

---

# 28. Screenshot Naming

Good screenshot names should include useful information.

Example:

```text
LoginTest_20260819_101530.png
```

A larger framework may use:

```text
TestName_Browser_DateTime.png
```

Example:

```text
LoginTest_Chrome_20260819_101530.png
```

---

# 29. Avoid Spaces in File Names

Instead of:

```text
Login Test Failed.png
```

prefer:

```text
LoginTest_Failed.png
```

or:

```text
LoginTest_20260819_101530.png
```

This makes files easier to handle in CI/CD systems.

---

# 30. Screenshot Directory Structure

A framework can organize screenshots like:

```text
project
│
├── src
│   ├── test
│   └── main
│
├── screenshots
│   ├── passed
│   └── failed
│
├── reports
│
└── testng.xml
```

Another approach:

```text
screenshots
│
├── LoginTest
├── RegistrationTest
├── SearchTest
└── CheckoutTest
```

Choose the structure that fits the reporting system.

---

# 31. Screenshot for Passed Tests

Screenshots do not need to be limited to failures.

Example:

```java
@Test
public void homePageTest() throws Exception {

    driver.get(
        "https://example.com"
    );

    ScreenshotUtils.captureScreenshot(
        driver,
        "HomePage"
    );
}
```

However, capturing every screenshot can increase storage requirements.

For large suites, screenshots are often captured only for failures unless visual evidence is explicitly required.

---

# 32. Screenshot for Specific Test Step

Example:

```java
driver.findElement(
    By.id("username")
).sendKeys("Selva");

ScreenshotUtils.captureScreenshot(
    driver,
    "AfterUsernameEntry"
);

driver.findElement(
    By.id("password")
).sendKeys("Password");

ScreenshotUtils.captureScreenshot(
    driver,
    "AfterPasswordEntry"
);
```

This can be useful during debugging.

---

# 33. Screenshot With Browser Name

Example:

```java
String browser =
    "Chrome";

String testName =
    "LoginTest";

ScreenshotUtils.captureScreenshot(
    driver,
    testName + "_" + browser
);
```

Result:

```text
LoginTest_Chrome_20260819_101530.png
```

---

# 34. Screenshot With Thread ID

This can be useful for parallel execution.

```java
String threadId =
    String.valueOf(
        Thread.currentThread()
              .getId()
    );

ScreenshotUtils.captureScreenshot(
    driver,
    "LoginTest_Thread_" + threadId
);
```

Example:

```text
LoginTest_Thread_15_20260819_101530.png
```

---

# 35. Screenshot in Parallel Execution

When tests run in parallel, avoid using the same file name.

Bad:

```text
LoginTest.png
```

Multiple threads can overwrite it.

Better:

```text
LoginTest_Thread_12.png
LoginTest_Thread_13.png
LoginTest_Thread_14.png
```

Even better:

```text
LoginTest_Chrome_Thread12_20260819_101530.png
```

---

# 36. Screenshot With ThreadLocal Driver

If the framework uses:

```java
ThreadLocal<WebDriver>
```

the screenshot utility should receive the current thread's driver.

Example:

```java
WebDriver driver =
    DriverManager.getDriver();

ScreenshotUtils.captureScreenshot(
    driver,
    "FailedTest"
);
```

This prevents one parallel test from accidentally capturing another test's browser.

---

# 37. Screenshot as Base64

```java
String screenshot =
    ((TakesScreenshot) driver)
        .getScreenshotAs(
            OutputType.BASE64
        );
```

This is useful for reporting systems that support embedded images.

---

# 38. Screenshot as Bytes

```java
byte[] screenshot =
    ((TakesScreenshot) driver)
        .getScreenshotAs(
            OutputType.BYTES
        );
```

This is useful when attaching the screenshot directly to a reporting API.

---

# 39. Full-Page Screenshot

A screenshot from:

```java
TakesScreenshot
```

typically captures the current browser viewport.

Full-page screenshot support depends on the browser and WebDriver implementation.

Modern Selenium provides additional screenshot functionality through:

```java
HasFullPageScreenshot
```

Example:

```java
import org.openqa.selenium.HasFullPageScreenshot;

File screenshot =
    ((HasFullPageScreenshot) driver)
        .getFullPageScreenshotAs(
            OutputType.FILE
        );
```

Browser support and behavior can vary, so verify the target browser/driver combination.

---

# 40. Full-Page Screenshot Example

```java
import java.io.File;
import java.nio.file.Files;
import java.nio.file.Path;

import org.openqa.selenium.HasFullPageScreenshot;
import org.openqa.selenium.OutputType;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class FullPageScreenshot {

    public static void main(String[] args)
            throws Exception {

        WebDriver driver =
            new ChromeDriver();

        driver.get(
            "https://example.com"
        );

        File source =
            ((HasFullPageScreenshot) driver)
                .getFullPageScreenshotAs(
                    OutputType.FILE
                );

        Files.copy(
            source.toPath(),
            Path.of(
                "screenshots/fullpage.png"
            )
        );

        driver.quit();
    }
}
```

---

# 41. Viewport Screenshot vs Full-Page Screenshot

## Viewport

```java
((TakesScreenshot) driver)
    .getScreenshotAs(
        OutputType.FILE
    );
```

Captures the current viewport.

## Full Page

```java
((HasFullPageScreenshot) driver)
    .getFullPageScreenshotAs(
        OutputType.FILE
    );
```

Attempts to capture the complete page.

---

# 42. Screenshot of an Element After Scrolling

Sometimes you may want to capture a specific element.

```java
WebElement element =
    driver.findElement(
        By.id("footer")
    );

((JavascriptExecutor) driver)
    .executeScript(
        "arguments[0].scrollIntoView(true);",
        element
    );

File source =
    element.getScreenshotAs(
        OutputType.FILE
    );
```

---

# 43. Screenshot and Explicit Wait

Wait for the page or element before taking a screenshot.

```java
WebDriverWait wait =
    new WebDriverWait(
        driver,
        Duration.ofSeconds(10)
    );

WebElement element =
    wait.until(
        ExpectedConditions.visibilityOfElementLocated(
            By.id("dashboard")
        )
    );

File source =
    ((TakesScreenshot) driver)
        .getScreenshotAs(
            OutputType.FILE
        );
```

Screenshot timing matters.

Taking a screenshot too early can capture an incomplete page.

---

# 44. Screenshot After Page Load

Example:

```java
WebDriverWait wait =
    new WebDriverWait(
        driver,
        Duration.ofSeconds(10)
    );

wait.until(
    webDriver ->
        ((JavascriptExecutor) webDriver)
            .executeScript(
                "return document.readyState"
            )
            .equals("complete")
);

ScreenshotUtils.captureScreenshot(
    driver,
    "PageLoaded"
);
```

This checks the document loading state.

For dynamic applications, `document.readyState` alone may not mean that all application data has loaded.

---

# 45. Screenshot Utility With Automatic Directory Creation

A robust utility should create the folder automatically.

```java
Path folder =
    Path.of("screenshots");

Files.createDirectories(folder);
```

Then:

```java
Path destination =
    folder.resolve(
        "test.png"
    );
```

This avoids failures caused by missing directories.

---

# 46. Screenshot Utility With Unique File Name

```java
public static String createFileName(
        String testName) {

    String timestamp =
        LocalDateTime.now()
            .format(
                DateTimeFormatter.ofPattern(
                    "yyyyMMdd_HHmmss_SSS"
                )
            );

    return testName
        + "_"
        + timestamp
        + ".png";
}
```

Including milliseconds reduces filename collisions.

---

# 47. Improved Screenshot Utility

```java
import java.io.File;
import java.nio.file.Files;
import java.nio.file.Path;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.WebDriver;

public class ScreenshotUtils {

    private static final DateTimeFormatter
            FORMATTER =
            DateTimeFormatter.ofPattern(
                "yyyyMMdd_HHmmss_SSS"
            );

    public static String captureScreenshot(
            WebDriver driver,
            String testName) {

        try {

            Path folder =
                Path.of("screenshots");

            Files.createDirectories(
                folder
            );

            String timestamp =
                LocalDateTime.now()
                    .format(FORMATTER);

            String fileName =
                testName
                + "_"
                + timestamp
                + ".png";

            Path destination =
                folder.resolve(
                    fileName
                );

            File source =
                ((TakesScreenshot) driver)
                    .getScreenshotAs(
                        OutputType.FILE
                    );

            Files.copy(
                source.toPath(),
                destination
            );

            return destination
                .toAbsolutePath()
                .toString();

        } catch (Exception e) {

            throw new RuntimeException(
                "Unable to capture screenshot",
                e
            );
        }
    }
}
```

---

# 48. Using Improved Utility

```java
String screenshotPath =
    ScreenshotUtils.captureScreenshot(
        driver,
        "LoginTest"
    );

System.out.println(
    screenshotPath
);
```

---

# 49. Screenshot on Failure Framework Flow

A typical framework design:

```text
TestNG Test
     |
     v
Test Executes
     |
     +---- PASS ----> Continue
     |
     +---- FAIL
             |
             v
       onTestFailure()
             |
             v
       Get ThreadLocal Driver
             |
             v
       Screenshot Utility
             |
             v
       Save Screenshot
             |
             v
       Attach to Report
```

---

# 50. Screenshot + TestNG Listener + ThreadLocal

Example architecture:

```text
DriverManager
     |
     +-- ThreadLocal<WebDriver>
                |
                v
         Current Test Driver
                |
                v
         TestNG Listener
                |
                v
         Screenshot Utility
                |
                v
            Screenshot
```

This is a common pattern for parallel Selenium frameworks.

---

# 51. Screenshot + Reporting

Screenshots can be attached to:

* Extent Reports
* Allure Reports
* TestNG reports
* Custom HTML reports
* CI/CD artifacts

A common flow is:

```text
Test Failure
     ↓
Capture Screenshot
     ↓
Get File Path / Bytes / Base64
     ↓
Attach to Report
     ↓
View Failure Evidence
```

---

# 52. Screenshot in CI/CD

When tests run locally:

```text
screenshots/
```

can be opened directly.

When tests run in Jenkins or another CI/CD system, screenshots should be preserved as build artifacts.

Typical flow:

```text
Jenkins
   ↓
Run Selenium Tests
   ↓
Test Failure
   ↓
Screenshot Created
   ↓
Build Artifacts
   ↓
Screenshot Available for Analysis
```

---

# 53. Screenshot Naming Best Practice

Recommended:

```text
<TestName>_<Browser>_<Timestamp>.png
```

Example:

```text
LoginTest_Chrome_20260819_101530_125.png
```

For parallel tests:

```text
LoginTest_Chrome_Thread12_20260819_101530_125.png
```

---

# 54. Common Mistakes

## Mistake 1: Forgetting to cast the driver

Incorrect:

```java
driver.getScreenshotAs(
    OutputType.FILE
);
```

Correct:

```java
((TakesScreenshot) driver)
    .getScreenshotAs(
        OutputType.FILE
    );
```

---

## Mistake 2: Saving to a directory that does not exist

Incorrect:

```text
screenshots/failure/test.png
```

when the directory doesn't exist.

Create it:

```java
Files.createDirectories(
    Path.of("screenshots/failure")
);
```

---

## Mistake 3: Reusing the same filename

Bad:

```text
failure.png
```

Use timestamped names.

---

## Mistake 4: Capturing before the page is ready

Use appropriate waits before capturing.

---

## Mistake 5: Taking screenshots for every step unnecessarily

This can create:

* Large storage usage
* Slow test execution
* Large CI artifacts
* Difficult report navigation

Capture screenshots strategically.

---

# 55. Best Practices

### 1. Capture screenshots on failure

This is usually the most valuable default.

### 2. Use unique names

Include:

* Test name
* Browser
* Timestamp
* Thread ID when needed

### 3. Create directories automatically

Use:

```java
Files.createDirectories()
```

### 4. Use a utility class

Keep screenshot logic in one place.

### 5. Use TestNG listeners

Automate failure screenshots.

### 6. Use ThreadLocal in parallel execution

Capture the screenshot from the correct browser instance.

### 7. Attach screenshots to reports

A screenshot path alone is less useful than a report with clickable evidence.

### 8. Don't store sensitive data unnecessarily

Screenshots can contain:

* User information
* Tokens
* Personal data
* Account information

Treat screenshots as test artifacts that may contain sensitive application data.

---

# 56. Interview Questions

## Q1. Which interface is used to take screenshots?

```java
TakesScreenshot
```

---

## Q2. Which method is used to capture a screenshot?

```java
getScreenshotAs()
```

---

## Q3. How do you capture a screenshot?

```java
File source =
    ((TakesScreenshot) driver)
        .getScreenshotAs(
            OutputType.FILE
        );
```

---

## Q4. What are common OutputType options?

```java
OutputType.FILE
OutputType.BYTES
OutputType.BASE64
```

---

## Q5. How do you take a screenshot of an element?

```java
File source =
    element.getScreenshotAs(
        OutputType.FILE
    );
```

---

## Q6. How do you capture a screenshot after test failure?

Use a TestNG listener:

```java
@Override
public void onTestFailure(
        ITestResult result) {

    // Capture screenshot
}
```

---

## Q7. Why should screenshot filenames be unique?

To prevent one test from overwriting another test's screenshot, especially during parallel execution.

---

## Q8. How do you create the screenshot directory?

```java
Files.createDirectories(
    Path.of("screenshots")
);
```

---

## Q9. What is the difference between FILE and BASE64?

`FILE` returns a screenshot as a file.

`BASE64` returns the screenshot encoded as a Base64 string.

---

## Q10. What is the difference between FILE and BYTES?

`FILE` returns a temporary screenshot file.

`BYTES` returns the screenshot as a byte array.

---

## Q11. How can screenshots be captured automatically for failed tests?

Using a TestNG `ITestListener` and overriding:

```java
onTestFailure()
```

---

## Q12. Why is ThreadLocal useful for screenshots?

When tests run in parallel, each thread has its own WebDriver.

ThreadLocal helps the listener retrieve the correct driver and therefore the correct browser screenshot.

---

## Q13. Can Selenium take an element screenshot?

Yes, modern Selenium versions support:

```java
element.getScreenshotAs(
    OutputType.FILE
);
```

---

## Q14. Does TakesScreenshot always mean a full-page screenshot?

No.

The normal `TakesScreenshot` screenshot generally captures the current viewport.

Full-page screenshot support depends on the WebDriver/browser and can be accessed through capabilities such as:

```java
HasFullPageScreenshot
```

where supported.

---

## Q15. Where should screenshot logic be placed in a framework?

Prefer a reusable utility class and call it from:

* TestNG listeners
* Failure handlers
* Reporting utilities
* Specific test steps when needed

---

# 57. Quick Revision

```text
WebDriver
   |
   v
TakesScreenshot
   |
   v
getScreenshotAs()
   |
   +-- OutputType.FILE
   |
   +-- OutputType.BYTES
   |
   +-- OutputType.BASE64
```

Element screenshot:

```text
WebElement
   |
   v
getScreenshotAs()
```

Failure screenshot:

```text
TestNG
   |
   v
onTestFailure()
   |
   v
DriverManager
   |
   v
ScreenshotUtils
   |
   v
Screenshot
   |
   v
Report / CI Artifact
```

---

# 58. Most Important Code

## Create screenshot

```java
File source =
    ((TakesScreenshot) driver)
        .getScreenshotAs(
            OutputType.FILE
        );
```

## Save screenshot

```java
Files.copy(
    source.toPath(),
    Path.of("screenshots/test.png")
);
```

## Element screenshot

```java
File source =
    element.getScreenshotAs(
        OutputType.FILE
    );
```

## Base64 screenshot

```java
String base64 =
    ((TakesScreenshot) driver)
        .getScreenshotAs(
            OutputType.BASE64
        );
```

## Byte screenshot

```java
byte[] bytes =
    ((TakesScreenshot) driver)
        .getScreenshotAs(
            OutputType.BYTES
        );
```

## Failure listener

```java
@Override
public void onTestFailure(
        ITestResult result) {

    WebDriver driver =
        DriverManager.getDriver();

    ScreenshotUtils.captureScreenshot(
        driver,
        result.getName()
    );
}
```

---

# 59. Key Takeaways

* `TakesScreenshot` is the primary Selenium interface for screenshots.
* `getScreenshotAs()` captures the screenshot.
* `OutputType.FILE` is convenient for saving screenshots.
* `OutputType.BYTES` is useful for programmatic processing and reporting.
* `OutputType.BASE64` is useful for embedding screenshots in reports.
* Selenium can capture individual element screenshots.
* Full-page screenshot support depends on browser/WebDriver capabilities.
* TestNG listeners can automatically capture screenshots on failures.
* ThreadLocal WebDriver is important when running tests in parallel.
* Timestamped filenames prevent overwriting.
* Screenshot utilities should create directories automatically.
* Screenshots are extremely useful for debugging Selenium failures.
* Screenshots should be treated as potentially sensitive test artifacts.
* In a production-quality framework, screenshot capture should be integrated with reporting and CI/CD.
