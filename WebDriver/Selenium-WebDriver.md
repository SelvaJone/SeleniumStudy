# Selenium WebDriver

## 1. What is Selenium WebDriver?

Selenium WebDriver is a browser automation tool used to automate web applications.

It allows us to:

* Open browsers
* Navigate to URLs
* Find web elements
* Click buttons
* Enter text
* Select dropdowns
* Handle alerts
* Handle frames
* Handle multiple windows/tabs
* Capture screenshots
* Execute JavaScript
* Read page information
* Automate end-to-end web application workflows

Selenium WebDriver supports browsers such as:

* Google Chrome
* Microsoft Edge
* Firefox
* Safari

---

# 2. Selenium WebDriver Architecture

A simplified Selenium architecture is:

```text
Java Test Code
      |
      v
Selenium WebDriver API
      |
      v
Browser Driver
      |
      v
Browser
      |
      v
Web Application
```

Example:

```java
WebDriver driver = new ChromeDriver();
```

The Java code communicates with Selenium WebDriver.

WebDriver communicates with the browser through the appropriate browser automation mechanism.

---

# 3. Selenium 4 Architecture

Selenium 4 uses the W3C WebDriver standard.

```text
Test Script
    |
    v
Selenium WebDriver API
    |
    v
W3C WebDriver Protocol
    |
    v
Browser Driver
    |
    v
Browser
```

Examples of browser drivers:

```text
Chrome  -> ChromeDriver
Edge    -> EdgeDriver
Firefox -> FirefoxDriver
Safari  -> SafariDriver
```

---

# 4. Selenium WebDriver Dependencies

If using Maven, add Selenium dependency to `pom.xml`.

```xml
<dependency>
    <groupId>org.seleniumhq.selenium</groupId>
    <artifactId>selenium-java</artifactId>
    <version>4.x.x</version>
</dependency>
```

Use the Selenium version required by your project.

---

# 5. Creating WebDriver

## Chrome

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class Test {

    public static void main(String[] args) {

        WebDriver driver = new ChromeDriver();

        driver.get("https://www.google.com");

        driver.quit();
    }
}
```

---

## Edge

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.edge.EdgeDriver;

public class Test {

    public static void main(String[] args) {

        WebDriver driver = new EdgeDriver();

        driver.get("https://www.google.com");

        driver.quit();
    }
}
```

---

## Firefox

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.firefox.FirefoxDriver;

public class Test {

    public static void main(String[] args) {

        WebDriver driver = new FirefoxDriver();

        driver.get("https://www.google.com");

        driver.quit();
    }
}
```

---

# 6. WebDriver Interface

`WebDriver` is an interface.

```java
WebDriver driver = new ChromeDriver();
```

Here:

```text
WebDriver     -> Interface
ChromeDriver  -> Implementation class
```

This is recommended:

```java
WebDriver driver = new ChromeDriver();
```

Instead of:

```java
ChromeDriver driver = new ChromeDriver();
```

Why?

Because using the interface provides better abstraction and makes it easier to change browsers.

Example:

```java
WebDriver driver;

driver = new ChromeDriver();
```

Later:

```java
driver = new EdgeDriver();
```

---

# 7. Opening a URL

There are two common methods.

## get()

```java
driver.get("https://www.google.com");
```

## navigate().to()

```java
driver.navigate().to("https://www.google.com");
```

Both can navigate to a URL.

---

# 8. get() vs navigate().to()

```java
driver.get("https://www.google.com");
```

and

```java
driver.navigate().to("https://www.google.com");
```

Both open a URL.

The `navigate()` interface also provides:

```java
driver.navigate().back();

driver.navigate().forward();

driver.navigate().refresh();
```

---

# 9. Browser Navigation

## Back

```java
driver.navigate().back();
```

Equivalent to clicking the browser Back button.

---

## Forward

```java
driver.navigate().forward();
```

Equivalent to clicking the browser Forward button.

---

## Refresh

```java
driver.navigate().refresh();
```

Refreshes the current page.

---

## Navigate to another URL

```java
driver.navigate().to("https://www.amazon.com");
```

---

# 10. Closing Browser

## close()

```java
driver.close();
```

`close()` closes the current browser window/tab controlled by WebDriver.

---

## quit()

```java
driver.quit();
```

`quit()` closes all browser windows/tabs associated with the WebDriver session and ends the WebDriver session.

---

# 11. close() vs quit()

| Method    | Purpose                                       |
| --------- | --------------------------------------------- |
| `close()` | Closes current window/tab                     |
| `quit()`  | Closes all windows and ends WebDriver session |

Recommended at the end of a test:

```java
driver.quit();
```

---

# 12. Getting Page Title

```java
String title = driver.getTitle();

System.out.println(title);
```

Example:

```java
driver.get("https://www.google.com");

String title = driver.getTitle();

System.out.println("Page Title: " + title);
```

---

# 13. Getting Current URL

```java
String url = driver.getCurrentUrl();

System.out.println(url);
```

Example:

```java
driver.get("https://www.google.com");

System.out.println(driver.getCurrentUrl());
```

---

# 14. Getting Page Source

```java
String source = driver.getPageSource();

System.out.println(source);
```

This returns the current page source.

It is generally not recommended to use page source as the primary way to locate elements.

Use proper locators instead.

---

# 15. Browser Window Size

Get browser window size:

```java
Dimension size = driver.manage().window().getSize();

System.out.println(size.getWidth());
System.out.println(size.getHeight());
```

Import:

```java
import org.openqa.selenium.Dimension;
```

---

# 16. Set Browser Window Size

```java
driver.manage().window().setSize(
    new Dimension(1200, 800)
);
```

---

# 17. Maximize Browser

```java
driver.manage().window().maximize();
```

---

# 18. Minimize Browser

```java
driver.manage().window().minimize();
```

---

# 19. Full Screen

```java
driver.manage().window().fullscreen();
```

---

# 20. WebDriver Manage Interface

The following are available through:

```java
driver.manage();
```

Examples:

```java
driver.manage().window();

driver.manage().timeouts();

driver.manage().cookies();
```

---

# 21. Implicit Wait

Implicit wait tells WebDriver to wait for a specified amount of time when searching for elements.

Example:

```java
driver.manage().timeouts()
       .implicitlyWait(Duration.ofSeconds(10));
```

Import:

```java
import java.time.Duration;
```

Example:

```java
WebDriver driver = new ChromeDriver();

driver.manage().timeouts()
       .implicitlyWait(Duration.ofSeconds(10));

driver.get("https://example.com");
```

---

# 22. Page Load Timeout

Page load timeout defines how long WebDriver waits for a page load to complete.

```java
driver.manage().timeouts()
       .pageLoadTimeout(Duration.ofSeconds(30));
```

Example:

```java
driver.manage().timeouts()
       .pageLoadTimeout(Duration.ofSeconds(30));
```

---

# 23. Script Timeout

Script timeout applies to asynchronous JavaScript execution.

```java
driver.manage().timeouts()
       .scriptTimeout(Duration.ofSeconds(20));
```

---

# 24. Explicit Wait

Explicit wait waits for a specific condition.

Example:

```java
WebDriverWait wait =
        new WebDriverWait(driver, Duration.ofSeconds(10));

WebElement element = wait.until(
        ExpectedConditions.visibilityOfElementLocated(
                By.id("username")
        )
);
```

Imports:

```java
import java.time.Duration;

import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
```

---

# 25. Fluent Wait

FluentWait allows us to configure:

* Maximum timeout
* Polling interval
* Exceptions to ignore

Example:

```java
Wait<WebDriver> wait = new FluentWait<>(driver)
        .withTimeout(Duration.ofSeconds(30))
        .pollingEvery(Duration.ofSeconds(2))
        .ignoring(NoSuchElementException.class);
```

---

# 26. Important Wait Rule

Avoid mixing implicit and explicit waits unnecessarily.

Example:

```java
driver.manage().timeouts()
       .implicitlyWait(Duration.ofSeconds(10));
```

and:

```java
WebDriverWait wait =
        new WebDriverWait(driver, Duration.ofSeconds(20));
```

Mixing them can make actual wait behavior difficult to predict.

For framework design, a consistent explicit-wait strategy is generally preferred.

---

# 27. Finding Web Elements

Selenium provides:

```java
driver.findElement(By.id("username"));
```

Multiple elements:

```java
driver.findElements(By.tagName("input"));
```

---

# 28. Common Locator Strategies

Selenium supports:

```text
id
name
className
tagName
linkText
partialLinkText
cssSelector
xpath
```

Example:

```java
driver.findElement(By.id("username"));

driver.findElement(By.name("email"));

driver.findElement(By.className("login"));

driver.findElement(By.tagName("input"));

driver.findElement(By.linkText("Login"));

driver.findElement(By.partialLinkText("Log"));

driver.findElement(By.cssSelector("#username"));

driver.findElement(By.xpath("//input[@id='username']"));
```

---

# 29. findElement()

```java
WebElement element =
        driver.findElement(By.id("username"));
```

If the element is not found, Selenium throws:

```text
NoSuchElementException
```

---

# 30. findElements()

```java
List<WebElement> elements =
        driver.findElements(By.tagName("input"));
```

If no elements are found:

```java
elements.size()
```

returns:

```text
0
```

Unlike `findElement()`, `findElements()` does not throw `NoSuchElementException` simply because no matching elements exist.

---

# 31. WebDriver vs WebElement

## WebDriver

Represents the browser/session.

Example:

```java
WebDriver driver = new ChromeDriver();
```

Used for:

```java
driver.get();

driver.close();

driver.quit();

driver.navigate();

driver.manage();
```

---

## WebElement

Represents an element on a webpage.

Example:

```java
WebElement username =
        driver.findElement(By.id("username"));
```

Used for:

```java
username.click();

username.sendKeys("Selva");

username.clear();

username.getText();

username.getAttribute("value");
```

---

# 32. Browser Cookies

Get all cookies:

```java
Set<Cookie> cookies =
        driver.manage().getCookies();
```

---

# 33. Add Cookie

```java
Cookie cookie =
        new Cookie("username", "Selva");

driver.manage().addCookie(cookie);
```

---

# 34. Get Specific Cookie

```java
Cookie cookie =
        driver.manage().getCookieNamed("username");
```

---

# 35. Delete Specific Cookie

```java
driver.manage().deleteCookieNamed("username");
```

---

# 36. Delete All Cookies

```java
driver.manage().deleteAllCookies();
```

---

# 37. Window Handle

Get current window handle:

```java
String currentWindow =
        driver.getWindowHandle();
```

A window handle uniquely identifies a browser window/tab within the current WebDriver session.

---

# 38. Get All Window Handles

```java
Set<String> windows =
        driver.getWindowHandles();
```

Example:

```java
for (String window : driver.getWindowHandles()) {

    System.out.println(window);
}
```

---

# 39. Switching Windows

Example:

```java
String parentWindow =
        driver.getWindowHandle();

Set<String> windows =
        driver.getWindowHandles();

for (String window : windows) {

    if (!window.equals(parentWindow)) {

        driver.switchTo().window(window);

        break;
    }
}
```

---

# 40. Switching Back to Parent Window

```java
driver.switchTo().window(parentWindow);
```

---

# 41. JavaScriptExecutor

`JavascriptExecutor` allows Selenium to execute JavaScript in the browser.

Example:

```java
JavascriptExecutor js =
        (JavascriptExecutor) driver;
```

Execute JavaScript:

```java
js.executeScript(
        "alert('Hello Selenium');"
);
```

---

# 42. Scroll Using JavaScript

Scroll down:

```java
js.executeScript(
        "window.scrollBy(0,500);"
);
```

Scroll to bottom:

```java
js.executeScript(
        "window.scrollTo(0, document.body.scrollHeight);"
);
```

Scroll to an element:

```java
WebElement element =
        driver.findElement(By.id("footer"));

js.executeScript(
        "arguments[0].scrollIntoView(true);",
        element
);
```

---

# 43. Click Using JavaScript

Normal click should be preferred:

```java
element.click();
```

JavaScript click can be used when a normal Selenium click is blocked by certain UI conditions.

```java
js.executeScript(
        "arguments[0].click();",
        element
);
```

Do not use JavaScript click as the default replacement for Selenium click because it can bypass normal browser interaction behavior.

---

# 44. Enter Text Using JavaScript

Normally use:

```java
element.sendKeys("Selva");
```

JavaScript alternative:

```java
js.executeScript(
        "arguments[0].value='Selva';",
        element
);
```

Again, prefer `sendKeys()` for normal user-like interaction.

---

# 45. Reading Text

```java
String text =
        element.getText();

System.out.println(text);
```

---

# 46. Reading Attribute

```java
String value =
        element.getAttribute("value");
```

Example:

```java
String placeholder =
        element.getAttribute("placeholder");
```

---

# 47. Reading DOM Property

Selenium 4 provides:

```java
String value =
        element.getDomProperty("value");
```

---

# 48. Reading DOM Attribute

```java
String attribute =
        element.getDomAttribute("id");
```

---

# 49. Checking Element State

## Is displayed?

```java
boolean displayed =
        element.isDisplayed();
```

## Is enabled?

```java
boolean enabled =
        element.isEnabled();
```

## Is selected?

```java
boolean selected =
        element.isSelected();
```

---

# 50. Taking Screenshot

Selenium supports screenshots using `TakesScreenshot`.

Example:

```java
TakesScreenshot screenshot =
        (TakesScreenshot) driver;

File source =
        screenshot.getScreenshotAs(
                OutputType.FILE
        );
```

Example using Java NIO:

```java
Files.copy(
    source.toPath(),
    Paths.get("screenshot.png"),
    StandardCopyOption.REPLACE_EXISTING
);
```

Imports:

```java
import java.io.File;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.nio.file.StandardCopyOption;

import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
```

---

# 51. Screenshot Utility Method

```java
public static void takeScreenshot(
        WebDriver driver,
        String filePath) throws IOException {

    TakesScreenshot screenshot =
            (TakesScreenshot) driver;

    File source =
            screenshot.getScreenshotAs(
                    OutputType.FILE
            );

    Files.copy(
            source.toPath(),
            Paths.get(filePath),
            StandardCopyOption.REPLACE_EXISTING
    );
}
```

Usage:

```java
takeScreenshot(
        driver,
        "screenshots/login.png"
);
```

---

# 52. Handling Alerts

Switch to alert:

```java
Alert alert =
        driver.switchTo().alert();
```

Accept:

```java
alert.accept();
```

Dismiss:

```java
alert.dismiss();
```

Get alert text:

```java
String text =
        alert.getText();
```

Enter text into prompt:

```java
alert.sendKeys("Selva");
```

---

# 53. Handling Frames

Switch by index:

```java
driver.switchTo().frame(0);
```

Switch by name/id:

```java
driver.switchTo().frame("frameName");
```

Switch using WebElement:

```java
WebElement frame =
        driver.findElement(By.id("frame"));

driver.switchTo().frame(frame);
```

---

# 54. Return From Frame

Return to parent frame:

```java
driver.switchTo().parentFrame();
```

Return to main document:

```java
driver.switchTo().defaultContent();
```

---

# 55. Select Dropdown

For standard HTML `<select>` dropdowns:

```java
Select select =
        new Select(
                driver.findElement(
                        By.id("country")
                )
        );
```

Select by visible text:

```java
select.selectByVisibleText("USA");
```

Select by value:

```java
select.selectByValue("US");
```

Select by index:

```java
select.selectByIndex(1);
```

---

# 56. Handling Multiple Select Options

Check whether dropdown supports multiple selection:

```java
boolean multiple =
        select.isMultiple();
```

Get all options:

```java
List<WebElement> options =
        select.getOptions();
```

---

# 57. WebDriver Navigation Example

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class NavigationExample {

    public static void main(String[] args) {

        WebDriver driver =
                new ChromeDriver();

        driver.manage().window().maximize();

        driver.get("https://www.google.com");

        System.out.println(
                "Title: " + driver.getTitle()
        );

        System.out.println(
                "URL: " + driver.getCurrentUrl()
        );

        driver.navigate().to(
                "https://www.wikipedia.org"
        );

        driver.navigate().back();

        driver.navigate().forward();

        driver.navigate().refresh();

        driver.quit();
    }
}
```

---

# 58. Complete Basic WebDriver Example

```java
import java.time.Duration;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class SeleniumWebDriverExample {

    public static void main(String[] args) {

        WebDriver driver =
                new ChromeDriver();

        try {

            driver.manage().window().maximize();

            driver.manage().timeouts()
                    .implicitlyWait(
                            Duration.ofSeconds(10)
                    );

            driver.get(
                    "https://example.com"
            );

            System.out.println(
                    "Title: " +
                    driver.getTitle()
            );

            System.out.println(
                    "URL: " +
                    driver.getCurrentUrl()
            );

            WebElement element =
                    driver.findElement(
                            By.tagName("h1")
                    );

            System.out.println(
                    "Text: " +
                    element.getText()
            );

        } finally {

            driver.quit();
        }
    }
}
```

---

# 59. Why Use try-finally?

Using:

```java
try {

    // test code

} finally {

    driver.quit();
}
```

ensures that the browser is closed even if an exception occurs.

Example:

```java
try {

    driver.get("https://example.com");

    driver.findElement(
            By.id("invalid")
    ).click();

} finally {

    driver.quit();
}
```

Even if the element is not found, `quit()` will execute.

---

# 60. WebDriver Exception Examples

Common Selenium exceptions include:

```text
NoSuchElementException
TimeoutException
StaleElementReferenceException
ElementNotInteractableException
ElementClickInterceptedException
InvalidSelectorException
InvalidArgumentException
NoSuchWindowException
NoSuchFrameException
NoAlertPresentException
SessionNotCreatedException
WebDriverException
```

---

# 61. NoSuchElementException

Occurs when Selenium cannot find the element.

```java
driver.findElement(
        By.id("doesNotExist")
);
```

Possible causes:

* Incorrect locator
* Element not loaded
* Wrong page
* Element inside iframe
* Dynamic content

---

# 62. StaleElementReferenceException

Occurs when a previously located element is no longer attached to the current DOM.

Example situation:

```text
Find element
     |
     v
Page refreshes / DOM changes
     |
     v
Use old WebElement
     |
     v
StaleElementReferenceException
```

Solution:

Locate the element again.

```java
WebElement element =
        driver.findElement(By.id("username"));

driver.navigate().refresh();

element =
        driver.findElement(By.id("username"));
```

---

# 63. ElementNotInteractableException

Occurs when Selenium finds an element but cannot interact with it.

Possible reasons:

* Hidden element
* Disabled element
* Incorrect element
* Element not ready

Use appropriate waits and verify the element state.

---

# 64. ElementClickInterceptedException

Occurs when another element prevents the target element from receiving the click.

Possible causes:

* Popup
* Overlay
* Modal
* Sticky header
* Animation

Use an appropriate wait and ensure the element is actually clickable.

---

# 65. TimeoutException

Occurs when a wait condition is not satisfied within the specified timeout.

Example:

```java
WebDriverWait wait =
        new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );

wait.until(
        ExpectedConditions.visibilityOfElementLocated(
                By.id("username")
        )
);
```

If the condition is not met, a timeout exception can occur.

---

# 66. SessionNotCreatedException

Can occur when a browser WebDriver session cannot be created.

Possible causes include:

* Browser/driver incompatibility
* Invalid browser configuration
* Unsupported capabilities
* Browser startup problems

Modern Selenium Manager can automatically manage browser drivers in many standard setups.

---

# 67. Selenium Manager

Modern Selenium versions include Selenium Manager.

Example:

```java
WebDriver driver =
        new ChromeDriver();
```

In many standard environments, you do not need to manually download and configure `chromedriver`.

Older approach:

```java
System.setProperty(
    "webdriver.chrome.driver",
    "path/to/chromedriver"
);
```

Modern approach:

```java
WebDriver driver =
        new ChromeDriver();
```

---

# 68. WebDriver Options

Chrome options example:

```java
ChromeOptions options =
        new ChromeOptions();

options.addArguments("--start-maximized");

WebDriver driver =
        new ChromeDriver(options);
```

---

# 69. Headless Browser

Chrome headless example:

```java
ChromeOptions options =
        new ChromeOptions();

options.addArguments("--headless=new");

WebDriver driver =
        new ChromeDriver(options);
```

Useful for:

* CI/CD
* Jenkins
* Docker
* Server environments
* Faster execution where UI display is unnecessary

---

# 70. Browser Arguments

Examples:

```java
options.addArguments("--start-maximized");

options.addArguments("--incognito");

options.addArguments("--disable-notifications");

options.addArguments("--headless=new");
```

Use only arguments appropriate for your browser and test environment.

---

# 71. WebDriver with TestNG

Example:

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

public class LoginTest {

    WebDriver driver;

    @BeforeMethod
    public void setUp() {

        driver = new ChromeDriver();

        driver.manage()
                .window()
                .maximize();
    }

    @Test
    public void loginTest() {

        driver.get(
                "https://example.com"
        );

        System.out.println(
                driver.getTitle()
        );
    }

    @AfterMethod
    public void tearDown() {

        if (driver != null) {
            driver.quit();
        }
    }
}
```

---

# 72. Why Use @BeforeMethod and @AfterMethod?

`@BeforeMethod`:

```java
@BeforeMethod
public void setUp()
```

is used to initialize the browser before each test.

`@AfterMethod`:

```java
@AfterMethod
public void tearDown()
```

is used to close the browser after each test.

This keeps test setup and cleanup separate from test logic.

---

# 73. WebDriver Best Practices

## 1. Use WebDriver interface

Prefer:

```java
WebDriver driver =
        new ChromeDriver();
```

instead of:

```java
ChromeDriver driver =
        new ChromeDriver();
```

---

## 2. Use explicit waits

Prefer meaningful conditions:

```java
wait.until(
    ExpectedConditions.visibilityOfElementLocated(
        By.id("username")
    )
);
```

instead of:

```java
Thread.sleep(5000);
```

---

## 3. Avoid unnecessary Thread.sleep()

Bad:

```java
Thread.sleep(5000);
```

Better:

```java
wait.until(
    ExpectedConditions.elementToBeClickable(
        By.id("login")
    )
);
```

---

## 4. Always quit the driver

```java
driver.quit();
```

Use cleanup even when tests fail.

---

## 5. Use stable locators

Prefer:

```java
By.id("username")
```

or a stable CSS/XPath.

Avoid fragile locators based on changing UI structure.

---

# 74. WebDriver Lifecycle

Typical Selenium test lifecycle:

```text
Create Driver
     |
     v
Configure Browser
     |
     v
Open Application
     |
     v
Find Elements
     |
     v
Perform Actions
     |
     v
Validate Results
     |
     v
Capture Evidence
     |
     v
Quit Driver
```

---

# 75. Typical Framework Driver Flow

In a real automation framework:

```text
TestNG Test
     |
     v
BaseTest
     |
     v
DriverFactory
     |
     v
WebDriver
     |
     v
Page Object
     |
     v
Web Application
```

Example:

```java
DriverFactory.getDriver();
```

The driver can then be passed into page objects.

---

# 76. DriverFactory Example

```java
public class DriverFactory {

    private static WebDriver driver;

    public static void initDriver() {

        driver = new ChromeDriver();
    }

    public static WebDriver getDriver() {

        return driver;
    }

    public static void quitDriver() {

        if (driver != null) {

            driver.quit();
            driver = null;
        }
    }
}
```

---

# 77. Why DriverFactory?

A DriverFactory can centralize:

* Browser creation
* Browser configuration
* Driver lifecycle
* Browser selection
* Remote execution
* ThreadLocal driver management

This is useful in larger automation frameworks.

---

# 78. ThreadLocal WebDriver

For parallel execution, each test thread should generally have its own WebDriver instance.

Example:

```java
public class DriverFactory {

    private static ThreadLocal<WebDriver>
            driver = new ThreadLocal<>();

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
}
```

This becomes especially important when using:

* TestNG parallel execution
* Selenium Grid
* Cloud execution

---

# 79. Local Execution vs Selenium Grid

## Local

```text
Test
 |
 v
WebDriver
 |
 v
Local Browser
```

## Selenium Grid

```text
Test
 |
 v
RemoteWebDriver
 |
 v
Selenium Grid
 |
 +---- Chrome
 |
 +---- Edge
 |
 +---- Firefox
```

---

# 80. RemoteWebDriver

Example:

```java
URL gridUrl =
        new URL("http://localhost:4444");

WebDriver driver =
        new RemoteWebDriver(
                gridUrl,
                new ChromeOptions()
        );
```

`RemoteWebDriver` is commonly used for:

* Selenium Grid
* Docker
* Remote machines
* Cloud testing platforms

---

# 81. WebDriver Important Methods

| Method               | Purpose                            |
| -------------------- | ---------------------------------- |
| `get()`              | Opens URL                          |
| `getTitle()`         | Gets page title                    |
| `getCurrentUrl()`    | Gets current URL                   |
| `getPageSource()`    | Gets page source                   |
| `findElement()`      | Finds one element                  |
| `findElements()`     | Finds multiple elements            |
| `close()`            | Closes current window              |
| `quit()`             | Ends session and closes windows    |
| `getWindowHandle()`  | Gets current window ID             |
| `getWindowHandles()` | Gets all window IDs                |
| `navigate()`         | Browser navigation                 |
| `manage()`           | Window, timeout, cookie management |
| `switchTo()`         | Switches frame/window/alert        |

---

# 82. switchTo() Methods

```java
driver.switchTo().window(windowHandle);

driver.switchTo().frame(frame);

driver.switchTo().defaultContent();

driver.switchTo().parentFrame();

driver.switchTo().alert();
```

---

# 83. manage() Methods

```java
driver.manage().window();

driver.manage().timeouts();

driver.manage().cookies();
```

---

# 84. navigate() Methods

```java
driver.navigate().to(url);

driver.navigate().back();

driver.navigate().forward();

driver.navigate().refresh();
```

---

# 85. Example: Browser Information

```java
WebDriver driver =
        new ChromeDriver();

driver.get("https://example.com");

System.out.println(
        "Title: " +
        driver.getTitle()
);

System.out.println(
        "Current URL: " +
        driver.getCurrentUrl()
);

System.out.println(
        "Window Handle: " +
        driver.getWindowHandle()
);

driver.quit();
```

---

# 86. Interview Questions

## Q1. What is Selenium WebDriver?

Selenium WebDriver is a browser automation API used to automate web applications.

---

## Q2. Is WebDriver a class or interface?

`WebDriver` is an interface.

Example:

```java
WebDriver driver =
        new ChromeDriver();
```

---

## Q3. What is ChromeDriver?

ChromeDriver is the browser-specific implementation used to automate Chrome.

---

## Q4. What is the difference between get() and navigate().to()?

Both can navigate to a URL.

`navigate()` additionally provides:

```java
back()
forward()
refresh()
```

---

## Q5. Difference between close() and quit()?

`close()` closes the current window.

`quit()` closes all windows associated with the session and terminates the WebDriver session.

---

## Q6. Difference between findElement() and findElements()?

`findElement()` returns one WebElement and throws `NoSuchElementException` if no matching element is found.

`findElements()` returns a list and returns an empty list when no matching elements are found.

---

## Q7. What is WebDriver?

WebDriver is the interface used to control a browser session.

---

## Q8. What is WebElement?

WebElement represents an element in the web page DOM.

---

## Q9. What is getWindowHandle()?

It returns the handle of the current browser window/tab.

---

## Q10. What is getWindowHandles()?

It returns handles for all browser windows/tabs in the current session.

---

## Q11. What is JavascriptExecutor?

It allows JavaScript code to be executed in the browser.

---

## Q12. What is Selenium Manager?

Selenium Manager is Selenium's built-in driver/browser management utility that can automatically manage browser drivers in many standard configurations.

---

## Q13. What is implicit wait?

Implicit wait tells WebDriver how long to wait when locating elements.

Example:

```java
driver.manage().timeouts()
       .implicitlyWait(
           Duration.ofSeconds(10)
       );
```

---

## Q14. What is explicit wait?

Explicit wait waits for a particular condition.

Example:

```java
wait.until(
    ExpectedConditions.visibilityOfElementLocated(
        By.id("username")
    )
);
```

---

## Q15. What is FluentWait?

FluentWait allows customization of:

* Timeout
* Polling interval
* Ignored exceptions

---

## Q16. Why should we avoid Thread.sleep()?

Because it introduces a fixed delay regardless of whether the application is ready.

Explicit waits are generally more efficient and reliable.

---

## Q17. How do you handle alerts?

```java
Alert alert =
        driver.switchTo().alert();

alert.accept();
```

---

## Q18. How do you handle frames?

```java
driver.switchTo().frame(
        frameElement
);
```

Return:

```java
driver.switchTo().defaultContent();
```

---

## Q19. How do you switch between windows?

```java
driver.switchTo().window(
        windowHandle
);
```

---

## Q20. How do you maximize a browser?

```java
driver.manage()
      .window()
      .maximize();
```

---

# 87. Quick Revision

```text
WebDriver
    |
    +-- get()
    +-- getTitle()
    +-- getCurrentUrl()
    +-- getPageSource()
    +-- findElement()
    +-- findElements()
    +-- close()
    +-- quit()
    +-- navigate()
    +-- manage()
    +-- switchTo()
    +-- getWindowHandle()
    +-- getWindowHandles()
```

Navigation:

```java
driver.get(url);

driver.navigate().to(url);

driver.navigate().back();

driver.navigate().forward();

driver.navigate().refresh();
```

Browser:

```java
driver.manage().window().maximize();

driver.manage().window().minimize();

driver.manage().window().fullscreen();
```

Wait:

```java
driver.manage().timeouts()
       .implicitlyWait(Duration.ofSeconds(10));
```

Explicit:

```java
WebDriverWait wait =
        new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );
```

Window:

```java
driver.getWindowHandle();

driver.getWindowHandles();

driver.switchTo().window(handle);
```

Frame:

```java
driver.switchTo().frame(frame);

driver.switchTo().defaultContent();
```

Alert:

```java
driver.switchTo().alert();

alert.accept();

alert.dismiss();
```

JavaScript:

```java
JavascriptExecutor js =
        (JavascriptExecutor) driver;

js.executeScript("...");
```

Cleanup:

```java
driver.quit();
```

---

# 88. Recommended Selenium Study Order

After completing this WebDriver file, continue with:

```text
01-Basics/
02-WebDriver/
03-WebElements/
04-Locators/
05-Waits/
06-Dropdowns/
07-Alerts/
08-Frames/
09-Windows/
10-Actions/
11-JavaScriptExecutor/
12-Screenshots/
13-Cookies/
14-Tables/
15-DatePickers/
16-FileUpload/
17-FileDownload/
18-Popups/
19-AdvancedWebDriver/
20-TestNG/
21-PageObjectModel/
22-PageFactory/
23-Utilities/
24-DataDrivenTesting/
25-Listeners/
26-ParallelExecution/
27-SeleniumGrid/
28-Framework/
29-Jenkins/
30-InterviewQuestions/
```

This sequence takes you from **Selenium fundamentals → WebDriver → WebElements → advanced Selenium → framework development → CI/CD**.

---

# 89. Key Takeaways

Remember these core concepts:

```text
WebDriver = Browser automation interface

ChromeDriver = Chrome implementation

WebElement = Element on webpage

findElement() = One element

findElements() = Multiple elements

get() = Open URL

navigate() = Navigate/back/forward/refresh

close() = Current window

quit() = Entire WebDriver session

getWindowHandle() = Current window

getWindowHandles() = All windows

switchTo() = Window/frame/alert switching

manage() = Window/timeout/cookie management

JavascriptExecutor = Execute JavaScript

WebDriverWait = Explicit wait

RemoteWebDriver = Remote/Grid execution

ThreadLocal<WebDriver> = Parallel execution support
```

**File path:**

```text
SeleniumStudy/
└── WebDriver/
    └── Selenium-WebDriver.md
```
