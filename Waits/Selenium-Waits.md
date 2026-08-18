# Selenium Waits

## 1. What is a Wait in Selenium?

A wait is used to synchronize Selenium automation with the web application's behavior.

Web applications often take time to:

- Load pages
- Display elements
- Enable buttons
- Load API data
- Render dynamic content
- Display popups
- Update the DOM
- Complete animations

Without proper synchronization, Selenium may try to interact with an element before it is ready.

Example problem:

    Selenium starts
        ↓
    Page starts loading
        ↓
    Selenium searches for element
        ↓
    Element is not ready
        ↓
    Test fails

A wait helps Selenium wait for the required condition.

---

# 2. Why Are Waits Important?

Consider:

    driver.get("https://example.com");

    driver.findElement(
        By.id("username")
    ).sendKeys("Selva");

The page may not have finished loading the username field when Selenium searches for it.

Instead of using:

    Thread.sleep(5000);

use an appropriate Selenium wait.

---

# 3. Types of Selenium Waits

The major synchronization approaches are:

1. Implicit Wait
2. Explicit Wait
3. Fluent Wait
4. Thread.sleep()

For professional automation, Explicit Wait is generally the most useful approach.

---

# 4. Implicit Wait

Implicit wait tells WebDriver to wait for a specified amount of time when searching for elements.

Example:

    driver.manage().timeouts()
          .implicitlyWait(
              Duration.ofSeconds(10)
          );

Now Selenium will wait up to 10 seconds when locating elements.

---

# 5. Implicit Wait Example

    WebDriver driver =
        new ChromeDriver();

    driver.manage()
          .timeouts()
          .implicitlyWait(
              Duration.ofSeconds(10)
          );

    driver.get(
        "https://example.com"
    );

    driver.findElement(
        By.id("username")
    ).sendKeys("Selva");

---

# 6. How Implicit Wait Works

Suppose:

    implicitlyWait(10 seconds)

Selenium searches for an element.

If the element appears after:

    2 seconds

Selenium continues immediately.

If the element appears after:

    8 seconds

Selenium waits until it appears.

If the element never appears:

    Timeout / NoSuchElementException

may occur after the configured wait period.

---

# 7. Implicit Wait Scope

Implicit wait applies globally to element location calls for the WebDriver instance.

Example:

    driver.manage()
          .timeouts()
          .implicitlyWait(
              Duration.ofSeconds(10)
          );

It affects calls such as:

    findElement()

and:

    findElements()

---

# 8. Advantages of Implicit Wait

Advantages:

- Simple
- Easy to configure
- Global
- Reduces some timing issues

Example:

    driver.manage()
          .timeouts()
          .implicitlyWait(
              Duration.ofSeconds(10)
          );

---

# 9. Disadvantages of Implicit Wait

Implicit waits:

- Apply globally
- Do not target a specific condition
- Are less flexible
- Can make timing behavior harder to reason about
- Should be used carefully with explicit waits

---

# 10. Explicit Wait

Explicit wait waits for a specific condition.

Example:

    WebDriverWait wait =
        new WebDriverWait(
            driver,
            Duration.ofSeconds(10)
        );

    WebElement element =
        wait.until(
            ExpectedConditions
                .visibilityOfElementLocated(
                    By.id("username")
                )
        );

---

# 11. Explicit Wait Components

An explicit wait usually contains:

    WebDriverWait
        +
    Duration
        +
    ExpectedCondition

Example:

    WebDriverWait wait =
        new WebDriverWait(
            driver,
            Duration.ofSeconds(10)
        );

    wait.until(
        ExpectedConditions
            .elementToBeClickable(
                By.id("login")
            )
    );

---

# 12. visibilityOfElementLocated()

Waits until the element is:

- Present in the DOM
- Visible

Example:

    WebElement username =
        wait.until(
            ExpectedConditions
                .visibilityOfElementLocated(
                    By.id("username")
                )
        );

Then:

    username.sendKeys("Selva");

---

# 13. presenceOfElementLocated()

Waits until the element is present in the DOM.

Example:

    WebElement element =
        wait.until(
            ExpectedConditions
                .presenceOfElementLocated(
                    By.id("username")
                )
        );

Presence does not necessarily mean the element is visible.

---

# 14. Presence vs Visibility

### Presence

Element exists in DOM.

    presenceOfElementLocated()

### Visibility

Element exists and is visible.

    visibilityOfElementLocated()

Example:

    WebElement element =
        wait.until(
            ExpectedConditions
                .visibilityOfElementLocated(
                    By.id("username")
                )
        );

Use visibility when you need to interact with a visible element.

---

# 15. elementToBeClickable()

Waits until an element is visible and enabled.

Example:

    WebElement login =
        wait.until(
            ExpectedConditions
                .elementToBeClickable(
                    By.id("login")
                )
        );

    login.click();

This is commonly used before clicking buttons.

---

# 16. titleIs()

Waits until the page title exactly matches the expected title.

Example:

    wait.until(
        ExpectedConditions
            .titleIs("Toyota")
    );

---

# 17. titleContains()

Waits until the page title contains expected text.

Example:

    wait.until(
        ExpectedConditions
            .titleContains("Toyota")
    );

---

# 18. urlToBe()

Waits until the URL exactly matches the expected URL.

Example:

    wait.until(
        ExpectedConditions
            .urlToBe(
                "https://example.com/home"
            )
    );

---

# 19. urlContains()

Waits until the URL contains specified text.

Example:

    wait.until(
        ExpectedConditions
            .urlContains("dashboard")
    );

---

# 20. textToBePresentInElement()

Waits until specified text appears inside an element.

Example:

    wait.until(
        ExpectedConditions
            .textToBePresentInElement(
                By.id("message"),
                "Success"
            )
    );

---

# 21. textToBePresentInElementLocated()

Example:

    wait.until(
        ExpectedConditions
            .textToBePresentInElementLocated(
                By.id("message"),
                "Success"
            )
    );

This is useful when waiting for dynamic messages.

---

# 22. visibilityOf()

Waits for an already located WebElement to become visible.

Example:

    WebElement message =
        driver.findElement(
            By.id("message")
        );

    wait.until(
        ExpectedConditions
            .visibilityOf(message)
    );

---

# 23. elementToBeSelected()

Waits until an element is selected.

Example:

    wait.until(
        ExpectedConditions
            .elementToBeSelected(
                By.id("remember")
            )
    );

Useful for:

- Checkboxes
- Radio buttons
- Select options

---

# 24. elementSelectionStateToBe()

Waits for an element to have the expected selection state.

Example:

    wait.until(
        ExpectedConditions
            .elementSelectionStateToBe(
                By.id("remember"),
                true
            )
    );

---

# 25. frameToBeAvailableAndSwitchToIt()

Waits until an iframe is available and switches into it.

Example:

    wait.until(
        ExpectedConditions
            .frameToBeAvailableAndSwitchToIt(
                By.id("paymentFrame")
            )
    );

After this, Selenium is inside the frame.

---

# 26. alertIsPresent()

Waits until an alert appears.

Example:

    wait.until(
        ExpectedConditions
            .alertIsPresent()
    );

Then:

    Alert alert =
        driver.switchTo().alert();

    alert.accept();

---

# 27. invisibilityOfElementLocated()

Waits until an element becomes invisible or is no longer present.

Example:

    wait.until(
        ExpectedConditions
            .invisibilityOfElementLocated(
                By.id("loading")
            )
    );

This is very useful for:

- Loading spinners
- Progress indicators
- Overlays

---

# 28. stalenessOf()

Waits until an element is no longer attached to the DOM.

Example:

    WebElement oldElement =
        driver.findElement(
            By.id("oldElement")
        );

    wait.until(
        ExpectedConditions
            .stalenessOf(oldElement)
    );

Useful when an application replaces an element during a refresh or AJAX update.

---

# 29. numberOfElementsToBe()

Waits until the number of matching elements equals a specified number.

Example:

    wait.until(
        ExpectedConditions
            .numberOfElementsToBe(
                By.cssSelector(".product"),
                5
            )
    );

---

# 30. numberOfElementsToBeMoreThan()

Example:

    wait.until(
        ExpectedConditions
            .numberOfElementsToBeMoreThan(
                By.cssSelector(".product"),
                3
            )
    );

---

# 31. numberOfElementsToBeLessThan()

Example:

    wait.until(
        ExpectedConditions
            .numberOfElementsToBeLessThan(
                By.cssSelector(".product"),
                10
            )
    );

---

# 32. Fluent Wait

Fluent Wait provides more control over synchronization.

It allows you to configure:

- Maximum timeout
- Polling interval
- Exceptions to ignore
- Custom conditions

Example:

    Wait<WebDriver> wait =
        new FluentWait<>(driver)
            .withTimeout(
                Duration.ofSeconds(30)
            )
            .pollingEvery(
                Duration.ofSeconds(2)
            )
            .ignoring(
                NoSuchElementException.class
            );

---

# 33. Fluent Wait Example

    WebElement element =
        new FluentWait<>(driver)
            .withTimeout(
                Duration.ofSeconds(30)
            )
            .pollingEvery(
                Duration.ofSeconds(2)
            )
            .ignoring(
                NoSuchElementException.class
            )
            .until(
                driver ->
                    driver.findElement(
                        By.id("username")
                    )
            );

---

# 34. Polling

Polling means checking the condition repeatedly.

Example:

    Timeout:
        30 seconds

    Polling:
        Every 2 seconds

Selenium checks approximately:

    0 sec
    2 sec
    4 sec
    6 sec
    ...
    30 sec

until the condition succeeds or the timeout is reached.

---

# 35. Explicit Wait vs Fluent Wait

| Explicit Wait | Fluent Wait |
|---|---|
| Uses WebDriverWait | Uses FluentWait |
| Simple | More customizable |
| Uses ExpectedConditions commonly | Allows custom polling |
| Good for most UI conditions | Good for complex synchronization |
| Easy to read | More configuration |

---

# 36. Thread.sleep()

Java provides:

    Thread.sleep()

Example:

    Thread.sleep(5000);

This pauses the current thread for exactly 5 seconds.

---

# 37. Why Thread.sleep() Is Usually Avoided

Suppose an element appears after:

    1 second

but you use:

    Thread.sleep(10 seconds);

The test unnecessarily waits 9 extra seconds.

If the element appears after:

    12 seconds

the 10-second sleep is not enough.

Explicit waits are better because they wait until the required condition is met.

---

# 38. Thread.sleep() vs Explicit Wait

| Thread.sleep() | Explicit Wait |
|---|---|
| Fixed delay | Condition-based |
| Always waits full duration | Continues when condition succeeds |
| Slower | Usually faster |
| Not intelligent | Condition-aware |
| Poor synchronization strategy | Preferred |

---

# 39. Do Not Mix Implicit and Explicit Waits Carelessly

Example:

    driver.manage()
          .timeouts()
          .implicitlyWait(
              Duration.ofSeconds(10)
          );

    WebDriverWait wait =
        new WebDriverWait(
            driver,
            Duration.ofSeconds(10)
        );

Mixing implicit and explicit waits can lead to confusing timing behavior.

For a clean framework, choose a consistent synchronization strategy.

---

# 40. Recommended Strategy

For a modern Selenium framework:

    Explicit Wait
        ↓
    WebDriverWait
        ↓
    ExpectedConditions

Example:

    WebDriverWait wait =
        new WebDriverWait(
            driver,
            Duration.ofSeconds(10)
        );

    WebElement login =
        wait.until(
            ExpectedConditions
                .elementToBeClickable(
                    By.id("login")
                )
        );

    login.click();

---

# 41. Custom Explicit Wait

You can create a custom condition.

Example:

    WebElement element =
        wait.until(
            driver -> {

                WebElement e =
                    driver.findElement(
                        By.id("username")
                    );

                if (e.isDisplayed()
                        && e.isEnabled()) {

                    return e;
                }

                return null;
            }
        );

This gives you custom control over the condition.

---

# 42. Wait Until Text Changes

Example:

    wait.until(
        driver -> {

            String text =
                driver.findElement(
                    By.id("status")
                ).getText();

            return text.equals(
                "Completed"
            );
        }
    );

---

# 43. Wait Until Button Is Enabled

Example:

    wait.until(
        driver -> {

            WebElement button =
                driver.findElement(
                    By.id("submit")
                );

            return button.isEnabled();
        }
    );

---

# 44. Wait Until Element Is Displayed

Example:

    wait.until(
        driver -> {

            WebElement element =
                driver.findElement(
                    By.id("message")
                );

            return element.isDisplayed();
        }
    );

---

# 45. Wait Until Element Is Selected

Example:

    wait.until(
        driver -> {

            WebElement checkbox =
                driver.findElement(
                    By.id("remember")
                );

            return checkbox.isSelected();
        }
    );

---

# 46. Wait for Page Load

You can use JavaScript to check the document readiness state.

Example:

    WebDriverWait wait =
        new WebDriverWait(
            driver,
            Duration.ofSeconds(30)
        );

    wait.until(
        driver ->
            ((JavascriptExecutor) driver)
                .executeScript(
                    "return document.readyState"
                )
                .equals("complete")
    );

---

# 47. document.readyState

Possible values include:

    loading
    interactive
    complete

Example:

    return document.readyState

A common condition is:

    complete

However, `document.readyState == "complete"` does not necessarily mean that all AJAX/API-driven content is ready.

---

# 48. Wait for AJAX / Dynamic Content

Modern applications often load data asynchronously.

Example:

    Page loads
        ↓
    API request
        ↓
    Loading spinner
        ↓
    API response
        ↓
    DOM update
        ↓
    Data displayed

Do not simply use:

    Thread.sleep(5000);

Instead, wait for an application-specific condition.

Example:

    wait.until(
        ExpectedConditions
            .invisibilityOfElementLocated(
                By.id("loading")
            )
    );

Then interact with the data.

---

# 49. Wait for Loading Spinner

Example:

    wait.until(
        ExpectedConditions
            .invisibilityOfElementLocated(
                By.cssSelector(
                    ".loading-spinner"
                )
            )
    );

Then:

    WebElement result =
        wait.until(
            ExpectedConditions
                .visibilityOfElementLocated(
                    By.id("result")
                )
        );

---

# 50. Wait for Button to Become Enabled

Example:

    WebElement submit =
        wait.until(
            ExpectedConditions
                .elementToBeClickable(
                    By.id("submit")
                )
        );

    submit.click();

---

# 51. Wait for Error Message

Example:

    WebElement error =
        wait.until(
            ExpectedConditions
                .visibilityOfElementLocated(
                    By.id("errorMessage")
                )
        );

    String message =
        error.getText();

---

# 52. Wait for Success Message

Example:

    wait.until(
        ExpectedConditions
            .textToBePresentInElementLocated(
                By.id("message"),
                "Success"
            )
    );

---

# 53. Wait for URL After Login

Example:

    driver.findElement(
        By.id("login")
    ).click();

    wait.until(
        ExpectedConditions
            .urlContains("dashboard")
    );

---

# 54. Wait for Page Title

Example:

    wait.until(
        ExpectedConditions
            .titleContains("Dashboard")
    );

---

# 55. Wait for Alert

Example:

    wait.until(
        ExpectedConditions
            .alertIsPresent()
    );

    Alert alert =
        driver.switchTo().alert();

    System.out.println(
        alert.getText()
    );

    alert.accept();

---

# 56. Wait for Frame

Example:

    wait.until(
        ExpectedConditions
            .frameToBeAvailableAndSwitchToIt(
                By.id("paymentFrame")
            )
    );

    driver.findElement(
        By.id("cardNumber")
    ).sendKeys("1234");

---

# 57. Wait for Window

Selenium does not provide a simple generic ExpectedCondition for every window/tab scenario.

A common approach is to wait until the number of window handles changes.

Example:

    String originalWindow =
        driver.getWindowHandle();

    driver.findElement(
        By.id("openWindow")
    ).click();

    wait.until(
        driver ->
            driver.getWindowHandles()
                  .size() > 1
    );

Then switch:

    for (String window :
            driver.getWindowHandles()) {

        if (!window.equals(
                originalWindow)) {

            driver.switchTo()
                  .window(window);

            break;
        }
    }

---

# 58. Custom Wait Utility

In a framework, you can create a reusable wait utility.

Example:

    public class WaitUtils {

        private WebDriver driver;

        private WebDriverWait wait;

        public WaitUtils(
                WebDriver driver) {

            this.driver = driver;

            this.wait =
                new WebDriverWait(
                    driver,
                    Duration.ofSeconds(10)
                );
        }

        public WebElement waitForVisible(
                By locator) {

            return wait.until(
                ExpectedConditions
                    .visibilityOfElementLocated(
                        locator
                    )
            );
        }

        public WebElement waitForClickable(
                By locator) {

            return wait.until(
                ExpectedConditions
                    .elementToBeClickable(
                        locator
                    )
            );
        }
    }

---

# 59. Using Wait Utility

Example:

    WaitUtils waitUtils =
        new WaitUtils(driver);

    WebElement username =
        waitUtils.waitForVisible(
            By.id("username")
        );

    username.sendKeys("Selva");

For clicking:

    WebElement login =
        waitUtils.waitForClickable(
            By.id("login")
        );

    login.click();

---

# 60. Page Object Model with Wait

Example:

    public class LoginPage {

        private WebDriver driver;

        private WebDriverWait wait;

        private By username =
            By.id("username");

        private By password =
            By.id("password");

        private By loginButton =
            By.id("login");

        public LoginPage(
                WebDriver driver) {

            this.driver = driver;

            this.wait =
                new WebDriverWait(
                    driver,
                    Duration.ofSeconds(10)
                );
        }

        public void login(
                String user,
                String pass) {

            wait.until(
                ExpectedConditions
                    .visibilityOfElementLocated(
                        username
                    )
            ).sendKeys(user);

            wait.until(
                ExpectedConditions
                    .visibilityOfElementLocated(
                        password
                    )
            ).sendKeys(pass);

            wait.until(
                ExpectedConditions
                    .elementToBeClickable(
                        loginButton
                    )
            ).click();
        }
    }

---

# 61. Wait Utility with Timeout

A better framework can centralize the timeout.

Example:

    public static final int
        DEFAULT_TIMEOUT = 10;

Then:

    new WebDriverWait(
        driver,
        Duration.ofSeconds(
            DEFAULT_TIMEOUT
        )
    );

This makes timeout management easier.

---

# 62. Avoid Hardcoded Waits

Avoid:

    Thread.sleep(3000);

    Thread.sleep(5000);

    Thread.sleep(10000);

Instead use:

    wait.until(
        ExpectedConditions
            .visibilityOfElementLocated(
                locator
            )
    );

---

# 63. Avoid Excessive Timeout

Do not automatically use:

    Duration.ofMinutes(5)

for every element.

Choose reasonable timeouts based on the application.

Example:

    10 seconds

for normal UI elements.

Longer timeouts may be appropriate for:

- File downloads
- Slow backend operations
- Large reports
- Environment-specific operations

---

# 64. TimeoutException

If an explicit wait cannot satisfy its condition within the timeout, Selenium can throw:

    TimeoutException

Example:

    wait.until(
        ExpectedConditions
            .visibilityOfElementLocated(
                By.id("missing")
            )
    );

If the element never becomes visible:

    TimeoutException

---

# 65. NoSuchElementException and Waits

Without explicit wait:

    driver.findElement(
        By.id("username")
    );

If the element is not found, Selenium can throw:

    NoSuchElementException

With explicit wait:

    wait.until(
        ExpectedConditions
            .visibilityOfElementLocated(
                By.id("username")
            )
    );

The wait gives the application time to make the element available.

---

# 66. StaleElementReferenceException and Waits

If an element is replaced during a DOM update, the old reference can become stale.

A strategy is to locate the element again inside the wait.

Example:

    wait.until(
        ExpectedConditions
            .visibilityOfElementLocated(
                By.id("username")
            )
    );

Instead of relying on an old WebElement reference after a major DOM update.

---

# 67. Wait and Dynamic Applications

Frameworks such as:

- React
- Angular
- Vue

can frequently update the DOM.

Therefore:

    Find element
        ↓
    DOM changes
        ↓
    Old WebElement becomes stale

Good synchronization is important when automating dynamic applications.

---

# 68. Waits in Parallel Execution

When tests run in parallel, each WebDriver instance should have its own wait.

Example:

    WebDriver driver =
        ThreadLocalDriver.getDriver();

    WebDriverWait wait =
        new WebDriverWait(
            driver,
            Duration.ofSeconds(10)
        );

Do not share a WebDriver or wait object incorrectly between parallel tests.

---

# 69. Waits and ThreadLocal

For parallel frameworks:

    Thread
       ↓
    ThreadLocal<WebDriver>
       ↓
    Thread-specific driver
       ↓
    Thread-specific WebDriverWait

This prevents different tests from accidentally using the same browser session.

---

# 70. Common Wait Mistakes

## Mistake 1

Using Thread.sleep everywhere:

    Thread.sleep(5000);

Better:

    wait.until(
        ExpectedConditions
            .visibilityOfElementLocated(
                locator
            )
    );

---

## Mistake 2

Using an extremely large timeout:

    Duration.ofMinutes(10)

for every element.

Better:

    Use reasonable timeouts based on the application.

---

## Mistake 3

Waiting for visibility when clickability is required.

Instead of:

    visibilityOfElementLocated()

use:

    elementToBeClickable()

when appropriate.

---

## Mistake 4

Waiting for presence when the element must be visible.

Presence only guarantees DOM presence.

---

## Mistake 5

Mixing implicit and explicit waits without understanding their interaction.

---

# 71. Recommended Wait Strategy

A practical Selenium framework can use:

    WebDriverWait
          ↓
    Explicit Conditions
          ↓
    Reusable Wait Utility
          ↓
    Page Objects
          ↓
    TestNG Tests

Example:

    waitForVisible(locator);

    waitForClickable(locator);

    waitForText(locator, text);

    waitForInvisible(locator);

    waitForAlert();

    waitForFrame(locator);

---

# 72. Common ExpectedConditions

Remember these important conditions:

    presenceOfElementLocated()

    visibilityOfElementLocated()

    elementToBeClickable()

    invisibilityOfElementLocated()

    textToBePresentInElementLocated()

    titleIs()

    titleContains()

    urlToBe()

    urlContains()

    alertIsPresent()

    frameToBeAvailableAndSwitchToIt()

    elementToBeSelected()

    elementSelectionStateToBe()

    stalenessOf()

    numberOfElementsToBe()

    numberOfElementsToBeMoreThan()

    numberOfElementsToBeLessThan()

---

# 73. Complete Wait Example

    import java.time.Duration;

    import org.openqa.selenium.By;
    import org.openqa.selenium.WebDriver;
    import org.openqa.selenium.WebElement;
    import org.openqa.selenium.chrome.ChromeDriver;

    import org.openqa.selenium.support.ui.ExpectedConditions;
    import org.openqa.selenium.support.ui.WebDriverWait;

    public class WaitExample {

        public static void main(
                String[] args) {

            WebDriver driver =
                new ChromeDriver();

            try {

                driver.manage()
                      .window()
                      .maximize();

                driver.get(
                    "https://example.com"
                );

                WebDriverWait wait =
                    new WebDriverWait(
                        driver,
                        Duration.ofSeconds(10)
                    );

                WebElement username =
                    wait.until(
                        ExpectedConditions
                            .visibilityOfElementLocated(
                                By.id("username")
                            )
                    );

                username.sendKeys(
                    "Selva"
                );

                WebElement password =
                    wait.until(
                        ExpectedConditions
                            .visibilityOfElementLocated(
                                By.id("password")
                            )
                    );

                password.sendKeys(
                    "password"
                );

                WebElement login =
                    wait.until(
                        ExpectedConditions
                            .elementToBeClickable(
                                By.id("login")
                            )
                    );

                login.click();

                wait.until(
                    ExpectedConditions
                        .urlContains(
                            "dashboard"
                        )
                );

            } finally {

                driver.quit();
            }
        }
    }

---

# 74. Waits Cheat Sheet

## Implicit Wait

    driver.manage()
          .timeouts()
          .implicitlyWait(
              Duration.ofSeconds(10)
          );

## Explicit Wait

    WebDriverWait wait =
        new WebDriverWait(
            driver,
            Duration.ofSeconds(10)
        );

## Visible Element

    wait.until(
        ExpectedConditions
            .visibilityOfElementLocated(
                locator
            )
    );

## Present Element

    wait.until(
        ExpectedConditions
            .presenceOfElementLocated(
                locator
            )
    );

## Clickable Element

    wait.until(
        ExpectedConditions
            .elementToBeClickable(
                locator
            )
    );

## Invisible Element

    wait.until(
        ExpectedConditions
            .invisibilityOfElementLocated(
                locator
            )
    );

## Alert

    wait.until(
        ExpectedConditions
            .alertIsPresent()
    );

## URL

    wait.until(
        ExpectedConditions
            .urlContains("dashboard")
    );

## Text

    wait.until(
        ExpectedConditions
            .textToBePresentInElementLocated(
                locator,
                "Success"
            )
    );

---

# 75. Interview Questions

1. What is synchronization in Selenium?
2. Why are waits required?
3. What are the types of waits in Selenium?
4. What is implicit wait?
5. What is explicit wait?
6. What is FluentWait?
7. What is the difference between implicit and explicit wait?
8. What is the difference between explicit wait and FluentWait?
9. What is polling?
10. What is ExpectedConditions?
11. What is `visibilityOfElementLocated()`?
12. What is `presenceOfElementLocated()`?
13. What is `elementToBeClickable()`?
14. What is `alertIsPresent()`?
15. What is `frameToBeAvailableAndSwitchToIt()`?
16. What is `stalenessOf()`?
17. What is `invisibilityOfElementLocated()`?
18. How do you wait for a spinner to disappear?
19. How do you wait for an AJAX response?
20. How do you wait for a button to become enabled?
21. How do you wait for text to appear?
22. How do you wait for a URL to change?
23. Why should Thread.sleep() generally be avoided?
24. Can implicit and explicit waits be used together?
25. What is TimeoutException?
26. How do you create a reusable wait utility?
27. How do waits work in Page Object Model?
28. How do you handle waits in parallel execution?
29. How do you handle stale elements?
30. How would you design synchronization for a Selenium framework?

---

# 76. Interview Answer: Implicit vs Explicit Wait

A strong interview answer:

"Implicit wait is a global timeout applied to element location operations. Explicit wait is condition-based and waits for a specific condition such as visibility, clickability, text, URL, or an alert. In a framework, I generally prefer explicit waits because they provide better control and make synchronization more predictable."

---

# 77. Interview Answer: Why Not Thread.sleep()?

A strong answer:

"`Thread.sleep()` introduces a fixed delay regardless of whether the application is ready. It can make tests unnecessarily slow and can still fail if the application takes longer than the fixed delay. Explicit waits are better because they poll for a specific condition and continue as soon as that condition is satisfied."

---

# 78. Final Summary

Synchronization is critical for reliable Selenium automation.

The key concepts are:

    Implicit Wait
    Explicit Wait
    Fluent Wait
    ExpectedConditions
    Polling
    TimeoutException
    Dynamic Elements
    AJAX
    DOM Updates
    Stale Elements

The preferred pattern is:

    WebDriver
        ↓
    WebDriverWait
        ↓
    ExpectedCondition
        ↓
    WebElement
        ↓
    Interaction

For a professional Selenium framework:

    Avoid unnecessary Thread.sleep()
    Use explicit waits
    Centralize reusable waits
    Use meaningful conditions
    Handle dynamic content
    Handle stale elements
    Keep synchronization inside page/framework utilities
    Use thread-safe driver/wait handling for parallel execution

The most important conditions to remember are:

    visibilityOfElementLocated()
    presenceOfElementLocated()
    elementToBeClickable()
    invisibilityOfElementLocated()
    textToBePresentInElementLocated()
    alertIsPresent()
    frameToBeAvailableAndSwitchToIt()
    stalenessOf()
    urlContains()
    titleContains()

Good synchronization:

    ↓

Fewer flaky tests

    ↓

Faster execution

    ↓

More reliable Selenium framework
