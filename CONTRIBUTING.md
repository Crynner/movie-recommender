## Creating Tests
`vitest` is used for all test cases, and follows an approximate structure:

```js
describe("TestSuiteName", () => {
    it("TestCaseName", async () => { // may not be async depending on test type
        const result = testFunction(arg1, arg2);
        expect(result).toBe(`${arg1}-${arg2}`)
    })
    it("TestCaseName2", async () => {
        const result = testFunction(arg1);
        expect(result).toBe(`${arg1}-${arg1}`)
    })
})
```
Given the above structure, each `it()` call makes one test case, while describe groups these tests together.

The test file for a function should be in the same folder as the file being tested, appending `.spec` before the file extension, e.g. `client/testFunction.ts` will have a test file `client/testFunction.spec.ts`.

### Frontend
Frontend tests make use of the react testing library, and work by emulating a DOM and rendering the components used.

Unit testing should be done per component or function file in isolation. These test whether the component or function by itself has all required functionality.

Integration testing should be done in a more practical environment, i.e. testing the `SearchBar` from the `App` component. These tests should ensure expected behaviour without colliding with other components.

In the case where a component needs to interact with the backend, this is currently mocked by MSW. in `client/src/test/handlers.ts`, there is a list of handlers; these assert that in testing, sending a particular http response to a specified link will return some `HttpResponse`, allowing tests to mock backends, external APIs, etc.

While frontend testing can confirm the logic of the DOM state e.g. what classes a component has, what text is displayed, it is unable to test direct visual state, e.g. responsiveness, component alignment, etc. This is more suitable to tools such as Playwright, which currently is not implemented, with no immediate plans of doing so.

### Backend
Backend testing is typically either unit tests on controller files, or integration tests on route files.

Unit tests ensure a backend function works as expected, similar to the frontend. These functions are called directly, and not via the backend route (i.e. use `testFunction()` instead of `fetch("api/testFunction")`).

Integration tests involve starting an instance of the express backend, and testing the routes (such as "api/testFunction"). External dependencies such as DBs may need to have mocked responses to function correctly. The `supertest` package allows sending HTTP responses via `request(app).get("/api/example")`, as an example, returning the response object which can be validated.