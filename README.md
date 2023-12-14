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
   2. query function - return null, wont throw any error if an element is not found
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

## Testing User Interaction & States

**userEvent -** is an object that helps us trigger user events in this virtual screen. by help of userEvent we can perform typical events like 'clicking', 'double clicking', 'hovering', 'typing into inputs'

```
  test('does not render "good to see you" if the button was clicked', () => {
     // Arrange
     render(<Greeting />);

     // Act
     const buttonElement = screen.getByRole('button');
     userEvent.click(buttonElement)

     // Assert
     const outputElement = screen.queryByText('good to see you', { exact: false });
     expect(outputElement).toBeNull();
  });
```

## Testing Asynchronous Code

```
import { render, screen } from '@testing-library/react';
import Async from './Async';

describe('Async component', () => {
  test('renders posts if request succeeds', async () => {
    render(<Async />)

    const listItemElements = await screen.findAllByRole('listitem');
    expect(listItemElements).not.toHaveLength(0);
  });
});
```

## Working with Mocks

When we run our tests we generally don't wanna send http requests to our server because -

1. it will hammer our servers with requests. especially when we have lots of tests with lots of requests.
2. if a component sends post request to a server, our tests might start inserting data into a database. and that's definitely not something we wanna do during testing.

In such scenarios where such kind of requests are being sent, and that's something we don't wanna send. So therefore what we generally do is -

1. don't even wanna send a real request.
2. send request to a fake/testing server

In this project we are taking first approach of replacing fetch function with a dummy function.

To replace default function, Jest provide us fn() function. which create an mock function.

**mockResolvedValueOnce() -** allows us to set a value. which fetch function return in resolve.

```
import { render, screen } from '@testing-library/react';
import Async from './Async';

describe('Async component', () => {
  test('renders posts if request succeeds', async () => {
    window.fetch = jest.fn();
    window.fetch.mockResolvedValueOnce({
      json: async () => [{ id: 'p1', title: 'First post' }],
    });
    render(<Async />);

    const listItemElements = await screen.findAllByRole('listitem');
    expect(listItemElements).not.toHaveLength(0);
  });
});
```
