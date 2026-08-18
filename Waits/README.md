## `Waits/README.md`

```markdown
# Waits

## 1. Why Waits are Needed

Web pages load dynamically, and elements may not be immediately available.
Waits help Selenium synchronize with page load and element readiness.

## 2. Types of Waits

### Implicit Wait

Applies globally to all `findElement` calls.

```java

driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
Explicit Wait
Waits for a specific condition on a specific element.
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
WebElement element = wait.until(ExpectedConditions.elementToBeClickable(By.id("submit")));
Fluent Wait
Customizable polling interval and ignored exceptions.
Wait<WebDriver> wait = new FluentWait<>(driver)
        .withTimeout(Duration.ofSeconds(30))
        .pollingEvery(Duration.ofSeconds(5))
        .ignoring(NoSuchElementException.class);

WebElement element = wait.until(driver -> driver.findElement(By.id("submit")));
3. Common ExpectedConditions
Condition

Description

visibilityOfElementLocated

Element is present and visible

elementToBeClickable

Element is visible and enabled

presenceOfElementLocated

Element exists in DOM

alertIsPresent

Alert is present

titleIs / titleContains

Page title matches
4. Best Practices
Avoid Thread.sleep() — it's a hardcoded, unreliable wait
Never mix implicit and explicit waits (can cause inconsistent timeouts)
Prefer explicit waits for dynamic elements

