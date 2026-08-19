# Selenium Advanced Interactions

## 1. Introduction

Selenium provides several APIs for interacting with complex web elements and user gestures.

Basic Selenium interactions include:

```java
element.click();
element.sendKeys("text");
element.clear();
```

Advanced interactions are useful when applications contain:

* Hover menus
* Drag and drop
* Sliders
* Custom dropdowns
* Context menus
* Double-click actions
* Keyboard shortcuts
* Dynamic menus
* Canvas elements
* Complex mouse gestures
* Scrolling behavior
* Multi-step user interactions

The primary Selenium API for advanced interactions is the:

```java
Actions
```

class.

---

# 2. Actions Class

The Selenium `Actions` class is used to perform complex user interactions.

Import:

```java
import org.openqa.selenium.interactions.Actions;
```

Create an Actions object:

```java
Actions actions = new Actions(driver);
```

Basic example:

```java
actions
    .moveToElement(element)
    .click()
    .perform();
```

---

# 3. `perform()` Method

The `perform()` method executes the action sequence.

Example:

```java
actions
    .moveToElement(element)
    .click()
    .perform();
```

Without:

```java
.perform();
```

the action sequence is not executed.

---

# 4. `build()` vs `perform()`

You may see:

```java
actions
    .moveToElement(element)
    .click()
    .build()
    .perform();
```

`build()` creates the composite action sequence.

In many Selenium 4 cases, you can simply use:

```java
actions
    .moveToElement(element)
    .click()
    .perform();
```

### Simple rule

Use:

```java
perform()
```

for most normal action chains.

---

# 5. Mouse Hover

Mouse hover is commonly used for menus.

Example:

```java
Actions actions = new Actions(driver);

WebElement menu =
        driver.findElement(By.id("products"));

actions
    .moveToElement(menu)
    .perform();
```

---

# 6. Hover and Click Submenu

```java
WebElement products =
        driver.findElement(By.id("products"));

WebElement laptops =
        driver.findElement(By.id("laptops"));

Actions actions = new Actions(driver);

actions
    .moveToElement(products)
    .moveToElement(laptops)
    .click()
    .perform();
```

Flow:

```text
Move to Products
       ↓
Menu appears
       ↓
Move to Laptops
       ↓
Click
```

---

# 7. Hover Example

```java
public void hoverOverProducts() {

    WebElement products =
            driver.findElement(
                    By.id("products"));

    Actions actions =
            new Actions(driver);

    actions
        .moveToElement(products)
        .perform();
}
```

---

# 8. Move by Offset

Selenium allows mouse movement using X/Y offsets.

```java
actions
    .moveByOffset(100, 50)
    .perform();
```

This moves the pointer:

```text
X = +100
Y = +50
```

---

# 9. Move Relative to an Element

```java
actions
    .moveToElement(
        element,
        20,
        10
    )
    .perform();
```

The pointer moves to a position relative to the element.

---

# 10. Double Click

```java
Actions actions =
        new Actions(driver);

actions
    .doubleClick(element)
    .perform();
```

Example:

```java
WebElement file =
        driver.findElement(
                By.id("file"));

actions
    .doubleClick(file)
    .perform();
```

---

# 11. Context Click

Context click means right-click.

```java
actions
    .contextClick(element)
    .perform();
```

Example:

```java
WebElement item =
        driver.findElement(
                By.id("item"));

actions
    .contextClick(item)
    .perform();
```

---

# 12. Click and Hold

```java
actions
    .clickAndHold(element)
    .perform();
```

Useful for:

* Sliders
* Drag operations
* Custom controls
* Canvas interactions

---

# 13. Release

```java
actions
    .release()
    .perform();
```

Usually used after:

```java
clickAndHold()
```

Example:

```java
actions
    .clickAndHold(source)
    .moveToElement(target)
    .release()
    .perform();
```

---

# 14. Drag and Drop

Selenium provides:

```java
dragAndDrop()
```

Example:

```java
WebElement source =
        driver.findElement(
                By.id("source"));

WebElement target =
        driver.findElement(
                By.id("target"));

Actions actions =
        new Actions(driver);

actions
    .dragAndDrop(
        source,
        target
    )
    .perform();
```

---

# 15. Drag and Drop by Offset

You can drag an element by X/Y offset.

```java
actions
    .dragAndDropBy(
        element,
        200,
        100
    )
    .perform();
```

This moves the element:

```text
X = 200 pixels
Y = 100 pixels
```

---

# 16. Manual Drag and Drop

Some HTML5 applications do not work reliably with:

```java
dragAndDrop()
```

In such cases:

```java
actions
    .clickAndHold(source)
    .moveToElement(target)
    .release()
    .perform();
```

This often provides better control.

---

# 17. Drag and Drop by Offset

```java
actions
    .clickAndHold(source)
    .moveByOffset(300, 0)
    .release()
    .perform();
```

This is useful for:

* Sliders
* Canvas controls
* Custom drag interfaces

---

# 18. Slider Handling

Consider a slider:

```text
0 ───────────────●────────────── 100
                 ↑
              Current
```

You can drag the slider using offsets.

```java
WebElement slider =
        driver.findElement(
                By.id("slider"));

Actions actions =
        new Actions(driver);

actions
    .clickAndHold(slider)
    .moveByOffset(100, 0)
    .release()
    .perform();
```

The exact offset depends on:

* Slider width
* Current position
* Desired value
* Browser rendering

---

# 19. Keyboard Actions

The `Actions` API also supports keyboard interactions.

Example:

```java
actions
    .sendKeys(Keys.ENTER)
    .perform();
```

Import:

```java
import org.openqa.selenium.Keys;
```

---

# 20. Ctrl + A

Select all text:

```java
actions
    .keyDown(Keys.CONTROL)
    .sendKeys("a")
    .keyUp(Keys.CONTROL)
    .perform();
```

On macOS, use:

```java
Keys.COMMAND
```

instead of:

```java
Keys.CONTROL
```

---

# 21. Ctrl + C

```java
actions
    .keyDown(Keys.CONTROL)
    .sendKeys("c")
    .keyUp(Keys.CONTROL)
    .perform();
```

---

# 22. Ctrl + V

```java
actions
    .keyDown(Keys.CONTROL)
    .sendKeys("v")
    .keyUp(Keys.CONTROL)
    .perform();
```

---

# 23. Keyboard Shortcut Example

```java
WebElement input =
        driver.findElement(
                By.id("username"));

input.click();

actions
    .keyDown(Keys.CONTROL)
    .sendKeys("a")
    .keyUp(Keys.CONTROL)
    .sendKeys("newuser")
    .perform();
```

---

# 24. SHIFT Key

Example:

```java
actions
    .keyDown(Keys.SHIFT)
    .sendKeys("hello")
    .keyUp(Keys.SHIFT)
    .perform();
```

---

# 25. ALT Key

```java
actions
    .keyDown(Keys.ALT)
    .sendKeys("a")
    .keyUp(Keys.ALT)
    .perform();
```

---

# 26. TAB Navigation

```java
actions
    .sendKeys(Keys.TAB)
    .perform();
```

Multiple tabs:

```java
actions
    .sendKeys(Keys.TAB)
    .sendKeys(Keys.TAB)
    .sendKeys(Keys.ENTER)
    .perform();
```

---

# 27. ENTER and ESCAPE

### Enter

```java
actions
    .sendKeys(Keys.ENTER)
    .perform();
```

### Escape

```java
actions
    .sendKeys(Keys.ESCAPE)
    .perform();
```

Useful for:

* Closing menus
* Closing dialogs
* Submitting forms
* Cancelling overlays

---

# 28. HOME and END

```java
actions
    .sendKeys(Keys.HOME)
    .perform();
```

```java
actions
    .sendKeys(Keys.END)
    .perform();
```

---

# 29. PAGE UP and PAGE DOWN

```java
actions
    .sendKeys(Keys.PAGE_DOWN)
    .perform();
```

```java
actions
    .sendKeys(Keys.PAGE_UP)
    .perform();
```

---

# 30. Scrolling

Modern Selenium can use:

```java
JavascriptExecutor
```

or Selenium's scrolling-related APIs.

Basic JavaScript scroll:

```java
JavascriptExecutor js =
        (JavascriptExecutor) driver;

js.executeScript(
    "window.scrollBy(0,500);"
);
```

---

# 31. Scroll to Element

```java
WebElement element =
        driver.findElement(
                By.id("footer"));

((JavascriptExecutor) driver)
    .executeScript(
        "arguments[0].scrollIntoView(true);",
        element
    );
```

---

# 32. Scroll to Bottom

```java
JavascriptExecutor js =
        (JavascriptExecutor) driver;

js.executeScript(
    "window.scrollTo(0, document.body.scrollHeight);"
);
```

---

# 33. Selenium Wheel Actions

Selenium provides wheel input support.

Example:

```java
Actions actions =
        new Actions(driver);

actions
    .scrollByAmount(0, 500)
    .perform();
```

---

# 34. Scroll to Element

With Selenium's wheel actions:

```java
actions
    .scrollToElement(element)
    .perform();
```

This is often cleaner than JavaScript when native WebDriver scrolling is sufficient.

---

# 35. Scroll from an Element

```java
actions
    .scrollFromOrigin(
        WheelInput.ScrollOrigin
            .fromElement(element),
        0,
        500
    )
    .perform();
```

This gives more precise control over the scrolling origin.

---

# 36. Mouse Wheel Actions

Example:

```java
actions
    .scrollByAmount(
        0,
        800
    )
    .perform();
```

Negative value:

```java
actions
    .scrollByAmount(
        0,
        -800
    )
    .perform();
```

---

# 37. Pause Between Actions

Selenium supports pauses in action chains.

```java
actions
    .moveToElement(menu)
    .pause(Duration.ofSeconds(1))
    .moveToElement(subMenu)
    .click()
    .perform();
```

Import:

```java
import java.time.Duration;
```

This can be useful when a hover menu needs a short period to appear.

Avoid using unnecessary pauses when explicit waits can be used instead.

---

# 38. Complex Action Chain

Example:

```java
actions
    .moveToElement(menu)
    .pause(Duration.ofSeconds(1))
    .moveToElement(subMenu)
    .click()
    .perform();
```

Flow:

```text
Move
 ↓
Pause
 ↓
Move
 ↓
Click
```

---

# 39. Pointer Actions

Selenium's low-level input APIs allow more control over pointer actions.

Pointer types include:

* Mouse
* Touch
* Pen

Example:

```java
PointerInput mouse =
        new PointerInput(
                PointerInput.Kind.MOUSE,
                "mouse"
        );
```

---

# 40. Low-Level Mouse Action

Example:

```java
PointerInput mouse =
        new PointerInput(
                PointerInput.Kind.MOUSE,
                "mouse"
        );

Sequence sequence =
        new Sequence(mouse, 0);

sequence.addAction(
        mouse.createPointerMove(
                Duration.ZERO,
                PointerInput.Origin.viewport(),
                500,
                300
        )
);

sequence.addAction(
        mouse.createPointerDown(
                PointerInput.MouseButton.LEFT.asArg()
        )
);

sequence.addAction(
        mouse.createPointerUp(
                PointerInput.MouseButton.LEFT.asArg()
        )
);

driver.perform(
        Arrays.asList(sequence)
);
```

This provides lower-level control than the `Actions` class.

---

# 41. Actions API vs Low-Level Actions

| Feature                 | Actions | Low-Level API    |
| ----------------------- | ------- | ---------------- |
| Ease of use             | Easy    | More complex     |
| Mouse                   | Yes     | Yes              |
| Keyboard                | Yes     | Yes              |
| Pointer                 | Yes     | Yes              |
| Drag/drop               | Easy    | Detailed control |
| Complex gestures        | Good    | Excellent        |
| Typical framework usage | Common  | Specialized      |

Use `Actions` for most normal automation.

Use low-level APIs when precise input control is required.

---

# 42. Handling Hover Menus with Waits

Avoid:

```java
Thread.sleep(3000);
```

Prefer:

```java
WebDriverWait wait =
        new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );
```

Example:

```java
actions
    .moveToElement(menu)
    .perform();

WebElement submenu =
        wait.until(
            ExpectedConditions
                .visibilityOfElementLocated(
                    By.id("submenu")
                )
        );

submenu.click();
```

---

# 43. Handling Dynamic Elements

Advanced interactions often involve dynamic elements.

Example:

```java
WebElement menu =
        wait.until(
            ExpectedConditions
                .elementToBeClickable(
                    By.id("menu")
                )
        );

actions
    .moveToElement(menu)
    .perform();
```

This is more reliable than immediately performing an action against an element that may not yet be ready.

---

# 44. Handling Stale Elements

A dynamic application may replace an element after you locate it.

Bad pattern:

```java
WebElement element =
        driver.findElement(
                By.id("dynamic"));

actions
    .click(element)
    .perform();
```

If the DOM changes, the element can become stale.

A better approach is to wait for the element again:

```java
WebElement element =
        wait.until(
            ExpectedConditions
                .elementToBeClickable(
                    By.id("dynamic")
                )
        );

element.click();
```

---

# 45. Hover Menu with Complete Example

```java
import java.time.Duration;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

public class HoverMenuExample {

    public static void main(String[] args) {

        WebDriver driver =
                new ChromeDriver();

        WebDriverWait wait =
                new WebDriverWait(
                        driver,
                        Duration.ofSeconds(10)
                );

        Actions actions =
                new Actions(driver);

        try {

            driver.get(
                    "https://example.com"
            );

            WebElement menu =
                    wait.until(
                        ExpectedConditions
                            .visibilityOfElementLocated(
                                By.id("products")
                            )
                    );

            actions
                .moveToElement(menu)
                .perform();

            WebElement submenu =
                    wait.until(
                        ExpectedConditions
                            .elementToBeClickable(
                                By.id("laptops")
                            )
                    );

            submenu.click();

        } finally {

            driver.quit();
        }
    }
}
```

---

# 46. Drag and Drop Complete Example

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;

public class DragDropExample {

    public static void main(String[] args) {

        WebDriver driver =
                new ChromeDriver();

        try {

            driver.get(
                    "https://example.com"
            );

            WebElement source =
                    driver.findElement(
                            By.id("source")
                    );

            WebElement target =
                    driver.findElement(
                            By.id("target")
                    );

            Actions actions =
                    new Actions(driver);

            actions
                .dragAndDrop(
                    source,
                    target
                )
                .perform();

        } finally {

            driver.quit();
        }
    }
}
```

---

# 47. Advanced Drag and Drop

If normal drag and drop is unreliable:

```java
actions
    .clickAndHold(source)
    .pause(Duration.ofMillis(500))
    .moveToElement(target)
    .pause(Duration.ofMillis(500))
    .release()
    .perform();
```

This can help applications that require a realistic sequence of pointer events.

---

# 48. Double Click Example

```java
WebElement element =
        driver.findElement(
                By.id("document")
        );

Actions actions =
        new Actions(driver);

actions
    .doubleClick(element)
    .perform();
```

---

# 49. Right Click Example

```java
WebElement element =
        driver.findElement(
                By.id("document")
        );

actions
    .contextClick(element)
    .perform();
```

If a custom context menu appears:

```java
WebElement delete =
        wait.until(
            ExpectedConditions
                .elementToBeClickable(
                    By.id("delete")
                )
        );

delete.click();
```

---

# 50. Keyboard + Mouse Combination

Example:

```java
actions
    .keyDown(Keys.CONTROL)
    .click(element1)
    .click(element2)
    .keyUp(Keys.CONTROL)
    .perform();
```

This can simulate selecting multiple items.

---

# 51. Shift + Click

```java
actions
    .click(firstElement)
    .keyDown(Keys.SHIFT)
    .click(lastElement)
    .keyUp(Keys.SHIFT)
    .perform();
```

Useful for applications that support range selection.

---

# 52. Multiple Actions in One Chain

```java
actions
    .moveToElement(element)
    .click()
    .sendKeys("Hello")
    .keyDown(Keys.CONTROL)
    .sendKeys("a")
    .keyUp(Keys.CONTROL)
    .sendKeys("Updated")
    .perform();
```

This executes a sequence of interactions.

---

# 53. Actions and Page Objects

Advanced interactions should generally be encapsulated inside Page Objects.

Example:

```java
public class ProductPage {

    private WebDriver driver;

    private Actions actions;

    public ProductPage(WebDriver driver) {

        this.driver = driver;

        this.actions =
                new Actions(driver);
    }

    public void hoverProduct(
            WebElement product) {

        actions
            .moveToElement(product)
            .perform();
    }
}
```

This keeps test classes clean.

---

# 54. Page Object Example

```java
public void selectProduct() {

    actions
        .moveToElement(productsMenu)
        .moveToElement(laptopsMenu)
        .click()
        .perform();
}
```

Test:

```java
@Test
public void selectLaptop() {

    productPage.selectProduct();
}
```

The test does not need to know the interaction details.

---

# 55. Common Problems with Actions

## Problem 1: Hover does not work

Possible causes:

* Element is not visible
* Overlay is present
* Element moves
* Menu requires a delay
* Incorrect locator

Solution:

```java
WebElement menu =
        wait.until(
            ExpectedConditions
                .visibilityOfElementLocated(
                    By.id("menu")
                )
        );

actions
    .moveToElement(menu)
    .perform();
```

---

# 56. Drag and Drop Does Not Work

Possible causes:

* HTML5 drag and drop
* Custom JavaScript implementation
* Element is not visible
* Incorrect target
* Browser-specific behavior

Try:

```java
actions
    .clickAndHold(source)
    .moveToElement(target)
    .release()
    .perform();
```

If necessary, investigate the application's actual drag/drop implementation rather than blindly switching to JavaScript.

---

# 57. Element Intercepted Exception

You may see:

```text
ElementClickInterceptedException
```

Possible cause:

```text
Overlay
Popup
Sticky header
Another element
```

Solution:

Wait until the element is clickable:

```java
WebElement button =
        wait.until(
            ExpectedConditions
                .elementToBeClickable(
                    By.id("button")
                )
        );

button.click();
```

---

# 58. Move Target Out of Bounds

You may see:

```text
MoveTargetOutOfBoundsException
```

Possible causes:

* Invalid coordinates
* Element outside viewport
* Incorrect offset

Try scrolling to the element:

```java
actions
    .scrollToElement(element)
    .perform();

actions
    .moveToElement(element)
    .click()
    .perform();
```

---

# 59. Stale Element During Actions

If an element is replaced by the application:

```text
DOM changes
   ↓
Old element reference
   ↓
Action
   ↓
StaleElementReferenceException
```

Solution:

Locate the element again after the DOM update.

---

# 60. JavaScript vs Actions

| Requirement             | Preferred Approach                                    |
| ----------------------- | ----------------------------------------------------- |
| Normal click            | `element.click()`                                     |
| Hover                   | `Actions`                                             |
| Double click            | `Actions`                                             |
| Right click             | `Actions`                                             |
| Drag/drop               | `Actions`                                             |
| Keyboard shortcut       | `Actions`                                             |
| Scrolling               | Selenium Wheel Actions / JavaScript depending on need |
| DOM manipulation        | JavaScript                                            |
| Complex pointer gesture | Actions / low-level API                               |

Do not automatically use JavaScript when an equivalent WebDriver action is available.

---

# 61. Advanced Interactions Best Practices

### 1. Prefer WebDriver APIs

Use:

```java
element.click();
```

before:

```java
JavascriptExecutor
```

### 2. Use Actions for user gestures

Use `Actions` for:

* Hover
* Drag/drop
* Double click
* Right click
* Keyboard combinations

### 3. Use explicit waits

Avoid:

```java
Thread.sleep()
```

when synchronization can be handled with conditions.

### 4. Keep interactions inside Page Objects

Do not put large action chains directly into test methods.

### 5. Keep offsets minimal

Pixel-based offsets can become fragile when page layouts change.

### 6. Avoid unnecessary complex chains

Simple code is easier to maintain.

### 7. Make parallel tests independent

Actions should use the current thread's WebDriver.

---

# 62. Real-World Example: Dealer Search

A realistic automation flow might contain:

```text
Open Dealer Search
       ↓
Click location field
       ↓
Enter ZIP code
       ↓
Press ENTER
       ↓
Hover over dealer
       ↓
Open dealer menu
       ↓
Select Service
       ↓
Scroll to appointment section
       ↓
Select date
       ↓
Select time
       ↓
Confirm appointment
```

Actions may be used for:

```java
actions
    .moveToElement(dealer)
    .click()
    .perform();
```

Keyboard:

```java
actions
    .sendKeys(Keys.ENTER)
    .perform();
```

Scrolling:

```java
actions
    .scrollToElement(appointmentSection)
    .perform();
```

---

# 63. Real-World Example: Shopping Application

Example flow:

```text
Product
   ↓
Hover
   ↓
Quick View
   ↓
Click
   ↓
Select Size
   ↓
Drag Slider
   ↓
Add to Cart
```

Possible interactions:

```java
actions
    .moveToElement(product)
    .perform();
```

```java
actions
    .dragAndDropBy(
        slider,
        100,
        0
    )
    .perform();
```

---

# 64. Complete Advanced Interaction Class

```java
import java.time.Duration;

import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;

public class AdvancedInteractions {

    public static void main(String[] args) {

        WebDriver driver =
                new ChromeDriver();

        Actions actions =
                new Actions(driver);

        try {

            driver.get(
                    "https://example.com"
            );

            WebElement menu =
                    driver.findElement(
                            By.id("menu")
                    );

            WebElement item =
                    driver.findElement(
                            By.id("item")
                    );

            // Hover
            actions
                .moveToElement(menu)
                .perform();

            // Click
            actions
                .click(item)
                .perform();

            // Double click
            actions
                .doubleClick(item)
                .perform();

            // Right click
            actions
                .contextClick(item)
                .perform();

            // Keyboard
            actions
                .sendKeys(Keys.ESCAPE)
                .perform();

            // Scroll
            actions
                .scrollByAmount(
                    0,
                    500
                )
                .perform();

            // Pause
            actions
                .pause(
                    Duration.ofSeconds(1)
                )
                .perform();

        } finally {

            driver.quit();
        }
    }
}
```

---

# 65. Interview Questions

## Q1. What is the Actions class?

`Actions` is Selenium's high-level API for performing complex mouse, keyboard, and pointer interactions.

---

## Q2. How do you perform mouse hover?

```java
actions
    .moveToElement(element)
    .perform();
```

---

## Q3. How do you perform double-click?

```java
actions
    .doubleClick(element)
    .perform();
```

---

## Q4. How do you perform right-click?

```java
actions
    .contextClick(element)
    .perform();
```

---

## Q5. How do you perform drag and drop?

```java
actions
    .dragAndDrop(source, target)
    .perform();
```

---

## Q6. What if `dragAndDrop()` doesn't work?

Try a manual sequence:

```java
actions
    .clickAndHold(source)
    .moveToElement(target)
    .release()
    .perform();
```

Then investigate whether the application uses HTML5/custom drag-and-drop behavior.

---

## Q7. How do you press Ctrl+A?

```java
actions
    .keyDown(Keys.CONTROL)
    .sendKeys("a")
    .keyUp(Keys.CONTROL)
    .perform();
```

---

## Q8. How do you scroll using Selenium Actions?

```java
actions
    .scrollByAmount(0, 500)
    .perform();
```

or:

```java
actions
    .scrollToElement(element)
    .perform();
```

---

## Q9. What is `clickAndHold()`?

It presses and holds the mouse button on an element until a later `release()` action.

---

## Q10. What is `moveByOffset()`?

It moves the pointer by a specified X/Y offset.

```java
actions
    .moveByOffset(100, 50)
    .perform();
```

---

## Q11. What is the difference between Actions and JavaScriptExecutor?

`Actions` simulates user input such as mouse and keyboard interactions.

`JavaScriptExecutor` executes JavaScript directly inside the browser.

For normal user interactions, prefer Selenium WebDriver APIs and Actions.

---

## Q12. What is a low-level pointer action?

It provides more granular control over pointer input such as:

* Mouse
* Touch
* Pen

It is useful when high-level Actions APIs do not provide enough control.

---

# 66. Quick Revision

```text
Advanced Selenium Interactions
│
├── Actions
│   ├── moveToElement()
│   ├── moveByOffset()
│   ├── click()
│   ├── doubleClick()
│   ├── contextClick()
│   ├── clickAndHold()
│   ├── release()
│   ├── dragAndDrop()
│   └── dragAndDropBy()
│
├── Keyboard
│   ├── keyDown()
│   ├── keyUp()
│   ├── sendKeys()
│   └── Keys
│
├── Scrolling
│   ├── scrollByAmount()
│   ├── scrollToElement()
│   └── scrollFromOrigin()
│
├── Pointer Actions
│   ├── Mouse
│   ├── Touch
│   └── Pen
│
└── Synchronization
    └── WebDriverWait
```

---

# 67. Key Takeaways

Remember these APIs for Selenium interviews and real-world automation:

```java
actions.moveToElement(element).perform();

actions.doubleClick(element).perform();

actions.contextClick(element).perform();

actions.clickAndHold(element).perform();

actions.release().perform();

actions.dragAndDrop(source, target).perform();

actions.dragAndDropBy(element, 100, 0).perform();

actions.sendKeys(Keys.ENTER).perform();

actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();

actions.scrollByAmount(0, 500).perform();

actions.scrollToElement(element).perform();
```

The key principle is:

```text
Simple interaction
      ↓
WebDriver API

Complex user gesture
      ↓
Actions API

Precise low-level gesture
      ↓
Pointer Actions

DOM/browser-level operation
      ↓
JavaScriptExecutor
```

A strong Selenium framework should use the **simplest reliable API for each interaction**, while keeping synchronization, Page Objects, and parallel execution in mind.
