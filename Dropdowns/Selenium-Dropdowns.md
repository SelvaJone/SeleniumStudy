# Selenium Dropdowns

## 1. Introduction

Dropdowns are common UI elements used to allow users to select one or more values from a list.

In Selenium, dropdown handling depends on the type of dropdown used by the application.

There are mainly three types:

1. Standard HTML `<select>` dropdown
2. Custom dropdown
3. Multi-select dropdown

For standard HTML `<select>` dropdowns, Selenium provides the `Select` class.

---

# 2. Import Select Class

```java
import org.openqa.selenium.support.ui.Select;
The Select class is available under:

org.openqa.selenium.support.ui.Select
3. Standard HTML Select Dropdown

Example HTML:

<select id="country">
    <option value="us">United States</option>
    <option value="ca">Canada</option>
    <option value="mx">Mexico</option>
</select>

Selenium code:

WebElement countryDropdown = driver.findElement(By.id("country"));


Select select = new Select(countryDropdown);

Now we can select an option using different methods.

4. Select By Visible Text
WebElement countryDropdown = driver.findElement(By.id("country"));


Select select = new Select(countryDropdown);


select.selectByVisibleText("Canada");

This selects:

<option value="ca">Canada</option>
Best use case

Use selectByVisibleText() when the displayed text is known and stable.

5. Select By Value

HTML:

<select id="country">
    <option value="us">United States</option>
    <option value="ca">Canada</option>
    <option value="mx">Mexico</option>
</select>

Java:

Select select = new Select(driver.findElement(By.id("country")));


select.selectByValue("ca");

This selects:

<option value="ca">Canada</option>
Important

selectByValue() uses the value attribute, not the visible text.

6. Select By Index
Select select = new Select(driver.findElement(By.id("country")));


select.selectByIndex(1);

Indexes start from:

0

Example:

Index 0 → United States
Index 1 → Canada
Index 2 → Mexico

Therefore:

select.selectByIndex(1);

selects:

Canada
Caution

Avoid using index when possible because dropdown order can change.

7. Complete Example
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.Select;


public class DropdownExample {


    public static void main(String[] args) {


        WebDriver driver = new ChromeDriver();


        driver.get("https://example.com");


        WebElement dropdown = driver.findElement(By.id("country"));


        Select select = new Select(dropdown);


        // Select by visible text
        select.selectByVisibleText("Canada");


        // Select by value
        select.selectByValue("ca");


        // Select by index
        select.selectByIndex(1);


        driver.quit();
    }
}
8. Get All Dropdown Options

Use:

getOptions()

Example:

Select select = new Select(driver.findElement(By.id("country")));


List<WebElement> options = select.getOptions();


for (WebElement option : options) {
    System.out.println(option.getText());
}

Import:

import java.util.List;

Output:

United States
Canada
Mexico
9. Get Selected Option

Use:

getFirstSelectedOption()

Example:

Select select = new Select(driver.findElement(By.id("country")));


select.selectByVisibleText("Canada");


WebElement selectedOption = select.getFirstSelectedOption();


System.out.println(selectedOption.getText());

Output:

Canada
10. Get All Selected Options

This is especially useful for multi-select dropdowns.

List<WebElement> selectedOptions =
        select.getAllSelectedOptions();


for (WebElement option : selectedOptions) {
    System.out.println(option.getText());
}
11. Check If Dropdown Supports Multiple Selection

Use:

isMultiple()

Example:

Select select = new Select(driver.findElement(By.id("skills")));


if (select.isMultiple()) {
    System.out.println("Multi-select dropdown");
} else {
    System.out.println("Single-select dropdown");
}
12. Multi-Select Dropdown

Example HTML:

<select id="skills" multiple>
    <option value="java">Java</option>
    <option value="selenium">Selenium</option>
    <option value="api">API Testing</option>
    <option value="sql">SQL</option>
</select>

Java:

Select select = new Select(driver.findElement(By.id("skills")));


select.selectByVisibleText("Java");
select.selectByVisibleText("Selenium");
select.selectByVisibleText("SQL");

Multiple values can be selected.

13. Deselect By Visible Text

For multi-select dropdowns:

select.deselectByVisibleText("Java");
14. Deselect By Value
select.deselectByValue("selenium");
15. Deselect By Index
select.deselectByIndex(1);
16. Deselect All

To remove all selections:

select.deselectAll();
Important

deselectAll() works only with multi-select dropdowns.

17. Check Selected Options
List<WebElement> selectedOptions =
        select.getAllSelectedOptions();


for (WebElement option : selectedOptions) {
    System.out.println(option.getText());
}
18. Select Dropdown Using XPath

You can locate the dropdown using XPath.

WebElement dropdown =
        driver.findElement(By.xpath("//select[@id='country']"));


Select select = new Select(dropdown);


select.selectByVisibleText("Canada");
19. Select Dropdown Using CSS Selector
WebElement dropdown =
        driver.findElement(By.cssSelector("#country"));


Select select = new Select(dropdown);


select.selectByVisibleText("Canada");
20. Important: Select Class Works Only With <select>

The Selenium Select class works with a real HTML:

<select>

Example:

<select id="country">
    <option>USA</option>
    <option>Canada</option>
</select>

It does NOT work with custom dropdowns such as:

<div class="dropdown">

or:

<button>Canada</button>

If you try:

Select select = new Select(element);

on a non-select element, Selenium can throw:

UnexpectedTagNameException
21. Custom Dropdown

Modern applications frequently use custom dropdowns.

Example:

<div class="dropdown">
    <button>Canada</button>


    <div class="options">
        <div>United States</div>
        <div>Canada</div>
        <div>Mexico</div>
    </div>
</div>

For this type of dropdown, do not use:

Select select = new Select(element);

Instead:

Click the dropdown
Find the desired option
Click the option

Example:

driver.findElement(By.id("countryDropdown")).click();


driver.findElement(
    By.xpath("//div[@class='option' and text()='Canada']")
).click();
22. Custom Dropdown Example
WebElement dropdown =
        driver.findElement(By.id("countryDropdown"));


dropdown.click();


WebElement canada =
        driver.findElement(
            By.xpath("//div[text()='Canada']")
        );


canada.click();
23. Custom Dropdown With Explicit Wait

Custom dropdowns may require waiting for options to appear.

WebDriverWait wait =
        new WebDriverWait(driver, Duration.ofSeconds(10));


WebElement dropdown =
        wait.until(ExpectedConditions.elementToBeClickable(
            By.id("countryDropdown")
        ));


dropdown.click();


WebElement canada =
        wait.until(ExpectedConditions.elementToBeClickable(
            By.xpath("//div[text()='Canada']")
        ));


canada.click();

Imports:

import java.time.Duration;


import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
24. Dynamic Dropdown

Dynamic dropdowns load options after entering text.

Example:

Enter city
    ↓
Type "New"
    ↓
New York
Newark
New Orleans

Example:

WebElement city =
        driver.findElement(By.id("city"));


city.sendKeys("New");

Wait for the options:

WebDriverWait wait =
        new WebDriverWait(driver, Duration.ofSeconds(10));


WebElement option =
        wait.until(ExpectedConditions.visibilityOfElementLocated(
            By.xpath("//li[text()='New York']")
        ));


option.click();
25. Auto-Suggestion Dropdown

Example:

driver.findElement(By.id("search"))
      .sendKeys("Toyota");


List<WebElement> suggestions =
        driver.findElements(By.cssSelector(".suggestion"));


for (WebElement suggestion : suggestions) {


    if (suggestion.getText().equals("Toyota Camry")) {
        suggestion.click();
        break;
    }
}

This is a common approach for autocomplete fields.

26. Handling Dropdown Options Dynamically

Instead of hardcoding an option:

driver.findElement(
    By.xpath("//li[text()='Canada']")
).click();

You can loop through all options:

List<WebElement> options =
        driver.findElements(By.cssSelector(".dropdown-option"));


for (WebElement option : options) {


    if (option.getText().equals("Canada")) {
        option.click();
        break;
    }
}

This is useful when dropdown options are dynamic.

27. Reusable Dropdown Utility Method

You can create a utility method:

public static void selectByVisibleText(
        WebDriver driver,
        By locator,
        String text) {


    WebElement dropdown = driver.findElement(locator);


    Select select = new Select(dropdown);


    select.selectByVisibleText(text);
}

Usage:

selectByVisibleText(
    driver,
    By.id("country"),
    "Canada"
);
28. Reusable Select By Value Method
public static void selectByValue(
        WebDriver driver,
        By locator,
        String value) {


    WebElement dropdown = driver.findElement(locator);


    Select select = new Select(dropdown);


    select.selectByValue(value);
}

Usage:

selectByValue(
    driver,
    By.id("country"),
    "ca"
);
29. Reusable Select By Index Method
public static void selectByIndex(
        WebDriver driver,
        By locator,
        int index) {


    WebElement dropdown = driver.findElement(locator);


    Select select = new Select(dropdown);


    select.selectByIndex(index);
}

Usage:

selectByIndex(
    driver,
    By.id("country"),
    1
);
30. Verify Selected Value

Example:

Select select =
        new Select(driver.findElement(By.id("country")));


select.selectByVisibleText("Canada");


String selectedValue =
        select.getFirstSelectedOption().getText();


System.out.println(selectedValue);

Validation:

Assert.assertEquals(selectedValue, "Canada");

Import:

import org.testng.Assert;
31. Complete Dropdown Utility Class
import java.util.List;


import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.ui.Select;


public class DropdownUtils {


    public static void selectByVisibleText(
            WebDriver driver,
            By locator,
            String text) {


        WebElement element = driver.findElement(locator);


        Select select = new Select(element);


        select.selectByVisibleText(text);
    }


    public static void selectByValue(
            WebDriver driver,
            By locator,
            String value) {


        WebElement element = driver.findElement(locator);


        Select select = new Select(element);


        select.selectByValue(value);
    }


    public static void selectByIndex(
            WebDriver driver,
            By locator,
            int index) {


        WebElement element = driver.findElement(locator);


        Select select = new Select(element);


        select.selectByIndex(index);
    }


    public static void printAllOptions(
            WebDriver driver,
            By locator) {


        Select select =
                new Select(driver.findElement(locator));


        List<WebElement> options =
                select.getOptions();


        for (WebElement option : options) {
            System.out.println(option.getText());
        }
    }


    public static String getSelectedOption(
            WebDriver driver,
            By locator) {


        Select select =
                new Select(driver.findElement(locator));


        return select.getFirstSelectedOption().getText();
    }
}
32. Using Dropdown Utility
DropdownUtils.selectByVisibleText(
    driver,
    By.id("country"),
    "Canada"
);

Print options:

DropdownUtils.printAllOptions(
    driver,
    By.id("country")
);

Get selected option:

String selected =
        DropdownUtils.getSelectedOption(
            driver,
            By.id("country")
        );


System.out.println(selected);
33. Dropdown Handling With Page Object Model

Page class:

public class RegistrationPage {


    private WebDriver driver;


    private By countryDropdown =
            By.id("country");


    public RegistrationPage(WebDriver driver) {
        this.driver = driver;
    }


    public void selectCountry(String country) {


        Select select =
                new Select(
                    driver.findElement(countryDropdown)
                );


        select.selectByVisibleText(country);
    }
}

Test:

RegistrationPage page =
        new RegistrationPage(driver);


page.selectCountry("Canada");
34. Dropdown With PageFactory
@FindBy(id = "country")
WebElement countryDropdown;

Method:

public void selectCountry(String country) {


    Select select =
            new Select(countryDropdown);


    select.selectByVisibleText(country);
}
35. Common Dropdown Methods
Method	Purpose
selectByVisibleText()	Select using displayed text
selectByValue()	Select using value attribute
selectByIndex()	Select using index
getOptions()	Get all options
getFirstSelectedOption()	Get selected option
getAllSelectedOptions()	Get all selected options
isMultiple()	Check multi-select
deselectByVisibleText()	Deselect by text
deselectByValue()	Deselect by value
deselectByIndex()	Deselect by index
deselectAll()	Deselect all
36. Select By Text vs Value vs Index
Visible Text
select.selectByVisibleText("Canada");

Best when:

Displayed option text is stable.
Value
select.selectByValue("ca");

Best when:

HTML value attribute is stable.
Index
select.selectByIndex(1);

Use carefully because:

Dropdown order can change.
Preferred Order

Generally prefer:

1. selectByVisibleText()
2. selectByValue()
3. selectByIndex()

depending on which attribute is stable in the application.

37. Common Exceptions
UnexpectedTagNameException

Occurs when Select is used with an element that is not a <select>.

Example:

WebElement dropdown =
        driver.findElement(By.id("customDropdown"));


Select select = new Select(dropdown);

If the element is:

<div>

instead of:

<select>

Selenium throws:

UnexpectedTagNameException

Solution:

Use normal click and option selection for custom dropdowns.

38. NoSuchElementException

Can occur when the requested option does not exist.

Example:

select.selectByVisibleText("Australia");

when Australia is not present.

Solution:

Verify available options first:

List<WebElement> options =
        select.getOptions();


for (WebElement option : options) {
    System.out.println(option.getText());
}
39. StaleElementReferenceException

Dynamic dropdowns can cause stale element problems when the DOM is refreshed.

Instead of storing an element for too long:

WebElement option = ...

locate it again after the dropdown is refreshed.

Explicit waits can also help:

wait.until(
    ExpectedConditions.elementToBeClickable(locator)
);
40. ElementClickInterceptedException

This can occur with custom dropdowns when another element is covering the option.

Use an explicit wait:

WebElement option =
    wait.until(
        ExpectedConditions.elementToBeClickable(locator)
    );


option.click();
41. Dropdown Interview Questions
Q1. How do you handle a standard dropdown in Selenium?

Use the Select class.

Select select =
    new Select(driver.findElement(By.id("country")));


select.selectByVisibleText("Canada");
Q2. Which class is used to handle <select> dropdowns?
org.openqa.selenium.support.ui.Select
Q3. What are the three selection methods?
selectByVisibleText()
selectByValue()
selectByIndex()
Q4. Can Select class handle custom dropdowns?

No.

Select works with HTML <select> elements.

Custom dropdowns must generally be handled using:

click()

followed by selecting the required option.

Q5. How do you get all options?
List<WebElement> options =
    select.getOptions();
Q6. How do you get the selected option?
WebElement option =
    select.getFirstSelectedOption();
Q7. How do you check whether a dropdown supports multiple selections?
select.isMultiple();
Q8. How do you deselect all options?
select.deselectAll();

This applies to multi-select dropdowns.

Q9. What is the difference between getOptions() and getAllSelectedOptions()?

getOptions() returns:

All available options

getAllSelectedOptions() returns:

Only selected options
Q10. Can selectByIndex() be used with a custom dropdown?

No.

It is a method of Selenium's Select class and therefore requires a standard <select> element.

42. Best Practices
1. Identify the dropdown type first

Check the HTML.

If you see:

<select>

use:

Select

Otherwise, treat it as a custom dropdown.

2. Prefer stable locators

Prefer:

By.id("country")

over fragile XPath.

3. Prefer visible text or stable value

Avoid index when the order can change.

4. Use explicit waits

Especially for dynamic/custom dropdowns.

WebDriverWait

is preferable to:

Thread.sleep()
5. Create reusable utility methods

This reduces duplicate dropdown code throughout the framework.

43. Quick Revision
Standard HTML Dropdown
        |
        v
<select>
        |
        v
Select class
        |
        +-- selectByVisibleText()
        |
        +-- selectByValue()
        |
        +-- selectByIndex()
        |
        +-- getOptions()
        |
        +-- getFirstSelectedOption()
        |
        +-- getAllSelectedOptions()
        |
        +-- isMultiple()
        |
        +-- deselectByVisibleText()
        |
        +-- deselectByValue()
        |
        +-- deselectByIndex()
        |
        +-- deselectAll()

Custom dropdown:

Custom Dropdown
       |
       v
Click dropdown
       |
       v
Wait for options
       |
       v
Find required option
       |
       v
Click option
44. Key Takeaways
Use Selenium's Select class for standard HTML <select> dropdowns.
selectByVisibleText() selects based on displayed text.
selectByValue() selects using the HTML value attribute.
selectByIndex() selects based on zero-based index.
getOptions() returns all dropdown options.
getFirstSelectedOption() returns the selected option.
getAllSelectedOptions() returns all selected options.
isMultiple() checks whether multiple selections are supported.
deselectAll() works with multi-select dropdowns.
Select does not work with custom dropdowns.
Custom dropdowns require normal Selenium interactions such as click, wait, locate, and click.
Dynamic/autocomplete dropdowns often require explicit waits.
For framework development, reusable dropdown utility methods are recommended.


### Next topic


After **Dropdowns**, a good next Selenium study topic is:


**`Frames/Selenium-Frames.md`** — handling iframes, nested frames, `switchTo().frame()`, `parentFrame()`, `defaultContent()`, and frame-related interview questions.

