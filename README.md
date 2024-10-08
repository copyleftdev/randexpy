# 🌟 `randexcpy` - Random Execution Library for Python

[![PyPI version](https://badge.fury.io/py/randexcpy.svg)](https://badge.fury.io/py/randexcpy)
[![Python application](https://github.com/copyleftdev/randexpy/actions/workflows/ci.yml/badge.svg)](https://github.com/copyleftdev/randexpy/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

`randexcpy` is a powerful Python library designed for executing actions at random times within a specified duration. It's perfect for scenarios like load testing, simulating real-world events, or implementing backoff strategies where randomness is key.

## 🚀 Features

- **Random Execution**: Execute actions at random intervals within a defined time frame.
- **Synchronous & Asynchronous**: Supports both synchronous and asynchronous task execution.
- **Customizable Randomness**: Use your own random seed for reproducible results.
- **Error Handling**: Built-in support for context-based cancellation and error management.
- **Lightweight**: No external dependencies required, making it easy to integrate into any project.

## 🎯 Use Cases

- **Load Testing**: Simulate real-world scenarios by introducing randomness in task execution.
- **Exponential Backoff**: Implement retry strategies with randomized delays to avoid system overloads.
- **Chaos Engineering**: Introduce controlled randomness to test the resilience of your systems.
- **Event Simulation**: Model user behavior or system events that occur at unpredictable times.

## 📦 Installation

Install `randexcpy` using pip:

```bash
pip install randexcpy
```

Alternatively, you can install the latest development version directly from GitHub:

```bash
pip install git+https://github.com/copleftdev/randexcpy.git
```

## 🛠️ Usage

### Synchronous Execution

```python
from randexcpy import Executor

executor = Executor(max_duration="5s")

# Execute a simple action
result = executor.execute(lambda: "Hello, world!")
print(result)  # Output: Hello, world!
```

### Asynchronous Execution

```python
from randexcpy import Executor

executor = Executor(max_duration="10s")

# Execute an action asynchronously
async_result = executor.execute_async(lambda: "Hello, async world!")

# Get the result with a timeout
print(async_result.get(timeout=5))  # Output: Hello, async world!
```

### Customizing Execution with a Random Seed

```python
from randexcpy import Executor

# Use a custom random seed for reproducibility
executor = Executor(max_duration="3s", seed=42)
result = executor.execute(lambda: "Reproducible result!")
print(result)  # Output will be consistent due to the seed
```

## 📖 API Documentation

### `Executor`

The `Executor` class is the core of `randexcpy`, providing methods for executing actions at random intervals.

#### `Executor.__init__(max_duration: Union[str, timedelta], seed: Optional[int] = None)`

- **Parameters**:
  - `max_duration` (str | timedelta): The maximum duration within which the action must be executed. Example formats: "10s", "5m", "1h".
  - `seed` (Optional[int]): A seed for the random number generator (useful for testing and reproducibility).

- **Raises**:
  - `ValueError`: If `max_duration` is not a valid duration string or `timedelta`.

#### `Executor.execute(action: Callable[[], T], timeout: Optional[float] = None) -> T`

- **Description**: Executes the given action synchronously within a random time frame.
- **Parameters**:
  - `action` (Callable[[], T]): The action to execute.
  - `timeout` (Optional[float]): Maximum time to wait for the action to complete.
- **Returns**: The result of the action.
- **Raises**:
  - `ExecutionError`: If the action fails or execution exceeds the timeout.

#### `Executor.execute_async(action: Callable[[], T]) -> Result`

- **Description**: Executes the given action asynchronously within a random time frame.
- **Parameters**:
  - `action` (Callable[[], T]): The action to execute.
- **Returns**: A `Result` object containing the outcome of the execution.

### `Result`

The `Result` class represents the outcome of an asynchronous execution.

#### `Result.get(timeout: Optional[float] = None) -> Any`

- **Description**: Retrieves the result of the asynchronous execution.
- **Parameters**:
  - `timeout` (Optional[float]): Maximum time to wait for the result.
- **Returns**: The result of the execution.
- **Raises**:
  - `ExecutionError`: If the action failed or the result is not available within the timeout.

## 🧪 Examples

### Example: Load Testing with Random Delays

```python
import requests
from randexcpy import Executor

def make_request():
    response = requests.get("https://example.com")
    return response.status_code

executor = Executor(max_duration="10s")

for _ in range(10):
    status_code = executor.execute(make_request)
    print(f"Received status code: {status_code}")
```

### Example: Asynchronous Event Simulation

```python
import time
from randexcpy import Executor

def log_event():
    print(f"Event logged at {time.time()}")

executor = Executor(max_duration="5s")

# Log events asynchronously
for _ in range(5):
    result = executor.execute_async(log_event)
    result.get(timeout=10)  # Wait for the result with a timeout
```

## 🎨 Code Style and Linting

This project adheres to strict code quality standards using `black`, `isort`, `flake8`, and `pylint`.

To check the code style and linting:

```bash
black .
isort .
flake8 .
pylint randexcpy tests
```

## 🔧 Development Setup

1. **Clone the repository**:

   ```bash
   git clone https://github.com/copleftdev/randexcpy.git
   cd randexcpy
   ```

2. **Create a virtual environment**:

   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows use `.venv\Scripts\activate`
   ```

3. **Install dependencies**:

   ```bash
   pip install --upgrade pip
   pip install -r requirements.txt
   ```

4. **Run tests**:

   ```bash
   pytest
   ```

## 🌟 Contributing

We welcome contributions! To get started:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature/your-feature`).
3. Commit your changes (`git commit -m 'Add new feature'`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a pull request.

## 📄 License

`randexcpy` is licensed under the MIT License. See the [LICENSE](LICENSE) file for more information.

## 📧 Contact

For any inquiries, feel free to reach out to [dj@codetestcode.io](mailto:dj@codetestcode.io).

---

### 📈 Project Status

This project is under active development. New features, bug fixes, and enhancements are regularly added. Stay tuned for updates!
