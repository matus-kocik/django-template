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

1. Use this template on GitHub:

    You can create a new repository using this template by clicking the "Use this template" button on the GitHub page of this repository.

2. Clone this repository:

    ```bash
    git clone https://github.com/matus-kocik/django-template.git
    cd django-template
    ```

3. Install dependencies using Poetry:

    ```bash
    poetry install --no-root
    ```

    **Note:** Use `poetry install --no-root` to skip installing the template as a package.

4. **Check Python Interpreter and Environment**

    After installing dependencies, verify that the correct Python interpreter is being used and that the Poetry environment is properly configured.

    To check your Python version:

    ```bash
    python --version
    ```

    To check the Poetry environment and interpreter:

    ```bash
    poetry env info
    ```

    Ensure that the `Executable` path points to the Python interpreter inside your Poetry-managed virtual environment. If not, activate the correct environment:

    ```bash
    poetry env use <path-to-python>
    ```

5. Create a `.env` file from the example:

    For local development, create a `.env` file by copying the provided `.env.example`:

    ```bash
    cp .env.example .env
    ```

    Customize the `.env` file with your specific settings. The `.env.example` file provides default values as a starting point.

    **Important:** Ensure `.env` is listed in your `.gitignore` file to avoid committing sensitive information.

6. **GitHub Actions Secrets:**

    For CI/CD workflows, you need to set up secrets in GitHub Actions to avoid exposing sensitive information. Add the following secrets in your repository's **Settings > Secrets and variables > Actions > Secrets**:

    - `SECRET_KEY`
    - `DATABASE_URL`
    - Any other necessary environment variables.

    Update your `.github/workflows/ci.yml` file to use these secrets.
    For example:

    ```yaml
    - name: Set Environment Variables from Secrets
      run: |
        echo "SECRET_KEY=dummy-secret-key" >> .env # Using dummy value for CI/CD later on set it to ${{ secrets.SECRET_KEY }}
        echo "ALLOWED_HOSTS=127.0.0.1,localhost" >> .env # Set ALLOWED_HOSTS for production environment later on set it to ${{ secrets.ALLOWED_HOSTS }}
        echo "DATABASE_URL=${{ secrets.DATABASE_URL }}" >> .env # Set DATABASE_URL from GitHub Secrets
        echo "DEBUG=False" >> .env  # Set DEBUG for production
    ```

7. Generate a new SECRET_KEY:

    You can generate a new secret key using Django's built-in functionality. Run the following command in the Django shell:

    ```bash
    poetry run python -c 'from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())'
    ```

    This command will generate a new secret key each time it is run. Copy the generated key and update the `SECRET_KEY` entry in your `.env` file:

    ```env
    SECRET_KEY=your-new-secret-key
    ```

8. Install pre-commit hooks:

    ```bash
    poetry run pre-commit install
    ```

9. Auto-update pre-commit hooks:

    To ensure you are using the latest versions of the pre-commit hooks, run:

    ```bash
    poetry run pre-commit autoupdate
    ```

10. Run pre-commit hooks on all files:

    ```bash
    poetry run pre-commit run --all-files
    ```

11. Start the Django project:

    The template includes a pre-configured Django project named `src/config`.

12. Create a new app:

    To create a new app, use the `startapp` command and specify the app name and its location within the `src/apps` directory. For example, to create an app named `my_app`:

    ```bash
    mkdir src/apps/my_app
    poetry run python src/manage.py startapp my_app src/apps/my_app
    ```

    **Note:** When adding your application to `INSTALLED_APPS` in the project settings `(src/config/settings.py)`, include the full path to the app, such as `apps.my_app`. Additionally, ensure the name attribute in the app’s `apps.py` file reflects the correct path. Update it as follows:

    ```python
    # src/apps/my_app/apps.py
    from django.apps import AppConfig

    class MyAppConfig(AppConfig):
        default_auto_field = 'django.db.models.BigAutoField'
        name = 'apps.my_app'
    ```

13. Apply migrations:

    After creating a new app and adding models, apply the migrations:

    ```bash
    poetry run python src/manage.py makemigrations
    poetry run python src/manage.py migrate
    ```

14. Run the development server:

    Start the Django development server:

    ```bash
    poetry run python src/manage.py runserver
    ```

15. (Optional) Set up pre-commit hooks:

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
    - Lint and ensure code quality using **Ruff** (configured via `[lint]` section).
    - Run automated tests with **Pytest**.

16. (Optional) Generate `requirements.txt`:

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

17. (Optional) Set up VS Code configuration:

    If you’re using Visual Studio Code, you can set up recommended settings for formatting, linting, and debugging.

    See the [Optional: VS Code Settings](#optional-vs-code-settings) section for more details.

18. (Optional) Learn about the CI/CD Workflow:

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

### Managing `.env` in CI/CD

To securely manage environment variables required by your workflow, use **GitHub Secrets**. Secrets ensure that sensitive information, such as API keys and database URLs, is not exposed in your version control.

1. Set up the necessary secrets in your repository's **Settings > Secrets and variables > Actions > Secrets**.
   Add the following secrets:
   - `SECRET_KEY`
   - `DATABASE_URL`
   - Any other necessary variables.

2. Update your workflow file to reference these secrets and dynamically create a `.env` file during the workflow execution. For example:

    ```yaml
    - name: Set Environment Variables from Secrets
      run: |
        echo "SECRET_KEY=dummy-secret-key" >> .env # Using dummy value for CI/CD later on set it to ${{ secrets.SECRET_KEY }}
        echo "ALLOWED_HOSTS=127.0.0.1,localhost" >> .env # Set ALLOWED_HOSTS for production environment later on set it to ${{ secrets.ALLOWED_HOSTS }}
        echo "DATABASE_URL=${{ secrets.DATABASE_URL }}" >> .env # Set DATABASE_URL from GitHub Secrets
        echo "DEBUG=False" >> .env  # Set DEBUG for production
    ```

**Note:** Avoid committing the `.env` file or any sensitive data directly to version control. Use **GitHub Secrets** for secure storage and injection during workflows.

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
