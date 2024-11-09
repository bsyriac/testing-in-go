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

### Writing Testable Functions

Ensuring that your functions are testable is essential for writing reliable and maintainable code. Below are practices for writing testable Go functions.

**1. Separate Logic from Dependencies**
Avoid embedding external dependencies, such as HTTP calls or database access, directly in functions. Instead, inject dependencies as interfaces or parameters, making them easier to mock in tests.

<em>Example:</em>
```
type DataService interface {
    FetchData(id string) (string, error)
}

func ProcessData(service DataService, id string) (string, error) {
    data, err := service.FetchData(id)
    if err != nil {
        return "", err
    }
    // Process data here
    return data, nil
}

```

**2. Use Interfaces for Dependency Injections**
Using interfaces allows for flexibility in testing. By defining interfaces for external services, you can inject mock implementations in tests.

<em>Example:</em>
```
type APIClient interface {
    GetData(id string) (string, error)
}

// Real function implementation
func (client *APIClientImpl) GetData(id string) (string, error) {
    // Actual API call logic here
}

// Mock implementation for testing
type MockAPIClient struct{}

func (m *MockAPIClient) GetData(id string) (string, error) {
    return "mocked data", nil
}

```

**3. Avoid Hard-Coding Values**
Avoid hard-coded values directly in your functions. Instead, pass them as parameters or define them as constants at the package level.

<em>Example:</em>
```
const MaxRetries = 3

func RetryOperation(operation func() error, maxRetries int) error {
    for i := 0; i < maxRetries; i++ {
        err := operation()
        if err == nil {
            return nil
        }
    }
    return errors.New("operation failed after max retries")
}

```

**4. Use Helper Functions and Table-Driven Tests**
Table-driven tests are highly recommended in Go for handling multiple test cases in a compact, readable format.

<em>Example:</em>
```
func TestSum(t *testing.T) {
    cases := []struct {
        a, b, expected int
    }{
        {2, 3, 5},
        {-1, 1, 0},
        {5, -5, 0},
    }

    for _, c := range cases {
        result := Sum(c.a, c.b)
        if result != c.expected {
            t.Errorf("Sum(%d, %d) = %d; expected %d", c.a, c.b, result, c.expected)
        }
    }
}

```
