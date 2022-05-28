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

- Intercept network calls and return specified response 
- we will not make network calls on tests 
- install npm install msw (mock service worker)
- create handlers 

lecture-code\sundaes-on-demand\03-scoop-option-images\mocks\handlers.js

- Create test server 

sundae-server\server.js

- make sure test server listens during all tests 

lecture-code\sundaes-on-demand\03-scoop-option-images\setupTests.js

- reset handlers after each test 

- with mock server worker, the network calls done in the component are intercepted and handler response is sent back 

- when waiting for something to appear async on page must use await and findBy

- wait for -> another function to await with conditions 

finished-projects\sundaes-on-demand\src\pages\entry\tests\OrderEntry.test.jsx


tip: test.only and test.skip 

## Find element by a partirl text 
const scoopsSubtotal = screen.getByText('Scoops total: $', { exact: false });


## Testing Context

1) create context with createContext. This is where you create the states and functions to update the state  
2) make a custom hook with useContext 
3) the context can be memoized 
4) return a JSX which will wrap the children inside the provider  
5) Apply the provider to app.js or where its needed
6) Use the hook to update the state inside the context

code-quiz\sundaes-on-demand\03-toppings-subtotal\App.js

```TS
// create custom hook to check whether we're inside a provider
export function useOrderDetails() {
  const context = useContext(OrderDetails);
  ///happens when used outside a provider context will be undefined
  if (!context) {
    throw new Error(
      'useOrderDetails must be used within an OrderDetailsProvider'
    );
  }

  return context;
}
```
- make context provider with internal state via useState 
- export custom hook and provider (ln 76)
```ts
export function OrderDetailsProvider(props) {
  const [optionCounts, setOptionCounts] = useState({
    scoops: new Map(),
    toppings: new Map(),
  });
  const zeroCurrency = formatCurrency(0);
  const [totals, setTotals] = useState({
    scoops: zeroCurrency,
    toppings: zeroCurrency,
    grandTotal: zeroCurrency,
  });

  useEffect(() => {
    const scoopsSubtotal = calculateSubtotal('scoops', optionCounts);
    const toppingsSubtotal = calculateSubtotal('toppings', optionCounts);
    const grandTotal = scoopsSubtotal + toppingsSubtotal;
    setTotals({
      scoops: formatCurrency(scoopsSubtotal),
      toppings: formatCurrency(toppingsSubtotal),
      grandTotal: formatCurrency(grandTotal),
    });
  }, [optionCounts]);
  // keep the value from being recalculated unnecessarly
  const value = useMemo(() => {
    function updateItemCount(itemName, newItemCount, optionType) {
      const newOptionCounts = { ...optionCounts }; /// copy for immutability

      // update option count for this item with the new value
      const optionCountsMap = optionCounts[optionType];
      optionCountsMap.set(itemName, parseInt(newItemCount));

      setOptionCounts(newOptionCounts);
    }

    // getter: object containing option counts for scoops and toppings, 
    // subtotals and totals
    // setter: updateOptionCount
    return [{ ...optionCounts, totals }, updateItemCount];
  }, [optionCounts, totals]);
  return <OrderDetails.Provider value={value} {...props} />;
}

```

## Testing a context and hook 

- the component must have to be wrapped by the context 

code-quiz\sundaes-on-demand\03-toppings-subtotal\test-utils\testing-library-utils.jsx

```
import { render } from '@testing-library/react';
import { OrderDetailsProvider } from '../contexts/OrderDetails';

const renderWithContext = (ui, options) =>
  render(ui, { wrapper: OrderDetailsProvider, ...options });

// re-export everything
export * from '@testing-library/react';

// override render method
export { renderWithContext as render };

```

OBS: To run just one test in watch mode type p and type the pattern of the name of the file you want to test 


tip : JS number format for currency 

```Ts
// format number as currency
function formatCurrency(amount) {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
    minimumFractionDigits: 2,
  }).format(amount);
}
```
