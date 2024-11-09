# 🚀 Unit Testing in Golang
This documentation was created to describe good practises when:
  1. Writing functions in Golang
  2. Writing unit test cases in Golang.
  3. Mocking third-party API calls.
  4. General structure of repository to ensure testable and maintainable code.

## Table of Contents
  1. **Principles of Function Design**
  2. **Writing Testable Functions**
  3. **Best Practices**
  3. **Examples**

### Principles of Function Design

When designing functions in Go, keep the following principles in mind:
  - **Single Responsibility:** <em>Each function should perform one task or have a single purpose. This improves readability and makes testing easier.</em>
  - **Keep Functions Small:** <em>Avoid complex logic or large blocks of code in a single function. If a function grows too large, consider breaking it into smaller helper functions.</em>
  - **Use Meaningful Names:** <em>Function names should be descriptive of their action. For example, use CalculateTotal instead of DoCalculation.</em>
  - **Minimize Side Effects:** <em>Avoid altering global state or shared data inside functions. Functions that modify global variables are harder to test and debug.</em>
