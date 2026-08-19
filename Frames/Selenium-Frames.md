# Selenium Frames

## 1. What is an iFrame?

An iframe (inline frame) is an HTML element used to embed another HTML document inside the current webpage.

Example:

```html
<iframe id="paymentFrame" src="payment.html"></iframe>
```

The iframe has its own document context.

Selenium cannot directly interact with elements inside an iframe until WebDriver switches into that iframe.

---

# 2. Why Do We Need to Switch to an iframe?

Consider:

```text
Main Page
    |
    +-- Header
    |
    +-- Login Form
    |
    +-- iframe
          |
          +-- Username
          +-- Password
          +-- Submit
```

If the username field is inside the iframe, this may fail:

```java
driver.findElement(By.id("username")).sendKeys("Selva");
```

First switch into the iframe:

```java
driver.switchTo().frame(...);
```

Then locate the element:

```java
driver.findElement(By.id("username"))
      .sendKeys("Selva");
```

---

# 3. Selenium Frame Methods

Selenium provides three common ways to switch into a frame:

```java
driver.switchTo().frame(int index);
```

```java
driver.switchTo().frame(String nameOrId);
```

```java
driver.switchTo().frame(WebElement frameElement);
```

---

# 4. Switch to Frame by Index

Example:

```java
driver.switchTo().frame(0);
```

This switches to the first frame.

If the page contains:

```text
Frame 0
Frame 1
Frame 2
```

then:

```java
driver.switchTo().frame(0);
```

selects the first frame.

## Important

Index-based frame switching can be fragile because the index can change when the page structure changes.

Prefer frame ID/name or WebElement when a stable identifier is available.

---

# 5. Switch to Frame by ID

HTML:

```html
<iframe id="loginFrame"></iframe>
```

Java:

```java
driver.switchTo().frame("loginFrame");
```

This is one of the simplest approaches when the iframe has a stable ID.

---

# 6. Switch to Frame by Name

HTML:

```html
<iframe name="paymentFrame"></iframe>
```

Java:

```java
driver.switchTo().frame("paymentFrame");
```

The string can refer to the frame's name or ID.

---

# 7. Switch to Frame Using WebElement

This is often the preferred approach when the iframe can be located reliably.

```java
WebElement frame =
        driver.findElement(
                By.id("paymentFrame")
        );

driver.switchTo().frame(frame);
```

Now Selenium is inside the frame.

---

# 8. Complete Frame Example

```java
WebElement frame =
        driver.findElement(
                By.id("paymentFrame")
        );

driver.switchTo().frame(frame);

driver.findElement(
        By.id("cardNumber")
).sendKeys("123456789");

driver.switchTo().defaultContent();
```

---

# 9. Returning to the Main Page

After working inside an iframe, switch back to the main document:

```java
driver.switchTo().defaultContent();
```

This returns WebDriver to the top-level page.

---

# 10. parentFrame()

`parentFrame()` moves one level up in the frame hierarchy.

```java
driver.switchTo().parentFrame();
```

This is particularly useful for nested frames.

---

# 11. defaultContent() vs parentFrame()

| Method             | Purpose                               |
| ------------------ | ------------------------------------- |
| `parentFrame()`    | Moves one level up                    |
| `defaultContent()` | Returns directly to the main document |

Example:

```text
Main Page
   |
   +-- Frame A
         |
         +-- Frame B
```

From Frame B:

```java
driver.switchTo().parentFrame();
```

moves to Frame A.

From Frame B:

```java
driver.switchTo().defaultContent();
```

moves directly to the Main Page.

---

# 12. Frame Switching Flow

```text
Main Page
    |
    | switchTo().frame()
    v
Inside Frame
    |
    | interact with elements
    |
    | switchTo().defaultContent()
    v
Main Page
```

---

# 13. Example: Login Inside iframe

HTML:

```html
<iframe id="loginFrame">
    ...
</iframe>
```

Test:

```java
driver.get("https://example.com");

driver.switchTo().frame("loginFrame");

driver.findElement(
        By.id("username")
).sendKeys("Selva");

driver.findElement(
        By.id("password")
).sendKeys("Password");

driver.findElement(
        By.id("login")
).click();

driver.switchTo().defaultContent();
```

---

# 14. Explicit Wait for Frame

Sometimes the iframe is loaded dynamically.

Use:

```java
WebDriverWait wait =
        new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );

wait.until(
        ExpectedConditions.frameToBeAvailableAndSwitchToIt(
                By.id("paymentFrame")
        )
);
```

Imports:

```java
import java.time.Duration;

import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
```

This waits for the frame and switches into it.

---

# 15. Wait Using Frame WebElement

```java
WebElement frame =
        driver.findElement(
                By.cssSelector("iframe.payment")
        );

WebDriverWait wait =
        new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );

wait.until(
        ExpectedConditions.frameToBeAvailableAndSwitchToIt(
                frame
        )
);
```

---

# 16. Frame by Index with Explicit Wait

Selenium also supports:

```java
wait.until(
        ExpectedConditions.frameToBeAvailableAndSwitchToIt(
                0
        )
);
```

However, using an index is less stable if the page contains dynamic frames.

---

# 17. Nested Frames

A nested iframe is a frame inside another frame.

Example:

```text
Main Document
     |
     +-- Frame A
           |
           +-- Frame B
                 |
                 +-- Username
```

To access Frame B:

```java
driver.switchTo().frame("frameA");

driver.switchTo().frame("frameB");

driver.findElement(
        By.id("username")
).sendKeys("Selva");
```

---

# 18. Return from Nested Frame

Suppose:

```text
Main
 |
 +-- Frame A
      |
      +-- Frame B
```

Currently inside Frame B.

Move to Frame A:

```java
driver.switchTo().parentFrame();
```

Move directly to Main:

```java
driver.switchTo().defaultContent();
```

---

# 19. Nested Frame Example

```java
driver.switchTo().frame("frameA");

driver.switchTo().frame("frameB");

driver.findElement(
        By.id("username")
).sendKeys("Selva");

driver.switchTo().parentFrame();

driver.findElement(
        By.id("frameAElement")
).click();

driver.switchTo().defaultContent();
```

---

# 20. Multiple Frames on a Page

Suppose the page contains:

```text
Main Page
 |
 +-- Header Frame
 |
 +-- Content Frame
 |
 +-- Footer Frame
```

You need to switch to the correct frame before interacting with its elements.

Example:

```java
driver.switchTo().frame("contentFrame");

driver.findElement(
        By.id("content")
).click();

driver.switchTo().defaultContent();

driver.switchTo().frame("footerFrame");

driver.findElement(
        By.id("footerLink")
).click();
```

---

# 21. Important Frame Rule

When Selenium is inside a frame:

```java
driver.findElement(...)
```

searches within the current frame context.

It does not automatically search the parent page.

Example:

```text
Main Page
 |
 +-- Frame
      |
      +-- Button
```

After:

```java
driver.switchTo().frame("frame");
```

Selenium searches inside the frame.

To search the main page again:

```java
driver.switchTo().defaultContent();
```

---

# 22. Common NoSuchElementException with Frames

Consider:

```java
driver.get("https://example.com");

driver.findElement(
        By.id("username")
).sendKeys("Selva");
```

If `username` is inside an iframe, Selenium may throw:

```text
NoSuchElementException
```

Solution:

```java
driver.switchTo().frame("loginFrame");

driver.findElement(
        By.id("username")
).sendKeys("Selva");
```

---

# 23. Common NoSuchFrameException

If Selenium cannot switch to the requested frame, it may throw:

```text
NoSuchFrameException
```

Possible causes:

* Incorrect frame ID
* Incorrect frame name
* Wrong index
* Frame not loaded
* Frame is no longer available
* Wrong page

Use an explicit wait when the frame loads dynamically.

---

# 24. NoSuchFrameException Example

```java
try {

    driver.switchTo().frame("invalidFrame");

} catch (NoSuchFrameException e) {

    System.out.println(
            "Frame could not be found."
    );
}
```

---

# 25. StaleElementReferenceException with Frames

If the page refreshes or the DOM changes after locating the iframe, the stored frame WebElement may become stale.

Example:

```java
WebElement frame =
        driver.findElement(
                By.id("paymentFrame")
        );

driver.navigate().refresh();

driver.switchTo().frame(frame);
```

This can result in:

```text
StaleElementReferenceException
```

Solution:

Locate the frame again after the DOM changes.

```java
WebElement frame =
        driver.findElement(
                By.id("paymentFrame")
        );

driver.switchTo().frame(frame);
```

---

# 26. Finding All iframes

You can find all iframe elements:

```java
List<WebElement> frames =
        driver.findElements(
                By.tagName("iframe")
        );

System.out.println(
        "Number of frames: " +
        frames.size()
);
```

---

# 27. Count Frames

```java
int frameCount =
        driver.findElements(
                By.tagName("iframe")
        ).size();

System.out.println(
        "Frame Count: " + frameCount
);
```

---

# 28. Print Frame Information

```java
List<WebElement> frames =
        driver.findElements(
                By.tagName("iframe")
        );

for (WebElement frame : frames) {

    System.out.println(
            "ID: " +
            frame.getAttribute("id")
    );

    System.out.println(
            "Name: " +
            frame.getAttribute("name")
    );
}
```

This can help when debugging pages containing multiple frames.

---

# 29. iframe vs frame

HTML may contain:

```html
<iframe></iframe>
```

or older:

```html
<frame></frame>
```

Selenium's `switchTo().frame()` is used to switch into a frame browsing context.

Modern web applications commonly use `<iframe>`.

---

# 30. Frame Detection

Use browser developer tools to identify an iframe.

Example:

```html
<iframe
    id="paymentFrame"
    class="payment-container">
</iframe>
```

Possible locators:

```java
By.id("paymentFrame")
```

```java
By.cssSelector("iframe.payment-container")
```

```java
By.xpath("//iframe[@id='paymentFrame']")
```

---

# 31. Frame Using CSS Selector

```java
WebElement frame =
        driver.findElement(
                By.cssSelector(
                        "iframe#paymentFrame"
                )
        );

driver.switchTo().frame(frame);
```

---

# 32. Frame Using XPath

```java
WebElement frame =
        driver.findElement(
                By.xpath(
                    "//iframe[@id='paymentFrame']"
                )
        );

driver.switchTo().frame(frame);
```

---

# 33. Frame Without ID or Name

Suppose:

```html
<iframe
    class="payment-frame"
    src="/payment">
</iframe>
```

Use:

```java
driver.switchTo().frame(
        driver.findElement(
                By.cssSelector(
                        "iframe.payment-frame"
                )
        )
);
```

---

# 34. Frame with Dynamic Attributes

Suppose:

```html
<iframe
    id="frame_12345"
    class="payment-frame">
</iframe>
```

The ID changes dynamically.

Use a stable attribute:

```java
driver.switchTo().frame(
        driver.findElement(
                By.cssSelector(
                    "iframe.payment-frame"
                )
        )
);
```

Or XPath:

```java
driver.switchTo().frame(
        driver.findElement(
                By.xpath(
                    "//iframe[contains(@class,'payment-frame')]"
                )
        )
);
```

---

# 35. iframe with src Attribute

Example:

```html
<iframe
    src="/payment/checkout">
</iframe>
```

CSS:

```java
driver.switchTo().frame(
        driver.findElement(
                By.cssSelector(
                    "iframe[src*='checkout']"
                )
        )
);
```

XPath:

```java
driver.switchTo().frame(
        driver.findElement(
                By.xpath(
                    "//iframe[contains(@src,'checkout')]"
                )
        )
);
```

---

# 36. Frame Inside Shadow DOM

Modern applications may contain iframes inside Shadow DOM.

Conceptually:

```text
Main Document
   |
   +-- Shadow Root
         |
         +-- iframe
```

You may first need to access the shadow root, locate the iframe, and then switch into it.

Example:

```java
WebElement host =
        driver.findElement(
                By.cssSelector("my-component")
        );

SearchContext shadowRoot =
        host.getShadowRoot();

WebElement frame =
        shadowRoot.findElement(
                By.cssSelector("iframe")
        );

driver.switchTo().frame(frame);
```

The exact approach depends on the application's Shadow DOM implementation.

---

# 37. Frame Utility Class

Create:

```text
Utilities/
└── FrameUtils.java
```

Example:

```java
import java.time.Duration;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

public class FrameUtils {

    public static void switchToFrame(
            WebDriver driver,
            By locator,
            int seconds) {

        WebDriverWait wait =
                new WebDriverWait(
                        driver,
                        Duration.ofSeconds(seconds)
                );

        wait.until(
                ExpectedConditions
                        .frameToBeAvailableAndSwitchToIt(
                                locator
                        )
        );
    }

    public static void switchToFrame(
            WebDriver driver,
            WebElement frame,
            int seconds) {

        WebDriverWait wait =
                new WebDriverWait(
                        driver,
                        Duration.ofSeconds(seconds)
                );

        wait.until(
                ExpectedConditions
                        .frameToBeAvailableAndSwitchToIt(
                                frame
                        )
        );
    }

    public static void switchToDefaultContent(
            WebDriver driver) {

        driver.switchTo().defaultContent();
    }

    public static void switchToParentFrame(
            WebDriver driver) {

        driver.switchTo().parentFrame();
    }
}
```

---

# 38. Using FrameUtils

Switch by locator:

```java
FrameUtils.switchToFrame(
        driver,
        By.id("paymentFrame"),
        10
);
```

Return to main document:

```java
FrameUtils.switchToDefaultContent(
        driver
);
```

Return to parent:

```java
FrameUtils.switchToParentFrame(
        driver
);
```

---

# 39. Complete Frame Test

```java
import java.time.Duration;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

public class FrameTest {

    public static void main(String[] args) {

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

            WebElement frame =
                    wait.until(
                            ExpectedConditions
                                    .presenceOfElementLocated(
                                            By.id(
                                                "paymentFrame"
                                            )
                                    )
                    );

            driver.switchTo().frame(frame);

            WebElement cardNumber =
                    wait.until(
                            ExpectedConditions
                                    .visibilityOfElementLocated(
                                            By.id(
                                                "cardNumber"
                                            )
                                    )
                    );

            cardNumber.sendKeys(
                    "123456789"
            );

            driver.switchTo()
                    .defaultContent();

        } finally {

            driver.quit();
        }
    }
}
```

---

# 40. Frame Switching with TestNG

```java
import java.time.Duration;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

public class FrameTest {

    private WebDriver driver;

    @BeforeMethod
    public void setUp() {

        driver = new ChromeDriver();

        driver.manage()
                .window()
                .maximize();
    }

    @Test
    public void frameTest() {

        driver.get(
                "https://example.com"
        );

        WebDriverWait wait =
                new WebDriverWait(
                        driver,
                        Duration.ofSeconds(10)
                );

        wait.until(
                ExpectedConditions
                        .frameToBeAvailableAndSwitchToIt(
                                By.id("paymentFrame")
                        )
        );

        driver.findElement(
                By.id("cardNumber")
        ).sendKeys("123456789");

        driver.switchTo()
                .defaultContent();
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

# 41. Frame Context Concept

This is an important interview concept.

WebDriver maintains a current browsing context.

Initially:

```text
Current Context
      |
      v
Main Document
```

After:

```java
driver.switchTo().frame("frameA");
```

the current context becomes:

```text
Current Context
      |
      v
Frame A
```

After:

```java
driver.switchTo().frame("frameB");
```

it becomes:

```text
Current Context
      |
      v
Frame B
```

After:

```java
driver.switchTo().defaultContent();
```

it becomes:

```text
Current Context
      |
      v
Main Document
```

---

# 42. Important Difference: Frame vs Window

A frame is embedded inside a webpage.

A window/tab is a separate browsing context.

Frame:

```java
driver.switchTo().frame(...);
```

Window:

```java
driver.switchTo().window(...);
```

Do not confuse these two.

---

# 43. Frame vs Window

| Feature          | Frame                                | Window/Tab              |
| ---------------- | ------------------------------------ | ----------------------- |
| Embedded in page | Yes                                  | No                      |
| Switch method    | `switchTo().frame()`                 | `switchTo().window()`   |
| Return method    | `defaultContent()` / `parentFrame()` | `switchTo().window()`   |
| Window handle    | Not used                             | Used                    |
| Common exception | `NoSuchFrameException`               | `NoSuchWindowException` |

---

# 44. Common Mistakes

## Mistake 1

Trying to find an element inside a frame without switching.

```java
driver.findElement(
        By.id("username")
).click();
```

Correct:

```java
driver.switchTo().frame("loginFrame");

driver.findElement(
        By.id("username")
).click();
```

---

## Mistake 2

Forgetting to return to the main page.

After completing the frame interaction:

```java
driver.switchTo().defaultContent();
```

---

## Mistake 3

Using `defaultContent()` when you only want the parent frame.

Use:

```java
driver.switchTo().parentFrame();
```

when moving one level up.

---

## Mistake 4

Using frame index unnecessarily.

Instead of:

```java
driver.switchTo().frame(0);
```

prefer a stable locator when available:

```java
driver.switchTo().frame(
        driver.findElement(
                By.id("paymentFrame")
        )
);
```

---

# 45. Best Practices

### 1. Prefer stable frame locators

Prefer:

```java
By.id("paymentFrame")
```

over:

```java
frame(0)
```

when possible.

### 2. Use explicit waits for dynamic frames

```java
ExpectedConditions
    .frameToBeAvailableAndSwitchToIt(...)
```

### 3. Return to the correct context

Use:

```java
driver.switchTo().defaultContent();
```

or:

```java
driver.switchTo().parentFrame();
```

### 4. Keep frame handling inside reusable utilities

This reduces duplicate code.

### 5. Verify whether the popup is actually an iframe

Not every popup is a frame.

Inspect the DOM before choosing the Selenium strategy.

---

# 46. Interview Questions

## Q1. What is an iframe?

An iframe is an HTML element that embeds another document inside the current webpage.

---

## Q2. How do you switch to an iframe?

Three common approaches:

```java
driver.switchTo().frame(0);
```

```java
driver.switchTo().frame("frameName");
```

```java
driver.switchTo().frame(frameElement);
```

---

## Q3. How do you switch back to the main page?

```java
driver.switchTo().defaultContent();
```

---

## Q4. What is parentFrame()?

It moves WebDriver one level up in the frame hierarchy.

```java
driver.switchTo().parentFrame();
```

---

## Q5. Difference between parentFrame() and defaultContent()?

`parentFrame()` moves one level up.

`defaultContent()` moves directly to the top-level document.

---

## Q6. What happens if you try to interact with an iframe element without switching?

Selenium generally cannot find the element in the current browsing context and may throw `NoSuchElementException`.

---

## Q7. What exception is thrown when a frame cannot be found?

```text
NoSuchFrameException
```

---

## Q8. How do you wait for an iframe?

```java
wait.until(
    ExpectedConditions
        .frameToBeAvailableAndSwitchToIt(
            By.id("paymentFrame")
        )
);
```

---

## Q9. Can frames be nested?

Yes.

Example:

```text
Main
 |
 +-- Frame A
      |
      +-- Frame B
```

Switch sequentially:

```java
driver.switchTo().frame("frameA");
driver.switchTo().frame("frameB");
```

---

## Q10. How do you switch from nested Frame B to Frame A?

```java
driver.switchTo().parentFrame();
```

---

## Q11. How do you switch directly from nested Frame B to the main page?

```java
driver.switchTo().defaultContent();
```

---

## Q12. Can you use XPath to locate an iframe?

Yes.

```java
driver.switchTo().frame(
    driver.findElement(
        By.xpath("//iframe[@id='paymentFrame']")
    )
);
```

---

## Q13. How do you count iframes?

```java
int count =
    driver.findElements(
        By.tagName("iframe")
    ).size();
```

---

## Q14. Can an iframe exist inside Shadow DOM?

Yes. Modern applications can contain iframes inside Shadow DOM. You may need to access the shadow root first and then locate and switch to the iframe.

---

## Q15. What is the difference between frame and window?

A frame is embedded in a webpage and is accessed using:

```java
driver.switchTo().frame(...)
```

A window/tab is a separate browsing context and is accessed using:

```java
driver.switchTo().window(...)
```

---

# 47. Senior-Level Scenario

### Scenario

A payment page contains:

```text
Main Page
    |
    +-- Payment iframe
          |
          +-- Card Number
          +-- Expiration
          +-- CVV
```

You need to enter the card number and then return to the main page.

Solution:

```java
WebDriverWait wait =
        new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );

wait.until(
        ExpectedConditions
                .frameToBeAvailableAndSwitchToIt(
                        By.id("paymentFrame")
                )
);

driver.findElement(
        By.id("cardNumber")
).sendKeys("123456789");

driver.switchTo()
        .defaultContent();
```

---

# 48. Senior-Level Nested Frame Scenario

```text
Main Page
    |
    +-- Payment Frame
           |
           +-- Secure Frame
                  |
                  +-- CVV
```

Solution:

```java
driver.switchTo()
        .frame("paymentFrame");

driver.switchTo()
        .frame("secureFrame");

driver.findElement(
        By.id("cvv")
).sendKeys("123");

driver.switchTo()
        .defaultContent();
```

---

# 49. Quick Revision

```text
iframe
   |
   v
switchTo().frame()
   |
   +-- index
   +-- name/id
   +-- WebElement
```

By index:

```java
driver.switchTo().frame(0);
```

By ID/name:

```java
driver.switchTo().frame("paymentFrame");
```

By WebElement:

```java
driver.switchTo().frame(frameElement);
```

Return one level:

```java
driver.switchTo().parentFrame();
```

Return main page:

```java
driver.switchTo().defaultContent();
```

Wait:

```java
ExpectedConditions
    .frameToBeAvailableAndSwitchToIt(...)
```

Find frames:

```java
driver.findElements(
    By.tagName("iframe")
);
```

Common exception:

```text
NoSuchFrameException
```

---

# 50. Key Takeaways

Remember these core Selenium frame concepts:

```text
Frame = Embedded document

switchTo().frame()
    = Enter frame

parentFrame()
    = Move one level up

defaultContent()
    = Return to main document

frameToBeAvailableAndSwitchToIt()
    = Wait + switch

NoSuchFrameException
    = Frame cannot be switched to

NoSuchElementException
    = Element cannot be found in current context
```

Most importantly:

> **Always switch to the correct frame before interacting with elements inside it, and switch back to the appropriate parent/main context when finished.**

---

# 51. Repository Location

```text
SeleniumStudy/
│
├── Selenium-Basics/
├── Browser/
├── WebDriver/
├── WebElements/
├── Locators/
├── Waits/
│
├── Alerts/
│   └── Selenium-Alerts.md
│
└── Frames/
    └── Selenium-Frames.md
```

Next recommended file:

```text
Windows/Selenium-Windows.md
```
