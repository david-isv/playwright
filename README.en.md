## Table of Contents

1. [Playwright API Overview 🌍](#playwright-api-overview)
2. [More than a browser automation tool 🌟](#more-than-a-browser-automation-tool)
3. [Jest-like Web-first Assertions 🔧](#jest-like-web-first-assertions)
4. [Playwright Test 🟢](#playwright-test)
5. [Scaling and Sharding with Playwright 🏗️](#scaling-and-sharding-with-playwright)
6. [Test Generation with Codegen ✍️](#test-generation-with-codegen)
7. [Using Locators with getByRole 🎯](#using-locators-with-getbyrole)
8. [Auto-waiting: Handling Pop-ups 🕒](#auto-waiting-handling-pop-ups)
9. [Variant 1: Using Custom Attributes (`data-testid`) 🎯](#variant-1-using-custom-attributes-data-testid)
10. [Variant 2: Using ARIA Roles 🎯](#variant-2-using-aria-roles)
11. [Chaînage de Locators et Sélection par Texte 📝](#chaînage-de-locators-et-sélection-par-texte)

### Change to English version FR

[Switch to the English version](README.md)

### Playwright API Overview 🌍

Playwright provides a unified API allowing developers to interact with multiple browsers. It supports three major engines:

- **Chromium (Chrome) 🟢**
- **Firefox 🦊**
- **WebKit (Safari 🍎)**

Each browser is accessible via a dedicated API, but Playwright's interface abstracts these details. This allows you to write a single set of tests that run across all supported browsers, ensuring cross-platform compatibility and simplifying testing.

---

### More than a browser automation tool 🌟

Playwright is not just designed for browser automation; it is also a powerful framework for conducting end-to-end (E2E) tests ✅. It lets you simulate real-world user scenarios, interact with web interfaces, and validate the behavior of applications at every step.

With Playwright, you can:

- **Automate user interactions** (clicks, typing, navigation) 🖱️
- Test complex applications across different browsers in parallel 🕹️
- **Simulate network conditions**, **screen dimensions**, and specific **geolocations** 📱🌍
- Capture **screenshots** or **record videos** of test sessions 📸

---

### Jest-like Web-first Assertions 🔧

Playwright offers **web-first assertions** similar to **Jest**, making it easy to validate the state and properties of elements on a page:

- **Element states:** `toBeChecked()`, `toBeVisible()`, `toBeInViewport()`
- **Properties:** `toHaveClass()`, `toHaveId()`, `toHaveText()`
- **Pages:** `toHaveTitle()`, `toHaveURL()`, `toHaveScreenshot()`

---

### Playwright Test 🟢

Playwright includes its own testing framework, offering several key features:

- Test generation with **codegen** ✍️
- Extensions for **VSCode** and **JetBrains** (paid)
- Automated **test reports** 📊
- **Parallel test execution** for faster runs ⚡

Although Playwright is designed for **JavaScript/TypeScript**, its usage is ***not mandatory***!

---

### Scaling and Sharding with Playwright 🏗️

- **Local scaling:** You can run multiple browsers in parallel using several workers. For example:
  ```bash
  npx playwright test --workers 2
  ```
  This speeds up testing by distributing tasks across multiple browser instances.

- **Sharding in CI:** When running in CI, you can distribute tests across multiple jobs. For example, by allocating specific groups of tests to different workers:
  ```bash
  npx playwright test --shard=1/3
  npx playwright test --shard=2/3
  npx playwright test --shard=3/3
  ```
  This allows you to assign specific test sets (e.g., the first 30 tests to worker 1, the next 40 to worker 2, etc.), optimizing processing time.

---

### Test Generation with Codegen ✍️

Playwright offers a **codegen** tool that records your interactions on a page and automatically generates test code. To use it:

```bash
npx playwright codegen
```

All your actions (clicks, typing, navigation) are converted into scripts, simplifying the creation of test scenarios without manually writing each step.

---

### Using Locators with getByRole 🎯

Playwright allows you to precisely target elements using **locators** and the `getByRole` method. By defining ARIA roles or attributes in advance, you simplify the creation of accessible and robust tests.

### Pre-defining roles and attributes in HTML 💻
```html
<button role="button" aria-label="Submit form">Submit</button>
```

### Test with Playwright 🚀
You can then target this element based on its role and ARIA attributes:

```javascript
const submitButton = page.getByRole('button', { name: 'Submit form' });
await submitButton.click();
```

### Using Custom Attributes 🔧
With attributes like `data-testid`:

```html
<button data-testid="submit-button">Submit</button>
```

And in the Playwright test:

```javascript
const submitButton = page.locator('[data-testid="submit-button"]');
await submitButton.click();
```

---

### Auto-waiting: Handling Pop-ups 🕒

Playwright simplifies handling interactions with dynamic elements like **pop-ups**. Thanks to auto-waiting, Playwright automatically waits for elements to be ready before performing an action, avoiding errors caused by elements that appear or disappear asynchronously.

#### Example: Interaction with a pop-up

---

### Variant 1: Using Custom Attributes (`data-testid`) 🎯

Playwright allows you to use custom attributes like `data-testid` to target elements more precisely.

#### Example:
```javascript
// Click a button that displays the pop-up, using a custom attribute
await page.click('[data-testid="show-popup-button"]');

// Wait for the pop-up to become visible, then interact with it
await page.waitForSelector('[data-testid="popup-dialog"]', { state: 'visible' });
await page.fill('[data-testid="popup-input"]', 'example');

// Close the pop-up using a specific button, based on a custom attribute
await page.click('[data-testid="close-popup-button"]');

// Wait for the pop-up to be hidden
await page.waitForSelector('[data-testid="popup-dialog"]', { state: 'hidden' });
```

---

### Variant 2: Using ARIA Roles 🎯

Playwright allows you to target elements using **ARIA roles**, making your tests more robust and accessible.

#### Example:
```javascript
// Click the button that displays the pop-up, based on the ARIA role "button"
await page.getByRole('button', { name: 'Open Popup' }).click();

// Wait for the pop-up with the role "dialog" to become visible and interact with it
await page.getByRole('dialog', { name: 'Popup Dialog' }).waitForElementState('visible');
await page.getByRole('textbox', { name: 'Popup Input' }).fill('example');

// Close the pop-up by clicking the close button, based on the role "button"
await page.getByRole('button', { name: 'Close Popup' }).click();

// Wait for the pop-up to be hidden after closing
await page.getByRole('dialog', { name: 'Popup Dialog' }).waitForElementState('hidden');
```

---

### Chaînage de Locators et Sélection par Texte 📝

Playwright allows you to chain locators and target elements with precision. In this example, we combine a CSS selector and specific text, then target another element directly via its text.

#### Example:
```javascript
await page
  .locator('.shoes-card_card', { hasText: 'Doc Martins' }) // Select an element with the CSS class and text 'Doc Martins'
  .locator('text=SEE PRODUCT DETAILS') // Inside, select an element with the text 'SEE PRODUCT DETAILS'
  .click(); // Click the button
```

**Explanation**:
1. **First locator**: Selects an element using the **CSS selector** (`.shoes-card_card`) that **contains** the text `'Doc Martins'`.
2. **Second locator**: Within this element, selects a sub-element whose **text is exactly** `'SEE PRODUCT DETAILS'`.
3. **Action**: Once the element is found, it clicks on it.
