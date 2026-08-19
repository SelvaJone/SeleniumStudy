# Selenium Mouse Actions

## 1. Introduction

Selenium WebDriver provides the `Actions` class for performing advanced mouse interactions.

Mouse actions are useful when a test needs to simulate real user behavior such as:

* Mouse hover
* Click
* Double-click
* Right-click
* Click and hold
* Mouse movement
* Drag and drop
* Drag by offset
* Moving to a specific location
* Chained mouse interactions

Import:

```java
import org.openqa.selenium.interactions.Actions;
```

Create the Actions object:

```java
Actions actions = new Actions(driver);
```

---

# 2. Basic Mouse Actions

The most commonly used mouse methods are:

```java
actions.click().perform();
actions.doubleClick().perform();
actions.contextClick().perform();
actions.clickAndHold().perform();
actions.release().perform();
actions.moveToElement(element).perform();
actions.moveByOffset(100, 50).perform();
```

---

# 3. Mouse Click

A normal Selenium click can be performed with:

```java
element.click();
```

Using `Actions`:

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

For a simple click, prefer:

```java
login.click();
```

Use `Actions` when the click is part of a more complex mouse interaction.

---

# 4. Click at Current Mouse Position

You can use:

```java
actions.click().perform();
```

This clicks at the current mouse position.

Example:

```java
actions.moveToElement(element)
       .click()
       .perform();
```

---

# 5. Mouse Hover

Mouse hover is one of the most common mouse operations.

Use:

```java
actions.moveToElement(element).perform();
```

Example:

```java
WebElement products =
        driver.findElement(By.id("products"));

Actions actions = new Actions(driver);

actions.moveToElement(products).perform();
```

---

# 6. Mouse Hover and Click

You can combine the actions:

```java
actions.moveToElement(element)
       .click()
       .perform();
```

Example:

```java
WebElement menu =
        driver.findElement(By.id("menu"));

actions.moveToElement(menu)
       .click()
       .perform();
```

---

# 7. Mouse Hover Menu

A typical application might have:

```text
Products
   |
   +-- Electronics
   +-- Laptops
   +-- Phones
```

Automation:

```java
WebElement products =
        driver.findElement(By.id("products"));

WebElement laptops =
        driver.findElement(By.id("laptops"));

actions.moveToElement(products)
       .moveToElement(laptops)
       .click()
       .perform();
```

---

# 8. moveToElement()

Syntax:

```java
actions.moveToElement(element).perform();
```

This moves the mouse pointer to the center of the element.

Example:

```java
WebElement menu =
        driver.findElement(By.id("menu"));

actions.moveToElement(menu).perform();
```

---

# 9. moveToElement() with Offset

You can specify an offset from the center of the element.

```java
actions.moveToElement(
        element,
        xOffset,
        yOffset
).perform();
```

Example:

```java
actions.moveToElement(element, 10, 20)
       .perform();
```

The mouse is moved relative to the element.

---

# 10. moveByOffset()

`moveByOffset()` moves the mouse relative to its current position.

Syntax:

```java
actions.moveByOffset(xOffset, yOffset).perform();
```

Example:

```java
actions.moveByOffset(100, 50).perform();
```

Meaning:

```text
Move 100 pixels horizontally
Move 50 pixels vertically
```

---

# 11. moveToElement() vs moveByOffset()

| Method                         | Movement                                 |
| ------------------------------ | ---------------------------------------- |
| `moveToElement()`              | Moves to an element                      |
| `moveToElement(element, x, y)` | Moves relative to an element             |
| `moveByOffset(x, y)`           | Moves relative to current mouse position |

Example:

```java
actions.moveToElement(element).perform();
```

versus:

```java
actions.moveByOffset(100, 50).perform();
```

---

# 12. Double Click

Use:

```java
actions.doubleClick(element).perform();
```

Example:

```java
WebElement row =
        driver.findElement(By.id("row"));

actions.doubleClick(row).perform();
```

---

# 13. Double Click at Current Location

```java
actions.doubleClick().perform();
```

Or:

```java
actions.moveToElement(element)
       .doubleClick()
       .perform();
```

---

# 14. Right Click

Right-click is called `contextClick()`.

```java
actions.contextClick(element).perform();
```

Example:

```java
WebElement file =
        driver.findElement(By.id("file"));

actions.contextClick(file).perform();
```

---

# 15. Right Click at Current Location

```java
actions.contextClick().perform();
```

---

# 16. Right-Click Menu

Example:

```java
WebElement file =
        driver.findElement(By.id("file"));

actions.contextClick(file).perform();

WebElement rename =
        driver.findElement(By.id("rename"));

rename.click();
```

The flow is:

```text
Locate element
      ↓
Right-click
      ↓
Context menu appears
      ↓
Select option
```

---

# 17. Click and Hold

Use:

```java
actions.clickAndHold(element).perform();
```

Example:

```java
WebElement slider =
        driver.findElement(By.id("slider"));

actions.clickAndHold(slider).perform();
```

This presses the mouse button and keeps it pressed.

---

# 18. Release Mouse

Use:

```java
actions.release().perform();
```

Example:

```java
actions.clickAndHold(source)
       .release()
       .perform();
```

---

# 19. Click and Hold + Move

This is commonly used for drag operations.

```java
actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();
```

Flow:

```text
Click and hold
      ↓
Move
      ↓
Release
```

---

# 20. Drag and Drop

Selenium provides a direct method:

```java
actions.dragAndDrop(source, target).perform();
```

Example:

```java
WebElement source =
        driver.findElement(By.id("source"));

WebElement target =
        driver.findElement(By.id("target"));

actions.dragAndDrop(source, target).perform();
```

---

# 21. Drag and Drop by Offset

Use:

```java
actions.dragAndDropBy(
        element,
        xOffset,
        yOffset
).perform();
```

Example:

```java
actions.dragAndDropBy(
        slider,
        100,
        0
).perform();
```

---

# 22. Drag and Drop Alternative

Some applications do not respond correctly to `dragAndDrop()`.

Use:

```java
actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();
```

Example:

```java
WebElement source =
        driver.findElement(By.id("source"));

WebElement target =
        driver.findElement(By.id("target"));

actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();
```

---

# 23. Drag Using Offset

```java
actions.clickAndHold(element)
       .moveByOffset(200, 0)
       .release()
       .perform();
```

This is useful for sliders.

---

# 24. Slider Example

Suppose a slider starts at:

```text
|--------------------|
^
Start
```

Move it 100 pixels:

```java
WebElement slider =
        driver.findElement(By.id("slider"));

actions.clickAndHold(slider)
       .moveByOffset(100, 0)
       .release()
       .perform();
```

---

# 25. Mouse Movement Sequence

You can chain several mouse actions:

```java
actions.moveToElement(element1)
       .click()
       .moveToElement(element2)
       .click()
       .perform();
```

Flow:

```text
Move → Click
       ↓
Move → Click
```

---

# 26. Hover → Submenu → Click

This is a common real-world scenario.

```java
WebElement mainMenu =
        driver.findElement(By.id("mainMenu"));

WebElement subMenu =
        driver.findElement(By.id("subMenu"));

actions.moveToElement(mainMenu)
       .moveToElement(subMenu)
       .click()
       .perform();
```

---

# 27. Hover → Wait → Click

Sometimes a submenu needs time to appear.

Use an explicit wait:

```java
WebDriverWait wait =
        new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );

actions.moveToElement(mainMenu)
       .perform();

wait.until(
        ExpectedConditions.visibilityOf(subMenu)
);

subMenu.click();
```

Imports:

```java
import java.time.Duration;

import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
```

---

# 28. Mouse Actions with WebElement

The Actions API accepts `WebElement`.

Example:

```java
WebElement element =
        driver.findElement(By.id("test"));

actions.moveToElement(element)
       .click()
       .perform();
```

---

# 29. Mouse Actions with Coordinates

You can use offsets:

```java
actions.moveByOffset(200, 100)
       .click()
       .perform();
```

Be careful with coordinate-based automation because the result can depend on:

* Browser window size
* Screen resolution
* Zoom level
* Scroll position
* Responsive layout

Prefer element-based interactions whenever possible.

---

# 30. Mouse Hover on Image

Example:

```java
WebElement image =
        driver.findElement(By.id("productImage"));

actions.moveToElement(image).perform();
```

A tooltip or additional information may appear.

Then:

```java
WebElement tooltip =
        driver.findElement(By.id("tooltip"));

System.out.println(tooltip.getText());
```

---

# 31. Mouse Hover and Tooltip

Complete example:

```java
WebElement product =
        driver.findElement(By.id("product"));

actions.moveToElement(product)
       .perform();

WebDriverWait wait =
        new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );

WebElement tooltip =
        wait.until(
                ExpectedConditions.visibilityOfElementLocated(
                        By.id("tooltip")
                )
        );

System.out.println(tooltip.getText());
```

---

# 32. Mouse Actions with Page Object Model

Mouse interactions should usually be placed inside the Page Object.

Example:

```java
public void hoverProductsMenu() {

    Actions actions = new Actions(driver);

    actions.moveToElement(productsMenu)
           .perform();
}
```

Then the test remains simple:

```java
homePage.hoverProductsMenu();
```

---

# 33. Reusable Mouse Hover Utility

Utility method:

```java
public static void mouseHover(
        WebDriver driver,
        WebElement element) {

    Actions actions = new Actions(driver);

    actions.moveToElement(element)
           .perform();
}
```

Usage:

```java
SeleniumUtils.mouseHover(driver, menu);
```

---

# 34. Reusable Double Click Utility

```java
public static void doubleClick(
        WebDriver driver,
        WebElement element) {

    Actions actions = new Actions(driver);

    actions.doubleClick(element)
           .perform();
}
```

---

# 35. Reusable Right Click Utility

```java
public static void rightClick(
        WebDriver driver,
        WebElement element) {

    Actions actions = new Actions(driver);

    actions.contextClick(element)
           .perform();
}
```

---

# 36. Reusable Drag and Drop Utility

```java
public static void dragAndDrop(
        WebDriver driver,
        WebElement source,
        WebElement target) {

    Actions actions = new Actions(driver);

    actions.dragAndDrop(source, target)
           .perform();
}
```

---

# 37. Reusable Drag-and-Drop Alternative

```java
public static void dragAndDropUsingMouse(
        WebDriver driver,
        WebElement source,
        WebElement target) {

    Actions actions = new Actions(driver);

    actions.clickAndHold(source)
           .moveToElement(target)
           .release()
           .perform();
}
```

---

# 38. Complete Mouse Actions Example

```java
import java.time.Duration;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

public class MouseActionsExample {

    public static void main(String[] args) {

        WebDriver driver = new ChromeDriver();

        driver.manage().window().maximize();

        driver.get("https://example.com");

        Actions actions = new Actions(driver);

        WebElement menu =
                driver.findElement(By.id("menu"));

        // Mouse hover
        actions.moveToElement(menu)
               .perform();

        // Click
        actions.click(menu)
               .perform();

        // Double click
        actions.doubleClick(menu)
               .perform();

        // Right click
        actions.contextClick(menu)
               .perform();

        driver.quit();
    }
}
```

---

# 39. Complete Drag and Drop Example

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

        actions.dragAndDrop(source, target)
               .perform();

        driver.quit();
    }
}
```

---

# 40. Complete Mouse Hover Example

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;

public class MouseHoverExample {

    public static void main(String[] args) {

        WebDriver driver = new ChromeDriver();

        driver.manage().window().maximize();

        driver.get("https://example.com");

        WebElement menu =
                driver.findElement(By.id("products"));

        WebElement submenu =
                driver.findElement(By.id("laptops"));

        Actions actions = new Actions(driver);

        actions.moveToElement(menu)
               .moveToElement(submenu)
               .click()
               .perform();

        driver.quit();
    }
}
```

---

# 41. Complete Right-Click Example

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;

public class RightClickExample {

    public static void main(String[] args) {

        WebDriver driver = new ChromeDriver();

        driver.manage().window().maximize();

        driver.get("https://example.com");

        WebElement element =
                driver.findElement(By.id("file"));

        Actions actions = new Actions(driver);

        actions.contextClick(element)
               .perform();

        driver.quit();
    }
}
```

---

# 42. Complete Double-Click Example

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;

public class DoubleClickExample {

    public static void main(String[] args) {

        WebDriver driver = new ChromeDriver();

        driver.manage().window().maximize();

        driver.get("https://example.com");

        WebElement element =
                driver.findElement(By.id("row"));

        Actions actions = new Actions(driver);

        actions.doubleClick(element)
               .perform();

        driver.quit();
    }
}
```

---

# 43. Mouse Actions vs Normal Selenium Commands

### Normal click

```java
element.click();
```

### Actions click

```java
actions.click(element).perform();
```

### Mouse hover

```java
actions.moveToElement(element).perform();
```

### Double-click

```java
actions.doubleClick(element).perform();
```

### Right-click

```java
actions.contextClick(element).perform();
```

### Drag and drop

```java
actions.dragAndDrop(source, target).perform();
```

---

# 44. When Should You Use Actions?

Use `Actions` when the test needs:

* Hovering
* Submenus
* Context menus
* Double-click
* Drag and drop
* Sliders
* Click-and-hold
* Mouse movement
* Complex mouse sequences

For simple operations, use simpler Selenium APIs.

Example:

```java
element.click();
```

instead of:

```java
actions.click(element).perform();
```

unless there is a reason to use the Actions API.

---

# 45. Common Mistakes

## Mistake 1: Forgetting perform()

Incorrect:

```java
actions.moveToElement(element);
```

Correct:

```java
actions.moveToElement(element).perform();
```

---

## Mistake 2: Using coordinates unnecessarily

Avoid:

```java
actions.moveByOffset(500, 300)
       .click()
       .perform();
```

when you can use:

```java
actions.click(element).perform();
```

---

## Mistake 3: Not waiting for hover menus

If the submenu takes time to appear, wait for it.

```java
actions.moveToElement(menu).perform();

wait.until(
        ExpectedConditions.visibilityOf(submenu)
);
```

---

## Mistake 4: Assuming dragAndDrop always works

If it fails, try:

```java
actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();
```

---

## Mistake 5: Reusing stale elements

If the page refreshes or DOM changes, locate the element again before performing the mouse action.

---

# 46. Important Methods

| Method            | Purpose                   |
| ----------------- | ------------------------- |
| `click()`         | Left mouse click          |
| `doubleClick()`   | Double click              |
| `contextClick()`  | Right click               |
| `clickAndHold()`  | Click and hold            |
| `release()`       | Release mouse button      |
| `moveToElement()` | Move mouse to element     |
| `moveByOffset()`  | Move mouse by coordinates |
| `dragAndDrop()`   | Drag source to target     |
| `dragAndDropBy()` | Drag by offset            |
| `perform()`       | Execute action            |

---

# 47. Interview Questions

## Q1. What is the Actions class?

The `Actions` class is used to perform advanced mouse interactions such as hover, right-click, double-click, drag-and-drop, and mouse movement.

---

## Q2. How do you perform mouse hover?

```java
Actions actions = new Actions(driver);

actions.moveToElement(element)
       .perform();
```

---

## Q3. How do you perform a double-click?

```java
actions.doubleClick(element)
       .perform();
```

---

## Q4. How do you perform a right-click?

```java
actions.contextClick(element)
       .perform();
```

---

## Q5. How do you perform drag and drop?

```java
actions.dragAndDrop(source, target)
       .perform();
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

## Q7. What is moveByOffset()?

`moveByOffset()` moves the mouse relative to its current position.

```java
actions.moveByOffset(100, 50)
       .perform();
```

---

## Q8. What is moveToElement()?

It moves the mouse pointer to an element.

```java
actions.moveToElement(element)
       .perform();
```

---

## Q9. What is the difference between moveToElement() and moveByOffset()?

```java
moveToElement(element)
```

moves to an element.

```java
moveByOffset(x, y)
```

moves relative to the current mouse position.

---

## Q10. Why is perform() important?

`perform()` executes the action sequence.

Example:

```java
actions.moveToElement(element)
       .click()
       .perform();
```

---

## Q11. Can Actions perform multiple mouse operations?

Yes.

```java
actions.moveToElement(menu)
       .click()
       .moveToElement(submenu)
       .click()
       .perform();
```

---

## Q12. How do you perform a slider movement?

```java
actions.clickAndHold(slider)
       .moveByOffset(100, 0)
       .release()
       .perform();
```

---

# 48. Quick Reference

```java
Actions actions = new Actions(driver);

// Click
actions.click(element).perform();

// Double click
actions.doubleClick(element).perform();

// Right click
actions.contextClick(element).perform();

// Hover
actions.moveToElement(element).perform();

// Move by offset
actions.moveByOffset(100, 50).perform();

// Click and hold
actions.clickAndHold(element).perform();

// Release
actions.release().perform();

// Drag and drop
actions.dragAndDrop(source, target).perform();

// Drag by offset
actions.dragAndDropBy(element, 100, 50).perform();

// Manual drag and drop
actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();
```

---

# 49. Mouse Actions Workflow

```text
Create Actions object
        ↓
Actions actions = new Actions(driver)
        ↓
Locate WebElement
        ↓
Choose mouse action
        ↓
moveToElement()
click()
doubleClick()
contextClick()
clickAndHold()
dragAndDrop()
        ↓
Chain actions if required
        ↓
perform()
        ↓
Validate result
```

---

# 50. Key Takeaway

The most important Selenium mouse actions are:

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
perform()
```

The most common mouse-hover pattern is:

```java
Actions actions = new Actions(driver);

actions.moveToElement(menu)
       .moveToElement(submenu)
       .click()
       .perform();
```

The most common drag-and-drop pattern is:

```java
actions.dragAndDrop(source, target)
       .perform();
```

If that does not work:

```java
actions.clickAndHold(source)
       .moveToElement(target)
       .release()
       .perform();
```

For reliable automation, prefer **element-based mouse actions** over screen coordinates whenever possible.

---

# 51. Selenium Study Progress

```text
Selenium-Basics
WebElements
Alerts
Windows
Frames
Tabs
Actions
Mouse Actions                ← Current
```

Next recommended topic:

```text
Keyboard/Selenium-Keyboard-Actions.md
```
