# Selenium WebTables

## 1. What is a Web Table?

A **Web Table** is an HTML table used to display data in rows and columns.

Typical HTML structure:

```html
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Age</th>
            <th>City</th>
        </tr>
    </thead>

    <tbody>
        <tr>
            <td>John</td>
            <td>30</td>
            <td>Dallas</td>
        </tr>
        <tr>
            <td>David</td>
            <td>35</td>
            <td>Houston</td>
        </tr>
    </tbody>
</table>
```

---

# 2. Types of Web Tables

### Static Web Table

The number of rows and columns remains mostly fixed.

Example:

```text
Name       Age      City
John       30       Dallas
David      35       Houston
Selva      40       Austin
```

### Dynamic Web Table

Rows or columns are dynamically generated based on application data.

Example:

```text
Name       Status       Action
John       Active       Edit
David      Inactive     Edit
Selva      Active       Edit
```

The number of rows may change every time the page loads.

---

# 3. Important Selenium Methods

For web tables, these methods are commonly used:

```java
findElement()
findElements()
getText()
getAttribute()
click()
```

Example:

```java
List<WebElement> rows =
        driver.findElements(By.xpath("//table[@id='employeeTable']//tbody/tr"));
```

---

# 4. Get Number of Rows

```java
List<WebElement> rows =
        driver.findElements(By.xpath("//table[@id='employeeTable']//tbody/tr"));

System.out.println("Number of rows: " + rows.size());
```

---

# 5. Get Number of Columns

```java
List<WebElement> columns =
        driver.findElements(By.xpath("//table[@id='employeeTable']//tbody/tr[1]/td"));

System.out.println("Number of columns: " + columns.size());
```

For header columns:

```java
List<WebElement> headers =
        driver.findElements(By.xpath("//table[@id='employeeTable']//thead/tr/th"));

System.out.println("Number of columns: " + headers.size());
```

---

# 6. Print All Rows

```java
List<WebElement> rows =
        driver.findElements(By.xpath("//table[@id='employeeTable']//tbody/tr"));

for (WebElement row : rows) {
    System.out.println(row.getText());
}
```

Output:

```text
John 30 Dallas
David 35 Houston
Selva 40 Austin
```

---

# 7. Print All Cells

```java
List<WebElement> rows =
        driver.findElements(By.xpath("//table[@id='employeeTable']//tbody/tr"));

for (WebElement row : rows) {

    List<WebElement> cells = row.findElements(By.tagName("td"));

    for (WebElement cell : cells) {
        System.out.print(cell.getText() + " | ");
    }

    System.out.println();
}
```

Output:

```text
John | 30 | Dallas |
David | 35 | Houston |
Selva | 40 | Austin |
```

---

# 8. Print Complete Table

```java
List<WebElement> rows =
        driver.findElements(By.xpath("//table[@id='employeeTable']//tr"));

for (WebElement row : rows) {

    List<WebElement> cells = row.findElements(By.xpath("./th|./td"));

    for (WebElement cell : cells) {
        System.out.print(cell.getText() + " | ");
    }

    System.out.println();
}
```

---

# 9. Get Specific Cell

Suppose the table is:

```text
Name       Age      City
John       30       Dallas
David      35       Houston
Selva      40       Austin
```

To get `David`:

```java
String name = driver.findElement(
        By.xpath("//table[@id='employeeTable']//tbody/tr[2]/td[1]")
).getText();

System.out.println(name);
```

Output:

```text
David
```

To get `Houston`:

```java
String city = driver.findElement(
        By.xpath("//table[@id='employeeTable']//tbody/tr[2]/td[3]")
).getText();

System.out.println(city);
```

---

# 10. XPath Indexing

XPath table indexing starts at **1**, not 0.

```text
tr[1] = first row
tr[2] = second row
tr[3] = third row

td[1] = first column
td[2] = second column
td[3] = third column
```

Example:

```java
//table[@id='employeeTable']//tbody/tr[2]/td[3]
```

Means:

```text
Table
 → tbody
   → second row
     → third column
```

---

# 11. Find Row Based on Cell Text

Instead of relying on row numbers, locate a row based on its data.

Example:

```java
WebElement row = driver.findElement(
        By.xpath("//table[@id='employeeTable']//tbody/tr[td[1]='David']")
);

System.out.println(row.getText());
```

This is more reliable for dynamic tables.

---

# 12. Find a Row and Click an Action

Suppose the table contains:

```text
Name       Status       Action
John       Active       Edit
David      Active       Edit
Selva      Inactive     Edit
```

To click Edit for David:

```java
driver.findElement(
        By.xpath("//table[@id='employeeTable']//tbody/tr[td[1]='David']//button[text()='Edit']")
).click();
```

---

# 13. Find Row Based on Multiple Conditions

Example:

```text
Name       Status
John       Active
David      Inactive
Selva      Active
```

Find the row where:

```text
Name = David
Status = Inactive
```

Code:

```java
WebElement row = driver.findElement(
        By.xpath("//table[@id='employeeTable']//tbody/tr[td[1]='David' and td[2]='Inactive']")
);

System.out.println(row.getText());
```

---

# 14. Click Checkbox in a Specific Row

Example:

```text
Name       Status       Select
John       Active       checkbox
David      Active       checkbox
Selva      Inactive     checkbox
```

Select David:

```java
driver.findElement(
        By.xpath("//table[@id='employeeTable']//tbody/tr[td[1]='David']//input[@type='checkbox']")
).click();
```

---

# 15. Click Link in a Specific Row

Example:

```text
Name       Action
John       View
David      View
Selva      View
```

Code:

```java
driver.findElement(
        By.xpath("//table[@id='employeeTable']//tbody/tr[td[1]='David']//a[text()='View']")
).click();
```

---

# 16. Get a Particular Column

Suppose you want all employee names.

```java
List<WebElement> names =
        driver.findElements(
                By.xpath("//table[@id='employeeTable']//tbody/tr/td[1]")
        );

for (WebElement name : names) {
    System.out.println(name.getText());
}
```

Output:

```text
John
David
Selva
```

---

# 17. Get All Cities

```java
List<WebElement> cities =
        driver.findElements(
                By.xpath("//table[@id='employeeTable']//tbody/tr/td[3]")
        );

for (WebElement city : cities) {
    System.out.println(city.getText());
}
```

---

# 18. Search for a Value in a Table

```java
String expectedName = "David";

List<WebElement> rows =
        driver.findElements(By.xpath("//table[@id='employeeTable']//tbody/tr"));

boolean found = false;

for (WebElement row : rows) {

    if (row.getText().contains(expectedName)) {
        found = true;
        System.out.println("Employee found: " + row.getText());
        break;
    }
}

System.out.println("Found: " + found);
```

---

# 19. Verify a Value Exists

```java
String expectedCity = "Houston";

List<WebElement> cells =
        driver.findElements(
                By.xpath("//table[@id='employeeTable']//tbody/tr/td[3]")
        );

boolean found = false;

for (WebElement cell : cells) {

    if (cell.getText().equals(expectedCity)) {
        found = true;
        break;
    }
}

Assert.assertTrue(found, "City not found: " + expectedCity);
```

---

# 20. Print Table Using Nested Loops

This is one of the most common interview questions.

```java
List<WebElement> rows =
        driver.findElements(By.xpath("//table[@id='employeeTable']//tbody/tr"));

for (int i = 0; i < rows.size(); i++) {

    List<WebElement> cells =
            rows.get(i).findElements(By.tagName("td"));

    for (int j = 0; j < cells.size(); j++) {

        System.out.print(cells.get(j).getText() + " | ");
    }

    System.out.println();
}
```

---

# 21. Find Row Number Dynamically

Suppose you need to find the row number of `David`.

```java
List<WebElement> rows =
        driver.findElements(By.xpath("//table[@id='employeeTable']//tbody/tr"));

int rowNumber = -1;

for (int i = 0; i < rows.size(); i++) {

    String name = rows.get(i)
            .findElement(By.xpath("./td[1]"))
            .getText();

    if (name.equals("David")) {
        rowNumber = i + 1;
        break;
    }
}

System.out.println("David is in row: " + rowNumber);
```

---

# 22. Dynamic XPath for Row

Instead of finding the row first:

```java
String employeeName = "David";

String xpath =
        "//table[@id='employeeTable']//tbody/tr[td[1='" +
        employeeName +
        "']]";

WebElement row = driver.findElement(By.xpath(xpath));

System.out.println(row.getText());
```

---

# 23. Dynamic XPath for Action

```java
String employeeName = "David";

String xpath =
        "//table[@id='employeeTable']//tbody/tr[td[1='" +
        employeeName +
        "']]//button[text()='Edit']";

driver.findElement(By.xpath(xpath)).click();
```

---

# 24. Handle Dynamic Tables

Dynamic tables often have:

* Variable number of rows
* Variable data
* Pagination
* Search/filter
* Buttons inside rows
* Checkboxes
* Links
* Dropdowns

Use:

```java
findElements()
```

instead of assuming a fixed number of rows.

Example:

```java
List<WebElement> rows =
        driver.findElements(By.xpath("//table[@id='employeeTable']//tbody/tr"));

for (WebElement row : rows) {
    System.out.println(row.getText());
}
```

---

# 25. Handle Pagination

Suppose a table has:

```text
Previous  1  2  3  4  Next
```

You can process each page.

```java
while (true) {

    List<WebElement> rows =
            driver.findElements(By.xpath("//table[@id='employeeTable']//tbody/tr"));

    for (WebElement row : rows) {
        System.out.println(row.getText());
    }

    WebElement nextButton = driver.findElement(
            By.id("next")
    );

    if (!nextButton.isEnabled()) {
        break;
    }

    nextButton.click();
}
```

Actual pagination logic depends on the application's HTML.

---

# 26. Find Data Across Multiple Pages

```java
String employeeName = "David";
boolean found = false;

while (true) {

    List<WebElement> rows =
            driver.findElements(By.xpath("//table[@id='employeeTable']//tbody/tr"));

    for (WebElement row : rows) {

        if (row.getText().contains(employeeName)) {
            found = true;
            System.out.println("Found: " + row.getText());
            break;
        }
    }

    if (found) {
        break;
    }

    WebElement nextButton = driver.findElement(By.id("next"));

    if (!nextButton.isEnabled()) {
        break;
    }

    nextButton.click();
}

Assert.assertTrue(found, "Employee not found: " + employeeName);
```

---

# 27. Handling Table with `div` Instead of `<table>`

Modern applications may use `<div>` elements instead of HTML table tags.

Example:

```html
<div class="row">
    <div class="cell">John</div>
    <div class="cell">30</div>
    <div class="cell">Dallas</div>
</div>
```

Selenium code:

```java
List<WebElement> rows =
        driver.findElements(By.cssSelector(".row"));

for (WebElement row : rows) {

    List<WebElement> cells =
            row.findElements(By.cssSelector(".cell"));

    for (WebElement cell : cells) {
        System.out.print(cell.getText() + " | ");
    }

    System.out.println();
}
```

Do not assume every visual table uses `<table>`.

---

# 28. Handling React/Angular Tables

Modern applications may generate table content dynamically.

Use:

```java
WebDriverWait wait = new WebDriverWait(
        driver,
        Duration.ofSeconds(10)
);

List<WebElement> rows = wait.until(
        ExpectedConditions.visibilityOfAllElementsLocatedBy(
                By.xpath("//table[@id='employeeTable']//tbody/tr")
        )
);
```

Then process the rows.

---

# 29. Verify Row Count

```java
List<WebElement> rows =
        driver.findElements(By.xpath("//table[@id='employeeTable']//tbody/tr"));

Assert.assertEquals(rows.size(), 5);
```

For dynamic applications, it is often better to verify a meaningful business condition rather than a hard-coded count.

---

# 30. Verify Column Headers

```java
List<WebElement> headers =
        driver.findElements(
                By.xpath("//table[@id='employeeTable']//thead/tr/th")
        );

for (WebElement header : headers) {
    System.out.println(header.getText());
}
```

Expected:

```text
Name
Age
City
```

---

# 31. Verify Table Header

```java
String actualHeader = driver.findElement(
        By.xpath("//table[@id='employeeTable']//thead/tr/th[1]")
).getText();

Assert.assertEquals(actualHeader, "Name");
```

---

# 32. Get Cell Value by Row and Column

Create a reusable method:

```java
public String getCellValue(
        WebDriver driver,
        int row,
        int column) {

    String xpath =
            "//table[@id='employeeTable']//tbody/tr[" +
            row +
            "]/td[" +
            column +
            "]";

    return driver.findElement(By.xpath(xpath)).getText();
}
```

Usage:

```java
String value = getCellValue(driver, 2, 3);

System.out.println(value);
```

---

# 33. Reusable Method to Print Table

```java
public void printTable(WebDriver driver) {

    List<WebElement> rows =
            driver.findElements(
                    By.xpath("//table[@id='employeeTable']//tbody/tr")
            );

    for (WebElement row : rows) {

        List<WebElement> cells =
                row.findElements(By.tagName("td"));

        for (WebElement cell : cells) {
            System.out.print(cell.getText() + " | ");
        }

        System.out.println();
    }
}
```

---

# 34. Reusable Method to Find Row

```java
public WebElement findRow(
        WebDriver driver,
        String employeeName) {

    String xpath =
            "//table[@id='employeeTable']//tbody/tr[td[1='" +
            employeeName +
            "']]";

    return driver.findElement(By.xpath(xpath));
}
```

Usage:

```java
WebElement row = findRow(driver, "David");

System.out.println(row.getText());
```

---

# 35. Reusable Method to Click Row Action

```java
public void clickAction(
        WebDriver driver,
        String employeeName,
        String action) {

    String xpath =
            "//table[@id='employeeTable']//tbody/tr[td[1='" +
            employeeName +
            "']]//*[normalize-space(text())='" +
            action +
            "']";

    driver.findElement(By.xpath(xpath)).click();
}
```

Usage:

```java
clickAction(driver, "David", "Edit");
```

---

# 36. Complete Web Table Example

```java
import java.time.Duration;
import java.util.List;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.WebDriverWait;

public class WebTableExample {

    public static void main(String[] args) {

        WebDriver driver = new ChromeDriver();

        driver.manage().window().maximize();

        driver.manage().timeouts()
                .implicitlyWait(Duration.ofSeconds(10));

        driver.get("https://example.com");

        List<WebElement> rows =
                driver.findElements(
                        By.xpath("//table[@id='employeeTable']//tbody/tr")
                );

        System.out.println("Rows: " + rows.size());

        for (WebElement row : rows) {

            List<WebElement> cells =
                    row.findElements(By.tagName("td"));

            for (WebElement cell : cells) {

                System.out.print(cell.getText() + " | ");
            }

            System.out.println();
        }

        driver.quit();
    }
}
```

---

# 37. Complete Dynamic Row Example

```java
String employeeName = "David";

String rowXpath =
        "//table[@id='employeeTable']//tbody/tr[td[1='" +
        employeeName +
        "']]";

WebElement row = driver.findElement(By.xpath(rowXpath));

List<WebElement> cells =
        row.findElements(By.tagName("td"));

for (WebElement cell : cells) {
    System.out.println(cell.getText());
}
```

---

# 38. Select Checkbox Based on Row Data

```java
String employeeName = "David";

String xpath =
        "//table[@id='employeeTable']//tbody/tr[td[1='" +
        employeeName +
        "']]//input[@type='checkbox']";

driver.findElement(By.xpath(xpath)).click();
```

---

# 39. Interview Question: How Do You Handle Dynamic Web Tables?

A good answer:

> I locate the table rows dynamically using `findElements()` instead of hard-coding the number of rows. I iterate through each row, retrieve the cells, and identify the required row using unique business data such as an employee name or ID. Once the row is identified, I perform the required action such as clicking a button, selecting a checkbox, or reading a cell value.

Example:

```java
List<WebElement> rows =
        driver.findElements(By.xpath("//table/tbody/tr"));

for (WebElement row : rows) {

    if (row.getText().contains("David")) {

        row.findElement(By.xpath(".//button[text()='Edit']"))
                .click();

        break;
    }
}
```

---

# 40. Interview Question: How Do You Find a Specific Cell?

Use row and column indexes:

```java
String value = driver.findElement(
        By.xpath("//table/tbody/tr[2]/td[3]")
).getText();
```

Or use business data:

```java
String value = driver.findElement(
        By.xpath("//table/tbody/tr[td[1]='David']/td[3]")
).getText();
```

The second approach is generally more robust when row ordering changes.

---

# 41. Interview Question: How Do You Click Edit for a Particular Record?

```java
driver.findElement(
        By.xpath(
            "//table/tbody/tr[td[1]='David']//button[text()='Edit']"
        )
).click();
```

---

# 42. Interview Question: How Do You Count Rows?

```java
int rowCount = driver.findElements(
        By.xpath("//table/tbody/tr")
).size();

System.out.println(rowCount);
```

---

# 43. Interview Question: How Do You Count Columns?

```java
int columnCount = driver.findElements(
        By.xpath("//table/tbody/tr[1]/td")
).size();

System.out.println(columnCount);
```

---

# 44. Interview Question: How Do You Get All Table Data?

```java
List<WebElement> rows =
        driver.findElements(By.xpath("//table/tbody/tr"));

for (WebElement row : rows) {

    List<WebElement> cells =
            row.findElements(By.xpath("./td"));

    for (WebElement cell : cells) {
        System.out.print(cell.getText() + " ");
    }

    System.out.println();
}
```

---

# 45. Best Practices

### Prefer stable locators

Good:

```java
By.id("employeeTable")
```

Better than:

```java
By.xpath("/html/body/div[2]/div[3]/table")
```

### Use relative XPath

Prefer:

```java
row.findElement(By.xpath("./td[2]"));
```

instead of:

```java
driver.findElement(By.xpath("//table/tbody/tr[2]/td[2]"));
```

### Avoid hard-coded row numbers

Avoid:

```java
//table/tbody/tr[5]
```

Prefer:

```java
//table/tbody/tr[td[1]='David']
```

### Use explicit waits when data loads asynchronously

```java
WebDriverWait wait =
        new WebDriverWait(driver, Duration.ofSeconds(10));
```

### Handle pagination separately

If all records are not displayed on one page, iterate through the pages.

---

# 46. Common Mistakes

### Mistake 1: Using `findElement()` when multiple rows exist

```java
driver.findElement(By.xpath("//table/tbody/tr"));
```

This returns only the first matching element.

Use:

```java
driver.findElements(By.xpath("//table/tbody/tr"));
```

---

### Mistake 2: Using zero-based XPath indexes

Incorrect:

```java
//tr[0]
```

Correct:

```java
//tr[1]
```

---

### Mistake 3: Assuming every table uses `<table>`

Some applications use:

```html
<div>
```

elements to create table-like structures.

Inspect the actual HTML before creating the locator.

---

### Mistake 4: Using absolute XPath

Avoid:

```java
/html/body/div[2]/div[1]/table/tbody/tr[3]/td[2]
```

Use:

```java
//table[@id='employeeTable']//tbody/tr[3]/td[2]
```

---

# 47. Key Selenium WebTable Pattern

Remember this pattern:

```java
List<WebElement> rows =
        driver.findElements(By.xpath("//table/tbody/tr"));

for (WebElement row : rows) {

    List<WebElement> cells =
            row.findElements(By.xpath("./td"));

    for (WebElement cell : cells) {

        System.out.println(cell.getText());
    }
}
```

For a specific record:

```java
//table/tbody/tr[td[1]='David']
```

For an action inside that record:

```java
//table/tbody/tr[td[1]='David']//button[text()='Edit']
```

---

# 48. Quick Reference

| Requirement      | Selenium Code                                |
| ---------------- | -------------------------------------------- |
| Get rows         | `findElements(By.xpath("//table/tbody/tr"))` |
| Get cells        | `row.findElements(By.xpath("./td"))`         |
| Get text         | `element.getText()`                          |
| Count rows       | `rows.size()`                                |
| Count columns    | `cells.size()`                               |
| First row        | `tr[1]`                                      |
| First column     | `td[1]`                                      |
| Find record      | `tr[td[1]='David']`                          |
| Click row button | `tr[td[1]='David']//button`                  |
| Find checkbox    | `tr[td[1]='David']//input[@type='checkbox']` |
| Find link        | `tr[td[1]='David']//a`                       |

---

# 49. Interview Must-Know Topics

For Selenium WebTables, prepare these especially well:

1. Static vs dynamic web tables
2. `findElement()` vs `findElements()`
3. Getting row count
4. Getting column count
5. Reading all table data
6. Finding a row by text
7. Finding a specific cell
8. Clicking an action inside a specific row
9. Selecting a checkbox inside a row
10. Handling dynamic rows
11. Handling pagination
12. Handling tables built with `<div>`
13. Using relative XPath
14. Using explicit waits
15. Creating reusable WebTable utility methods

---

# 50. Key Takeaway

The most important WebTable pattern to remember is:

```java
List<WebElement> rows =
        driver.findElements(By.xpath("//table/tbody/tr"));

for (WebElement row : rows) {

    List<WebElement> cells =
            row.findElements(By.xpath("./td"));

    for (WebElement cell : cells) {
        System.out.print(cell.getText() + " | ");
    }

    System.out.println();
}
```

For real-world automation, avoid depending on row numbers whenever possible. **Identify the row using unique business data and then perform the action within that row.**

Example:

```java
driver.findElement(
        By.xpath("//table/tbody/tr[td[1]='David']//button[text()='Edit']")
).click();
```

This is one of the most useful Selenium WebTable patterns for automation projects and interviews.
