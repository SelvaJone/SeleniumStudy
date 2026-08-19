# Selenium Exceptions

Selenium exceptions are errors thrown by Selenium WebDriver when an operation cannot be completed successfully.

Understanding Selenium exceptions is important for:

- Debugging automation failures
- Writing stable Selenium tests
- Handling synchronization problems
- Identifying locator issues
- Troubleshooting browser and WebDriver problems
- Selenium interviews

---

## 1. Selenium Exception Hierarchy

Most Selenium exceptions inherit from `WebDriverException`.

```text
Throwable
   |
   +-- Exception
        |
        +-- WebDriverException
              |
              +-- NoSuchElementException
              +-- StaleElementReferenceException
              +-- TimeoutException
              +-- ElementNotInteractableException
              +-- ElementClickInterceptedException
              +-- NoSuchFrameException
              +-- NoSuchWindowException
              +-- NoAlertPresentException
              +-- InvalidSelectorException
              +-- InvalidArgumentException
              +-- SessionNotCreatedException
              +-- JavascriptException
              +-- MoveTargetOutOfBoundsException
2. WebDriverException

WebDriverException is the base exception for many Selenium WebDriver-related errors.

Example
try {
    driver.get("https://example.com");
} catch (WebDriverException e) {
    System.out.println("WebDriver error: " + e.getMessage());
}
Common causes
Browser crashes
Driver problems
Browser/driver incompatibility
Invalid WebDriver operations
Session problems
3. NoSuchElementException

Occurs when Selenium cannot find an element using the provided locator.

Example
driver.findElement(By.id("username")).sendKeys("admin");

If the element does not exist:

NoSuchElementException
Common causes
Incorrect locator
Element not loaded yet
Wrong page
Element inside an iframe
Dynamic content
Better approach

Use an explicit wait:

WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));


WebElement username = wait.until(
    ExpectedConditions.visibilityOfElementLocated(By.id("username"))
);


username.sendKeys("admin");
4. StaleElementReferenceException

Occurs when an element was previously located, but the DOM has changed and the old element reference is no longer valid.

Example
WebElement button = driver.findElement(By.id("submit"));


driver.navigate().refresh();


button.click();

This may throw:

StaleElementReferenceException
Common causes
Page refresh
DOM refresh
AJAX update
Navigation
Element recreated by JavaScript
Solution

Locate the element again.

driver.navigate().refresh();


WebElement button = driver.findElement(By.id("submit"));
button.click();
With explicit wait
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));


WebElement button = wait.until(
    ExpectedConditions.elementToBeClickable(By.id("submit"))
);


button.click();
5. ElementNotInteractableException

Occurs when Selenium finds the element but cannot interact with it.

Example
driver.findElement(By.id("username")).sendKeys("admin");

The element may exist but be:

Hidden
Disabled
Not ready
Outside an interactable state
Solution

Use appropriate waits.

WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));


WebElement username = wait.until(
    ExpectedConditions.elementToBeClickable(By.id("username"))
);


username.sendKeys("admin");
6. ElementClickInterceptedException

Occurs when Selenium tries to click an element but another element is blocking it.

Example
driver.findElement(By.id("submit")).click();

A popup, overlay, or another element may be covering the button.

Common causes
Popup
Loading overlay
Sticky header
Modal dialog
Animation
Another element overlapping the target
Solution

Wait for the element to become clickable.

WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));


WebElement button = wait.until(
    ExpectedConditions.elementToBeClickable(By.id("submit"))
);


button.click();
Scroll into view
WebElement button = driver.findElement(By.id("submit"));


((JavascriptExecutor) driver).executeScript(
    "arguments[0].scrollIntoView(true);",
    button
);


button.click();

Use JavaScript click only when appropriate. Prefer normal Selenium clicks whenever possible.

7. TimeoutException

Occurs when an expected condition is not satisfied within the specified timeout.

Example
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));


wait.until(
    ExpectedConditions.visibilityOfElementLocated(By.id("username"))
);

If the element never becomes visible:

TimeoutException
Common causes
Element takes too long to load
Incorrect locator
Application issue
Network delay
Wrong expected condition
Best practice

Use meaningful timeout values and correct expected conditions.

8. NoSuchFrameException

Occurs when Selenium tries to switch to a frame that does not exist.

Example
driver.switchTo().frame("paymentFrame");

If the frame is not found:

NoSuchFrameException
Correct approach

Wait for the frame:

WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));


wait.until(
    ExpectedConditions.frameToBeAvailableAndSwitchToIt("paymentFrame")
);
Switch back
driver.switchTo().defaultContent();
9. NoSuchWindowException

Occurs when Selenium tries to switch to a browser window or tab that does not exist.

Example
driver.switchTo().window("invalid-window-handle");
Correct window handling
String parentWindow = driver.getWindowHandle();


for (String window : driver.getWindowHandles()) {
    if (!window.equals(parentWindow)) {
        driver.switchTo().window(window);
    }
}
10. NoSuchWindowException Example
String parentWindow = driver.getWindowHandle();


Set<String> windows = driver.getWindowHandles();


for (String window : windows) {
    driver.switchTo().window(window);


    System.out.println(driver.getTitle());
}


driver.switchTo().window(parentWindow);

Always verify that the window handle exists before switching.

11. NoAlertPresentException

Occurs when Selenium tries to interact with an alert that is not currently present.

Example
driver.switchTo().alert();

If there is no alert:

NoAlertPresentException
Better approach
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));


Alert alert = wait.until(
    ExpectedConditions.alertIsPresent()
);


alert.accept();
12. InvalidSelectorException

Occurs when an invalid XPath or CSS selector is used.

Incorrect XPath
driver.findElement(By.xpath("//input[@id="username"]"));

The quotation marks are incorrect.

Correct XPath
driver.findElement(By.xpath("//input[@id='username']"));
Another example
driver.findElement(By.cssSelector("input["));

This can cause:

InvalidSelectorException
Best practice

Validate XPath and CSS selectors before using them in automation.

13. InvalidArgumentException

Occurs when an invalid argument is passed to a WebDriver command.

Example
driver.get(null);

This can result in:

InvalidArgumentException
Common causes
Invalid URL
Invalid timeout
Invalid script argument
Incorrect WebDriver command parameters
14. SessionNotCreatedException

Occurs when Selenium cannot create a browser session.

Example
WebDriver driver = new ChromeDriver();

The browser session may fail to start.

Common causes
Browser and driver incompatibility
Invalid browser configuration
Browser not installed
Driver configuration problem
Incorrect capabilities
Remote WebDriver configuration issue
Example message
SessionNotCreatedException:
This version of ChromeDriver only supports Chrome version ...
Solution

Make sure the browser and driver versions are compatible.

Modern Selenium Manager can automatically manage browser drivers in many configurations:

WebDriver driver = new ChromeDriver();
15. JavascriptException

Occurs when JavaScript execution fails.

Example
JavascriptExecutor js = (JavascriptExecutor) driver;


js.executeScript("invalid javascript");

This can result in:

JavascriptException
Correct example
JavascriptExecutor js = (JavascriptExecutor) driver;


js.executeScript(
    "arguments[0].click();",
    driver.findElement(By.id("submit"))
);

Use JavaScript carefully because normal Selenium interactions should generally be preferred.

16. MoveTargetOutOfBoundsException

Occurs when Selenium tries to move the mouse to a location outside the browser's valid area.

Example
Actions actions = new Actions(driver);


actions.moveByOffset(-10000, -10000).perform();
Common causes
Invalid coordinates
Element outside viewport
Incorrect offset
Page layout changes
Better approach

Move directly to the element:

WebElement element = driver.findElement(By.id("menu"));


Actions actions = new Actions(driver);


actions.moveToElement(element).perform();
17. ElementNotSelectableException

Occurs when an element cannot be selected.

This is more commonly associated with attempting to select an option that is not selectable.

Example
WebElement option = driver.findElement(
    By.xpath("//option[@value='invalid']")
);

If the option cannot be selected, Selenium may throw an element-selection-related exception.

For standard HTML <select> elements, use Select:

Select select = new Select(
    driver.findElement(By.id("country"))
);


select.selectByVisibleText("USA");
18. InvalidElementStateException

Occurs when an element is in an invalid state for the requested operation.

Example
WebElement element = driver.findElement(By.id("username"));


element.clear();

If the element cannot be cleared because of its state, Selenium may throw:

InvalidElementStateException
Common causes
Element is disabled
Element is read-only
Element is not editable
19. DetachedShadowRootException

Modern web applications may use Shadow DOM.

If a previously obtained shadow root becomes detached from the DOM, Selenium may throw:

DetachedShadowRootException
Common cause

The application replaces the Shadow DOM component.

Solution

Locate the shadow root again after the DOM changes.

20. NoSuchShadowRootException

Occurs when Selenium attempts to access a Shadow DOM root that does not exist.

Example
WebElement host = driver.findElement(By.cssSelector("my-component"));


SearchContext shadowRoot = host.getShadowRoot();

If the element does not have a shadow root:

NoSuchShadowRootException
21. DetachedShadowRootException vs StaleElementReferenceException
Exception	Meaning
StaleElementReferenceException	Previously located element is no longer attached to the DOM
DetachedShadowRootException	Previously obtained Shadow DOM root is detached
NoSuchShadowRootException	Requested Shadow DOM root does not exist
22. Common Selenium Exceptions
Exception	Typical Cause	Common Solution
NoSuchElementException	Element not found	Check locator / use wait
StaleElementReferenceException	DOM changed	Locate element again
ElementNotInteractableException	Element cannot be interacted with	Wait for correct state
ElementClickInterceptedException	Another element blocks click	Wait / close overlay
TimeoutException	Condition not met in time	Fix condition / synchronization
NoSuchFrameException	Frame not found	Verify frame / wait
NoSuchWindowException	Window not found	Verify window handle
NoAlertPresentException	Alert does not exist	Wait for alert
InvalidSelectorException	Invalid XPath/CSS	Fix locator
InvalidArgumentException	Invalid argument	Check method parameters
SessionNotCreatedException	Browser session failed	Check browser/driver/config
JavascriptException	JavaScript failed	Fix JavaScript
MoveTargetOutOfBoundsException	Invalid mouse position	Move to valid element
InvalidElementStateException	Element state invalid	Check enabled/editable state
NoSuchShadowRootException	Shadow root unavailable	Verify Shadow DOM
DetachedShadowRootException	Shadow root detached	Locate shadow root again
23. Exception Handling Using try-catch

Java allows Selenium exceptions to be handled using try-catch.

try {


    driver.findElement(By.id("username")).sendKeys("admin");


} catch (NoSuchElementException e) {


    System.out.println("Username field was not found.");


}
24. Multiple Catch Blocks
try {


    driver.findElement(By.id("submit")).click();


} catch (NoSuchElementException e) {


    System.out.println("Element was not found.");


} catch (ElementClickInterceptedException e) {


    System.out.println("Another element is blocking the click.");


} catch (TimeoutException e) {


    System.out.println("Operation timed out.");


}
25. Catch WebDriverException

You can catch the parent exception when appropriate.

try {


    driver.get("https://example.com");


} catch (WebDriverException e) {


    System.out.println(
        "WebDriver error: " + e.getMessage()
    );
}

However, catching a specific exception is usually better when you know what failure you want to handle.

26. finally Block

The finally block executes whether an exception occurs or not.

WebDriver driver = null;


try {


    driver = new ChromeDriver();


    driver.get("https://example.com");


} catch (WebDriverException e) {


    System.out.println("WebDriver error.");


} finally {


    if (driver != null) {
        driver.quit();
    }
}
27. throw vs throws

These are Java concepts that are commonly used when discussing Selenium exception handling.

throw

Used to explicitly throw an exception.

if (username == null) {
    throw new IllegalArgumentException(
        "Username cannot be null"
    );
}
throws

Used in a method declaration.

public void login() throws Exception {
    // code
}
28. Do Not Use Thread.sleep() as the Main Solution

A common beginner approach is:

Thread.sleep(5000);


driver.findElement(By.id("submit")).click();

This is not ideal because:

It always waits the full duration
Tests become slower
Application timing can vary
It does not guarantee that the element is ready

Prefer explicit waits.

WebDriverWait wait = new WebDriverWait(
    driver,
    Duration.ofSeconds(10)
);


WebElement button = wait.until(
    ExpectedConditions.elementToBeClickable(By.id("submit"))
);


button.click();
29. Handling StaleElementReferenceException

A simple retry approach can be used carefully.

int attempts = 0;


while (attempts < 3) {


    try {


        driver.findElement(By.id("submit")).click();
        break;


    } catch (StaleElementReferenceException e) {


        attempts++;


    }
}

A better framework approach is usually to re-locate the element through an explicit wait.

30. Handling ElementClickInterceptedException
WebDriverWait wait = new WebDriverWait(
    driver,
    Duration.ofSeconds(10)
);


try {


    WebElement button = wait.until(
        ExpectedConditions.elementToBeClickable(
            By.id("submit")
        )
    );


    button.click();


} catch (ElementClickInterceptedException e) {


    System.out.println(
        "Click was intercepted: " + e.getMessage()
    );
}
31. Handling TimeoutException
WebDriverWait wait = new WebDriverWait(
    driver,
    Duration.ofSeconds(10)
);


try {


    wait.until(
        ExpectedConditions.visibilityOfElementLocated(
            By.id("dashboard")
        )
    );


} catch (TimeoutException e) {


    System.out.println(
        "Dashboard did not load within timeout."
    );
}
32. Handling NoSuchElementException in Utility Methods

A utility method can handle common Selenium exceptions.

public boolean isElementDisplayed(
        WebDriver driver,
        By locator) {


    try {


        return driver.findElement(locator).isDisplayed();


    } catch (NoSuchElementException e) {


        return false;
    }
}

Usage:

boolean displayed = isElementDisplayed(
    driver,
    By.id("username")
);


System.out.println(displayed);
33. Better Element Validation Utility
public boolean isElementPresent(
        WebDriver driver,
        By locator) {


    try {


        driver.findElement(locator);
        return true;


    } catch (NoSuchElementException e) {


        return false;
    }
}
34. Exception Handling in Page Object Model

Example Page Object:

public class LoginPage {


    private WebDriver driver;


    private By username =
        By.id("username");


    private By password =
        By.id("password");


    private By loginButton =
        By.id("login");


    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }


    public void login(
            String user,
            String pass) {


        try {


            driver.findElement(username)
                  .sendKeys(user);


            driver.findElement(password)
                  .sendKeys(pass);


            driver.findElement(loginButton)
                  .click();


        } catch (NoSuchElementException e) {


            System.out.println(
                "Login element not found: "
                + e.getMessage()
            );


            throw e;
        }
    }
}
35. Why We Should Not Silently Ignore Exceptions

Avoid this:

try {


    driver.findElement(By.id("submit")).click();


} catch (Exception e) {


}

This is dangerous because the test may continue even though something failed.

Instead:

try {


    driver.findElement(By.id("submit")).click();


} catch (Exception e) {


    System.out.println(
        "Click failed: " + e.getMessage()
    );


    throw e;
}
36. Do Not Catch Exception Too Broadly

Avoid:

catch (Exception e) {
    // handle everything
}

Prefer:

catch (NoSuchElementException e) {
    // Handle missing element
}

or:

catch (TimeoutException e) {
    // Handle timeout
}

Specific exception handling makes debugging easier.

37. Logging Exceptions

Instead of relying only on System.out.println(), enterprise frameworks commonly use logging frameworks such as Log4j or SLF4J.

Example:

try {


    driver.findElement(By.id("submit")).click();


} catch (ElementClickInterceptedException e) {


    logger.error(
        "Unable to click Submit button",
        e
    );


    throw e;
}

Logging the exception stack trace helps identify the root cause.

38. Taking Screenshot When an Exception Occurs

A common automation framework practice is to capture a screenshot when a test fails.

try {


    driver.findElement(By.id("submit")).click();


} catch (Exception e) {


    // Take screenshot here


    throw e;
}

A reusable screenshot utility can be called:

ScreenshotUtil.captureScreenshot(
    driver,
    "SubmitButtonFailure"
);

This is especially useful in CI/CD environments.

39. TestNG Exception Handling

TestNG can report exceptions automatically as test failures.

@Test
public void loginTest() {


    driver.findElement(
        By.id("invalidLocator")
    ).click();
}

If Selenium throws:

NoSuchElementException

TestNG marks the test as failed.

40. TestNG Expected Exceptions

TestNG provides expectedExceptions.

@Test(
    expectedExceptions = NoSuchElementException.class
)
public void testInvalidElement() {


    driver.findElement(
        By.id("invalidElement")
    );
}

This test passes if the expected exception occurs.

Use this feature mainly when the exception itself is the behavior you intentionally want to validate.

41. Assertions vs Exceptions

An assertion verifies an expected result.

Assert.assertEquals(
    actualTitle,
    expectedTitle
);

An exception indicates that an operation could not be completed.

driver.findElement(
    By.id("submit")
).click();

If the element is missing:

NoSuchElementException

Both are important in test automation, but they serve different purposes.

42. Selenium Exception Debugging Strategy

When a Selenium test fails:

Step 1: Read the exception name

Example:

NoSuchElementException
Step 2: Read the message

Look for:

Locator
URL
Element information
Browser details
Step 3: Check the locator

Verify:

By.id("username")
Step 4: Check page state

Ask:

Am I on the correct page?
Is the element inside an iframe?
Is a popup open?
Did navigation complete?
Step 5: Check synchronization

Use:

WebDriverWait

instead of unnecessary fixed sleeps.

Step 6: Check DOM changes

For:

StaleElementReferenceException

re-locate the element.

Step 7: Check browser/driver compatibility

For:

SessionNotCreatedException

check browser and driver configuration.

43. Common Exception → Root Cause Mapping
NoSuchElementException
        ↓
Element cannot be located
        ↓
Check locator / iframe / wait
StaleElementReferenceException
        ↓
DOM changed
        ↓
Locate element again
ElementNotInteractableException
        ↓
Element exists but cannot be interacted with
        ↓
Check visibility / enabled state / wait
ElementClickInterceptedException
        ↓
Another element blocks the target
        ↓
Check overlay / popup / animation
TimeoutException
        ↓
Condition not satisfied
        ↓
Check wait condition / locator / application
SessionNotCreatedException
        ↓
Browser session cannot start
        ↓
Check browser / driver / capabilities
44. Best Practices for Selenium Exception Handling
Use explicit waits.
Avoid unnecessary Thread.sleep().
Use stable locators.
Re-locate elements after DOM refreshes.
Handle frames correctly.
Handle windows/tabs using window handles.
Wait for alerts before interacting with them.
Do not swallow exceptions.
Catch specific exceptions whenever possible.
Log exception details.
Capture screenshots on failures.
Use reusable utility methods.
Keep exception handling close to the operation that needs recovery.
Re-throw exceptions when the test should fail.
Investigate the root cause instead of simply increasing timeout values.
Keep browser and driver versions compatible.
Use framework-level listeners for centralized failure handling.
Avoid excessive retry logic because it can hide real application defects.
45. Interview Questions
Q1. What is NoSuchElementException?

It occurs when Selenium cannot locate an element using the specified locator.

Q2. What causes StaleElementReferenceException?

It occurs when an element reference is no longer attached to the current DOM, usually because the DOM changed or the page was refreshed.

Q3. How do you handle StaleElementReferenceException?

Re-locate the element after the DOM changes and use appropriate explicit waits.

Q4. What is the difference between NoSuchElementException and StaleElementReferenceException?
NoSuchElementException
→ Element cannot be found.


StaleElementReferenceException
→ Element was found earlier but the old reference is no longer valid.
Q5. What is ElementNotInteractableException?

It occurs when Selenium finds the element but the element cannot currently be interacted with.

Q6. What is ElementClickInterceptedException?

It occurs when another element is blocking the element Selenium is trying to click.

Q7. What is TimeoutException?

It occurs when an expected condition is not satisfied within the specified timeout.

Q8. What is NoSuchFrameException?

It occurs when Selenium attempts to switch to a frame that cannot be found.

Q9. What is NoSuchWindowException?

It occurs when Selenium attempts to switch to a browser window or tab that does not exist.

Q10. What is NoAlertPresentException?

It occurs when Selenium tries to interact with an alert that is not currently present.

Q11. What is InvalidSelectorException?

It occurs when an invalid XPath or CSS selector is provided.

Q12. What is SessionNotCreatedException?

It occurs when Selenium cannot create a browser session, commonly because of browser/driver incompatibility or invalid configuration.

Q13. How do you handle Selenium exceptions?

Use:

Explicit waits
Correct locators
Proper frame/window handling
Specific try-catch blocks where recovery is appropriate
Logging
Screenshots
TestNG listeners
Proper exception propagation
Q14. Should we catch every Selenium exception?

No.

Catch exceptions when you can meaningfully recover, add useful diagnostics, or perform cleanup.

Do not catch exceptions simply to prevent the test from failing.

Q15. What is the difference between throw and throws?
throw
→ Explicitly throws an exception.


throws
→ Declares that a method may throw an exception.

Example:

throw new RuntimeException("Test failed");
public void login() throws Exception {
}
Q16. Why should Thread.sleep() not be used for exception handling?

Because Thread.sleep() does not verify whether the required condition has actually been satisfied.

Explicit waits are more reliable.

Q17. How can you capture a screenshot when an exception occurs?

Use Selenium's TakesScreenshot.

TakesScreenshot screenshot =
    (TakesScreenshot) driver;


File source =
    screenshot.getScreenshotAs(OutputType.FILE);

The framework can save the file and attach it to reports.

Q18. How do you handle exceptions globally?

A Selenium framework can use TestNG listeners such as ITestListener.

Example:

public class TestListener
        implements ITestListener {


    @Override
    public void onTestFailure(
            ITestResult result) {


        // Capture screenshot
        // Log exception
        // Attach screenshot to report
    }
}
Q19. Why is global exception handling useful?

It provides centralized handling for:

Screenshots
Logs
Reports
Failure diagnostics
Cleanup

Instead of repeating the same code in every test.

Q20. What is the best way to debug a Selenium exception?

Use this sequence:

Exception name
      ↓
Exception message
      ↓
Locator
      ↓
Page state
      ↓
Frame/window/alert
      ↓
Synchronization
      ↓
DOM changes
      ↓
Browser/driver configuration
46. Practical Login Example
import java.time.Duration;


import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;


public class LoginTest {


    public static void main(String[] args) {


        WebDriver driver = new ChromeDriver();


        try {


            driver.manage().window().maximize();


            driver.get("https://example.com");


            WebDriverWait wait =
                new WebDriverWait(
                    driver,
                    Duration.ofSeconds(10)
                );


            WebElement username =
                wait.until(
                    ExpectedConditions.visibilityOfElementLocated(
                        By.id("username")
                    )
                );


            username.sendKeys("admin");


            WebElement password =
                wait.until(
                    ExpectedConditions.visibilityOfElementLocated(
                        By.id("password")
                    )
                );


            password.sendKeys("password");


            WebElement loginButton =
                wait.until(
                    ExpectedConditions.elementToBeClickable(
                        By.id("login")
                    )
                );


            loginButton.click();


        } catch (NoSuchElementException e) {


            System.out.println(
                "Element not found: "
                + e.getMessage()
            );


        } catch (TimeoutException e) {


            System.out.println(
                "Operation timed out: "
                + e.getMessage()
            );


        } catch (WebDriverException e) {


            System.out.println(
                "WebDriver error: "
                + e.getMessage()
            );


        } finally {


            driver.quit();
        }
    }
}
47. Recommended Framework Approach

In a real Selenium framework, avoid putting large try-catch blocks in every test.

Instead:

Test
 ↓
Page Object
 ↓
Utility / Wait
 ↓
Exception
 ↓
TestNG Listener
 ↓
Screenshot + Log + Report

For example:

LoginTest
    |
    +-- LoginPage
    |
    +-- WaitUtils
    |
    +-- ScreenshotUtils
    |
    +-- TestListener
    |
    +-- Extent/Report

This keeps the framework maintainable.

48. Important Interview Summary

Remember these common mappings:

Element not found
→ NoSuchElementException


Element reference became invalid
→ StaleElementReferenceException


Element exists but cannot be interacted with
→ ElementNotInteractableException


Click blocked by another element
→ ElementClickInterceptedException


Expected condition not satisfied
→ TimeoutException


Frame not found
→ NoSuchFrameException


Window/tab not found
→ NoSuchWindowException


Alert not present
→ NoAlertPresentException


Invalid XPath/CSS
→ InvalidSelectorException


Invalid WebDriver argument
→ InvalidArgumentException


Browser session cannot start
→ SessionNotCreatedException


JavaScript execution failed
→ JavascriptException


Mouse target is outside valid area
→ MoveTargetOutOfBoundsException


Shadow root does not exist
→ NoSuchShadowRootException


Shadow root became detached
→ DetachedShadowRootException
49. Final Selenium Exception Strategy

A robust Selenium framework should follow:

1. Use stable locators
        ↓
2. Use explicit waits
        ↓
3. Perform Selenium operation
        ↓
4. Catch specific exception when recovery is possible
        ↓
5. Log useful diagnostic information
        ↓
6. Capture screenshot on failure
        ↓
7. Re-throw the exception when the test must fail
        ↓
8. Let TestNG listener/reporting handle centralized diagnostics

The goal is not to hide Selenium exceptions.

The goal is to make failures:

Understandable
Traceable
Reproducible
Reportable
Easy to debug

