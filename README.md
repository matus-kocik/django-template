# Django Template

A simple and ready-to-use template for starting new Django projects with modern tools.

## 🚀 Features

- **Poetry** - For dependency management and virtual environments.
- **Pre-commit hooks** - Automates code checks before each commit to maintain code quality.
- **Python-decouple** - For managing environment variables securely and flexibly.
- **Black** - Ensures consistent code formatting across the project.
- **Ruff** - A fast linter to catch errors and enforce style guidelines.
- **isort** - Automatically organizes and sorts imports.
- **pytest** - For running automated tests.
- **MyPy** - Static type checker for Python.

## 📋 Usage

1. Clone this repository:

    ```bash
    git clone https://github.com/matus-kocik/django-template.git
    cd django-template
    ```

2. Install dependencies using Poetry:

    ```bash
    poetry install
    ```

3. Create a `.env` file from the example (copy env.example):

    ```bash
    cp .env.example .env
    ```

4. Initialize a new Django project (replace *my_project* with your desired project name):

    ```bash
    poetry run django-admin startproject my_project .
    ```

5. Start the development server:

    ```bash
    poetry run python manage.py runserver
    ```

6. (Optional) Set up pre-commit hooks:

    Install pre-commit hooks to automatically check and format your code before each commit:

    ```bash
    poetry run pre-commit install
    ```

    To manually run all hooks on the entire codebase, use:

    ```bash
    poetry run pre-commit run --all-files
    ```

    These hooks will:
    - Format code with **Black**.
    - Sort imports with **isort**.
    - Lint and check code quality with **Ruff**.
    - Check for type correctness with **MyPy**.
    - Run automated tests with **Pytest**.

7. (Optional) Generate `requirements.txt`:

    If you need a `requirements.txt` file for deployment or compatibility with certain tools, you can generate it from `pyproject.toml` using Poetry.

    To include package hashes (recommended for security):

    ```bash
    poetry export --format=requirements.txt --output=requirements.txt
    ```

    If you prefer a simpler `requirements.txt` without hashes:

    ```bash
    poetry export --without-hashes --output=requirements.txt
    ```

    **Notes:**
    - Use `requirements.txt` if deploying to platforms that don't support Poetry.
    - Keep your `requirements.txt` updated whenever you add or update dependencies in `pyproject.toml`.

8. (Optional) Set up VS Code configuration:

    If you’re using Visual Studio Code, you can set up recommended settings for formatting, linting, and debugging.

    See the [Optional: VS Code Settings](#optional-vs-code-settings) section for more details.

## 🧪 Running Tests

To run all tests in the project, use:

```bash
poetry run pytest
```

Test File Structure

Test files are located in the tests/ directory. Example:

```plaintext
tests/
├── conftest.py        # Shared fixtures for tests
├── test_sample.py     # Example test file
```

- conftest.py: Contains reusable fixtures for tests.
- test_sample.py: A simple test file to demonstrate how tests are structured.

## Optional: VS Code Settings

This template includes optional configuration files for Visual Studio Code. If you’re using VS Code, these files can streamline your setup:

- `.vscode/settings.json.example`: Configures auto-formatting and linting on save.
- `.vscode/launch.json.example`: Prepares the debugger for Django.

To use these files:

1. Copy the examples to their respective locations:

    ```bash
    cp .vscode/settings.json.example .vscode/settings.json
    cp .vscode/launch.json.example .vscode/launch.json
    ```

2. Adjust settings if needed to match your environment.

## 🛠 Tools and Dependencies

### Core Tools

- **Python** - >=3.13.0
  [Documentation](https://docs.python.org/) | [GitHub](https://github.com/python/cpython)
- **Django** - >=5.1.5, <6.0.0
  [Documentation](https://docs.djangoproject.com/) | [GitHub](https://github.com/django/django)
- **Poetry** - Manages dependencies and virtual environments.
  [Documentation](https://python-poetry.org/docs/) | [GitHub](https://github.com/python-poetry/poetry)

### Development Tools

- **Black** - Automatically formats your code for consistency.
  [Documentation](https://black.readthedocs.io/) | [GitHub](https://github.com/psf/black)
- **Ruff** - A fast linter for code quality and style enforcement.
  [Documentation](https://beta.ruff.rs/docs/) | [GitHub](https://github.com/astral-sh/ruff)
- **isort** - Ensures imports are sorted and organized.
  [Documentation](https://pycqa.github.io/isort/) | [GitHub](https://github.com/PyCQA/isort)
- **pytest** - A testing framework for running unit tests.
  [Documentation](https://docs.pytest.org/) | [GitHub](https://github.com/pytest-dev/pytest)
- **MyPy** - A static type checker to ensure type correctness.
  [Documentation](https://mypy.readthedocs.io/) | [GitHub](https://github.com/python/mypy)
- **Pre-commit hooks** - Automates running Black, Ruff, isort, and MyPy before commits.
  [Documentation](https://pre-commit.com/) | [GitHub](https://github.com/pre-commit/pre-commit)

## ⚙️ Configuration

### Python-decouple

This template uses **Python-decouple** to manage environment variables. Create a `.env` file based on the provided `env.example` and update it with your specific settings (e.g., database name, secrets, etc.).
[Documentation](https://github.com/henriquebastos/python-decouple)

### Tools Configuration

#### **Black**

```toml
[tool.black]
line-length = 88
```

#### **isort**

```toml
[tool.isort]
profile = "black"
```

#### **Ruff**

```toml
[tool.ruff]
line-length = 88
select = ["E", "F", "I"]  # Check for errors (E), formatting (F), and imports (I).
fixable = ["I"]           # Allow Ruff to fix import sorting if needed.
```

#### **MyPy**

This template includes a basic MyPy configuration file (mypy.ini):

```ini
[mypy]
plugins =
    mypy_django_plugin.main

[mypy.plugins.django-stubs]
django_settings_module = "my_project.settings"  # Replace 'my_project' with your project name

[mypy-*.migrations.*]
ignore_errors = True  # Ignore errors in Django migrations (auto-generated code)
```

To run MyPy checks:

```bash
poetry run mypy .
```

## 📄 License

This project is licensed under the MIT License.
