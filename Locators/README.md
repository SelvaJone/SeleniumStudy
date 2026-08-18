## `Locators/README.md`

```markdown
# Locators

## 1. What are Locators?

Locators are strategies used to find (locate) elements on a web page.

## 2. Types of Locators

| Locator | Example | Description |
|---|---|---|
| ID | `driver.findElement(By.id("username"))` | Fastest and most reliable |
| Name | `driver.findElement(By.name("email"))` | Uses the `name` attribute |
| Class Name | `driver.findElement(By.className("btn-primary"))` | Uses the `class` attribute |
| Tag Name | `driver.findElement(By.tagName("input"))` | Uses HTML tag |
| Link Text | `driver.findElement(By.linkText("Sign In"))` | Exact link text |
| Partial Link Text | `driver.findElement(By.partialLinkText("Sign"))` | Partial link text match |
| CSS Selector | `driver.findElement(By.cssSelector("#login .btn"))` | Flexible, fast |
| XPath | `driver.findElement(By.xpath("//input[@id='username']"))` | Most flexible, slower |

## 3. CSS Selector Examples

```css

#id
.class
tag[attribute='value']
tag:nth-child(2)
 XPath Examples
//tag[@attribute='value']
//tag[contains(@attribute,'partial')]
//tag[text()='Exact Text']
//parent/child
//parent//descendant
5. Best Practices
Prefer ID and CSS Selector over XPath for performance
Avoid using absolute XPath (/html/body/div[1]/...) — it's fragile
Use relative XPath and unique attributes when possible
