## Language / Library Used

1. React JS
2. Jest

# NOTES:

**npm test - to run unit testing.**

to write a test file we should use **.test.js** extension.

## Writing test case

1. use 'test('unit test name', ()=>{ //actual testing code })' function which is globally available.
2. Writing tests - the three "A"s
   **Arrange -** Set up the test data, test conditions and test environment.
   **Act -** Run logic that should be tested (eg, execute function).
   **Assert -** Compare execution results with expected results.

   ```
    import { render, screen } from "@testing-library/react";
    import Greeting from "./Greeting";

    test("renders Hello World as a text", () => {
        // Arrange
        render(<Greeting />);

        // Act
        // ... nothing here

        // Assert
        const helloWorldElement = screen.getByText("Hello World!");
        expect(helloWorldElement).toBeInTheDocument();
    });
   ```

   here screen gives us access to a virtual screen to this virtual DOM, which was rendered.
   for screen we get various function available -

   1. get function - throw an error if an element is not found
   2. query function - wont throw an error if an element is not found
   3. find function - will return a promise

3. Assertion -
   to check whether the element exist. for this we got the globally available **expect** function.

   1. **.toBeInTheDocument -** checks whether the the html element we expect here is in the document.
   2. **.not** - its a prefix to check opposite condition.

## Test suites

As your application grows we typically have hundreds of test cases and to organize and group those different tests we often organize them into different testing suites

To create testing suites we use 'describe()' function which is globally available

describe takes two arguments -

1. description
2. anonymous function which contains unit tests under this test suites.
