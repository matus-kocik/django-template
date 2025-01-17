# Django Template

A simple and ready-to-use template for starting new Django projects with modern tools.

## 🚀 Features

- **Poetry** - Simplifies dependency management and virtual environments.
- **Pre-commit hooks** - Maintains code quality by running checks automatically before commits.
- **Python-decouple** - Helps securely manage environment variables.
- **Black** - Auto-formats code to follow consistent style.
- **Ruff** - Provides fast linting and code style enforcement.
- **isort** - Automatically organizes and sorts imports.
- **pytest** - Enables writing and running unit tests efficiently.
- **VS Code Configuration** - Offers pre-configured settings for a seamless developer experience in Visual Studio Code.
- **GitHub Actions** - Automates CI/CD workflows for testing, linting, and deployment.

## 📋 Usage

1. Clone this repository:

    ```bash
    git clone https://github.com/matus-kocik/django-template.git
    cd django-template
    ```

2. Install dependencies using Poetry:

    ```bash
    poetry install --no-root
    ```

    **Note:** Use `poetry install --no-root` to skip installing the template as a package.

3. Create a `.env` file from the example (copy env.example):

    ```bash
    cp .env.example .env
    ```

4. Generate a new secret key:

    You can generate a new secret key using Django's built-in functionality. Run the following command in the Django shell:

    ```bash
    poetry run python -c 'from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())'
    ```

    This command will generate a new secret key each time it is run. Copy the generated key and update the `SECRET_KEY` entry in your `.env` file:

    ```env
    SECRET_KEY=your-new-secret-key
    ```

5. Install pre-commit hooks:

    ```bash
    poetry run pre-commit install
    ```

6. Run pre-commit hooks on all files:

    ```bash
    poetry run pre-commit run --all-files
    ```

7. Auto-update pre-commit hooks:

    To ensure you are using the latest versions of the pre-commit hooks, run:

    ```bash
    poetry run pre-commit autoupdate
    ```

8. Start the Django project:

    The template includes a pre-configured Django project named `config`.
    After installing dependencies, you can immediately start the development server:

    ```bash
    poetry run python manage.py runserver
    ```

9. (Optional) Generate `requirements.txt`:

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

10. (Optional) Set up VS Code configuration:

    If you’re using Visual Studio Code, you can set up recommended settings for formatting, linting, and debugging.

    See the [Optional: VS Code Settings](#optional-vs-code-settings) section for more details.

11. (Optional) Learn about the CI/CD Workflow:

    See the [Continuous Integration and Deployment (CI/CD)](#continuous-integration-and-deployment-cicd) section for details.

## Continuous Integration and Deployment (CI/CD)

This template uses **GitHub Actions** to automate tasks like testing, linting, and dependency management for every `push` or `pull request` to the `main` branch.

### Workflow Steps

1. **Install Dependencies**
   Installs dependencies using Poetry with the `--no-root` flag to avoid packaging the template itself.
2. **Run Black**
   Ensures code adheres to consistent formatting.
3. **Run Ruff**
   Ensures code style adherence and identifies linting issues.
   Note: Ruff is now updated to use settings within the `[lint]` section of `pyproject.toml`.
4. **Run isort**
   Verifies that all imports are correctly sorted.
5. **Run Pytest**
   Executes all unit tests.

### How to View CI/CD Status

1. Go to the **Actions** tab in this repository.
2. Click on the latest workflow run for detailed logs and results.

If any step fails, logs in **GitHub Actions** will help you diagnose and fix the issue.

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

This template includes optional configuration files for Visual Studio Code. These files streamline your development workflow.

### Provided Files

- **`.vscode/settings.json.example`**
  Enables auto-formatting and linting on save.
- **`.vscode/launch.json.example`**
  Configures the debugger for Django.

### How to Use

1. Copy the example files:

    ```bash
    cp .vscode/settings.json.example .vscode/settings.json
    cp .vscode/launch.json.example .vscode/launch.json
    ```

2. Adjust settings to fit your environment.

### Example Settings

```json
{
    "[python]": {
        "editor.defaultFormatter": "charliermarsh.ruff",
        "editor.formatOnSave": true,
        "editor.codeActionsOnSave": {
            "source.organizeImports": true
        }
    },
    "python.testing.unittestEnabled": false,
    "python.testing.pytestEnabled": true
}
```

## 🛠 Tools and Dependencies

### Core Tools

- **Python** - >=3.13.0
  [Documentation](https://docs.python.org/) | [GitHub](https://github.com/python/cpython)
- **Django** - >=5.1.5, <6.0.0
  [Documentation](https://docs.djangoproject.com/) | [GitHub](https://github.com/django/django)
- **Poetry** - Manages dependencies and virtual environments.
  [Documentation](https://python-poetry.org/docs/) | [GitHub](https://github.com/python-poetry/poetry)

### Development Tools

- **Black** - Formats code automatically.
  [Documentation](https://black.readthedocs.io/) | [GitHub](https://github.com/psf/black)
- **Ruff** - A fast linter for code style and quality.
  [Documentation](https://beta.ruff.rs/docs/) | [GitHub](https://github.com/astral-sh/ruff)
- **isort** - Sorts imports automatically.
  [Documentation](https://pycqa.github.io/isort/) | [GitHub](https://github.com/PyCQA/isort)
- **pytest** - Framework for running unit tests.
  [Documentation](https://docs.pytest.org/) | [GitHub](https://github.com/pytest-dev/pytest)

### CI/CD Tools

- **GitHub Actions** - Automates workflows for testing, linting, and CI/CD.
  [Documentation](https://docs.github.com/en/actions) | [GitHub](https://github.com/features/actions)
- **Pre-commit hooks** - Runs code checks automatically before commits.
  [Documentation](https://pre-commit.com/) | [GitHub](https://github.com/pre-commit/pre-commit)

## ⚙️ Configuration

### Python-decouple

This template uses **Python-decouple** to manage environment variables. Create a `.env` file based on the provided `env.example` and update it with your specific settings (e.g., database name, secrets, etc.).
[Documentation](https://github.com/henriquebastos/python-decouple)

#### Example `.env` file

```plaintext
SECRET_KEY=your-secret-key
DEBUG=True
ALLOWED_HOSTS=localhost,127.0.0.1
DATABASE_URL=sqlite:///db.sqlite3
```

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

[tool.ruff.lint]
select = ["E", "F", "I"]  # Check for errors (E), formatting (F), and imports (I).
fixable = ["I"]           # Allow Ruff to fix import sorting if needed.
```

## 📄 License

This project is licensed under the MIT License.
