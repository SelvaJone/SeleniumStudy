# Selenium Alerts

## 1. What is an Alert?

An alert is a browser dialog displayed by a web application to provide information, request confirmation, or accept user input.

Selenium cannot interact with a browser alert using normal `WebElement` methods.

We must switch the WebDriver's focus to the alert first.

```java
Alert alert = driver.switchTo().alert();
```

Import:

```java
import org.openqa.selenium.Alert;
```

---

# 2. Types of JavaScript Alerts

There are three common JavaScript browser dialogs:

```text
1. Simple Alert
2. Confirmation Alert
3. Prompt Alert
```

---

# 3. Simple Alert

A simple alert displays a message and usually has an **OK** button.

Example:

```text
+---------------------------+
|       Alert               |
|                           |
|   Data saved successfully |
|                           |
|              [ OK ]       |
+---------------------------+
```

HTML/JavaScript example:

```html
<script>
    alert("Data saved successfully");
</script>
```

---

# 4. Accept Simple Alert

First switch to the alert:

```java
Alert alert = driver.switchTo().alert();
```

Then click OK:

```java
alert.accept();
```

Complete example:

```java
Alert alert = driver.switchTo().alert();

alert.accept();
```

---

# 5. Get Alert Text

Use:

```java
String message = alert.getText();
```

Example:

```java
Alert alert = driver.switchTo().alert();

String message = alert.getText();

System.out.println("Alert Message: " + message);

alert.accept();
```

---

# 6. Simple Alert Example

```java
import java.time.Duration;

import org.openqa.selenium.Alert;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class SimpleAlertExample {

    public static void main(String[] args) {

        WebDriver driver = new ChromeDriver();

        try {

            driver.manage().window().maximize();

            driver.manage().timeouts()
                    .implicitlyWait(Duration.ofSeconds(10));

            driver.get("https://example.com");

            // Perform action that opens alert
            // driver.findElement(By.id("alertButton")).click();

            Alert alert = driver.switchTo().alert();

            String message = alert.getText();

            System.out.println(
                    "Alert Message: " + message
            );

            alert.accept();

        } finally {

            driver.quit();
        }
    }
}
```

---

# 7. Confirmation Alert

A confirmation alert normally contains:

```text
OK
Cancel
```

Example:

```text
+-----------------------------+
|       Confirm Action        |
|                             |
|   Do you want to continue?  |
|                             |
|       [OK]      [Cancel]    |
+-----------------------------+
```

JavaScript:

```javascript
confirm("Do you want to continue?");
```

---

# 8. Accept Confirmation Alert

Use:

```java
Alert alert = driver.switchTo().alert();

alert.accept();
```

`accept()` clicks the **OK** button.

---

# 9. Dismiss Confirmation Alert

Use:

```java
Alert alert = driver.switchTo().alert();

alert.dismiss();
```

`dismiss()` clicks the **Cancel** button.

---

# 10. Confirmation Alert Example

```java
Alert alert = driver.switchTo().alert();

String message = alert.getText();

System.out.println(message);

alert.accept();
```

For Cancel:

```java
Alert alert = driver.switchTo().alert();

alert.dismiss();
```

---

# 11. Prompt Alert

A prompt alert allows the user to enter text.

Example:

```text
+-----------------------------+
|       Enter Name            |
|                             |
|   [____________________]    |
|                             |
|       [OK]      [Cancel]    |
+-----------------------------+
```

JavaScript:

```javascript
prompt("Enter your name:");
```

---

# 12. Enter Text in Prompt Alert

Use:

```java
Alert alert = driver.switchTo().alert();

alert.sendKeys("Selva");
```

Then accept:

```java
alert.accept();
```

Complete:

```java
Alert alert = driver.switchTo().alert();

alert.sendKeys("Selva");

alert.accept();
```

---

# 13. Prompt Alert Example

```java
Alert alert = driver.switchTo().alert();

String message = alert.getText();

System.out.println(
        "Prompt Message: " + message
);

alert.sendKeys("Selva");

alert.accept();
```

---

# 14. Alert Interface

Selenium provides the:

```java
org.openqa.selenium.Alert
```

interface.

Important methods:

```java
accept()
dismiss()
getText()
sendKeys()
```

---

# 15. Alert Methods

## accept()

Clicks OK.

```java
alert.accept();
```

---

## dismiss()

Clicks Cancel.

```java
alert.dismiss();
```

---

## getText()

Gets alert message.

```java
String text = alert.getText();
```

---

## sendKeys()

Enters text into a prompt alert.

```java
alert.sendKeys("Selva");
```

---

# 16. Alert Method Summary

| Method       | Purpose                 |
| ------------ | ----------------------- |
| `accept()`   | Clicks OK               |
| `dismiss()`  | Clicks Cancel           |
| `getText()`  | Gets alert message      |
| `sendKeys()` | Enters text into prompt |

---

# 17. Important: Switch to Alert First

This will not work correctly if the browser currently has an alert open:

```java
driver.findElement(By.id("button")).click();
```

After opening the alert, switch to it:

```java
Alert alert = driver.switchTo().alert();
```

Then interact:

```java
alert.accept();
```

---

# 18. One-Line Alert Handling

You can also write:

```java
driver.switchTo().alert().accept();
```

Dismiss:

```java
driver.switchTo().alert().dismiss();
```

Get text:

```java
String text =
        driver.switchTo().alert().getText();
```

Send text:

```java
driver.switchTo()
      .alert()
      .sendKeys("Selva");
```

---

# 19. Handling No Alert Present

If you call:

```java
driver.switchTo().alert();
```

when no alert exists, Selenium can throw:

```text
NoAlertPresentException
```

Example:

```java
try {

    Alert alert = driver.switchTo().alert();

    alert.accept();

} catch (NoAlertPresentException e) {

    System.out.println(
            "No alert is present."
    );
}
```

Import:

```java
import org.openqa.selenium.NoAlertPresentException;
```

---

# 20. Check Whether Alert Is Present

A useful approach is to create a helper method.

```java
public static boolean isAlertPresent(
        WebDriver driver) {

    try {

        driver.switchTo().alert();

        return true;

    } catch (NoAlertPresentException e) {

        return false;
    }
}
```

Usage:

```java
if (isAlertPresent(driver)) {

    driver.switchTo()
          .alert()
          .accept();
}
```

---

# 21. Better Alert Utility

A reusable utility can return the alert itself.

```java
public static Alert getAlert(
        WebDriver driver) {

    try {

        return driver.switchTo().alert();

    } catch (NoAlertPresentException e) {

        return null;
    }
}
```

Usage:

```java
Alert alert = getAlert(driver);

if (alert != null) {

    System.out.println(
            alert.getText()
    );

    alert.accept();
}
```

---

# 22. Explicit Wait for Alert

Sometimes the alert does not appear immediately.

Use:

```java
WebDriverWait wait =
        new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );

Alert alert = wait.until(
        ExpectedConditions.alertIsPresent()
);
```

Imports:

```java
import java.time.Duration;

import org.openqa.selenium.Alert;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
```

Then:

```java
alert.accept();
```

---

# 23. Complete Explicit Wait Example

```java
WebDriverWait wait =
        new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );

Alert alert = wait.until(
        ExpectedConditions.alertIsPresent()
);

System.out.println(
        "Alert: " + alert.getText()
);

alert.accept();
```

This is generally better than:

```java
Thread.sleep(5000);
```

because Selenium waits for the alert condition instead of blindly waiting for a fixed amount of time.

---

# 24. Alert Utility Class

Create:

```text
utils/
└── AlertUtils.java
```

Example:

```java
import java.time.Duration;

import org.openqa.selenium.Alert;
import org.openqa.selenium.NoAlertPresentException;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

public class AlertUtils {

    public static Alert waitForAlert(
            WebDriver driver,
            int seconds) {

        WebDriverWait wait =
                new WebDriverWait(
                        driver,
                        Duration.ofSeconds(seconds)
                );

        return wait.until(
                ExpectedConditions.alertIsPresent()
        );
    }

    public static void acceptAlert(
            WebDriver driver,
            int seconds) {

        Alert alert =
                waitForAlert(driver, seconds);

        alert.accept();
    }

    public static void dismissAlert(
            WebDriver driver,
            int seconds) {

        Alert alert =
                waitForAlert(driver, seconds);

        alert.dismiss();
    }

    public static String getAlertText(
            WebDriver driver,
            int seconds) {

        Alert alert =
                waitForAlert(driver, seconds);

        return alert.getText();
    }

    public static void enterText(
            WebDriver driver,
            String text,
            int seconds) {

        Alert alert =
                waitForAlert(driver, seconds);

        alert.sendKeys(text);
    }

    public static boolean isAlertPresent(
            WebDriver driver) {

        try {

            driver.switchTo().alert();

            return true;

        } catch (NoAlertPresentException e) {

            return false;
        }
    }
}
```

---

# 25. Using Alert Utility

Accept:

```java
AlertUtils.acceptAlert(driver, 10);
```

Dismiss:

```java
AlertUtils.dismissAlert(driver, 10);
```

Get text:

```java
String message =
        AlertUtils.getAlertText(driver, 10);
```

Enter text:

```java
AlertUtils.enterText(
        driver,
        "Selva",
        10
);
```

Check:

```java
if (AlertUtils.isAlertPresent(driver)) {

    AlertUtils.acceptAlert(driver, 5);
}
```

---

# 26. Alert vs WebElement

A browser alert is **not a WebElement**.

Incorrect:

```java
WebElement alert =
        driver.findElement(
                By.id("alert")
        );
```

Correct:

```java
Alert alert =
        driver.switchTo().alert();
```

Reason:

JavaScript browser alerts are browser-level dialogs rather than normal DOM elements.

---

# 27. JavaScript Alert vs HTML Modal

This distinction is very important.

## JavaScript Alert

Example:

```javascript
alert("Hello");
```

Handle using:

```java
driver.switchTo().alert();
```

---

## HTML Modal

An HTML modal is part of the webpage DOM.

Example:

```html
<div class="modal">
    <button>OK</button>
</div>
```

Handle it using normal Selenium locators:

```java
driver.findElement(
        By.cssSelector(".modal button")
).click();
```

Do **not** use:

```java
driver.switchTo().alert();
```

for an HTML modal.

---

# 28. Browser Alert vs HTML Popup

| Feature              | JavaScript Alert | HTML Modal                    |
| -------------------- | ---------------- | ----------------------------- |
| Part of DOM          | No               | Yes                           |
| `findElement()`      | No               | Yes                           |
| `switchTo().alert()` | Yes              | No                            |
| `accept()`           | Yes              | No                            |
| `dismiss()`          | Yes              | No                            |
| `sendKeys()`         | Prompt only      | Normal WebElement interaction |
| CSS/XPath            | No               | Yes                           |

---

# 29. Alert Handling Flow

```text
Action
  |
  v
Alert appears
  |
  v
switchTo().alert()
  |
  +---- getText()
  |
  +---- accept()
  |
  +---- dismiss()
  |
  +---- sendKeys()
  |
  v
Return to webpage
```

---

# 30. Returning to the Web Page

After accepting or dismissing the alert:

```java
alert.accept();
```

WebDriver automatically returns to the normal page context.

You don't need:

```java
driver.switchTo().defaultContent();
```

for a browser alert.

`defaultContent()` is primarily used for switching out of frames.

---

# 31. Alert with TestNG

```java
import java.time.Duration;

import org.openqa.selenium.Alert;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.Assert;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

public class AlertTest {

    WebDriver driver;

    @BeforeMethod
    public void setUp() {

        driver = new ChromeDriver();

        driver.manage()
                .window()
                .maximize();
    }

    @Test
    public void verifyAlert() {

        driver.get("https://example.com");

        // Perform action that opens alert

        WebDriverWait wait =
                new WebDriverWait(
                        driver,
                        Duration.ofSeconds(10)
                );

        Alert alert = wait.until(
                ExpectedConditions.alertIsPresent()
        );

        String message =
                alert.getText();

        System.out.println(message);

        Assert.assertNotNull(message);

        alert.accept();
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

# 32. Handling Confirmation Based on Expected Result

Example:

```java
Alert alert =
        driver.switchTo().alert();

String message =
        alert.getText();

Assert.assertEquals(
        message,
        "Are you sure?"
);

alert.accept();
```

For cancellation:

```java
alert.dismiss();
```

---

# 33. Prompt Validation

Example:

```java
Alert alert =
        driver.switchTo().alert();

alert.sendKeys("Toyota");

alert.accept();
```

Then validate the result on the webpage:

```java
String result =
        driver.findElement(
                By.id("result")
        ).getText();

System.out.println(result);
```

---

# 34. Common Alert Problems

## Problem 1: NoAlertPresentException

Code:

```java
driver.switchTo().alert();
```

Problem:

No alert exists yet.

Solution:

```java
WebDriverWait wait =
        new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );

Alert alert = wait.until(
        ExpectedConditions.alertIsPresent()
);
```

---

## Problem 2: Treating HTML modal as Alert

Incorrect:

```java
driver.switchTo().alert();
```

Solution:

Inspect the page.

If it is HTML:

```java
driver.findElement(
        By.cssSelector(".modal button")
).click();
```

---

## Problem 3: Alert appears after an asynchronous operation

Use:

```java
ExpectedConditions.alertIsPresent()
```

instead of:

```java
Thread.sleep(5000);
```

---

# 35. Alert Handling Best Practices

### 1. Always switch to the alert

```java
Alert alert =
        driver.switchTo().alert();
```

### 2. Use explicit wait when necessary

```java
ExpectedConditions.alertIsPresent()
```

### 3. Validate alert text

```java
Assert.assertEquals(
        alert.getText(),
        "Expected message"
);
```

### 4. Use accept/dismiss appropriately

```java
alert.accept();
```

or:

```java
alert.dismiss();
```

### 5. Don't confuse HTML modals with JavaScript alerts

HTML modal:

```java
driver.findElement(...);
```

JavaScript alert:

```java
driver.switchTo().alert();
```

### 6. Create reusable AlertUtils

Centralizing alert handling keeps test code cleaner.

---

# 36. Alert Cheat Sheet

```java
// Switch
Alert alert =
        driver.switchTo().alert();

// Get text
String text =
        alert.getText();

// Accept
alert.accept();

// Dismiss
alert.dismiss();

// Enter text
alert.sendKeys("Selva");
```

Explicit wait:

```java
Alert alert = new WebDriverWait(
        driver,
        Duration.ofSeconds(10)
).until(
        ExpectedConditions.alertIsPresent()
);
```

---

# 37. Important Selenium Alert Exceptions

```text
NoAlertPresentException
TimeoutException
WebDriverException
```

The most common alert-specific exception is:

```text
NoAlertPresentException
```

---

# 38. Interview Questions

## Q1. How do you handle alerts in Selenium?

Use:

```java
driver.switchTo().alert();
```

Then use:

```java
accept()
dismiss()
getText()
sendKeys()
```

---

## Q2. What are the different types of JavaScript alerts?

Three common types:

1. Simple alert
2. Confirmation alert
3. Prompt alert

---

## Q3. How do you accept an alert?

```java
driver.switchTo()
      .alert()
      .accept();
```

---

## Q4. How do you dismiss an alert?

```java
driver.switchTo()
      .alert()
      .dismiss();
```

---

## Q5. How do you get alert text?

```java
String text =
        driver.switchTo()
              .alert()
              .getText();
```

---

## Q6. How do you enter text into a prompt?

```java
driver.switchTo()
      .alert()
      .sendKeys("Selva");
```

---

## Q7. What happens if no alert is present?

Selenium can throw:

```text
NoAlertPresentException
```

---

## Q8. How do you wait for an alert?

```java
WebDriverWait wait =
        new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );

Alert alert = wait.until(
        ExpectedConditions.alertIsPresent()
);
```

---

## Q9. Can you locate a JavaScript alert using XPath?

No.

A JavaScript browser alert is not a normal DOM element.

Use:

```java
driver.switchTo().alert();
```

---

## Q10. Can you use `findElement()` for an alert?

No, not for a native JavaScript browser alert.

Use:

```java
driver.switchTo().alert();
```

---

## Q11. What is the difference between an alert and an HTML popup?

A JavaScript alert is a browser-level dialog.

An HTML popup/modal is part of the webpage DOM and can normally be located using Selenium locators.

---

## Q12. What is `NoAlertPresentException`?

It occurs when WebDriver tries to switch to an alert when no alert is currently present.

---

## Q13. Can Selenium handle authentication browser dialogs using Alert?

Not every browser-level dialog is a JavaScript `Alert`.

For example, HTTP authentication and certain browser-native dialogs require different approaches depending on the browser and application.

Do not assume every browser popup can be handled with:

```java
driver.switchTo().alert();
```

---

# 39. Senior-Level Interview Scenario

### Scenario

The application displays a confirmation dialog after clicking Delete.

You need to:

1. Click Delete
2. Wait for confirmation
3. Read alert text
4. Verify message
5. Click OK

Solution:

```java
driver.findElement(
        By.id("delete")
).click();

WebDriverWait wait =
        new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );

Alert alert = wait.until(
        ExpectedConditions.alertIsPresent()
);

String message =
        alert.getText();

Assert.assertEquals(
        message,
        "Are you sure you want to delete?"
);

alert.accept();
```

---

# 40. Senior-Level Scenario: Cancel Delete

```java
driver.findElement(
        By.id("delete")
).click();

WebDriverWait wait =
        new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );

Alert alert = wait.until(
        ExpectedConditions.alertIsPresent()
);

Assert.assertEquals(
        alert.getText(),
        "Are you sure you want to delete?"
);

alert.dismiss();
```

Then verify that the record still exists.

---

# 41. Senior-Level Scenario: Prompt

```java
driver.findElement(
        By.id("enterName")
).click();

WebDriverWait wait =
        new WebDriverWait(
                driver,
                Duration.ofSeconds(10)
        );

Alert alert = wait.until(
        ExpectedConditions.alertIsPresent()
);

Assert.assertEquals(
        alert.getText(),
        "Enter your name"
);

alert.sendKeys("Selva");

alert.accept();
```

Then validate the value displayed by the application.

---

# 42. Recommended Framework Structure

After adding this topic, your repository can contain:

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
└── Alerts/
    └── Selenium-Alerts.md
```

Later, the alert utility can be placed under:

```text
Utilities/
└── AlertUtils.md
```

and the Java implementation can become:

```text
src/
└── main/
    └── java/
        └── utilities/
            └── AlertUtils.java
```

---

# 43. Final Alert Revision

Remember:

```text
JavaScript Alert
      |
      v
driver.switchTo().alert()
      |
      +---- getText()
      |
      +---- accept()
      |
      +---- dismiss()
      |
      +---- sendKeys()
```

Simple alert:

```java
alert.accept();
```

Confirmation:

```java
alert.accept();
```

or:

```java
alert.dismiss();
```

Prompt:

```java
alert.sendKeys("Text");
alert.accept();
```

Wait:

```java
ExpectedConditions.alertIsPresent()
```

Exception:

```text
NoAlertPresentException
```

HTML modal:

```java
driver.findElement(...)
```

JavaScript alert:

```java
driver.switchTo().alert()
```

**Key rule:**

> If the popup is a native JavaScript alert, use `switchTo().alert()`. If it is an HTML modal, use normal Selenium locators.

---

# 44. Quick Interview Cheat Sheet

```text
Alert interface:
    org.openqa.selenium.Alert

Switch:
    driver.switchTo().alert()

Accept:
    alert.accept()

Dismiss:
    alert.dismiss()

Read:
    alert.getText()

Enter:
    alert.sendKeys("text")

Wait:
    ExpectedConditions.alertIsPresent()

Common exception:
    NoAlertPresentException

JavaScript alert:
    switchTo().alert()

HTML modal:
    findElement()

Simple alert:
    accept()

Confirmation:
    accept() / dismiss()

Prompt:
    sendKeys() + accept()
```
