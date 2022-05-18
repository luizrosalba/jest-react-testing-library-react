# jest-react-testing-library-react

[RTL](https://www.udemy.com/course/react-testing-library/)

## Introduction 

- Test software the way users actually use it 
- find elements by accessibility markers not test IDs

- RTL provides virtual DOM for tests 
- Jest is a test runner : Finds tests, runs tests and determines wheteher tests pass or fails 

- npx create-react-app color-button

- npm test 

- render creates a Virtual Dom
- you can access Virtual Dom using screen
- jest assertions starts with expect 

```ts
expect(linkElement).toBeInTheDocument()
```
- jest-dom comes with CRA 
src/setupTeEsts.js import it before each test makes matchers available 

### RTL : 
Helps with : 
1) Rendering components into VDom 
2) searches VDom
3) Interacts with VDom 

- needs test runner : Jest, mocha ... 

- force fail : throw new Error('test failed')

### TDD: 
- Red-Green tests 
- test before code 
- write shell function -> write tests -> tests fail -> write code -> tests pass 

## Types of tests 

- Unit : TEsts one unit of code in isolation 
- Integrations : How Multiple units work together 
- Functional : Tests a particular function of software 
- Acceptance / E2E tests : Use actual browser and server (Cypress, Selenium)




## Priority 

https://testing-library.com/docs/queries/about/#priority

Based on the Guiding Principles, your test should resemble how users interact with your code (component, page, etc.) as much as possible. With this in mind, we recommend this order of priority:

1) Queries Accessible to Everyone
- getByRole
- getByLabelText
- getByPlaceholderText
- getByText
- getByDisplayValue 
2) Semantic Queries
- getByAltText
- getByTitle
3) Test IDs
getByTestId

matchers: 

https://github.com/testing-library/jest-dom

## Finding elements 

https://www.w3.org/TR/wai-aria/#role_definitions

### 01 - click app 

lecture-code\color-button\01-click-button-to-change-color\App.test.js

### 02 - Find elemente in Vdom 

lecture-code\color-button\02-initial-condition-of-button-and-checkbox\App.test.js


Testing Styles from Imported CSS Modules

A common question about testing styles is "why doesn't .toHaveStyle() work with classes from my imported CSS module?"

Mocking CSS modules

In the case of create-react-app applications -- or applications that have otherwise mocked css modules for Jest -- CSS module imports are simply ignored for Jest test.

Cosmetic Styles vs. Functional Styles

In many cases, the classes are merely cosmetic and won't affect functional tests (such as placement of the element on the page). In these cases, mocking the CSS modules works fine. However, sometimes classes do affect function. For example, say you have a CSS module that uses a hidden class, which results in display: none when interpreted. Without adding a Jest transformer to interpret the CSS, Testing Library will not know that hidden class would result in display: none. Tests around element visibility that rely on this class will fail.

Transformers

For styles to be interpreted in tests, you need a transformer to, well, transform the CSS classes into styles. Here are a couple options:

https://www.npmjs.com/package/jest-transform-css

https://www.npmjs.com/package/jest-css-modules-transform

The latter has more downloads per week, but the former seems to be more actively maintained.

Testing for Class Name
Another possibility would be to check explicitly for the class name (hidden in this example), using toHaveClass. This would be simpler, but farther from the actual user experience (this is testing implementation details, rather than how the user sees the page). It's always a balance, and I think either this approach or transforming the CSS would be defensible.

- describe for testing : 

lecture-code\color-button\04-unit-test-replace-camel-with-spaces\App.test.js

## ESlint and prettier 
Enforces best practices 

https://github.com/testing-library/eslint-plugin-testing-library
https://github.com/testing-library/eslint-plugin-jest-dom

EsLint : linter: Analyzes static text and marks syntax that breaks rules. Catches errors in code 

Prettier: formatter: address format and style ;

## Form Review and Popover 

- styles can determinate how the tests are written. Hidden stills on code.

- user-event can simulate user events. In general its recommended to use user-event than fire event. 

lecture-code\sundaes-on-demand\02-terms-and-conditions-popover\pages\summary\test\SummaryForm.test.jsx

### Screen query methods 

https://testing-library.com/docs/queries/about/
https://testing-library.com/docs/react-testing-library/cheatsheet/
https://testing-library.com/docs/queries/about/#priority

Sintax: command[All]ByQueryType

Command:

- get : Expect element to be in DOM 
- query: Expect element not to be in DOM 
- find: Expect element to appear async 

[All]

- (exclude) expect only one match 
- (include) expect more than one match (array of matches)


QueryType: (what you are searching by)

- Role (preferred)
- AltText(images)
- Text(display elem)
- Form elements (placeholderText, labelText, DysplayValue)

async example : a popup desapears after the tests finishes, so the test fails. We should await for ElementTo be removed before test 

https://testing-library.com/docs/guide-disappearance

```ts
import {
  render,
  screen,
  waitForElementToBeRemoved,
} from '@testing-library/react';
  // popover disappears when we mouse out
  userEvent.unhover(termsAndConditions);
  await waitForElementToBeRemoved(() =>
    screen.queryByText(/no ice cream will actually be delivered/i)
  );
```

## Mock service worker and server response 

## test components wrapped in a provider 



