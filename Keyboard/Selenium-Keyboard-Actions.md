# Selenium Keyboard Actions

## 1. Introduction

Selenium WebDriver provides the `Actions` class to simulate keyboard interactions.

Keyboard actions are useful when automation needs to perform operations such as:

* Entering text
* Pressing Enter
* Pressing Tab
* Pressing Escape
* Selecting all text
* Copy and paste
* Cut and paste
* Using Shift
* Using Ctrl
* Using Alt
* Using function keys
* Navigating with arrow keys
* Chaining keyboard and mouse actions
* Performing keyboard shortcuts

Import:

```java
import org.openqa.selenium.Keys;
import org.openqa.selenium.interactions.Actions;
```

Create an `Actions` object:

```java
Actions actions = new Actions(driver);
```

---

# 2. Actions Class for Keyboard

The main Selenium class used for advanced keyboard interactions is:

```java
Actions
```

Example:

```java
Actions actions = new Actions(driver);

actions.sendKeys(Keys.ENTER).perform();
```

The general pattern is:

```java
actions
    .keyboardAction()
    .perform();
```

---

# 3. sendKeys()

The `sendKeys()` method sends keyboard input.

Example:

```java
actions.sendKeys("Hello").perform();
```

You can also send special keys:

```java
actions.sendKeys(Keys.ENTER).perform();
```

---

# 4. sendKeys() to a WebElement

You can send keys directly to an element:

```java
actions.sendKeys(element, "Hello").perform();
```

Example:

```java
WebElement username =
        driver.findElement(By.id("username"));

Actions actions = new Actions(driver);

actions.sendKeys(username, "Selva").perform();
```

For normal text input, this is also commonly used:

```java
username.sendKeys("Selva");
```

---

# 5. Press ENTER

Use:

```java
actions.sendKeys(Keys.ENTER).perform();
```

Example:

```java
WebElement search =
        driver.findElement(By.id("search"));

search.sendKeys("Selenium");

actions.sendKeys(Keys.ENTER).perform();
```

You can also send Enter directly:

```java
search.sendKeys(Keys.ENTER);
```

---

# 6. ENTER vs RETURN

Selenium provides:

```java
Keys.ENTER
```

and:

```java
Keys.RETURN
```

Both are commonly used to submit or confirm an action.

Example:

```java
actions.sendKeys(Keys.ENTER).perform();
```

or:

```java
actions.sendKeys(Keys.RETURN).perform();
```

---

# 7. Press TAB

Use:

```java
actions.sendKeys(Keys.TAB).perform();
```

Example:

```java
WebElement username =
        driver.findElement(By.id("username"));

username.sendKeys("Selva");

actions.sendKeys(Keys.TAB).perform();
```

The focus moves to the next focusable element.

---

# 8. Multiple TAB Operations

You can press Tab multiple times:

```java
actions.sendKeys(Keys.TAB)
       .sendKeys(Keys.TAB)
       .sendKeys(Keys.TAB)
       .perform();
```

Or:

```java
actions.sendKeys(Keys.TAB, Keys.TAB, Keys.TAB)
       .perform();
```

---

# 9. Press ESCAPE

Use:

```java
actions.sendKeys(Keys.ESCAPE).perform();
```

Useful for:

* Closing menus
* Closing popups
* Closing dialogs
* Canceling an operation

Example:

```java
actions.sendKeys(Keys.ESCAPE).perform();
```

---

# 10. Press SPACE

Use:

```java
actions.sendKeys(Keys.SPACE).perform();
```

Example:

```java
actions.sendKeys(Keys.SPACE).perform();
```

Useful when interacting with:

* Checkboxes
* Buttons
* Custom controls

---

# 11. Arrow Keys

Selenium provides:

```java
Keys.ARROW_UP
Keys.ARROW_DOWN
Keys.ARROW_LEFT
Keys.ARROW_RIGHT
```

Example:

```java
actions.sendKeys(Keys.ARROW_DOWN).perform();
```

---

# 12. Multiple Arrow Keys

```java
actions.sendKeys(
        Keys.ARROW_DOWN,
        Keys.ARROW_DOWN,
        Keys.ARROW_DOWN
).perform();
```

---

# 13. HOME Key

Use:

```java
actions.sendKeys(Keys.HOME).perform();
```

This can move the cursor to the beginning of a text field or move to the top of a page depending on the focused element.

---

# 14. END Key

Use:

```java
actions.sendKeys(Keys.END).perform();
```

This can move the cursor to the end of a text field or move to the bottom of a page depending on the focused element.

---

# 15. PAGE UP

Use:

```java
actions.sendKeys(Keys.PAGE_UP).perform();
```

---

# 16. PAGE DOWN

Use:

```java
actions.sendKeys(Keys.PAGE_DOWN).perform();
```

---

# 17. BACKSPACE

Use:

```java
actions.sendKeys(Keys.BACK_SPACE).perform();
```

Example:

```java
WebElement input =
        driver.findElement(By.id("username"));

input.click();

actions.sendKeys(Keys.BACK_SPACE).perform();
```

---

# 18. DELETE

Use:

```java
actions.sendKeys(Keys.DELETE).perform();
```

Example:

```java
actions.sendKeys(Keys.DELETE).perform();
```

---

# 19. Select All - CTRL + A

On Windows/Linux:

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();
```

This performs:

```text
Press CTRL
   ↓
Press A
   ↓
Release CTRL
```

---

# 20. Copy - CTRL + C

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("c")
       .keyUp(Keys.CONTROL)
       .perform();
```

---

# 21. Paste - CTRL + V

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("v")
       .keyUp(Keys.CONTROL)
       .perform();
```

---

# 22. Cut - CTRL + X

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("x")
       .keyUp(Keys.CONTROL)
       .perform();
```

---

# 23. Undo - CTRL + Z

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("z")
       .keyUp(Keys.CONTROL)
       .perform();
```

---

# 24. Redo - CTRL + Y

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("y")
       .keyUp(Keys.CONTROL)
       .perform();
```

Some applications may use:

```java
CTRL + SHIFT + Z
```

for redo.

---

# 25. Save - CTRL + S

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("s")
       .keyUp(Keys.CONTROL)
       .perform();
```

Be aware that browser-level shortcuts may behave differently depending on browser and application behavior.

---

# 26. Find - CTRL + F

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("f")
       .keyUp(Keys.CONTROL)
       .perform();
```

This may open the browser's Find UI rather than an application control.

---

# 27. Refresh - CTRL + R

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("r")
       .keyUp(Keys.CONTROL)
       .perform();
```

For browser-level navigation, prefer:

```java
driver.navigate().refresh();
```

when refreshing the page is the actual test requirement.

---

# 28. SHIFT Key

Use:

```java
actions.keyDown(Keys.SHIFT)
       .sendKeys("hello")
       .keyUp(Keys.SHIFT)
       .perform();
```

The `SHIFT` key can be used for:

* Selecting ranges
* Uppercase text
* Keyboard shortcuts
* Multiple selections

---

# 29. SHIFT + Click

Example:

```java
actions.keyDown(Keys.SHIFT)
       .click(element)
       .keyUp(Keys.SHIFT)
       .perform();
```

Useful for selecting a range of items.

---

# 30. CTRL + Click

Useful for selecting multiple independent items.

```java
actions.keyDown(Keys.CONTROL)
       .click(element1)
       .click(element2)
       .click(element3)
       .keyUp(Keys.CONTROL)
       .perform();
```

---

# 31. ALT Key

Use:

```java
actions.keyDown(Keys.ALT)
       .sendKeys("x")
       .keyUp(Keys.ALT)
       .perform();
```

ALT-based shortcuts are highly application/browser dependent.

---

# 32. Function Keys

Selenium supports function keys:

```java
Keys.F1
Keys.F2
Keys.F3
Keys.F4
Keys.F5
Keys.F6
Keys.F7
Keys.F8
Keys.F9
Keys.F10
Keys.F11
Keys.F12
```

Example:

```java
actions.sendKeys(Keys.F5).perform();
```

For browser refresh, prefer:

```java
driver.navigate().refresh();
```

when possible.

---

# 33. keyDown()

`keyDown()` presses and holds a key.

Example:

```java
actions.keyDown(Keys.CONTROL).perform();
```

The key remains logically pressed until released.

Usually it is paired with:

```java
keyUp()
```

---

# 34. keyUp()

`keyUp()` releases a previously pressed key.

Example:

```java
actions.keyUp(Keys.CONTROL).perform();
```

Typical pattern:

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();
```

---

# 35. keyDown() + keyUp()

The standard pattern is:

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();
```

Think of it as:

```text
keyDown()
   ↓
Press shortcut key
   ↓
Perform another key
   ↓
keyUp()
```

---

# 36. Using Modifier Keys

Modifier keys include:

```java
Keys.CONTROL
Keys.SHIFT
Keys.ALT
Keys.COMMAND
```

Example:

```java
actions.keyDown(Keys.CONTROL)
       .click(element)
       .keyUp(Keys.CONTROL)
       .perform();
```

---

# 37. COMMAND Key on macOS

For macOS, keyboard shortcuts commonly use:

```java
Keys.COMMAND
```

Example:

```java
actions.keyDown(Keys.COMMAND)
       .sendKeys("a")
       .keyUp(Keys.COMMAND)
       .perform();
```

For Windows/Linux, use:

```java
Keys.CONTROL
```

---

# 38. CTRL + A Example

```java
WebElement input =
        driver.findElement(By.id("username"));

input.click();

Actions actions = new Actions(driver);

actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();

input.sendKeys("NewValue");
```

---

# 39. CTRL + A + Type

You can chain everything:

```java
actions.click(input)
       .keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .sendKeys("NewValue")
       .perform();
```

Flow:

```text
Click input
   ↓
CTRL down
   ↓
A
   ↓
CTRL up
   ↓
Type NewValue
```

---

# 40. CTRL + C and CTRL + V

Example:

```java
WebElement source =
        driver.findElement(By.id("source"));

WebElement target =
        driver.findElement(By.id("target"));

Actions actions = new Actions(driver);

actions.click(source)
       .keyDown(Keys.CONTROL)
       .sendKeys("a")
       .sendKeys("c")
       .keyUp(Keys.CONTROL)
       .click(target)
       .keyDown(Keys.CONTROL)
       .sendKeys("v")
       .keyUp(Keys.CONTROL)
       .perform();
```

This simulates:

```text
Select source text
      ↓
Copy
      ↓
Click target
      ↓
Paste
```

---

# 41. Keys.chord()

Selenium also provides:

```java
Keys.chord()
```

It is useful for creating key combinations.

Example:

```java
String selectAll =
        Keys.chord(Keys.CONTROL, "a");
```

Then:

```java
element.sendKeys(selectAll);
```

---

# 42. CTRL + A Using Keys.chord()

```java
WebElement input =
        driver.findElement(By.id("username"));

input.sendKeys(
        Keys.chord(Keys.CONTROL, "a")
);

input.sendKeys("Selva");
```

---

# 43. CTRL + C Using Keys.chord()

```java
String copy =
        Keys.chord(Keys.CONTROL, "c");

element.sendKeys(copy);
```

---

# 44. CTRL + V Using Keys.chord()

```java
String paste =
        Keys.chord(Keys.CONTROL, "v");

element.sendKeys(paste);
```

---

# 45. Actions vs Keys.chord()

### Actions

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();
```

### Keys.chord()

```java
element.sendKeys(
        Keys.chord(Keys.CONTROL, "a")
);
```

Use `Actions` when you need a sequence of keyboard and mouse interactions.

Use `Keys.chord()` when you need a simple key combination.

---

# 46. TAB Navigation

Suppose a form contains:

```text
Username
Password
Login
```

You can navigate using Tab:

```java
WebElement username =
        driver.findElement(By.id("username"));

username.sendKeys("Selva");

actions.sendKeys(Keys.TAB)
       .sendKeys("Password123")
       .sendKeys(Keys.TAB)
       .sendKeys(Keys.ENTER)
       .perform();
```

Flow:

```text
Username
   ↓ TAB
Password
   ↓ TAB
Login
   ↓ ENTER
```

---

# 47. Keyboard Navigation Without Locating Every Element

You can sometimes navigate a form entirely using keyboard actions:

```java
actions.sendKeys("Selva")
       .sendKeys(Keys.TAB)
       .sendKeys("Password123")
       .sendKeys(Keys.TAB)
       .sendKeys(Keys.ENTER)
       .perform();
```

This is useful for testing keyboard accessibility.

However, the tab order must be stable and predictable.

---

# 48. Arrow Key Navigation

Example:

```java
WebElement dropdown =
        driver.findElement(By.id("country"));

dropdown.click();

actions.sendKeys(Keys.ARROW_DOWN)
       .sendKeys(Keys.ARROW_DOWN)
       .sendKeys(Keys.ENTER)
       .perform();
```

This can be useful for custom dropdown controls.

---

# 49. Escape Key for Popup

Example:

```java
WebElement popup =
        driver.findElement(By.id("popup"));

actions.sendKeys(Keys.ESCAPE)
       .perform();
```

You can then validate that the popup is no longer displayed.

---

# 50. Space Key for Checkbox

For a focused checkbox:

```java
actions.sendKeys(Keys.SPACE).perform();
```

Example:

```java
WebElement checkbox =
        driver.findElement(By.id("terms"));

checkbox.click();

actions.sendKeys(Keys.SPACE)
       .perform();
```

For a normal checkbox, `click()` is generally simpler.

---

# 51. HOME and END in Text Fields

Example:

```java
WebElement input =
        driver.findElement(By.id("username"));

input.click();

actions.sendKeys(Keys.HOME)
       .perform();
```

Moves the cursor to the beginning.

To move to the end:

```java
actions.sendKeys(Keys.END).perform();
```

---

# 52. Page Navigation

Page-level navigation can use:

```java
actions.sendKeys(Keys.PAGE_DOWN)
       .perform();
```

and:

```java
actions.sendKeys(Keys.PAGE_UP)
       .perform();
```

However, for deterministic scrolling, Selenium's JavaScript or element scrolling APIs may sometimes be more appropriate.

---

# 53. Combining Mouse and Keyboard

One of the strongest features of `Actions` is combining mouse and keyboard interactions.

Example:

```java
actions.keyDown(Keys.CONTROL)
       .click(element1)
       .click(element2)
       .keyUp(Keys.CONTROL)
       .perform();
```

Another:

```java
actions.moveToElement(menu)
       .click()
       .sendKeys(Keys.ARROW_DOWN)
       .sendKeys(Keys.ENTER)
       .perform();
```

---

# 54. Complete Keyboard Example

```java
import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;

public class KeyboardActionsExample {

    public static void main(String[] args) {

        WebDriver driver = new ChromeDriver();

        driver.manage().window().maximize();

        driver.get("https://example.com");

        WebElement input =
                driver.findElement(By.id("username"));

        Actions actions = new Actions(driver);

        // Enter text
        actions.sendKeys(input, "Selva")
               .perform();

        // Select all
        actions.keyDown(Keys.CONTROL)
               .sendKeys("a")
               .keyUp(Keys.CONTROL)
               .perform();

        // Replace text
        actions.sendKeys("NewValue")
               .perform();

        // Press TAB
        actions.sendKeys(Keys.TAB)
               .perform();

        // Press ENTER
        actions.sendKeys(Keys.ENTER)
               .perform();

        driver.quit();
    }
}
```

---

# 55. Complete CTRL+A Example

```java
import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;

public class SelectAllExample {

    public static void main(String[] args) {

        WebDriver driver = new ChromeDriver();

        driver.get("https://example.com");

        WebElement input =
                driver.findElement(By.id("username"));

        Actions actions = new Actions(driver);

        input.click();

        actions.keyDown(Keys.CONTROL)
               .sendKeys("a")
               .keyUp(Keys.CONTROL)
               .perform();

        input.sendKeys("UpdatedValue");

        driver.quit();
    }
}
```

---

# 56. Complete Keyboard Navigation Example

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.Keys;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;

public class KeyboardNavigationExample {

    public static void main(String[] args) {

        WebDriver driver = new ChromeDriver();

        driver.manage().window().maximize();

        driver.get("https://example.com");

        WebElement username =
                driver.findElement(By.id("username"));

        Actions actions = new Actions(driver);

        username.sendKeys("Selva");

        actions.sendKeys(Keys.TAB)
               .sendKeys("Password123")
               .sendKeys(Keys.TAB)
               .sendKeys(Keys.ENTER)
               .perform();

        driver.quit();
    }
}
```

---

# 57. Complete Copy and Paste Example

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.Keys;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;

public class CopyPasteExample {

    public static void main(String[] args) {

        WebDriver driver = new ChromeDriver();

        driver.get("https://example.com");

        WebElement source =
                driver.findElement(By.id("source"));

        WebElement target =
                driver.findElement(By.id("target"));

        Actions actions = new Actions(driver);

        source.click();

        actions.keyDown(Keys.CONTROL)
               .sendKeys("a")
               .sendKeys("c")
               .keyUp(Keys.CONTROL)
               .perform();

        target.click();

        actions.keyDown(Keys.CONTROL)
               .sendKeys("v")
               .keyUp(Keys.CONTROL)
               .perform();

        driver.quit();
    }
}
```

---

# 58. Complete SHIFT Selection Example

```java
WebElement first =
        driver.findElement(By.id("first"));

WebElement last =
        driver.findElement(By.id("last"));

Actions actions = new Actions(driver);

first.click();

actions.keyDown(Keys.SHIFT)
       .click(last)
       .keyUp(Keys.SHIFT)
       .perform();
```

This simulates:

```text
Click first item
      ↓
Hold SHIFT
      ↓
Click last item
      ↓
Release SHIFT
```

---

# 59. Complete CTRL Selection Example

```java
WebElement item1 =
        driver.findElement(By.id("item1"));

WebElement item2 =
        driver.findElement(By.id("item2"));

WebElement item3 =
        driver.findElement(By.id("item3"));

Actions actions = new Actions(driver);

actions.keyDown(Keys.CONTROL)
       .click(item1)
       .click(item2)
       .click(item3)
       .keyUp(Keys.CONTROL)
       .perform();
```

---

# 60. Keyboard Actions with Page Object Model

Example:

```java
public void selectAllUsername() {

    Actions actions = new Actions(driver);

    actions.click(username)
           .keyDown(Keys.CONTROL)
           .sendKeys("a")
           .keyUp(Keys.CONTROL)
           .perform();
}
```

Test:

```java
loginPage.selectAllUsername();
```

---

# 61. Keyboard Utility Methods

A reusable utility method:

```java
public static void pressEnter(
        WebDriver driver) {

    Actions actions = new Actions(driver);

    actions.sendKeys(Keys.ENTER)
           .perform();
}
```

Usage:

```java
SeleniumUtils.pressEnter(driver);
```

---

# 62. Press Tab Utility

```java
public static void pressTab(
        WebDriver driver) {

    Actions actions = new Actions(driver);

    actions.sendKeys(Keys.TAB)
           .perform();
}
```

---

# 63. Escape Utility

```java
public static void pressEscape(
        WebDriver driver) {

    Actions actions = new Actions(driver);

    actions.sendKeys(Keys.ESCAPE)
           .perform();
}
```

---

# 64. Select All Utility

```java
public static void selectAll(
        WebDriver driver) {

    Actions actions = new Actions(driver);

    actions.keyDown(Keys.CONTROL)
           .sendKeys("a")
           .keyUp(Keys.CONTROL)
           .perform();
}
```

---

# 65. Common Keyboard Keys

| Selenium Key         | Purpose       |
| -------------------- | ------------- |
| `Keys.ENTER`         | Enter         |
| `Keys.RETURN`        | Return        |
| `Keys.TAB`           | Tab           |
| `Keys.ESCAPE`        | Escape        |
| `Keys.SPACE`         | Space         |
| `Keys.BACK_SPACE`    | Backspace     |
| `Keys.DELETE`        | Delete        |
| `Keys.HOME`          | Home          |
| `Keys.END`           | End           |
| `Keys.PAGE_UP`       | Page Up       |
| `Keys.PAGE_DOWN`     | Page Down     |
| `Keys.ARROW_UP`      | Up arrow      |
| `Keys.ARROW_DOWN`    | Down arrow    |
| `Keys.ARROW_LEFT`    | Left arrow    |
| `Keys.ARROW_RIGHT`   | Right arrow   |
| `Keys.SHIFT`         | Shift         |
| `Keys.CONTROL`       | Ctrl          |
| `Keys.ALT`           | Alt           |
| `Keys.COMMAND`       | Command       |
| `Keys.F1`–`Keys.F12` | Function keys |

---

# 66. Important Keyboard Shortcuts

| Shortcut        | Purpose               |
| --------------- | --------------------- |
| `CTRL + A`      | Select all            |
| `CTRL + C`      | Copy                  |
| `CTRL + V`      | Paste                 |
| `CTRL + X`      | Cut                   |
| `CTRL + Z`      | Undo                  |
| `CTRL + Y`      | Redo                  |
| `CTRL + S`      | Save                  |
| `CTRL + F`      | Find                  |
| `CTRL + R`      | Refresh               |
| `SHIFT + Click` | Select range          |
| `CTRL + Click`  | Select multiple items |

---

# 67. Actions vs WebElement.sendKeys()

### WebElement.sendKeys()

Best for typing into a specific element:

```java
username.sendKeys("Selva");
```

### Actions.sendKeys()

Useful for:

* Global keyboard events
* Keyboard navigation
* Key combinations
* Chained keyboard and mouse actions

Example:

```java
actions.sendKeys(Keys.TAB)
       .sendKeys(Keys.ENTER)
       .perform();
```

---

# 68. Actions vs JavaScript

Keyboard actions should normally use Selenium's native keyboard APIs.

Prefer:

```java
actions.sendKeys(Keys.ENTER).perform();
```

rather than trying to simulate keyboard behavior using JavaScript.

JavaScript can manipulate the DOM, but it does not always reproduce the same browser/user interaction behavior as a real WebDriver keyboard action.

---

# 69. Common Mistakes

## Mistake 1: Forgetting perform()

Incorrect:

```java
actions.sendKeys(Keys.ENTER);
```

Correct:

```java
actions.sendKeys(Keys.ENTER)
       .perform();
```

---

## Mistake 2: Forgetting keyUp()

Incorrect:

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .perform();
```

Better:

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();
```

---

## Mistake 3: Using CTRL on Mac

Windows/Linux:

```java
Keys.CONTROL
```

macOS:

```java
Keys.COMMAND
```

---

## Mistake 4: Using keyboard navigation when tab order is unstable

Keyboard-only automation depends on the application's focus order.

If the focus order changes, the test may become unreliable.

---

## Mistake 5: Using browser shortcuts unnecessarily

For example, instead of:

```java
actions.sendKeys(Keys.F5).perform();
```

prefer:

```java
driver.navigate().refresh();
```

when the test simply needs to refresh the page.

---

# 70. Accessibility Testing

Keyboard automation is especially useful for accessibility testing.

You can verify that users can navigate the application using:

```java
Keys.TAB
Keys.SHIFT
Keys.ENTER
Keys.SPACE
Keys.ARROW_UP
Keys.ARROW_DOWN
Keys.ESCAPE
```

Example:

```java
actions.sendKeys(Keys.TAB)
       .sendKeys(Keys.TAB)
       .sendKeys(Keys.ENTER)
       .perform();
```

You can validate:

* Focus movement
* Keyboard-only navigation
* Button activation
* Menu navigation
* Dialog closing
* Form navigation

---

# 71. Keyboard Focus

The browser maintains a current focused element.

You can inspect the active element with JavaScript:

```java
JavascriptExecutor js =
        (JavascriptExecutor) driver;

WebElement activeElement =
        (WebElement) js.executeScript(
                "return document.activeElement;"
        );
```

Then:

```java
System.out.println(
        activeElement.getAttribute("id")
);
```

This can help debug keyboard-navigation issues.

---

# 72. Keyboard and Dropdowns

For a native HTML `<select>`, Selenium's `Select` class is usually preferable.

For custom dropdowns, keyboard navigation may be useful:

```java
dropdown.click();

actions.sendKeys(Keys.ARROW_DOWN)
       .sendKeys(Keys.ARROW_DOWN)
       .sendKeys(Keys.ENTER)
       .perform();
```

---

# 73. Keyboard and Auto-Complete

Example:

```java
WebElement search =
        driver.findElement(By.id("search"));

search.sendKeys("Toyota");

actions.sendKeys(Keys.ARROW_DOWN)
       .sendKeys(Keys.ENTER)
       .perform();
```

This is common for autocomplete fields.

---

# 74. Keyboard and Modal Dialog

Example:

```java
actions.sendKeys(Keys.ESCAPE)
       .perform();
```

Then validate:

```java
Assert.assertFalse(
        modal.isDisplayed()
);
```

---

# 75. Keyboard and Search

Example:

```java
WebElement search =
        driver.findElement(By.id("search"));

search.sendKeys("Selenium");

actions.sendKeys(Keys.ENTER)
       .perform();
```

---

# 76. Keyboard Action Workflow

```text
Create Actions object
        ↓
Locate/focus element
        ↓
Choose keyboard action
        ↓
sendKeys()
keyDown()
keyUp()
        ↓
Chain actions
        ↓
perform()
        ↓
Validate result
```

---

# 77. Interview Questions

## Q1. How do you press Enter using Selenium?

```java
actions.sendKeys(Keys.ENTER)
       .perform();
```

---

## Q2. How do you press Tab?

```java
actions.sendKeys(Keys.TAB)
       .perform();
```

---

## Q3. How do you press Escape?

```java
actions.sendKeys(Keys.ESCAPE)
       .perform();
```

---

## Q4. How do you perform CTRL+A?

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();
```

---

## Q5. How do you perform CTRL+C?

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("c")
       .keyUp(Keys.CONTROL)
       .perform();
```

---

## Q6. How do you perform CTRL+V?

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("v")
       .keyUp(Keys.CONTROL)
       .perform();
```

---

## Q7. What is keyDown()?

`keyDown()` presses and holds a keyboard key.

```java
actions.keyDown(Keys.CONTROL)
       .perform();
```

---

## Q8. What is keyUp()?

`keyUp()` releases a previously pressed key.

```java
actions.keyUp(Keys.CONTROL)
       .perform();
```

---

## Q9. What is Keys.chord()?

`Keys.chord()` creates a combination of keys.

Example:

```java
element.sendKeys(
        Keys.chord(Keys.CONTROL, "a")
);
```

---

## Q10. What is the difference between Actions.sendKeys() and WebElement.sendKeys()?

`WebElement.sendKeys()` sends text or keys to a specific element.

```java
element.sendKeys("Hello");
```

`Actions.sendKeys()` is useful for keyboard events and combinations, especially when combined with other actions.

```java
actions.sendKeys(Keys.TAB)
       .sendKeys(Keys.ENTER)
       .perform();
```

---

## Q11. How do you perform keyboard navigation?

Example:

```java
actions.sendKeys(Keys.TAB)
       .sendKeys(Keys.TAB)
       .sendKeys(Keys.ENTER)
       .perform();
```

---

## Q12. How do you select multiple elements with CTRL?

```java
actions.keyDown(Keys.CONTROL)
       .click(element1)
       .click(element2)
       .keyUp(Keys.CONTROL)
       .perform();
```

---

## Q13. How do you select a range using SHIFT?

```java
actions.keyDown(Keys.SHIFT)
       .click(element2)
       .keyUp(Keys.SHIFT)
       .perform();
```

---

## Q14. What key should be used for shortcuts on macOS?

Use:

```java
Keys.COMMAND
```

instead of:

```java
Keys.CONTROL
```

for common Mac command-key shortcuts.

---

# 78. Quick Reference

```java
Actions actions = new Actions(driver);

// Text
actions.sendKeys("Hello").perform();

// Enter
actions.sendKeys(Keys.ENTER).perform();

// Tab
actions.sendKeys(Keys.TAB).perform();

// Escape
actions.sendKeys(Keys.ESCAPE).perform();

// Space
actions.sendKeys(Keys.SPACE).perform();

// Backspace
actions.sendKeys(Keys.BACK_SPACE).perform();

// Delete
actions.sendKeys(Keys.DELETE).perform();

// Arrow
actions.sendKeys(Keys.ARROW_DOWN).perform();

// Home
actions.sendKeys(Keys.HOME).perform();

// End
actions.sendKeys(Keys.END).perform();

// Page Down
actions.sendKeys(Keys.PAGE_DOWN).perform();

// Page Up
actions.sendKeys(Keys.PAGE_UP).perform();

// CTRL + A
actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();

// CTRL + C
actions.keyDown(Keys.CONTROL)
       .sendKeys("c")
       .keyUp(Keys.CONTROL)
       .perform();

// CTRL + V
actions.keyDown(Keys.CONTROL)
       .sendKeys("v")
       .keyUp(Keys.CONTROL)
       .perform();

// CTRL + X
actions.keyDown(Keys.CONTROL)
       .sendKeys("x")
       .keyUp(Keys.CONTROL)
       .perform();

// SHIFT + Click
actions.keyDown(Keys.SHIFT)
       .click(element)
       .keyUp(Keys.SHIFT)
       .perform();

// CTRL + Click
actions.keyDown(Keys.CONTROL)
       .click(element)
       .keyUp(Keys.CONTROL)
       .perform();
```

---

# 79. Key Takeaway

The most important Selenium keyboard APIs are:

```java
sendKeys()
keyDown()
keyUp()
```

Combined with:

```java
Keys.ENTER
Keys.TAB
Keys.ESCAPE
Keys.SPACE
Keys.BACK_SPACE
Keys.DELETE
Keys.HOME
Keys.END
Keys.PAGE_UP
Keys.PAGE_DOWN
Keys.ARROW_UP
Keys.ARROW_DOWN
Keys.ARROW_LEFT
Keys.ARROW_RIGHT
Keys.CONTROL
Keys.SHIFT
Keys.ALT
Keys.COMMAND
```

The most important interview pattern is:

```java
actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();
```

And for keyboard navigation:

```java
actions.sendKeys(Keys.TAB)
       .sendKeys(Keys.TAB)
       .sendKeys(Keys.ENTER)
       .perform();
```

---

# 80. Selenium Study Progress

```text
Selenium-Basics
WebElements
Alerts
Windows
Frames
Tabs
Actions
Mouse Actions
Keyboard Actions             ← Current
```

Next recommended topic:

```text
Waits/Selenium-Waits.md
```

The next topic should cover **Implicit Wait, Explicit Wait, Fluent Wait, ExpectedConditions, WebDriverWait, polling, timeout handling, and common synchronization problems**.
