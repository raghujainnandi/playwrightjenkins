# OpenCart Playwright Automation

A comprehensive end-to-end automation test suite for the OpenCart e-commerce platform built using **Playwright** and **TypeScript**. This project demonstrates modern testing practices including Page Object Model (POM), data-driven testing, and organized test categorization.

## 📋 Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [Running Tests](#running-tests)
- [Test Organization](#test-organization)
- [Page Object Model](#page-object-model)
- [Test Data](#test-data)
- [Reports](#reports)
- [Configuration](#configuration)
- [Contributing](#contributing)

## 🎯 Overview

This automation suite provides comprehensive testing coverage for OpenCart functionality including:

- **User Authentication** - Login and Logout operations
- **User Registration** - Account creation and validation
- **Product Search** - Search functionality and filtering
- **Shopping Cart** - Add/Remove items and cart management
- **Checkout Process** - Complete end-to-end purchase flow
- **Account Management** - User profile and account operations

## 🛠️ Tech Stack

- **Playwright** (v1.52.0) - Modern browser automation framework
- **TypeScript** - Type-safe test development
- **Node.js** - Runtime environment
- **Allure Reports** - Advanced test reporting
- **Faker.js** - Random test data generation
- **CSV-Parse** - CSV file parsing for data-driven tests
- **XLSX** - Excel file support

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (v16 or higher) - [Download](https://nodejs.org/)
- **npm** (v8 or higher) - Comes with Node.js
- **Git** - [Download](https://git-scm.com/)
- **VS Code** (recommended) - [Download](https://code.visualstudio.com/)

## 💻 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/pavanoltraining/opencartplaywright.git
cd opencartplaywright
```

### 2. Install Dependencies

```bash
npm install
```

This will install all required dependencies including Playwright, TypeScript, and reporting tools.

### 3. Install Playwright Browsers

```bash
npx playwright install
```

### 4. Configure Environment (Optional)

Update `test.config.ts` if you want to change:
- Application URL
- Test credentials
- Product details for testing

```typescript
appUrl="https://tutorialsninja.com/demo/" // Target URL
email="pavanol@abc.com"                    // Test email
password="test@123"                        // Test password
```

## 📁 Project Structure

```
opencartplaywright/
├── pages/                      # Page Object Model (POM)
│   ├── HomePage.ts
│   ├── LoginPage.ts
│   ├── RegistrationPage.ts
│   ├── SearchResultsPage.ts
│   ├── ProductPage.ts
│   ├── ShoppingCartPage.ts
│   ├── CheckoutPage.ts
│   ├── MyAccountPage.ts
│   └── LogoutPage.ts
│
├── tests/                      # Test Specifications
│   ├── Login.spec.ts
│   ├── Logout.spec.ts
│   ├── AccountRegistration.spec.ts
│   ├── SearchProduct.spec.ts
│   ├── AddToCart.spec.ts
│   ├── LoginDataDriven.spec.ts
│   └── EndToEndTest.spec.ts
│
├── utils/                      # Utility Functions
│   ├── dataProvider.ts         # CSV/JSON data parsing
│   └── randomDataGenerator.ts  # Faker.js integration
│
├── testdata/                   # Test Data Files
│   ├── logindata.csv
│   └── logindata.json
│
├── playwright.config.ts        # Playwright Configuration
├── test.config.ts              # Application Configuration
├── package.json                # Project Dependencies
└── README.md                   # This file
```

## 🧪 Running Tests

### Run All Tests

```bash
npm test
```

### Run Tests by Category (Tags)

The project uses Playwright tags for test categorization:

#### Master Suite (Quick Smoke Tests)
```bash
npm run test:master
```

#### Sanity Tests
```bash
npm run test:sanity
```

#### Regression Tests
```bash
npm run test:regression
```

#### Data-Driven Tests
```bash
npm run test:datadriven
```

#### End-to-End Tests (Headed Mode)
```bash
npm run test:end-to-end
```

#### Debug Mode
```bash
npm run test:sanity:debug
```

### Custom Test Execution

Run specific test file:
```bash
npx playwright test tests/Login.spec.ts
```

Run tests with headed mode (see browser):
```bash
npx playwright test --headed
```

Run tests in debug mode:
```bash
npx playwright test --debug
```

## 🏷️ Test Organization

Tests are organized using tags for easy filtering and categorization:

| Tag | Description | Use Case |
|-----|-------------|----------|
| `@master` | Critical smoke tests | Quick validation |
| `@sanity` | Basic functionality tests | Daily verification |
| `@regression` | Comprehensive test suite | Full test coverage |
| `@datadriven` | Data-driven parameterized tests | Multiple scenarios |
| `@end-to-end` | Complete user workflows | Full journey testing |

## 📄 Page Object Model

This project follows the **Page Object Model** pattern to maintain clean and maintainable test code.

### Example Page Object (HomePage.ts)

```typescript
export class HomePage {
    private readonly page: Page;
    private readonly lnkMyAccount: Locator;
    private readonly lnkRegister: Locator;

    constructor(page: Page) {
        this.page = page;
        this.lnkMyAccount = page.locator('span:has-text("My Account")');
        this.lnkRegister = page.locator('a:has-text("Register")');
    }

    async clickMyAccount() {
        await this.lnkMyAccount.click();
    }

    async clickRegister() {
        await this.lnkRegister.click();
    }
}
```

### Example Test Usage

```typescript
test('User Registration', async ({ page }) => {
    const homePage = new HomePage(page);
    const registrationPage = new RegistrationPage(page);

    await page.goto(config.appUrl);
    await homePage.clickMyAccount();
    await homePage.clickRegister();
    await registrationPage.fillRegistrationForm(userData);
});
```

## 📊 Test Data

Test data is managed through multiple formats:

### JSON Format (testdata/logindata.json)
```json
{
  "validUsers": [
    {
      "email": "user1@test.com",
      "password": "password123"
    }
  ]
}
```

### CSV Format (testdata/logindata.csv)
```csv
email,password
user1@test.com,password123
user2@test.com,password456
```

### Data-Driven Testing

Use the `dataProvider.ts` utility to read and parse test data:

```typescript
const testData = await dataProvider.getLoginData();
testData.forEach(data => {
    test(`Login with ${data.email}`, async () => {
        // Test logic
    });
});
```

## 📈 Reports

### HTML Report

Generated automatically after test runs. View with:
```bash
npx playwright show-report
```

### Allure Reports

Generate Allure report:
```bash
allure generate ./allure-results -o ./allure-report
allure open ./allure-report
```

Reports include:
- Test execution timeline
- Pass/Fail statistics
- Screenshots on failure
- Video recordings (retained on failure)
- Detailed error logs

## ⚙️ Configuration

### Playwright Configuration (playwright.config.ts)

Key settings:

```typescript
{
  timeout: 30 * 1000,           // Test timeout (30 seconds)
  testDir: './tests',           // Test file location
  fullyParallel: true,          // Parallel execution
  retries: 1,                   // Retry failed tests
  workers: 1,                   // Single worker (can increase)
  
  use: {
    trace: 'on-first-retry',    // Trace on failure
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
    viewport: { width: 1280, height: 720 }
  }
}
```

### Application Configuration (test.config.ts)

Update test credentials and URLs:

```typescript
export class TestConfig {
    appUrl = "https://tutorialsninja.com/demo/";
    email = "pavanol@abc.com";
    password = "test@123";
    productName = "MacBook";
    productQuantity = "2";
    totalPrice = "$1,204.00";
}
```

## 🚀 Best Practices

- **Use Page Objects** - Maintain clean test code with POM pattern
- **Organize Tests** - Use tags for categorization and filtering
- **Data-Driven Testing** - Parameterize tests for multiple scenarios
- **Meaningful Assertions** - Clear and specific assertions
- **Error Handling** - Proper timeout and error management
- **Keep Tests Independent** - Each test should run independently
- **Use Descriptive Names** - Clear test names indicate purpose

## 🐛 Troubleshooting

### Tests Not Running
```bash
npm install
npx playwright install
```

### Port Already in Use
- Ensure no other instances are running on the target port
- Update `test.config.ts` with correct URL

### Element Not Found
- Verify selectors in Page Objects
- Use Playwright Inspector: `npx playwright codegen <URL>`
- Check for dynamic content or waits

### Timeout Issues
- Increase timeout in `playwright.config.ts`
- Add explicit waits for dynamic content
- Check network connectivity

## 📚 Resources

- [Playwright Documentation](https://playwright.dev/)
- [Playwright Best Practices](https://playwright.dev/docs/best-practices)
- [TypeScript Documentation](https://www.typescriptlang.org/docs/)
- [OpenCart Platform](https://www.opencart.com/)

## 📝 License

ISC License - See LICENSE file for details

## 👤 Author

**Pavan Oltraining**

- GitHub: [@pavanoltraining](https://github.com/pavanoltraining)
- Repository: [opencartplaywright](https://github.com/pavanoltraining/opencartplaywright)

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### Steps to Contribute

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📞 Support

For issues, questions, or suggestions, please open an issue on the [GitHub Issues](https://github.com/pavanoltraining/opencartplaywright/issues) page.

---

**Last Updated:** May 2026  
**Playwright Version:** 1.52.0  
**Node Version:** 16+
