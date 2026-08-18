## `WebElements/README.md`

```markdown
# WebElements

## 1. What is a WebElement?

A `WebElement` represents an HTML element on a web page (button, input, link, etc.).

## 2. Finding Elements

```java

WebElement element = driver.findElement(By.id("username"));
List<WebElement> elements = driver.findElements(By.className("item"));
3. Common WebElement Methods
Method

Description

click()

Clicks the element

sendKeys(String text)

Types text into the element

clear()

Clears input field text

getText()

Returns visible text of the element

getAttribute(String name)

Returns attribute value
4. Working with Dropdowns
Select dropdown = new Select(driver.findElement(By.id("country")));
dropdown.selectByVisibleText("India");
dropdown.selectByValue("IN");
dropdown.selectByIndex(2);

5. Working with Checkboxes/Radio Buttons
WebElement checkbox = driver.findElement(By.id("terms"));
if (!checkbox.isSelected()) {
    checkbox.click();
}
6. Actions Class (Mouse/Keyboard Actions)
Actions actions = new Actions(driver);
actions.moveToElement(element).click().perform();
actions.dragAndDrop(source, target).perform();
