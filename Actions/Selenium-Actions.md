# Selenium Actions

## 1. Introduction

The Selenium `Actions` class is used to perform advanced user interactions with web elements.

It is mainly used for:

* Mouse hover
* Right-click
* Double-click
* Drag and drop
* Click and hold
* Mouse movement
* Keyboard interactions
* Combining multiple actions

The `Actions` class is available in Selenium WebDriver.

```java
import org.openqa.selenium.interactions.Actions;
```

Create an `Actions` object:

```java
Actions actions = new Actions(driver);
```

---

# 2. Why Use Actions?

Normal Selenium commands work for many basic interactions:

```java
element.click();
element.sendKeys("Hello");
```

However, some UI interactions require more advanced browser events.

Examples:

```text
Mouse Hover
    ↓
Menu appears
    ↓
Move to submenu
    ↓
Click submenu item
```

For these scenarios, the `Actions` class is useful.

---

# 3. Creating Actions Object

```java
Actions actions = new Actions(driver);
```

Complete example:

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;

public class ActionsExample {

    public static void main(String[] args) {

        WebDriver driver = new ChromeDriver();

        driver.get("https://example.com");

        Actions actions = new Actions(driver);

        driver.quit();
    }
}
```

---

# 4. perform()

Most Actions operations are completed by calling:

```java
perform();
```

Example:

```java
actions.moveToElement(element).perform();
```

Another example:

```java
actions.doubleClick(element).perform();
```

Think of:

```java
perform();
```

as:

> Execute the action that was built.

---

# 5. Mouse Hover

Mouse hover is one of the most common uses of the `Actions` class.

Use:

```java
actions.moveToElement(element).perform();
```

Example:

```java
WebElement menu =
        driver.findElement(By.id("products"));

Actions actions = new Actions(driver);

actions.moveToElement(menu).perform();
```

This moves the mouse pointer over the element.

---

# 6. Mouse Hover Example

```java
WebElement products =
        driver.findElement(By.id("products"));

Actions actions = new Actions(driver);

actions.moveToElement(products).perform();

WebElement submenu =
        driver.findElement(By.id("submenu"));

submenu.click();
```

Typical use case:

```text
Products
    ↓
Mouse Hover
    ↓
Electronics
    ↓
Click
```

---

# 7. moveToElement()

Syntax:

```java
actions.moveToElement(element).perform();
```

You can also specify an offset:

```java
actions.moveToElement(element, xOffset, yOffset).perform();
```

Example:

```java
actions.moveToElement(element, 10, 20).perform();
```

The mouse is moved to a location relative to the element.

---

# 8. Click Using Actions

You can click an element using:

```java
actions.click(element).perform();
```

Example:

```java
WebElement login =
        driver.findElement(By.id("login"));

Actions actions = new Actions(driver);

actions.click(login).perform();
```

For a normal click, this is usually simpler:

```java
login.click();
```

Use `Actions` when you need to build more complex interactions.

---

# 9. Click Current Mouse Location

You can also use:

```java
actions.click().perform();
```

This clicks at the current mouse location.

Example:

```java
actions.moveToElement(element)
       .click()
       .perform();
```

This combines:

```text
Move → Click
```

---

# 10. Double Click

Use:

```java
actions.doubleClick(element).perform();
```

Example:

```java
WebElement file =
        driver.findElement(By.id("file"));

Actions actions = new Actions(driver);

actions.doubleClick(file).perform();
```

---

# 11. Double Click Current Location

You can use:

```java
actions.doubleClick().perform();
```

Example:

```java
actions.moveToElement(element)
       .doubleClick()
       .perform();
```

---

# 12. Right Click / Context Click

Right-click is performed using:

```java
actions.contextClick(element).perform();
```

Example:

```java
WebElement element =
        driver.findElement(By.id("menu"));

Actions actions = new Actions(driver);

actions.contextClick(element).perform();
```

This generates a context-menu/right-click event.

---

# 13. Right Click Without Element

You can also right-click at the current mouse position:

```java
actions.contextClick().perform();
```

---

# 14. Click and Hold

Use:

```java
actions.clickAndHold(element).perform();
```

Example:

```java
WebElement element =
        driver.findElement(By.id("slider"));

Actions actions = new Actions(driver);

actions.clickAndHold(element).perform();
```

This presses the mouse button and holds it.

---

# 15. Release

Use:

```java
actions.release().perform();
```

Example:

```java
actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();
```

This is useful for drag-and-drop scenarios.

---

# 16. Drag and Drop

Selenium provides:

```java
actions.dragAndDrop(source, target).perform();
```

Example:

```java
WebElement source =
        driver.findElement(By.id("source"));

WebElement target =
        driver.findElement(By.id("target"));

Actions actions = new Actions(driver);

actions.dragAndDrop(source, target).perform();
```

---

# 17. Drag and Drop by Offset

You can drag an element by a specific number of pixels.

```java
actions.dragAndDropBy(element, xOffset, yOffset)
       .perform();
```

Example:

```java
actions.dragAndDropBy(element, 100, 50)
       .perform();
```

Meaning:

```text
X = 100 pixels
Y = 50 pixels
```

---

# 18. Drag and Drop Using Click and Hold

Some applications do not work correctly with:

```java
dragAndDrop()
```

In those cases, use:

```java
actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();
```

Complete example:

```java
WebElement source =
        driver.findElement(By.id("source"));

WebElement target =
        driver.findElement(By.id("target"));

Actions actions = new Actions(driver);

actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();
```

---

# 19. Drag and Drop with Offset

You can move the mouse by an offset:

```java
actions.clickAndHold(source)
       .moveByOffset(100, 50)
       .release()
       .perform();
```

---

# 20. moveByOffset()

Syntax:

```java
actions.moveByOffset(xOffset, yOffset).perform();
```

Example:

```java
actions.moveByOffset(100, 200).perform();
```

This moves the mouse relative to its current position.

---

# 21. moveToElement() vs moveByOffset()

### moveToElement()

Moves the mouse to an element.

```java
actions.moveToElement(element).perform();
```

### moveByOffset()

Moves the mouse relative to its current location.

```java
actions.moveByOffset(100, 50).perform();
```

---

# 22. Chaining Actions

One of the most powerful features of the `Actions` class is chaining.

Example:

```java
actions.moveToElement(menu)
       .click()
       .perform();
```

Another example:

```java
actions.moveToElement(element)
       .doubleClick()
       .perform();
```

Another:

```java
actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();
```

---

# 23. Multiple Actions in One Chain

Example:

```java
actions.moveToElement(menu)
       .click()
       .moveToElement(submenu)
       .click()
       .perform();
```

Flow:

```text
Move to Menu
     ↓
Click
     ↓
Move to Submenu
     ↓
Click
     ↓
Perform
```

---

# 24. build() vs perform()

You may see:

```java
actions.moveToElement(element)
       .click()
       .build()
       .perform();
```

`build()` creates a composite action.

`perform()` executes the action.

For many Selenium use cases, this is enough:

```java
actions.moveToElement(element)
       .click()
       .perform();
```

You do not always need to call `build()` explicitly.

---

# 25. Example Using build()

```java
Action action =
        actions.moveToElement(element)
               .click()
               .build();

action.perform();
```

Import:

```java
import org.openqa.selenium.interactions.Action;
```

---

# 26. Actions and Keyboard

The `Actions` class can also generate keyboard events.

Example:

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();
```

This is equivalent to:

```text
Press CTRL
    ↓
Press A
    ↓
Release CTRL
```

---

# 27. CTRL + A

To select all text:

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();
```

On Windows/Linux:

```java
Keys.CONTROL
```

For Mac:

```java
Keys.COMMAND
```

---

# 28. CTRL + C

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("c")
       .keyUp(Keys.CONTROL)
       .perform();
```

---

# 29. CTRL + V

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("v")
       .keyUp(Keys.CONTROL)
       .perform();
```

---

# 30. Keyboard Shortcut Example

```java
WebElement input =
        driver.findElement(By.id("username"));

input.click();

Actions actions = new Actions(driver);

actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .sendKeys("Selva")
       .perform();
```

---

# 31. Using Keys.chord()

You can also use:

```java
String selectAll =
        Keys.chord(Keys.CONTROL, "a");
```

Then:

```java
element.sendKeys(selectAll);
```

Example:

```java
WebElement input =
        driver.findElement(By.id("username"));

input.sendKeys(Keys.chord(Keys.CONTROL, "a"));
```

---

# 32. Key Down

Use:

```java
actions.keyDown(Keys.SHIFT).perform();
```

Example:

```java
actions.keyDown(Keys.SHIFT)
       .sendKeys("hello")
       .keyUp(Keys.SHIFT)
       .perform();
```

---

# 33. Key Up

Use:

```java
actions.keyUp(Keys.SHIFT).perform();
```

Normally `keyDown()` and `keyUp()` are used together.

```java
actions.keyDown(Keys.SHIFT)
       .sendKeys("hello")
       .keyUp(Keys.SHIFT)
       .perform();
```

---

# 34. SHIFT + Click

You can select multiple items using SHIFT.

```java
actions.keyDown(Keys.SHIFT)
       .click(secondElement)
       .keyUp(Keys.SHIFT)
       .perform();
```

---

# 35. CTRL + Click

You can select multiple independent elements using CTRL.

```java
actions.keyDown(Keys.CONTROL)
       .click(element1)
       .click(element2)
       .click(element3)
       .keyUp(Keys.CONTROL)
       .perform();
```

---

# 36. Hover and Click Submenu

A very common real-world scenario:

```java
WebElement menu =
        driver.findElement(By.id("menu"));

WebElement submenu =
        driver.findElement(By.id("submenu"));

Actions actions = new Actions(driver);

actions.moveToElement(menu)
       .moveToElement(submenu)
       .click()
       .perform();
```

---

# 37. Hover Menu Example

```java
WebElement products =
        driver.findElement(By.xpath("//span[text()='Products']"));

WebElement laptops =
        driver.findElement(By.xpath("//span[text()='Laptops']"));

Actions actions = new Actions(driver);

actions.moveToElement(products)
       .moveToElement(laptops)
       .click()
       .perform();
```

---

# 38. Right Click and Select Option

```java
WebElement element =
        driver.findElement(By.id("file"));

Actions actions = new Actions(driver);

actions.contextClick(element).perform();

WebElement rename =
        driver.findElement(By.id("rename"));

rename.click();
```

---

# 39. Double Click Example

```java
WebElement row =
        driver.findElement(By.id("customerRow"));

Actions actions = new Actions(driver);

actions.doubleClick(row).perform();
```

---

# 40. Slider Example

Suppose a slider needs to move 100 pixels.

```java
WebElement slider =
        driver.findElement(By.id("slider"));

Actions actions = new Actions(driver);

actions.clickAndHold(slider)
       .moveByOffset(100, 0)
       .release()
       .perform();
```

---

# 41. Complete Mouse Actions Example

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;

public class MouseActionsExample {

    public static void main(String[] args) {

        WebDriver driver = new ChromeDriver();

        driver.manage().window().maximize();

        driver.get("https://example.com");

        Actions actions = new Actions(driver);

        WebElement menu =
                driver.findElement(By.id("menu"));

        // Hover
        actions.moveToElement(menu).perform();

        // Click
        actions.click(menu).perform();

        // Double click
        actions.doubleClick(menu).perform();

        // Right click
        actions.contextClick(menu).perform();

        driver.quit();
    }
}
```

---

# 42. Complete Drag and Drop Example

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;

public class DragDropExample {

    public static void main(String[] args) {

        WebDriver driver = new ChromeDriver();

        driver.manage().window().maximize();

        driver.get("https://example.com");

        WebElement source =
                driver.findElement(By.id("source"));

        WebElement target =
                driver.findElement(By.id("target"));

        Actions actions = new Actions(driver);

        actions.dragAndDrop(source, target).perform();

        driver.quit();
    }
}
```

---

# 43. Alternative Drag and Drop

If `dragAndDrop()` does not work:

```java
actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();
```

This approach can be useful for applications that implement drag-and-drop using custom JavaScript events.

---

# 44. Actions with Page Object Model

Example:

```java
public void hoverProducts() {

    Actions actions = new Actions(driver);

    actions.moveToElement(productsMenu)
           .perform();
}
```

Then:

```java
page.hoverProducts();
```

This keeps interaction logic inside the Page Object.

---

# 45. Reusable Utility Method

You can create a utility method:

```java
public static void mouseHover(
        WebDriver driver,
        WebElement element) {

    Actions actions = new Actions(driver);

    actions.moveToElement(element).perform();
}
```

Usage:

```java
SeleniumUtils.mouseHover(driver, menu);
```

---

# 46. Utility Method for Double Click

```java
public static void doubleClick(
        WebDriver driver,
        WebElement element) {

    Actions actions = new Actions(driver);

    actions.doubleClick(element).perform();
}
```

Usage:

```java
SeleniumUtils.doubleClick(driver, element);
```

---

# 47. Utility Method for Right Click

```java
public static void rightClick(
        WebDriver driver,
        WebElement element) {

    Actions actions = new Actions(driver);

    actions.contextClick(element).perform();
}
```

---

# 48. Utility Method for Drag and Drop

```java
public static void dragAndDrop(
        WebDriver driver,
        WebElement source,
        WebElement target) {

    Actions actions = new Actions(driver);

    actions.dragAndDrop(source, target).perform();
}
```

---

# 49. Actions vs WebElement.click()

### WebElement click

```java
element.click();
```

Use for a normal click.

### Actions click

```java
actions.click(element).perform();
```

Useful when the interaction is part of a larger action sequence.

Example:

```java
actions.moveToElement(element)
       .click()
       .perform();
```

---

# 50. Actions vs JavaScript Click

Normal Selenium click:

```java
element.click();
```

Actions click:

```java
actions.click(element).perform();
```

JavaScript click:

```java
JavascriptExecutor js =
        (JavascriptExecutor) driver;

js.executeScript(
        "arguments[0].click();",
        element);
```

Preferred order:

```text
1. WebElement.click()
2. Actions click
3. JavaScript click when appropriate
```

Do not use JavaScript click as the first solution for every click problem.

---

# 51. Common Exceptions

## MoveTargetOutOfBoundsException

Can occur when the requested mouse movement is outside the valid browser viewport.

Example:

```java
actions.moveByOffset(10000, 10000).perform();
```

---

## ElementNotInteractableException

Can occur when the element cannot currently receive the interaction.

Possible causes:

* Element hidden
* Element disabled
* Element covered
* Element not ready

---

## StaleElementReferenceException

Can occur if the DOM changes after the element was located.

Solution:

Locate the element again.

---

# 52. Common Problems with Drag and Drop

`dragAndDrop()` may not work correctly with some modern JavaScript applications.

Possible alternatives:

```java
actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();
```

or:

```java
actions.clickAndHold(source)
       .moveByOffset(100, 50)
       .release()
       .perform();
```

Always validate the actual application behavior.

---

# 53. Best Practices

### Create one Actions object

```java
Actions actions = new Actions(driver);
```

### Chain related actions

```java
actions.moveToElement(menu)
       .click()
       .perform();
```

### Use perform()

```java
actions.doubleClick(element).perform();
```

### Prefer normal Selenium interactions when sufficient

```java
element.click();
```

### Use explicit waits when the UI requires synchronization

```java
WebDriverWait wait =
        new WebDriverWait(driver, Duration.ofSeconds(10));
```

### Keep complex interactions in Page Objects or utilities

This improves framework maintainability.

---

# 54. Important Actions Methods

| Method            | Purpose                |
| ----------------- | ---------------------- |
| `click()`         | Mouse click            |
| `doubleClick()`   | Double click           |
| `contextClick()`  | Right click            |
| `clickAndHold()`  | Press and hold         |
| `release()`       | Release mouse          |
| `moveToElement()` | Move mouse to element  |
| `moveByOffset()`  | Move mouse by offset   |
| `dragAndDrop()`   | Drag element to target |
| `dragAndDropBy()` | Drag by offset         |
| `keyDown()`       | Press keyboard key     |
| `keyUp()`         | Release keyboard key   |
| `sendKeys()`      | Send keyboard input    |
| `build()`         | Build composite action |
| `perform()`       | Execute action         |

---

# 55. Interview Questions

## Q1. What is the Actions class?

The Selenium `Actions` class is used to perform advanced mouse and keyboard interactions.

---

## Q2. How do you perform mouse hover?

```java
Actions actions = new Actions(driver);

actions.moveToElement(element).perform();
```

---

## Q3. How do you perform double-click?

```java
actions.doubleClick(element).perform();
```

---

## Q4. How do you perform right-click?

```java
actions.contextClick(element).perform();
```

---

## Q5. How do you perform drag and drop?

```java
actions.dragAndDrop(source, target).perform();
```

---

## Q6. What if dragAndDrop() does not work?

Try:

```java
actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();
```

---

## Q7. What does perform() do?

`perform()` executes the built action sequence.

Example:

```java
actions.moveToElement(element)
       .click()
       .perform();
```

---

## Q8. What does build() do?

`build()` creates a composite `Action` from the sequence of actions.

Example:

```java
Action action =
        actions.moveToElement(element)
               .click()
               .build();

action.perform();
```

---

## Q9. Can Actions handle keyboard operations?

Yes.

Example:

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();
```

---

## Q10. What is the difference between moveToElement() and moveByOffset()?

`moveToElement()` moves the mouse to an element.

```java
actions.moveToElement(element).perform();
```

`moveByOffset()` moves the mouse relative to its current position.

```java
actions.moveByOffset(100, 50).perform();
```

---

## Q11. How do you perform CTRL+A?

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();
```

---

## Q12. How do you select multiple elements using CTRL?

```java
actions.keyDown(Keys.CONTROL)
       .click(element1)
       .click(element2)
       .keyUp(Keys.CONTROL)
       .perform();
```

---

## Q13. Can Actions operations be chained?

Yes.

```java
actions.moveToElement(menu)
       .click()
       .moveToElement(submenu)
       .click()
       .perform();
```

---

# 56. Quick Reference

```java
Actions actions = new Actions(driver);

// Hover
actions.moveToElement(element).perform();

// Click
actions.click(element).perform();

// Double click
actions.doubleClick(element).perform();

// Right click
actions.contextClick(element).perform();

// Click and hold
actions.clickAndHold(element).perform();

// Release
actions.release().perform();

// Drag and drop
actions.dragAndDrop(source, target).perform();

// Drag by offset
actions.dragAndDropBy(element, 100, 50).perform();

// Move by offset
actions.moveByOffset(100, 50).perform();

// Keyboard
actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();
```

---

# 57. Actions Workflow

```text
Create Actions object
        ↓
Actions actions = new Actions(driver)
        ↓
Define interaction
        ↓
moveToElement()
click()
doubleClick()
dragAndDrop()
keyDown()
keyUp()
        ↓
Chain actions if required
        ↓
perform()
        ↓
Interaction executed
```

---

# 58. Key Takeaway

The Selenium `Actions` class is essential when automation requires realistic mouse or keyboard interactions.

The most important methods to remember are:

```java
moveToElement()
click()
doubleClick()
contextClick()
clickAndHold()
release()
dragAndDrop()
dragAndDropBy()
moveByOffset()
keyDown()
keyUp()
sendKeys()
perform()
```

The most common real-world example is:

```java
Actions actions = new Actions(driver);

actions.moveToElement(menu)
       .moveToElement(submenu)
       .click()
       .perform();
```

For drag and drop:

```java
actions.dragAndDrop(source, target).perform();
```

And when that does not work:

```java
actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();
```

---

# 59. Selenium Study Progress

```text
Selenium-Basics
WebElements
Alerts
Windows
Frames
Tabs
Actions                     ← Current
```

Next recommended topic:

```text
Mouse/Selenium-Mouse-Actions.md
```
