# Sharing app constants

When the app works but the tests fail, we have a false negative. **False negatives could happen**, especially when the tests rely on something external (like the E2E tests do). Anyway, **we must reduce them** as much as possible. False negatives are a smell of bad tests and, sooner or later, is one of the reasons that make you a test-hater.

A lot of things can cause false negatives, one of them is the difference between the constants used by the web app and the ones used by the tests. What are the constants used by the actual test?

<i>File: <a href="../cypress/integration/examples/signup/signup-4.e2e.spec.js" target="_blank">cypress/integration/examples/signup/signup-4.e2e.spec.js</a></i>
[include](../cypress/integration/examples/signup/signup-4.e2e.spec.js)

The route and all the contents

- "/register"
- "Username"
- "Email"
- "Password"
- "Sign up"
- "No articles are here... yet."

could change because of a lot of reasons. If some of them change the app still works but the tests would break. How we can solve the problem? Importing them right from the source code!

We adapted the `realworld/frontend/src/components/Register.js` file exporting all the strings used internally

```javascript
export const strings = {
  username: "Username",
  email: "Email",
  password: "Password",
  signUp: "Sign up"
};
```

and we need to update the test too

```diff
+import { strings } from "../../../../realworld/frontend/src/components/Register";

context("Signup flow", () => {
  it("The happy path should work", () => {
    cy.visit("/register");
    const random = Math.floor(Math.random() * 100000);
-   cy.getByPlaceholderText("Username").type(`Tester${random}`);
+   cy.getByPlaceholderText(strings.username).type(`Tester${random}`);
-   cy.getByPlaceholderText("Email").type(`user+${random}@realworld.io`);
+   cy.getByPlaceholderText(strings.email).type(`user+${random}@realworld.io`);
-   cy.getByPlaceholderText("Password").type("mysupersecretpassword");
+   cy.getByPlaceholderText(strings.password).type("mysupersecretpassword");
    cy.get("form")
-     .within(() => cy.getByText("Sign up"))
+     .within(() => cy.getByText(strings.signUp))
      .click();
    cy.getByText("No articles are here... yet.", { timeout: 10000 }).should("be.visible");
  });
});
```

The updated test looks like this

```javascript
import { strings } from "../../../../realworld/frontend/src/components/Register";

context("Signup flow", () => {
  it("The happy path should work", () => {
    cy.visit("/register");
    const random = Math.floor(Math.random() * 100000);
    cy.getByPlaceholderText(strings.username).type(`Tester${random}`);
    cy.getByPlaceholderText(strings.email).type(`user+${random}@realworld.io`);
    cy.getByPlaceholderText(strings.password).type("mysupersecretpassword");
    cy.get("form")
      .within(() => cy.getByText(strings.signUp))
      .click();
    cy.getByText("No articles are here... yet.", { timeout: 10000 }).should("be.visible");
  });
});
```

The same logic could be applied to the `/register` route and the "_No articles are here... yet._" string, the final test is the following

<i>File: <a href="../cypress/integration/examples/signup/signup-5.e2e.spec.js" target="_blank">cypress/integration/examples/signup/signup-5.e2e.spec.js</a></i>
[include](../cypress/integration/examples/signup/signup-5.e2e.spec.js)

The result is that if you update some of the above-cited constants because of whatever reasons... You do not have to update the test accordingly and they still work as expected, one less thing to care about.