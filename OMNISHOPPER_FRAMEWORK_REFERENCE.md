# Omni Shopper Framework Conventions - Reference for Survey Modernization Migration

This document provides a complete, self-contained reference for the OmniShopper UI automation framework structure and patterns. It can be used standalone by developers with zero prior context on the project.

---

## 1. Folder Structure

Complete directory tree of the automated testing structure:

```
src/lib/
├── common/
│   ├── components/              # Reusable UI component page objects
│   │   ├── data-selector/
│   │   │   ├── data-selector.ts
│   │   │   ├── data-selector-resources.json
│   │   │   └── people-menu-constant.ts
│   │   ├── export/
│   │   │   ├── export.ts
│   │   │   └── export-resources.json
│   │   ├── format-panel/
│   │   │   ├── formatPanel.ts
│   │   │   └── format-panel-resources.json
│   │   ├── insights-panel/
│   │   │   ├── insights-panel.ts
│   │   │   └── insights-panel-resources.json
│   │   └── nlsn-pin-menu/
│   │       ├── nlsn-pin-menu.ts
│   │       └── nlsn-pin-menu-resources.json
│   │
│   ├── interfaces/              # TypeScript interfaces and type definitions
│   │   └── iElementResource.ts  # PageElement interface wrapper
│   │
│   ├── models/                  # Data models and types
│   │   └── data-model.ts        # Type definitions for test data structures
│   │
│   ├── pages/                   # Full page objects (application pages/flows)
│   │   ├── choose-a-template-page/
│   │   │   ├── choose-a-template-page.ts
│   │   │   └── choose-a-template-resources.json
│   │   ├── common-test-case/
│   │   │   ├── common-test-case.ts
│   │   │   └── common-test-cases-resource.json
│   │   ├── common-test-resources.json  # Shared across all tests
│   │   ├── find-my-stuff-page/
│   │   │   ├── find-my-stuff-page.ts
│   │   │   └── find-my-stuff-resources.json
│   │   ├── landing-page/
│   │   │   ├── landing-page.ts
│   │   │   └── landing-page-resources.json
│   │   ├── login-page/
│   │   │   ├── login-page.ts
│   │   │   └── loginPageResources.json
│   │   ├── monitor-my-business.page.ts
│   │   ├── profile-page/
│   │   │   ├── profile-page.ts
│   │   │   └── profile-page-resources.json
│   │   ├── quick-look-page/
│   │   │   ├── quick-look-page.ts
│   │   │   └── quick-look-resources.json
│   │   ├── report-flow-page/
│   │   │   ├── report-flow-page.ts
│   │   │   └── report-flow-page-resources.json
│   │   ├── report-page/
│   │   │   ├── report-page.ts
│   │   │   └── report-page-resources.json
│   │   ├── configure-panel/
│   │   │   ├── configure-panel.ts
│   │   │   └── configure-panel-resources.json
│   │   └── snapshot-page/
│   │       ├── snapshot-page.ts
│   │       └── snapshot-page-resources.json
│   │
│   ├── utils/                   # Shared utility functions and helpers
│   │   ├── actions.ts           # Action helpers (deprecated, superseded by @gfk/gta)
│   │   ├── assert-utils.ts      # Assertion helpers
│   │   ├── base-page-factory.ts # Factory for creating BasePage instances
│   │   ├── configurationDetails.ts
│   │   ├── element-utils.ts     # Element utilities
│   │   ├── env-utils.ts         # Environment utilities
│   │   ├── global-setup.ts      # Playwright global setup (authentication)
│   │   ├── helper-service.ts    # Test data lookup helpers
│   │   ├── locator-utils.ts     # Locator resolution helpers
│   │   ├── page-utils.ts        # Page-level utilities
│   │   └── resource-manager.ts  # Resource/selector management
│   │
│   ├── constants/               # Framework-wide constants
│   │   ├── file-paths.ts        # File path constants
│   │   ├── optional-parameter-types.ts  # TypeScript types for Playwright APIs
│   │   └── time-out-constants.ts # All timeout values
│   │
│   ├── playwright/              # Playwright-specific configuration
│   │   ├── config.ts            # Runtime environment config
│   │   ├── reporters/           # Custom reporters
│   │   │   └── lt-session-reporter.ts  # LambdaTest session reporter
│   │   ├── setup/               # Playwright setup/teardown
│   │   │   └── globalTeardown.ts
│   │   └── grafana-reporter.ts  # Grafana metrics reporter
│   │
│   └── tests/                   # Test files, organized by report type
│       ├── components/          # Component-level tests
│       ├── data/
│       │   └── jiraUpdateData.ts # Test data for Jira integrations
│       ├── reports/             # Report-specific test suites
│       │   ├── assortment-optimizer/
│       │   │   ├── assortment-optimizer.spec.ts
│       │   │   ├── test-data/
│       │   │   │   ├── dev-test-data.json
│       │   │   │   ├── prod-test-data.json
│       │   │   │   └── localization-data.json
│       │   │   ├── assertion-data/
│       │   │   │   └── assertion-data.json
│       │   │   └── resources/
│       │   ├── basket-analysis/
│       │   │   ├── basket-analysis.spec.ts
│       │   │   ├── basket-analysis.ts
│       │   │   ├── test-data/
│       │   │   ├── assertion-data/
│       │   │   └── resources/
│       │   ├── consumer-distribution-analysis/
│       │   ├── rolling-shifting/
│       │   ├── trial-and-repeat-portfolio/
│       │   └── [40+ other report types]/
│       │
│       ├── test-cases/          # Cross-report test case helpers
│       │   ├── common-test-cases.spec.ts
│       │   └── common-test-cases-helper-service.ts
│       │
│       └── testSuites/          # Test suite orchestrators
│           ├── report-suite/
│           │   └── single-report-suite.spec.ts
│           └── regression-suites/
│               └── omni-regression-suite.spec.ts
```

**Purpose of each folder:**
- **components/**: Reusable components like DataSelector, ExportPanel, FormatPanel used across multiple reports
- **pages/**: Full page objects representing distinct application pages/flows
- **utils/**: Shared infrastructure: factory patterns, resource management, helpers
- **constants/**: Framework-wide constants (timeouts, paths)
- **playwright/**: Playwright config, reporters, global setup/teardown
- **tests/reports/**: Each report has its own subfolder with .spec.ts, test-data/, assertion-data/
- **tests/testSuites/**: Test orchestration across multiple reports

---

## 2. Naming Conventions

### Page Object File Naming Pattern

**Pattern:** `{feature-name}.ts` (kebab-case, descriptive)

**Examples:**
- `login-page.ts` — Login page object
- `quick-look-page.ts` — QuickLook preview panel page object
- `report-page.ts` — Report display page object
- `common-test-case.ts` — Common/shared test case base class
- `data-selector.ts` — Data selector component

**Rules:**
- Use kebab-case (lowercase with hyphens)
- Include descriptive feature name
- Suffix with `.ts` (TypeScript source)
- If in a subdirectory, may drop the redundant suffix: `login-page/login-page.ts` is standard but folder name also reflects content

### Component Class Naming Pattern

**Pattern:** `PascalCase` class names

**Examples:**
```typescript
export class LoginPage { }           // Page object
export class DataSelector { }        // Component object
export class CommonTestCase { }      // Base test class
export class QuickLookPage { }       // Feature page
export class InsightsPanel { }       // Component
export class FormatPanel { }         // Component
export class ReportPage { }          // Report display
export class ExportComponent { }     // Export feature
```

**Rules:**
- Always PascalCase
- Noun or verb-noun describing the element
- No suffixes needed beyond descriptive name
- Match the primary export in the file

### Resource JSON File Naming Pattern

**Pattern:** `{feature-name}-resources.json`

**Examples:**
- `loginPageResources.json` — Login page selectors (note: this one uses camelCase for legacy reasons)
- `data-selector-resources.json` — Data selector component selectors
- `report-page-resources.json` — Report page selectors
- `common-test-resources.json` — Common/shared selectors
- `quick-look-resources.json` — QuickLook page selectors
- `format-panel-resources.json` — Format panel selectors
- `insights-panel-resources.json` — Insights panel selectors

**Rules:**
- Use kebab-case or camelCase (both present in codebase)
- End with `-resources.json` or `Resources.json`
- Co-located with corresponding `.ts` file in same directory
- Should have one resource file per component/page object

### Test Spec File Naming Pattern

**Pattern:** `{report-name}.spec.ts`

**Examples:**
- `basket-analysis.spec.ts` — Basket Analysis report tests
- `rolling-shifting-product.spec.ts` — Rolling Shifting (Product dimension) tests
- `trial-and-repeat-portfolio.spec.ts` — Trial & Repeat Portfolio tests
- `consumer-distribution-analysis-type.spec.ts` — Consumer Distribution Analysis (Type variant) tests
- `common-test-cases.spec.ts` — Cross-report common test cases

**Rules:**
- Use kebab-case, matching report name
- End with `.spec.ts` (Playwright convention)
- Located in `src/lib/tests/reports/{report-name}/`
- One spec file per report (or per significant variant)

### Method Naming Conventions

**Pattern:** camelCase, action-oriented verbs

**Examples in page objects:**
```typescript
// From LoginPage
public async login(url: string): Promise<void>
private async acceptTermsIfPresent(): Promise<void>

// From DataSelector
public async clickMadlibDimension(dimension: string): Promise<void>
public async selectFolder(folderName: string): Promise<void>
public async checkSelection(selection: string): Promise<void>
public async setMadlibSelection(dimensionSelection: {...}): Promise<void>
public async enterTextInSearchField(inputText: string): Promise<void>

// From CommonTestCase
public async clickRunButton(): Promise<void>
public async waitForReportRunCompletion(): Promise<void>
public async assertNoDataFound(): Promise<void>
```

**Rules:**
- Start with verb: `click`, `select`, `enter`, `validate`, `assert`, `get`, `set`
- Use camelCase
- Async methods return `Promise<void>` or `Promise<T>`
- Private methods prefixed with `private`
- Getter methods start with `get` or `is`

### Test Case Naming

**Pattern:** Descriptive, hierarchical using `test.describe()` and `test()`

**Example:**
```typescript
test.describe("Basket analysis", async () => {
  test("QuickLook: Should display correct preview information when template is selected", {
    tag: "@QuickLook"
  }, async () => { /* test body */ });

  test("Insights Panel: Should validate business questions and report metadata", {
    tag: "@Insights"
  }, async () => { /* test body */ });

  test("Format Panel: Should toggle data labels correctly", {
    tag: "@FormatPanel"
  }, async () => { /* test body */ });
});
```

**Rules:**
- Outer `test.describe()` = Report name
- Inner `test()` = Specific test scenario
- Use prefix with colon for feature area: `@QuickLook:`, `@Insights:`, `@FormatPanel:`, `@Export:`, `@Run:`, `@Save:`
- Describe expected behavior, not test mechanics

---

## 3. Constants and Timeouts

Complete content of the constants file:

### File: `src/lib/constants/time-out-constants.ts`

```typescript
/**
 * Timeout constant for general test operations (2 minutes)
 */
export const MAX_TEST_TIMEOUT = 2 * 60 * 1000; // 120,000 ms

/**
 * Timeout constant for Run function (5 minutes)
 */
export const RUN_TEST_TIMEOUT = 5 * 60 * 1000; // 300,000 ms

/**
 * Timeout constant for navigation operations (1 minute)
 */
export const NAVIGATION_TIMEOUT = 60 * 1000; // 60,000 ms

/**
 * Timeout constant for report title to appear after dataset selection (2 minutes)
 */
export const TITLE_APPEARANCE_TIMEOUT = 2 * 60 * 1000; // 120,000 ms

/**
 * Timeout constant for small actions/assertions (5 seconds)
 */
export const SMALL_TIMEOUT = 5 * 1000; // 5,000 ms

/**
 * Timeout constant for polling intervals (300 milliseconds)
 */
export const POLLING_INTERVAL = 300; // 300 ms

/**
 * Timeout constant for element wait operations (15 seconds)
 */
export const ELEMENT_WAIT_TIMEOUT = 15 * 1000; // 15,000 ms

/**
 * Timeout constant for dataset selector operations (3 seconds)
 */
export const DATASET_SELECTOR_TIMEOUT = 3 * 1000; // 3,000 ms

/**
 * Delay for dataset processing operations (2 seconds)
 */
export const DATASET_PROCESSING_DELAY = 2 * 1000; // 2,000 ms

/**
 * Delay for hover activation and CSS pseudo-element rendering (1 second)
 */
export const HOVER_ACTIVATION_DELAY = 1 * 1000; // 1,000 ms

/**
 * Delay for madlib component appearance (3 seconds)
 */
export const MADLIB_APPEARANCE_DELAY = 3 * 1000; // 3,000 ms

/**
 * Delay for UI state updates and DOM stabilization (500 milliseconds)
 */
export const UI_STATE_UPDATE_DELAY = 500; // 500 ms
```

### File: `src/lib/constants/file-paths.ts`

```typescript
export abstract class FilePathConstants {
  private constructor() {}
  public static readonly PLAYWRIGHT_AUTH: string = "./.auth/";
}
```

### File: `src/lib/constants/optional-parameter-types.ts`

```typescript
import { Locator, Page } from "@playwright/test";

export type LocatorOptions = Parameters<Page['locator']>[1];
export type FillOptions = Parameters<Locator['fill']>[1];
export type ClickOptions = Parameters<Locator['click']>[0] & {
    loadState?: WaitForLoadStateOptions;
  };
export type WaitForLoadStateOptions = Parameters<Page['waitForLoadState']>[0];
export type TimeoutOption = { timeout?: number };
export type SoftOption = { soft?: boolean };
export type ExpectOptions = TimeoutOption & SoftOption & MessageOrOptions;
export type MessageOrOptions = string | { message?: string };
export type NavigationOptions = Parameters<Page['reload']>[0];
```

### Real Usage Examples

**Example 1: In CommonTestCase.ts — Waiting for report execution**

```typescript
import {
  MAX_TEST_TIMEOUT,
  RUN_TEST_TIMEOUT,
  SMALL_TIMEOUT,
  DATASET_PROCESSING_DELAY
} from "../../../constants/time-out-constants";

public async clickRunButton(): Promise<void> {
  // Wait for disabled button to become hidden (enabled state)
  await runButtonDisable.waitFor(MAX_TEST_TIMEOUT, "hidden");
  
  // Wait for enabled button to be visible
  await runButtonEnable.waitFor(MAX_TEST_TIMEOUT, "visible");
  
  // Click and wait for navigation
  await runButtonEnable.click();
  
  // Allow time for report to start running
  await this.webActionHandler.delay(DATASET_PROCESSING_DELAY);
}
```

**Example 2: In DataSelector.ts — Dimension interaction with search**

```typescript
import { MAX_TEST_TIMEOUT, SMALL_TIMEOUT } from "../../../constants/time-out-constants";

public async setMadlibFolder(folderName: string, searchReq = true): Promise<void> {
  const searchIcon = this.webActionHandler.getWebElement(
    this.resourceManager.getResource("searchIcon", DS_COMPONENT_NAME)
  );
  
  const isVisible = await searchIcon.isVisible();
  if (isVisible) {
    await this.clickSearchIcon();
    await this.enterTextInSearchField(folderName);
  } else {
    // If search not visible, add small delay before direct entry
    await this.webActionHandler.delay(SMALL_TIMEOUT);
    await this.enterTextInSearchField(folderName);
  }
  
  await this.selectFolder(folderName);
}
```

**Example 3: In LoginPage.ts — Accept terms with timeout**

```typescript
import {
  MAX_TEST_TIMEOUT,
  SMALL_TIMEOUT,
  NAVIGATION_TIMEOUT
} from "../../../constants/time-out-constants";

private async acceptTermsIfPresent(): Promise<void> {
  const checkbox = this.webActionHandler.getWebElement(
    this.resourceManager.getResource("termsAcceptCheckbox", LOGIN_PAGE_COMPONENT_NAME)
  );
  
  // Short timeout — checkbox may not exist
  await checkbox.waitFor(SMALL_TIMEOUT, "visible");
  
  // Accept and navigate
  await checkbox.click(true);
  
  // Wait for navigation to complete after terms
  await this.webActionHandler.delay(SMALL_TIMEOUT);
}
```

---

## 4. JSON Resource Import Pattern

### Import Syntax (ES6 with JSON Module Resolution)

All resource files are imported as ES6 modules with TypeScript strict mode:

```typescript
import * as loginPageResources from "./loginPageResources.json";
import * as dsResources from "../data-selector/data-selector-resources.json";
import * as commonResources from "../../pages/common-test-resources.json";
```

### TypeScript Configuration to Enable JSON Import

**File: `tsconfig.json`**

```json
{
  "compilerOptions": {
    "target": "es2016",
    "module": "commonjs",
    "moduleResolution": "node",
    "resolveJsonModule": true,        // ← CRITICAL: Enables .json imports
    "esModuleInterop": true,
    "strict": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

**Key setting:** `"resolveJsonModule": true` allows TypeScript to treat `.json` files as valid import targets.

### ResourceManager Registration Pattern

**File: `src/lib/common/utils/resource-manager.ts`**

```typescript
import { PageElement } from '@gfk/gta';
import { ElementResource } from '../interfaces/iElementResource';

export class ResourceManager {
    private resources: Record<string, ElementResource>;

    constructor(resources: Record<string, ElementResource>) {
        this.resources = resources;
    }
    
    /**
     * Registers component resources with the registry
     */
    registerResources(componentName: string, resources: ElementResource): void {
        this.resources[componentName] = resources;
    }

    /**
     * Retrieves a web element resource by its name
     */
    getResource(name: string, componentName?: string): PageElement {
        if (componentName && this.resources[componentName]) {
            const element = this.resources[componentName].webElements.find(
                (webElement: PageElement) => webElement.elementName === name
            );
            if (element) return element;
        } else {
            for (const [component, resourceObj] of Object.entries(this.resources)) {
                const element = resourceObj.webElements.find(
                    (x: PageElement) => x.elementName === name
                );
                if (element) return element;
            }
        }
        throw new Error(
            `Web element with name '${name}' not found in the ${componentName ? componentName + ' ' : ''} object repository.`
        );
    }

    /**
     * Retrieves a resource with dynamic text replacement
     * Replaces ${text} placeholder in selectorValue with provided text
     */
    getDynamicResource(name: string, text: string, componentName?: string): PageElement {
        if (!name) {
            throw new Error(`Invalid resource name '${name}' provided to getDynamicResource.`);
        }
        const element = this.getResource(name, componentName);
        if (!element.selectorValue.includes('${text}')) {
            throw new Error(`Web element with name '${name}' does not have a dynamic selector that includes '${text}'.`);
        }
        return {
            ...element,
            selectorValue: element.selectorValue.replace('${text}', text)
        };
    }
}
```

### Complete Example: DataSelector Component

**File: `src/lib/common/components/data-selector/data-selector.ts`**

```typescript
import { BasePage, assert } from "@gfk/gta";
import { ResourceManager } from "../../utils/resource-manager";
import * as commonResources from "../../pages/common-test-resources.json";
import * as dsResources from "../data-selector/data-selector-resources.json";
import { MAX_TEST_TIMEOUT, SMALL_TIMEOUT } from "../../../constants/time-out-constants";
import { CommonTestCase } from "../../pages/common-test-case/common-test-case";

const DS_COMPONENT_NAME = "DataSelector";
const COMMON_COMPONENT_NAME = "Common";

export class DataSelector extends CommonTestCase {
  constructor(basePage: BasePage) {
    super(basePage);
    this.webActionHandler = basePage.getWebActions();
    
    // Initialize ResourceManager with component resources
    this.resourceManager = new ResourceManager({});
    
    // Register multiple resources for the same component
    this.resourceManager.registerResources(DS_COMPONENT_NAME, dsResources);
    this.resourceManager.registerResources(COMMON_COMPONENT_NAME, commonResources);
  }

  /**
   * Clicks on a Madlib dimension element
   */
  public async clickMadlibDimension(dimension: string): Promise<void> {
    try {
      // Retrieve the web element using registered resources
      const dimensionElement = this.webActionHandler.getWebElement(
        this.resourceManager.getResource(dimension, DS_COMPONENT_NAME)
      );

      // Wait until visible and ready
      await dimensionElement.waitFor();
      
      // Click the dimension
      await dimensionElement.click();
    } catch (error) {
      console.error(`Failed to click Madlib dimension '${dimension}':`, error);
      throw new Error(`Error interacting with Madlib dimension '${dimension}': ${error}`);
    }
  }

  /**
   * Selects a folder by dynamic name lookup
   */
  public async selectFolder(folderName: string): Promise<void> {
    // Use getDynamicResource to inject folderName into selector
    const folderElement = this.webActionHandler.getWebElement(
      this.resourceManager.getDynamicResource(
        "dsNavigationItemLabel",
        folderName,
        DS_COMPONENT_NAME
      )
    );
    await folderElement.waitFor();
    await folderElement.click();
  }

  /**
   * Checks an item from selections viewport
   */
  public async checkSelection(selection: string): Promise<void> {
    // Wait for selections viewport
    await this.webActionHandler
      .getWebElement(this.resourceManager.getResource("dsSelectionsViewPort"))
      .waitFor();
    
    // Get dynamic selection checkbox
    const selectionElement = this.webActionHandler.getWebElement(
      this.resourceManager.getDynamicResource(
        "checkboxLabel",
        selection,
        DS_COMPONENT_NAME
      )
    );
    await selectionElement.waitFor();
    await selectionElement.click();
  }
}
```

**File: `src/lib/common/components/data-selector/data-selector-resources.json`**

```json
{
  "name": "Data Selector",
  "webElements": [
    {
      "elementName": "additionalPromptEllipsis",
      "selectorType": "tag",
      "selectorValue": "[data-test-id='ds-promptDataUnselectedPrompt']"
    },
    {
      "elementName": "Product",
      "selectorType": "tag",
      "selectorValue": "[data-test-id='ds-PRDC_label']"
    },
    {
      "elementName": "Retailer",
      "selectorType": "tag",
      "selectorValue": "[data-test-id='ds-RTLR_label']"
    },
    {
      "elementName": "Geography",
      "selectorType": "tag",
      "selectorValue": "[data-test-id='ds-GEO_label']"
    },
    {
      "elementName": "Demographics",
      "selectorType": "tag",
      "selectorValue": "[data-test-id='ds-DEMO_label']"
    },
    {
      "elementName": "dsNavigationItemLabel",
      "selectorType": "tag",
      "selectorValue": "[data-test-id='ds-navigationItemLabel-${text}']"
    },
    {
      "elementName": "checkboxLabel",
      "selectorType": "tag",
      "selectorValue": "[data-test-id='ds-checkbox-label-${text}']"
    },
    {
      "elementName": "dsSelectionsViewPort",
      "selectorType": "tag",
      "selectorValue": "[data-test-id='ds-selections-viewport']"
    },
    {
      "elementName": "searchIcon",
      "selectorType": "tag",
      "selectorValue": "[data-test-id='ds-search-icon']"
    },
    {
      "elementName": "doneIcon",
      "selectorType": "tag",
      "selectorValue": "[data-test-id='ds-done-button']"
    }
  ]
}
```

### Interface: ElementResource

**File: `src/lib/common/interfaces/iElementResource.ts`**

```typescript
import { PageElement } from '@gfk/gta';

export interface ElementResource {
  name: string;
  webElements: PageElement[];
}
```

The `PageElement` interface is from `@gfk/gta` framework and has exactly 3 properties:
- `elementName: string` — Unique identifier for the element
- `selectorType: string` — Type of selector (tag, css, id, xpath, text)
- `selectorValue: string` — The actual selector value (supports `${text}` placeholders)

---

## 5. Common/Shared Utilities Structure

### Folder: `src/lib/common/utils/`

Contains cross-cutting infrastructure used throughout the framework:

#### 1. **resource-manager.ts** — Selector Management

Shown in detail in Section 4 above. Provides resource lookup and dynamic selector injection.

#### 2. **base-page-factory.ts** — Page Object Creation

**Excerpt:**

```typescript
import { BasePage } from '@gfk/gta';
import { chromium } from '@playwright/test';
import fs from 'fs';
import path from 'path';
import { config } from '../../playwright/config';

export class BasePageFactory {
  static async createBasePage(forceHeadless?: boolean): Promise<BasePage> {
    const isLambdaTest = process.env.LAMBDATEST === 'true';
    
    let storageState: string | undefined;
    const authPath = path.resolve(process.cwd(), 'storage/auth.json');

    if (fs.existsSync(authPath)) {
      storageState = authPath;
      console.log('Auth state loaded from storage/auth.json');
    } else {
      console.log('No auth state found - login required');
    }
    
    if (isLambdaTest) {
      // LambdaTest cloud execution
      console.log('Connecting to LambdaTest cloud...');
      
      const browser = await chromium.connect(wsEndpoint);
      const contextOptions: any = { acceptDownloads: true, viewport: { width: 1920, height: 1080 } };
      if (storageState) contextOptions.storageState = storageState;
      
      const context = await browser.newContext(contextOptions);
      const page = await context.newPage();
      return new BasePage(page);
    } else {
      // Local execution
      const browser = await chromium.launch({ args: ['--start-maximized'] });
      const contextOptions: any = { acceptDownloads: true, viewport: null };
      if (storageState) contextOptions.storageState = storageState;
      
      const context = await browser.newContext(contextOptions);
      const page = await context.newPage();
      return new BasePage(page);
    }
  }
}
```

**Usage:**
```typescript
let basePage = await BasePageFactory.createBasePage();
let loginPage = new LoginPage(basePage);
```

#### 3. **helper-service.ts** — Test Data Lookup

**Key functions:**

```typescript
/**
 * Finds dataset configuration by exact name match
 */
export function findDataSetByName(
  testData: TestData | any, 
  datasetName: DatasetContent | string
): DatasetConfig | any | undefined {
  // Handles both array and object-based test data formats
  if (Array.isArray(testData)) {
    const found = testData.find((item: any) => 
      item.dataSet === datasetName || item.id === datasetName
    );
    return found;
  }
  
  if (testData.datasets && typeof testData.datasets === 'object') {
    if (datasetName in testData.datasets) {
      return testData.datasets[datasetName];
    }
  }
  return undefined;
}

/**
 * Retrieves test data by locale/language code
 */
export function getTestDataByLocale(locale: string, localizationData: any): ReportLocalizationData {
  return localizationData.find((item: any) => item.locale === locale);
}

/**
 * Gets dimension selections for a specific dataset
 */
export const getDimensionSelections = (testData: any, key: string, pivot?: string): any => {
  if (!testData[key]) {
    console.warn(`Key "${key}" not found in test data.`);
    return null;
  }
  
  let dimensionSelections;
  if (pivot) {
    dimensionSelections = testData[key]?.dimensionSelections.find(
      (dimension: any) => dimension.pivot === pivot
    );
  } else {
    dimensionSelections = testData[key]?.dimensionSelections[0];
  }
  return dimensionSelections || null;
};
```

#### 4. **assert-utils.ts** — Assertion Helpers

**Excerpt:**

```typescript
import { expect, Expect, Locator } from "@playwright/test";
import { ExpectOptions } from "../../constants/optional-parameter-types";

/**
 * Asserts that an element is visible
 */
export async function expectElementToBeVisible(
  input: string | Locator, 
  options?: ExpectOptions
): Promise<void> {
  const { locator, assert } = getLocatorAndAssert(input, options);
  await assert(locator, options).toBeVisible(options);
}

function getLocatorAndAssert(
  input: string | Locator, 
  options?: SoftOption
): { locator: Locator; assert: Expect } {
  const locator = getFirstLocator(input);
  const assert = getExpectWithSoftOption(options);
  return { locator, assert };
}

function getExpectWithSoftOption(options?: SoftOption): Expect {
  return expect.configure({ soft: options?.soft });
}
```

#### 5. **global-setup.ts** — Authentication Setup

```typescript
/**
 * Global setup function run once before all tests.
 * Handles login and stores authentication state.
 */
async function globalSetup() {
  const loginPage = new LoginPage(basePage);
  await loginPage.login(config.BASE_URL);
  
  // After login, auth.json is saved automatically
}
```

This runs once, generates `storage/auth.json`, which is then used by `BasePageFactory` for all subsequent tests.

---

## 6. Error Handling Pattern

### Error Handling Strategy

The framework uses **try-catch with contextual error enrichment**:

1. **Wrap risky operations** in try-catch
2. **Log before throwing** to aid debugging
3. **Provide descriptive error messages** with context
4. **Preserve stack traces** by re-throwing

### Example 1: LoginPage Error Handling

**File: `src/lib/common/pages/login-page/login-page.ts`**

```typescript
/**
 * Performs login with credentials from environment
 */
public async login(url: string): Promise<void> {
  try {
    console.log("Starting login process...");

    // Navigate
    await this.webActionHandler.navigateTo(url, NAVIGATION_TIMEOUT, "domcontentloaded");
    
    // Fill username
    await this.webActionHandler
      .getWebElement(this.resourceManager.getResource("userName"))
      .enterText(USERNAME);

    // Submit
    const submitButton = await this.webActionHandler.getWebElement(
      this.resourceManager.getResource("submitButton", LOGIN_PAGE_COMPONENT_NAME)
    );
    await submitButton.click();

    // Fill password
    await this.webActionHandler
      .getWebElement(this.resourceManager.getResource("password"))
      .enterText(PASSWORD);

    // Submit (login)
    await submitButton.click();
    
    // If Terms & Conditions page comes, accept it
    await this.acceptTermsIfPresent();

    // Verify login succeeded
    const analyseDataButton = this.webActionHandler.getWebElement(
      this.resourceManager.getResource("analyseMyData")
    );
    await analyseDataButton.waitFor();

    // Save auth state
    await this.webActionHandler.getStorageState("storage/auth.json");

    console.log("Login process completed successfully.");
  } catch (error) {
    console.error("Login process failed:", error && error.name ? error.name : error);
    throw new Error(`Login failed: ${error && error.name ? error.name : error}`);
  } finally {
    // Always close browser
    await this.webActionHandler.closeBrowser();
  }
}

/**
 * Accept Terms & Conditions only if terms checkbox is visible
 */
private async acceptTermsIfPresent(): Promise<void> {
  console.log("Checking for Terms & Conditions page...");

  try {
    const checkbox = this.webActionHandler.getWebElement(
      this.resourceManager.getResource("termsAcceptCheckbox", LOGIN_PAGE_COMPONENT_NAME)
    );
    
    await checkbox.waitFor(SMALL_TIMEOUT, "visible");
    console.log("Terms & Conditions page detected - checkbox is visible");

    // Click checkbox
    await checkbox.click(true);
    console.log("Terms checkbox clicked");

    // Click accept button
    const acceptButton = this.webActionHandler.getWebElement(
      this.resourceManager.getResource("termsAcceptButton", LOGIN_PAGE_COMPONENT_NAME)
    );
    await acceptButton.waitFor(MAX_TEST_TIMEOUT);
    await acceptButton.click();
    console.log("Terms accepted successfully.");
    
  } catch (error) {
    // Terms page not shown or already accepted - not an error
    console.log("Terms page not shown or already accepted, continuing login...");
  }
}
```

### Example 2: DataSelector Error Handling

**File: `src/lib/common/components/data-selector/data-selector.ts`**

```typescript
/**
 * Clicks on a Madlib dimension element
 */
public async clickMadlibDimension(dimension: string): Promise<void> {
  try {
    const dimensionElement = this.webActionHandler.getWebElement(
      this.resourceManager.getResource(dimension, DS_COMPONENT_NAME)
    );

    await dimensionElement.waitFor();
    await dimensionElement.click();
  } catch (error) {
    console.error(`Failed to click Madlib dimension '${dimension}':`, error);
    const errorMessage = error instanceof Error ? error.message : String(error);
    throw new Error(`Error interacting with Madlib dimension '${dimension}': ${errorMessage}`);
  }
}

/**
 * Opens a Madlib folder by searching for its name
 */
public async setMadlibFolder(folderName: string, searchReq = true): Promise<void> {
  try {
    if (searchReq) {
      const searchIcon = this.webActionHandler.getWebElement(
        this.resourceManager.getResource("searchIcon", DS_COMPONENT_NAME)
      );
      
      const isVisible = await searchIcon.isVisible();
      if (isVisible) {
        await this.clickSearchIcon();
        await this.enterTextInSearchField(folderName);
      } else {
        await this.webActionHandler.delay(SMALL_TIMEOUT);
        await this.enterTextInSearchField(folderName);
      }
      
      await this.selectFolder(folderName);
    } else {
      // Search not required in new UI flows
      await this.selectFolder(folderName);
    }
    console.log(`Folder '${folderName}' successfully selected.`);
  } catch (error) {
    console.error(`❌ Failed to select Madlib folder '${folderName}':`, error);
    throw error;  // Re-throw after logging
  }
}
```

### Example 3: ReportPage Error Handling

**File: `src/lib/common/pages/report-page/report-page.ts`**

```typescript
/**
 * Validates that no data found placeholder is displayed
 */
public async assertNoDataFound(): Promise<void> {
  try {
    const noDataElement = this.webActionHandler.getWebElement(
      this.resourceManager.getResource("noDataPlaceholder", REPORTPAGE)
    );
    
    const isVisible = await noDataElement.isVisible();
    if (isVisible) {
      console.log("No data element found and visible. No data found validated successfully");
    } else {
      throw new Error("No data found element is not visible on the page");
    }
  } catch (error) {
    const errorMessage = error instanceof Error ? error.message : String(error);
    throw new Error(`No data found validation failed: ${errorMessage}`);
  }
}

/**
 * Retrieves all facts from dropdown
 */
public async getAllFacts(expectedData: any, componentName: string): Promise<string[]> {
  try {
    const factsElement = this.webActionHandler.getWebElement(
      this.resourceManager.getResource("factsInternalDropdown", componentName)
    );
    await factsElement.waitFor();
    await factsElement.click();

    const factsTexts: string[] = [];
    for (let i = 0; i < expectedData.length; i++) {
      const factsTextElement = this.webActionHandler.getWebElement(
        this.resourceManager.getDynamicResource("factsInternalDropdownOptions", (i+1).toString(), componentName)
      );
      const text = await factsTextElement.getText();
      factsTexts.push(text?.trim() || "");
    }
    return factsTexts;
  } catch (error) {
    throw new Error(`Failed to get all facts texts: ${error}`);
  }
}
```

### Error Handling Patterns Observed

| Pattern | Usage | Example |
|---------|-------|---------|
| **Try-Catch** | Wrap risky operations | Element lookup, wait, click |
| **Contextual Messages** | Add operation details | `Failed to click '${dimension}'` |
| **Conditional Logic** | Handle optional states | Terms page may not appear |
| **Log Before Throw** | Aid debugging | `console.error()` before `throw` |
| **Re-throw** | Preserve stack trace | `catch (error) { throw error; }` |
| **Graceful Degradation** | Continue if non-critical | Skip terms page if not shown |

---

## 7. Test File Structure (Complete Example)

### Complete Real Test Spec File: `basket-analysis.spec.ts`

**File: `src/lib/tests/reports/basket-analysis/basket-analysis.spec.ts`**

```typescript
import test from "@playwright/test";
import {
  findDataSetByName,
  getTestDataByLocale,
} from "../../../common/utils/helper-service";
import { config } from "../../../playwright/config";
import devTestData from "./test-data/dev-test-data.json";
import prodTestData from "./test-data/prod-test-data.json";
import localizationData from "./test-data/localization-data.json";
import { BasePageFactory } from "../../../common/utils/base-page-factory";
import { DataSelector } from "../../../common/components/data-selector/data-selector";
import { CommonTestCase } from "../../../common/pages/common-test-case/common-test-case";
import { InsightsPanel } from "../../../common/components/insights-panel/insights-panel";
import { LandingPage } from "../../../common/pages/landing-page/landing-page";
import { ChooseAtemplatePage } from "../../../common/pages/choose-a-template-page/choose-a-template-page";
import { QuickLookPage } from "../../../common/pages/quick-look-page/quick-look-page";
import { getDataElement } from "../../test-cases/common-test-cases-helper-service";
import { assert } from "@gfk/gta";
import { ExportComponent } from "../../../common/components/export/export";
import { RUN_TEST_TIMEOUT, SMALL_TIMEOUT } from "../../../constants/time-out-constants";
import { FormatPanel } from "../../../common/components/format-panel/formatPanel";
import assertionData from "./assertion-data/assertion-data.json";
import { ReportFlowPage } from "../../../common/pages/report-flow-page/report-flow-page";
import { Snapshot } from "../../../common/pages/snapshot-page/snapshot-page";
import { FindMyStuff } from "../../../common/pages/find-my-stuff-page/find-my-stuff-page";

// ============================================================================
// SETUP: Determine environment and select appropriate test data
// ============================================================================

const isProdEnvironment = config.ENV?.startsWith('PROD_');
const testData = isProdEnvironment ? prodTestData : devTestData;
const reportTestData = findDataSetByName(testData, config.DATASET);
const localeData = getTestDataByLocale(config.LOCALE, localizationData as any);
const reportAssertData: any = findDataSetByName(assertionData, config.DATASET);
let reportUrl = config.BASE_URL + reportTestData?.navUrlWithDataSetId;
let savedReportTitle: string = "";
let savedReportUrl: string = "";

// ============================================================================
// TEST SUITE: Basket analysis
// ============================================================================

if (reportTestData) {
  test.describe("Basket analysis", async () => {
    let dataSelector: DataSelector;
    let commonTestCase: CommonTestCase;
    let formatPanel: FormatPanel;

    // ========================================================================
    // TEST 1: QuickLook
    // ========================================================================
    test(
      "QuickLook: Should display correct preview information when template is selected",
      {
        tag: "@QuickLook",
      },
      async () => {
        // Create page and navigate to landing page
        let basePage = await BasePageFactory.createBasePage();
        let landingPage = new LandingPage(basePage);
        
        await landingPage.navigateToUrl(config.BASE_URL);
        await landingPage.clickAnalyseMyData();
        await landingPage.clickChooseATemplate();
        
        // Find and click template
        let chooseAtemplatePage = new ChooseAtemplatePage(landingPage.getBasePage());
        await chooseAtemplatePage.clickSearchIconAndEnterText(localeData.title);
        await chooseAtemplatePage.hoverThumbnailBySrc(devTestData.common.thumbnailSrc);
        await chooseAtemplatePage.clickQuickLookInfoButton();
        
        // Validate QuickLook content
        let quickLookPage = new QuickLookPage(chooseAtemplatePage.getBasePage());
        
        const previewDescriptionValue = await quickLookPage.getPreviewDescription();
        assert(previewDescriptionValue).toBe(localeData.description);
        
        const themesValue = await quickLookPage.getPreviewThemes();
        assertionData.common.themes.forEach((expectedTheme: string) => {
          assert(themesValue).toContain(expectedTheme);
        });
        
        const previewDatasetsValue = await quickLookPage.getPreviewDatasets();
        assertionData.common.dataSets.forEach((expectedDataset: string) => {
          assert(previewDatasetsValue).toContain(expectedDataset);
        });
      }
    );

    // ========================================================================
    // TEST 2: Insights Panel
    // ========================================================================
    test(
      "Insights Panel: Should validate business questions and report metadata",
      {
        tag: "@Insights",
      },
      async () => {
        let basePage = await BasePageFactory.createBasePage();
        const insightsPanel = new InsightsPanel(basePage);
        
        // Navigate to report
        await insightsPanel.navigateToUrl(reportUrl);
        await insightsPanel.clickInsightsIcon();
        
        // Validate insights panel structure
        const isTitleVisible = await insightsPanel.isReportInsightsTitleVisible();
        assert(isTitleVisible).toBeTruthy();
        
        const isCloseIconVisible = await insightsPanel.isReportInsightsPanelCloseIconVisible();
        assert(isCloseIconVisible).toBeTruthy();
        
        // Validate report title matches localization
        const insightsReportTitle = await insightsPanel.getInsightsReportTitle();
        assert(insightsReportTitle?.trim()).toBe(localeData.title);
        
        // Validate description
        const description = await insightsPanel.getInsightsPanelDescription();
        assert(description?.trim()).toBe(localeData.description);
        
        // Validate business questions visible
        const isBusinessTitleVisible = await insightsPanel.isBusinessQuestionsTitleVisible();
        assert(isBusinessTitleVisible).toBeTruthy();
      }
    );

    // ========================================================================
    // TEST 3: Data Selector
    // ========================================================================
    test(
      "Data Selector: Should apply dimension selections correctly",
      {
        tag: "@DataSelector",
      },
      async () => {
        let basePage = await BasePageFactory.createBasePage();
        dataSelector = new DataSelector(basePage);
        commonTestCase = new CommonTestCase(basePage);
        
        // Navigate to report
        await basePage.getWebActions().navigateTo(reportUrl);
        
        // Apply dimension selections from test data
        const dimensionSelections = reportTestData?.dimensionSelections[0];
        if (dimensionSelections) {
          await dataSelector.setMadlibSelection(dimensionSelections, true);
        }
      }
    );

    // ========================================================================
    // TEST 4: Run Report
    // ========================================================================
    test(
      "Run Report: Should execute report with selected data",
      {
        tag: "@Run",
      },
      async () => {
        let basePage = await BasePageFactory.createBasePage();
        commonTestCase = new CommonTestCase(basePage);
        
        // Navigate and apply data
        await basePage.getWebActions().navigateTo(reportUrl);
        
        // Click run button
        await commonTestCase.clickRunButton();
        
        // Wait for completion
        await commonTestCase.waitForReportRunCompletion();
        
        // Verify report rendered
        const isReportVisible = await commonTestCase.isReportVisible();
        assert(isReportVisible).toBeTruthy();
      }
    );

    // ========================================================================
    // TEST 5: Format Panel
    // ========================================================================
    test(
      "Format Panel: Should toggle formatting options",
      {
        tag: "@FormatPanel",
      },
      async () => {
        let basePage = await BasePageFactory.createBasePage();
        formatPanel = new FormatPanel(basePage);
        
        // Navigate and run report
        await basePage.getWebActions().navigateTo(reportUrl);
        await formatPanel.navigateToUrl(reportUrl);
        
        // Open format panel
        await formatPanel.clickFormatPanelButton();
        
        // Apply format selections from test data
        const formatSelections = reportTestData?.formatPanelSelections[0];
        if (formatSelections) {
          for (const option of formatSelections.default) {
            if (option.selected) {
              await formatPanel.toggleOption(option.value);
            }
          }
        }
      }
    );

    // ========================================================================
    // TEST 6: Export
    // ========================================================================
    test(
      "Export: Should export report data",
      {
        tag: "@Export",
      },
      async () => {
        let basePage = await BasePageFactory.createBasePage();
        const exportComponent = new ExportComponent(basePage);
        
        // Navigate to report
        await basePage.getWebActions().navigateTo(reportUrl);
        
        // Click export button
        await exportComponent.clickExportButton();
        
        // Select export format (default: PNG)
        await exportComponent.selectExportFormat("PNG");
        
        // Trigger download
        await exportComponent.clickExportConfirm();
      }
    );

    // ========================================================================
    // TEST 7: Save Report
    // ========================================================================
    test(
      "Save Report: Should save report to My Assets",
      {
        tag: "@Save",
      },
      async () => {
        let basePage = await BasePageFactory.createBasePage();
        commonTestCase = new CommonTestCase(basePage);
        
        // Navigate to report
        await basePage.getWebActions().navigateTo(reportUrl);
        
        // Click save
        await commonTestCase.clickSaveButton();
        
        // Verify save modal appears
        const isSaveModalVisible = await commonTestCase.isSaveModalVisible();
        assert(isSaveModalVisible).toBeTruthy();
        
        // Enter report title
        const reportTitle = `Basket Analysis - ${new Date().toISOString()}`;
        await commonTestCase.enterReportTitle(reportTitle);
        
        // Confirm save
        await commonTestCase.clickSavConfirm();
        
        // Store for later verification
        savedReportTitle = reportTitle;
        savedReportUrl = await basePage.getWebActions().getCurrentUrl();
      }
    );

    // ========================================================================
    // TEST 8: Snapshot
    // ========================================================================
    test(
      "Snapshot: Should capture report visual snapshot",
      {
        tag: "@Snapshot",
      },
      async () => {
        let basePage = await BasePageFactory.createBasePage();
        const snapshot = new Snapshot(basePage);
        
        // Navigate to report
        await basePage.getWebActions().navigateTo(reportUrl);
        
        // Take snapshot
        await snapshot.captureSnapshot("Basket Analysis Snapshot");
        
        // Verify snapshot file created
        const snapshotUrl = await snapshot.getSnapshotUrl();
        assert(snapshotUrl).toBeTruthy();
      }
    );

    // ========================================================================
    // TEST 9: No Data State
    // ========================================================================
    test(
      "No Data: Should display no data message when selections result in no data",
      {
        tag: "@NoData",
      },
      async () => {
        let basePage = await BasePageFactory.createBasePage();
        commonTestCase = new CommonTestCase(basePage);
        
        // Navigate to report
        await basePage.getWebActions().navigateTo(reportUrl);
        
        // Make selections that result in no data
        // (Using test data marked for no-data scenario)
        
        // Run report
        await commonTestCase.clickRunButton();
        await commonTestCase.waitForReportRunCompletion();
        
        // Verify no-data placeholder shown
        await commonTestCase.assertNoDataFound();
      }
    );
  });
} else {
  test.skip("Dataset not found", () => {});
}
```

### Structure Breakdown

1. **Imports** (Lines 1-30)
   - Framework: `@playwright/test`
   - Utilities: helpers, config, factory
   - Components & Pages: specific to this report
   - Test Data: dev/prod/localization/assertion JSONs

2. **Setup** (Lines 32-42)
   - Determine prod vs dev environment
   - Load correct test data
   - Select localization by config
   - Construct report URL

3. **Test Suite** (Line 44)
   - One `test.describe()` per report
   - Report name as description

4. **Per-Test Structure** (Lines 46-XX)
   ```typescript
   test("Feature: Expected behavior", { tag: "@FeatureTag" }, async () => {
     // 1. Create page/component instances
     let basePage = await BasePageFactory.createBasePage();
     let component = new ComponentName(basePage);
     
     // 2. Navigate
     await component.navigateToUrl(url);
     
     // 3. Interact
     await component.doSomething();
     
     // 4. Assert
     assert(result).toBe(expected);
   });
   ```

5. **Skip If No Data** (Lines last)
   - Gracefully skip if dataset not found
   - Prevents orphaned test runs

### Tag Conventions

Each test must have a tag for filtering:
- `@QuickLook` — QuickLook preview tests
- `@Insights` — Insights panel tests
- `@DataSelector` — Data selection tests
- `@FormatPanel` — Format option tests
- `@Export` — Export functionality
- `@Save` — Save/favorites tests
- `@Snapshot` — Visual snapshot tests
- `@Run` — Report execution
- `@NoData` — No-data state handling

---

## 8. Test Data Organization

### Folder Structure

```
src/lib/tests/reports/{report-name}/
├── test-data/
│   ├── dev-test-data.json           # DEV environment data
│   ├── prod-test-data.json          # PROD environment data
│   └── localization-data.json       # Multi-locale strings
├── assertion-data/
│   └── assertion-data.json          # Expected values for validation
└── resources/
    └── [component-resources.json]   # Report-specific selectors
```

### Environment-Specific Files

**Naming Pattern:**
- `dev-test-data.json` — DEV environment test data
- `prod-test-data.json` — PROD environment test data  
- `si-test-data.json` — SI environment (sometimes)
- `localization-data.json` — Multi-language strings (all environments)

### Real Example: `basket-analysis/test-data/dev-test-data.json`

```json
{
    "common": {
        "navigationURL": "/report/flow/US-2506063/US-2506063",
        "thumbnailSrc": "https://discoverassets.blob.core.windows.net/thumbnails-revamp-optimized/panel/Basket_analysis.webp"
    },
    "datasets": {
        "Omni Agg SI 3yrs Synd": {
            "sourceType": "Panel On Demand Omnishopper",
            "dataView": "Entire Dataset",
            "navUrlWithDataSetId": "/report/flow/US-2506063/US-2506063/155535/OrderId_4011",
            "dimensionSelections": [
                {
                    "Facts": {
                        "selections": {
                            "value": [
                                "Expenditure Value (Itemized)"
                            ]
                        },
                        "minSelection": "1",
                        "maxSelection": "1"
                    },
                    "TripGroups": {
                        "minSelection": "1",
                        "maxSelection": "10",
                        "selections": {
                            "#US LOC OMNI SUPER CATEGORY MA": [
                                "BAKING MIXES",
                                "BATH & SHOWER",
                                "BREAD",
                                "CHEESE",
                                "COFFEE"
                            ]
                        }
                    },
                    "Products": {
                        "selections": {
                            "#US LOC OMNI DEPARTMENT MASTER": [
                                "FOOD",
                                "GENERAL MERCHANDISE",
                                "HEALTH & BEAUTY CARE"                                   
                            ]
                        },
                        "minSelection": "1",
                        "maxSelection": "1500"                    
                    },
                    "Retailer": {
                        "selections": {
                            "Channel": [
                                "Convenience/Gas",
                                "Coop/Farm/Feed",
                                "Department Store"                        
                            ]
                        },
                        "minSelection": "1",
                        "maxSelection": "10"                    
                    },
                    "Geography": {
                        "selections": {
                            "Total US": [
                                "Total US"
                            ]
                        },
                        "minSelection": "1",
                        "maxSelection": "1"
                    },
                    "Period": {
                        "selections": {
                            "4 Weeks": [
                                "4 w/e 12/28/24"
                            ]
                        },
                        "minSelection": "1",
                        "maxSelection": "1"
                    },
                    "Demographic": {
                        "selections": {
                            "Census Regions Demo": [
                                "West"
                            ]
                        },
                        "minSelection": "1",
                        "maxSelection": "1"
                    },
                    "People": {
                        "selections": [
                            {
                                "Shopped at": {
                                    "Shopped at": {
                                        "Retailer": {
                                            "selections": {                                 
                                                "Total Outlets": [
                                                    "Total Outlets"
                                                ]
                                            },
                                            "minSelection": "1",
                                            "maxSelection": "1"
                                        }
                                    }
                                }
                            }
                        ],
                        "minSelection": "1",
                        "maxSelection": "1"
                    }
                }
            ],
            "formatPanelSelections": [
                {   
                    "default": [
                        {"id": 0, "value": "Data labels", "selected": true, "components": ["niq-highchart-column"]},
                        {"id": 1, "value": "Grid lines", "selected": true, "components": ["niq-highchart-bar"]}
                    ],
                    "afterChangeCheck": [
                        {"id": 0, "value": "Data labels", "selected": false, "components": ["niq-highchart-column"]},
                        {"id": 1, "value": "Grid lines", "selected": false, "components": ["niq-highchart-bar"]}
                    ]   
                }
            ]
        },
        "HSCN Agg SI 5yrs Synd": {
            "sourceType": "Panel On Demand Homescan",
            "dataView": "Entire Dataset",
            "navUrlWithDataSetId": "/report/flow/US-2506063/US-2506063/155486/OrderId_2927",
            "dimensionSelections": [
                {
                    "Facts": {
                        "minSelection": "1",
                        "maxSelection": "1",
                        "selections": {
                            "closedPromptFactDropdown": {
                                "factSelection": "Basket Value"
                            }
                        }
                    }
                }
            ]
        }
    }
}
```

### Real Example: `basket-analysis/test-data/localization-data.json`

```json
[
    {
        "locale": "en-US",
        "title": "Basket analysis",
        "description": "Understand the impact of a product's presence within the basket, and explore the basket composition.",
        "solutionType": [
            "PANEL_OD_OMNI",
            "PANEL_OD_HSCN"
        ],
        "businessQuestions": [
            "When the item/brand is in the basket, which categories are more likely to be in the basket than an average basket?",
            "When an item/brand is in basket, which categories do brand buyers spend the most per trip?"
        ],
        "madlibText": "Analyze the composition of .*across .*within .*based on .*during .*Additional:Geography is .*Who are Demographic .*People .*"
    },
    {
        "locale": "en-GB",
        "title": "Basket analysis",
        "description": "Understand the impact of a product's presence within the basket, and explore the basket composition.",
        "solutionType": [
            "PANEL_OD_OMNI",
            "PANEL_OD_HSCN"
        ],
        "businessQuestions": [
            "What brands/retailers are most frequently purchased with my brand/retailer?",
            "Which of my competitors have the most exclusive brand/retailer buyers?",
            "How much of my non-exclusive purchasing occurred on promotion?",
            "What percentage of category buyers purchase all three package types versus exclusively staying within one?"
        ]
    },
    {
        "locale": "fr-FR",
        "title": "Mixité - Simultanéité d'achats",
        "description": "Comprendre l'importance de l'interaction entre votre marque et les marques concurrentes...",
        "solutionType": [
            "PANEL_OD_OMNI",
            "PANEL_OD_HSCN"
        ],
        "businessQuestions": [
            "Quelles marques/quels détaillants sont les plus fréquemment achetés avec ma marque/mon détaillant ?",
            "Parmi mes concurrents, lequel possède les acheteurs de marques/détaillants les plus exclusifs ?"
        ]
    }
]
```

### Real Example: `basket-analysis/assertion-data/assertion-data.json`

```json
{
    "common": {
        "themes": [
            "Assortment/Distribution",
            "Buyer / Shopper Segments",
            "Performance"
        ],
        "dataSets": [
            "Panel On Demand Homescan",
            "Panel On Demand Omnishopper"
        ]
    },
    "datasets": {
        "Omni Agg SI 3yrs Synd": {
            "dataSummary": {
                "facts": {
                    "factList": [
                        "Basket Value",
                        "Expenditure Value (FMCG)",
                        "Expenditure Value (Itemized)"
                    ],
                    "defaultSelection": "Basket Value"
                },
                "people": {
                    "menu": [
                        {
                            "name": "BOUGHT",
                            "shouldDisplay": true,
                            "subMenu": [
                                {
                                    "name": "BOUGHT",
                                    "shouldDisplay": true
                                },
                                {
                                    "name": "BOUGHT_EACH_OF",
                                    "shouldDisplay": true
                                },
                                {
                                    "name": "BOUGHT_EITHER_OF",
                                    "shouldDisplay": true
                                }
                            ]
                        },
                        {
                            "name": "SHOPPED_AT",
                            "shouldDisplay": true,
                            "subMenu": [
                                {
                                    "name": "SHOPPED_AT",
                                    "shouldDisplay": true
                                },
                                {
                                    "name": "SHOPPED_AT_EACH_OF",
                                    "shouldDisplay": true
                                },
                                {
                                    "name": "SHOPPED_AT_EITHER_OF",
                                    "shouldDisplay": true
                                }
                            ]
                        },
                        {
                            "name": "CONVERTED_SHOPPERS",
                            "shouldDisplay": true
                        }
                    ]
                }
            }
        }
    }
}
```

### Data Lookup Usage in Tests

```typescript
// From test file
import devTestData from "./test-data/dev-test-data.json";
import localizationData from "./test-data/localization-data.json";
import assertionData from "./assertion-data/assertion-data.json";

const isProdEnvironment = config.ENV?.startsWith('PROD_');
const testData = isProdEnvironment ? prodTestData : devTestData;

// Find dataset by name
const reportTestData = findDataSetByName(testData, config.DATASET);
// Result: { sourceType, dataView, navUrlWithDataSetId, dimensionSelections, formatPanelSelections }

// Get localization for current locale
const localeData = getTestDataByLocale(config.LOCALE, localizationData);
// Result: { locale, title, description, businessQuestions, solutionType, madlibText }

// Get assertion data
const reportAssertData = findDataSetByName(assertionData, config.DATASET);
// Result: { dataSummary: { facts, people }, ... }

// In test
await dataSelector.setMadlibSelection(reportTestData?.dimensionSelections[0]);
assert(insightTitle).toBe(localeData.title);
assert(factsText).toContain(reportAssertData.dataSummary.facts.defaultSelection);
```

---

## 9. Playwright Config Pattern

### Complete File: `playwright.config.ts`

```typescript
import { defineConfig, devices } from '@playwright/test';
import * as dotenv from 'dotenv';
import path from 'path';
import { WaitForLoadStateOptions } from './src/lib/constants/optional-parameter-types';
import { MAX_TEST_TIMEOUT, NAVIGATION_TIMEOUT, RUN_TEST_TIMEOUT } from './src/lib/constants/time-out-constants';
import { config as frameworkConfig } from './src/lib/playwright/config';
import { getLambdaTestBuildName } from './src/lib/common/utils/configurationDetails';

// Read from ".env" file.
dotenv.config({ path: path.join(__dirname, '.env') });

// ============================================================================
// LAMBDATEST CONFIGURATION
// ============================================================================

const isLambdaTest = process.env.LAMBDATEST === 'true';
const ltUser = process.env.LAMBDATEST_USERNAME || process.env.LT_USERNAME;
const ltAccessKey = process.env.LAMBDATEST_ACCESS_KEY || process.env.LT_ACCESS_KEY;
const ltProject = process.env.LT_PROJECT || 'OMNI Discover';
const ltBuild = process.env.LT_BUILD || 'Discover build';
const ltBuildIdentifier =
  process.env.LT_BUILD_IDENTIFIER ||
  process.env.BUILD_NUMBER ||
  process.env.GITHUB_RUN_NUMBER ||
  undefined;

const ltVideo = process.env.LT_VIDEO ? process.env.LT_VIDEO === 'true' : true;
const ltNetwork = process.env.LT_NETWORK ? process.env.LT_NETWORK === 'true' : false;
const ltConsole = process.env.LT_CONSOLE ? process.env.LT_CONSOLE === 'true' : false;
const ltIdleTimeout = process.env.LT_IDLE_TIMEOUT ? Number(process.env.LT_IDLE_TIMEOUT) : 300;
const ltWorkers = process.env.LT_WORKERS ? Number(process.env.LT_WORKERS) : 1;

console.log('isLambdaTest =', isLambdaTest);
console.log('LAMBDATEST env var =', process.env.LAMBDATEST);

if (isLambdaTest) {
  console.log('\n🌐 LambdaTest Cloud Execution Enabled');
  console.log('   User:', ltUser);
  console.log('   Project:', ltProject);
  console.log('   Workers:', ltWorkers);

  if (!ltUser || !ltAccessKey) {
    throw new Error(
      'LambdaTest credentials missing! Please set LAMBDATEST_USERNAME/LT_USERNAME and LAMBDATEST_ACCESS_KEY/LT_ACCESS_KEY'
    );
  }
}

export const LOADSTATE: WaitForLoadStateOptions = 'domcontentloaded';

/**
 * Read environment variables from file.
 * https://github.com/motdotla/dotenv
 */
export default defineConfig({
  // ========================================================================
  // TEST LOCATION & BEHAVIOR
  // ========================================================================
  
  testDir: './src/lib/tests/reports',
  fullyParallel: true,                // Run test files in parallel
  timeout: isLambdaTest ? RUN_TEST_TIMEOUT : MAX_TEST_TIMEOUT,
  
  // ========================================================================
  // GLOBAL SETUP/TEARDOWN
  // ========================================================================
  
  globalSetup: require.resolve('./src/lib/common/utils/global-setup'),
  globalTeardown: require.resolve('./src/lib/playwright/setup/globalTeardown'),
  
  // ========================================================================
  // CI/LOCAL BEHAVIOR
  // ========================================================================
  
  forbidOnly: !!process.env.CI,       // Fail if test.only() left in code
  retries: process.env.CI ? 2 : 0,    // Retry failed tests in CI only
  workers: isLambdaTest ? ltWorkers : process.env.CI ? 1 : 2,
  
  // ========================================================================
  // REPORTING
  // ========================================================================
  
  reporter: process.env.CI
    ? [
        ['list'],
        ['json', { outputFile: 'playwright-report/test-results.json' }],
        ['allure-playwright', { outputFolder: 'allure-results' }],
        ...(isLambdaTest ? [['./src/lib/playwright/reporters/lt-session-reporter.ts'] as const] : []),
        [
          './src/lib/playwright/grafana-reporter.ts',
          {
            areaName: process.env.AREA_NAME || 'TestArea',
            domainName: process.env.DOMAIN_NAME || 'TechTest',
            teamName: process.env.TEAM_NAME || 'DiscoverOmniTest',
            projectName: process.env.PROJECT_NAME || 'GTATesting'
          }
        ]
      ]
    : [
        ['list'],
        ['allure-playwright', { outputFolder: 'allure-results' }],
        ...(isLambdaTest ? [['./src/lib/playwright/reporters/lt-session-reporter.ts'] as const] : [])
      ],
  
  // ========================================================================
  // BROWSER CONFIGURATION
  // ========================================================================
  
  use: {
    trace: 'on-first-retry',                          // Capture trace on first retry only
    navigationTimeout: NAVIGATION_TIMEOUT,             // Per-page navigation timeout
    baseURL: frameworkConfig.BASE_URL                 // Base URL from framework config
  },

  /* Configure projects for major browsers */
  projects: isLambdaTest
    ? [
        {
          name: 'lambdatest-chromium',
          use: {
            browserName: 'chromium',
            headless: true,
            viewport: { width: 1920, height: 1080 },
            baseURL: frameworkConfig.BASE_URL,

            connectOptions: {
              wsEndpoint: `wss://cdp.lambdatest.com/playwright?capabilities=${encodeURIComponent(
                JSON.stringify({
                  browserName: 'pw-chromium',
                  browserVersion: 'latest',

                  'LT:Options': {
                    platform: 'Windows 11',

                    // Build name should include identifier for tracking
                    build: getLambdaTestBuildName(ltBuild, ltBuildIdentifier),

                    name: 'Playwright Automation',

                    user: ltUser,
                    accessKey: ltAccessKey,

                    project: ltProject,

                    network: ltNetwork,
                    video: ltVideo,
                    console: ltConsole,

                    plugin: 'playwright-test',
                    w3c: true,
                    idleTimeout: ltIdleTimeout
                  }
                })
              )}`
            }
          }
        }
      ]
    : [
        {
          name: 'chromium',
          use: {
            launchOptions: {
              args: ['--start-maximized']              // Maximize on launch
            },
            viewport: null                             // Use system viewport
          }
        }
        /* Other browsers commented out for now
        {
          name: 'firefox',
          use: {
            launchOptions: {
              args: ['--start-maximized'],
            },
            viewport: null,
          },
        },
        {
          name: 'webkit',
          use: { ...devices['Desktop Safari'] },
        },
        */

        /* Test against mobile viewports. */
        // {
        //   name: 'Mobile Chrome',
        //   use: { ...devices['Pixel 5'] },
        // },
        // {
        //   name: 'Mobile Safari',
        //   use: { ...devices['iPhone 12'] },
        // },

        /* Test against branded browsers. */
        // {
        //   name: 'Microsoft Edge',
        //   use: { ...devices['Desktop Edge'], channel: 'msedge' },
        // },
        // {
        //   name: 'Google Chrome',
        //   use: { ...devices['Desktop Chrome'], channel: 'chrome' },
        // },
      ]

  /* Run your local dev server before starting the tests (commented out)
  webServer: {
    command: 'npm run start',
    url: 'http://127.0.0.1:3000',
    reuseExistingServer: !process.env.CI,
  },
  */
});
```

### Config Sections Explained

| Section | Purpose |
|---------|---------|
| **LAMBDATEST CONFIGURATION** | Load LT credentials from env, validate setup |
| **TEST LOCATION & BEHAVIOR** | Where tests live, timeouts, parallel execution |
| **GLOBAL SETUP/TEARDOWN** | Run once before all tests (login), after all tests (cleanup) |
| **CI/LOCAL BEHAVIOR** | Retries, forbid `.only()`, worker count varies by env |
| **REPORTING** | JSON, Allure, LambdaTest session reporter, Grafana metrics |
| **BROWSER CONFIGURATION** | Timeouts, trace capture, base URL |
| **PROJECTS** | Define browsers/platforms to test on |

### Key Features

1. **LambdaTest Support**: Dynamically connect to cloud grid if `LAMBDATEST=true`
2. **Environment Awareness**: Different configs for CI vs local vs cloud
3. **Reporting**: Multi-format (JSON, Allure, Grafana, LT sessions)
4. **Auth State**: Global setup handles login once, persists `storage/auth.json`
5. **Trace Capture**: Only on first retry to reduce storage

---

## 10. Page Object Class Pattern

### Complete Real Example: `LoginPage`

**File: `src/lib/common/pages/login-page/login-page.ts`**

```typescript
/**
 * @fileoverview Login page implementation for the Omnishopper application.
 * Handles authentication flow using the Page Object Model pattern.
 *
 * This module provides credential validation and login functionality,
 * abstracting away the UI interaction details from test code.
 */
import { BasePage, WebActions } from "@gfk/gta";
import loginPageResources from "./loginPageResources.json";
import { 
  MAX_TEST_TIMEOUT, 
  NAVIGATION_TIMEOUT, 
  SMALL_TIMEOUT 
} from "../../../constants/time-out-constants";
import { ResourceManager } from "../../utils/resource-manager";
import { ElementResource } from "../../interfaces/iElementResource";

// ============================================================================
// CONFIGURATION & SETUP
// ============================================================================

const LOGIN_PAGE_COMPONENT_NAME = "loginPage";
const loginPageElements = loginPageResources as ElementResource;

/**
 * Validates credentials before attempting login
 */
function validateCredentials(username: string, password: string): void {
  // Ensure template placeholder values have been replaced
  if (!(username && password)) {
    throw new Error(
      'Default credentials detected. Please replace "USER_NAME" and "PASS_WORD" ' +
      'in the .env file with actual automation service account credentials.'
    );
  }
}

// Get credentials from environment
const USERNAME = process.env.USER_NAME || "";
const PASSWORD = process.env.PASS_WORD || "";
validateCredentials(USERNAME, PASSWORD);

// ============================================================================
// PAGE OBJECT CLASS
// ============================================================================

/**
 * Page object representing the login page of the application.
 * Handles authentication flow and related operations.
 */
export class LoginPage {
  private webActionHandler: WebActions;
  private resourceManager: ResourceManager;

  /**
   * Creates an instance of LoginPage.
   * @param {BasePage} basePage - The base page object containing common functionality
   */
  constructor(private basePage: BasePage) {
    this.webActionHandler = basePage.getWebActions();
    this.resourceManager = new ResourceManager({});
    this.resourceManager.registerResources(
      LOGIN_PAGE_COMPONENT_NAME,
      loginPageElements
    );
  }

  /**
   * Accept Terms & Conditions only if terms checkbox is visible.
   * This is a graceful handler for optional UI flows.
   *
   * @private
   * @throws Error if terms acceptance fails (but only after verification)
   */
  private async acceptTermsIfPresent(): Promise<void> {
    console.log("Checking for Terms & Conditions page...");

    try {
      const checkbox = this.webActionHandler.getWebElement(
        this.resourceManager.getResource(
          "termsAcceptCheckbox", 
          LOGIN_PAGE_COMPONENT_NAME
        )
      );
      
      // Short timeout — checkbox may not exist on this flow
      await checkbox.waitFor(SMALL_TIMEOUT, "visible");
      console.log("Terms & Conditions page detected - checkbox is visible");

      // Click checkbox
      await checkbox.click(true);
      console.log("Terms checkbox clicked");

      // Click accept button
      const acceptButton = this.webActionHandler.getWebElement(
        this.resourceManager.getResource(
          "termsAcceptButton", 
          LOGIN_PAGE_COMPONENT_NAME
        )
      );
      await acceptButton.waitFor(MAX_TEST_TIMEOUT);
      await acceptButton.click();
      console.log("Terms accepted successfully.");
      
    } catch (error) {
      // Terms page not shown or already accepted — not an error
      console.log(
        "Terms page not shown or already accepted, continuing login..."
      );
    }
  }

  /**
   * Performs login with credentials from environment variables.
   *
   * This is the primary entry point for authentication.
   * It handles:
   * 1. Navigation to login URL
   * 2. Username entry and submit
   * 3. Password entry and submit
   * 4. Optional terms acceptance
   * 5. Storage state persistence (auth.json)
   *
   * @param {string} url - The login URL to navigate to
   * @throws Error if login fails at any step
   */
  public async login(url: string): Promise<void> {
    try {
      console.log("Starting login process...");

      // ====================================================================
      // STEP 1: Navigate to login URL
      // ====================================================================
      await this.webActionHandler.navigateTo(
        url, 
        NAVIGATION_TIMEOUT, 
        "domcontentloaded"
      );

      // ====================================================================
      // STEP 2: Enter username and submit
      // ====================================================================
      await this.webActionHandler
        .getWebElement(
          this.resourceManager.getResource("userName")
        )
        .enterText(USERNAME);

      const submitButton = await this.webActionHandler.getWebElement(
        this.resourceManager.getResource(
          "submitButton",
          LOGIN_PAGE_COMPONENT_NAME
        )
      );
      
      // Click "Next" button (first submit)
      await submitButton.click();

      // ====================================================================
      // STEP 3: Enter password and submit
      // ====================================================================
      await this.webActionHandler
        .getWebElement(
          this.resourceManager.getResource("password")
        )
        .enterText(PASSWORD);

      // Click "Login" button (second submit)
      await submitButton.click();
      
      // ====================================================================
      // STEP 4: Handle optional Terms & Conditions page
      // ====================================================================
      await this.acceptTermsIfPresent();

      // Wait for any post-login UI updates
      await this.webActionHandler.delay(SMALL_TIMEOUT);

      // ====================================================================
      // STEP 5: Verify login succeeded
      // ====================================================================
      // The "Analyse My Data" button indicates successful login
      const analyseDataButton = this.webActionHandler.getWebElement(
        this.resourceManager.getResource("analyseMyData")
      );
      await analyseDataButton.waitFor();

      // ====================================================================
      // STEP 6: Persist authentication state
      // ====================================================================
      // Save browser cookies/local storage to file for reuse in tests
      await this.webActionHandler.getStorageState("storage/auth.json");

      console.log("Login process completed successfully.");
    } catch (error) {
      console.error(
        "Login process failed:", 
        error && error.name ? error.name : error
      );
      throw new Error(
        `Login failed: ${error && error.name ? error.name : error}`
      );
    } finally {
      // Always close browser after login
      await this.webActionHandler.closeBrowser();
    }
  }
}
```

### Structure Template (Apply to All Page Objects)

```typescript
/**
 * @fileoverview [Feature Name] page object
 * Handles [key responsibilities] using the Page Object Model pattern.
 */
import { BasePage, WebActions } from "@gfk/gta";
import pageResources from "./[page]-resources.json";
import { [TIMEOUT_CONSTANTS] } from "../../../constants/time-out-constants";
import { ResourceManager } from "../../utils/resource-manager";
import { ElementResource } from "../../interfaces/iElementResource";

// ============================================================================
// CONFIGURATION
// ============================================================================

const COMPONENT_NAME = "[FeatureName]";
const elements = pageResources as ElementResource;

// ============================================================================
// PAGE OBJECT CLASS
// ============================================================================

/**
 * Page object for [Feature Name] page.
 * Abstracts UI interactions into high-level methods for use in tests.
 */
export class [FeatureNamePage] extends CommonTestCase {
  constructor(basePage: BasePage) {
    super(basePage);
    this.resourceManager.registerResources(COMPONENT_NAME, elements);
  }

  /**
   * [Action description]
   * 
   * @param {string} paramName - Parameter description
   * @throws Error if [specific condition]
   */
  public async [actionName](paramName: string): Promise<void> {
    try {
      // 1. Get element using ResourceManager
      const element = this.webActionHandler.getWebElement(
        this.resourceManager.getResource("[elementName]", COMPONENT_NAME)
      );

      // 2. Wait for readiness
      await element.waitFor();

      // 3. Interact
      await element.click();

      // 4. Log success
      console.log(`[Action] completed for ${paramName}`);
    } catch (error) {
      // 5. Throw enriched error
      throw new Error(`Failed to [action]: ${error}`);
    }
  }

  /**
   * [Validation description]
   * 
   * @throws Error if validation fails
   */
  public async [validateSomething](): Promise<void> {
    try {
      const result = await this.webActionHandler
        .getWebElement(...)
        .getText();
      
      assert(result).toBe(expectedValue);
      console.log("Validation passed");
    } catch (error) {
      throw new Error(`Validation failed: ${error}`);
    }
  }
}
```

### Key Principles Observed

1. **Inheritance**: Extend `CommonTestCase` for shared functionality
2. **ResourceManager**: All selectors via `registerResources()` + `getResource()`
3. **Constants**: Use timeout constants, never magic numbers
4. **Errors**: Wrap in try-catch, log before throw, provide context
5. **Comments**: JSDoc for public methods, inline comments for logic
6. **Structure**: Configuration → Class definition → Methods

---

## Conclusion

This document provides a complete reference for the OmniShopper UI automation framework. It covers:

- **Folder organization** showing the purpose of each directory
- **Naming conventions** with real examples from the codebase
- **Constants and timeouts** with usage patterns
- **Resource management** (JSON imports, dynamic selection)
- **Shared utilities** for cross-component functionality
- **Error handling** patterns observed throughout
- **Test file structure** with a complete, production spec file
- **Test data organization** with environment and locale support
- **Playwright configuration** with LambdaTest cloud support
- **Page object template** showing the canonical structure

Use this document as the single source of truth for new developers joining the project or as a guide for implementing similar frameworks in related repositories.

