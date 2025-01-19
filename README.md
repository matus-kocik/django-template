# Django Template

A simple and ready-to-use template for starting new Django projects with modern tools.

## 🚀 Features

- **Poetry** - Simplifies dependency management and virtual environments.
- **Pre-commit hooks** - Maintains code quality by running checks automatically before commits.
- **Python-decouple** - Helps securely manage environment variables for different environments.
- **Black** - Auto-formats Python code to follow consistent style.
- **Ruff** - Provides fast linting and code style enforcement.
- **isort** - Automatically organizes and sorts imports for better code readability.
- **pytest** - Enables efficient writing and running of unit tests.
- **VS Code Configuration** - Offers pre-configured settings for a seamless developer experience in Visual Studio Code.
- **GitHub Actions** - Automates CI/CD workflows for testing, linting, and deployment.
- **PostgreSQL** - Uses PostgreSQL as the default database for all projects, powered by the modern **`psycopg`** driver.
- **Docker for Dependencies** - Runs services like PostgreSQL in Docker for consistent environments and simplified setup.

## 📋 Usage

1. Use this template on GitHub:

    You can create a new repository using this template by clicking the "Use this template" button on the GitHub page of this repository.

2. Clone this repository:

    ```bash
    git clone https://github.com/matus-kocik/django-template.git
    cd django-template
    ```

3. Create a `.env` file:

    Copy the `.env.example` file to `.env` and configure the environment variables:

    ```bash
    cp .env.example .env
    ```

    Update the database credentials and other settings in the `.env` file if necessary.

    **Important:** Ensure that your `.env` file is listed in `.gitignore` to prevent it from being committed to the repository. This file contains sensitive information and should always remain private.

4. Start Docker services for dependencies:

    Use Docker Compose to start the PostgreSQL database:

    ```bash
    docker-compose up -d
    ```

    This will start the PostgreSQL database locally. Ensure that Docker is installed and running on your machine.

5. Install dependencies using Poetry:

    ```bash
    poetry install --no-root
    ```

    **Note:** Use `poetry install --no-root` to skip installing the template as a package.

6. **Check Python Interpreter and Environment**

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

7. **GitHub Actions Secrets:**

    For CI/CD workflows, you need to set up secrets in GitHub Actions to avoid exposing sensitive information. Add the following secrets in your repository's **Settings > Secrets and variables > Actions > Secrets**:

    - `SECRET_KEY` - The Django secret key.
    - `POSTGRES_DB` - Name of the PostgreSQL database.
    - `POSTGRES_USER` - Username for the PostgreSQL database.
    - `POSTGRES_PASSWORD` - Password for the PostgreSQL database.
    - Any other necessary environment variables (e.g., email settings).

    Update your `.github/workflows/ci.yml` file to use these secrets. For example:

    ```yaml
    - name: Set Environment Variables from Secrets
      run: |
        echo "SECRET_KEY=${{ secrets.SECRET_KEY }}" >> .env
        echo "ALLOWED_HOSTS=127.0.0.1,localhost" >> .env
        echo "DB_ENGINE=django.db.backends.postgresql" >> .env
        echo "DB_NAME=${{ secrets.POSTGRES_DB }}" >> .env
        echo "DB_USER=${{ secrets.POSTGRES_USER }}" >> .env
        echo "DB_PASSWORD=${{ secrets.POSTGRES_PASSWORD }}" >> .env
        echo "DB_HOST=localhost" >> .env  # Use 'localhost' for local development or 'db' if using Docker Compose
        echo "DB_PORT=5432" >> .env
        echo "DEBUG=True" >> .env  # Set DEBUG False for production
    ```

8. Generate a new `SECRET_KEY`:

    You can generate a new secret key using Django's built-in functionality. Run the following command in the Django shell:

    ```bash
    poetry run python -c 'from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())'
    ```

    This command will generate a new secret key each time it is run. Copy the generated key and update the `SECRET_KEY` entry in your `.env` file:

    ```env
    SECRET_KEY=your-new-secret-key
    ```

    **Note:** Always keep your `SECRET_KEY` safe and never share it publicly. For CI/CD workflows, store it securely in GitHub Actions Secrets.

9. Set up and update pre-commit hooks:

    Pre-commit hooks help maintain code quality by running checks automatically. Follow these steps to set up and update them:

    1. Install pre-commit hooks:

        ```bash
        poetry run pre-commit install
        ```

    2. Auto-update to the latest versions of the hooks:

        ```bash
        poetry run pre-commit autoupdate
        ```

    3. Run pre-commit hooks on all files to ensure compliance:

        ```bash
        poetry run pre-commit run --all-files
        ```

10. Start the Django project:

    The template includes a pre-configured Django project named `src/config`.

11. Create a new app:

    To create a new app, use the `startapp` command and specify the app name and its location within the `src/apps` directory. For example, to create an app named `my_app`:

    ```bash
    mkdir src/apps/my_app
    poetry run python src/manage.py startapp my_app src/apps/my_app
    ```

    After creating the app, follow these steps:

    1. **Add the app to `INSTALLED_APPS`:**
       Open the `src/config/settings.py` file and include the full path to your app in the `INSTALLED_APPS` list:

       ```python
       INSTALLED_APPS = [
           # Other installed apps
           'apps.my_app',
       ]
       ```

    2. **Update the `apps.py` file:**
       Ensure the `name` attribute in the app’s `apps.py` file reflects the correct path:

       ```python
       # src/apps/my_app/apps.py
       from django.apps import AppConfig

       class MyAppConfig(AppConfig):
           default_auto_field = 'django.db.models.BigAutoField'
           name = 'apps.my_app'
       ```

12. Apply migrations:

    After creating a new app and adding models, apply the migrations to update the database schema:

    ```bash
    poetry run python src/manage.py makemigrations
    poetry run python src/manage.py migrate
    ```

13. Run the development server:

    Start the Django development server to test your changes:

    ```bash
    poetry run python src/manage.py runserver
    ```

    The server will be available at [http://localhost:8000](http://localhost:8000).

    **Note:** These steps are the same as those outlined earlier for starting the project but are repeated here for convenience when adding new apps.

14. (Optional) Set up pre-commit hooks:

    Pre-commit hooks help maintain code quality by automating checks before each commit. These hooks include:

    - Formatting code with **Black**.
    - Sorting imports with **isort**.
    - Linting and ensuring code quality using **Ruff** (configured via `[lint]` section in `pyproject.toml`).
    - Running automated tests with **Pytest**.

    To set up and run pre-commit hooks, follow the steps outlined in the section "Set up and update pre-commit hooks."

15. (Optional) Generate `requirements.txt`:

    If you need a `requirements.txt` file for deployment or compatibility with certain tools, you can generate it from `pyproject.toml` using Poetry.

    1. **To include package hashes (recommended for security):**

        ```bash
        poetry export --format=requirements.txt --output=requirements.txt
        ```

    2. **To generate a simpler `requirements.txt` without hashes:**

        ```bash
        poetry export --without-hashes --output=requirements.txt
        ```

    **Notes:**
    - Use `requirements.txt` if deploying to platforms that don't support Poetry (e.g., some server environments or third-party tools).
    - Keep your `requirements.txt` updated whenever you add or update dependencies in `pyproject.toml`.
    - Including hashes enhances security by ensuring exact dependency versions during installation.

16. (Optional) Set up VS Code configuration:

    If you’re using Visual Studio Code, this template provides recommended settings for formatting, linting, and debugging.

    To get started, follow the instructions in the [Optional: VS Code Settings](#optional-vs-code-settings) section below.

17. (Optional) Learn about the CI/CD Workflow:

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

To securely manage environment variables required by your workflow, use **GitHub Secrets**. Secrets ensure that sensitive information, such as API keys and database credentials, is not exposed in your version control.

1. Set up the necessary secrets in your repository's **Settings > Secrets and variables > Actions > Secrets**.
   Add the following secrets:
   - `SECRET_KEY` - The Django secret key.
   - `POSTGRES_DB` - Name of the PostgreSQL database.
   - `POSTGRES_USER` - Username for the PostgreSQL database.
   - `POSTGRES_PASSWORD` - Password for the PostgreSQL database.
   - Any other necessary variables (e.g., email settings).

2. Update your workflow file to reference these secrets and dynamically create a `.env` file during the workflow execution. For example:

    ```yaml
    - name: Set Environment Variables from Secrets
      run: |
        echo "SECRET_KEY=${{ secrets.SECRET_KEY }}" >> .env
        echo "ALLOWED_HOSTS=127.0.0.1,localhost" >> .env
        echo "DB_ENGINE=django.db.backends.postgresql" >> .env
        echo "DB_NAME=${{ secrets.POSTGRES_DB }}" >> .env
        echo "DB_USER=${{ secrets.POSTGRES_USER }}" >> .env
        echo "DB_PASSWORD=${{ secrets.POSTGRES_PASSWORD }}" >> .env
        echo "DB_HOST=localhost" >> .env  # Use 'localhost' for local development or 'db' if using Docker Compose
        echo "DB_PORT=5432" >> .env
        echo "DEBUG=True" >> .env  # Set DEBUG False for production
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

This template includes optional configuration files for Visual Studio Code to streamline your development workflow.

### Provided Files

- **`.vscode/settings.json.example`**
  Enables auto-formatting and linting on save.
- **`.vscode/launch.json.example`**
  Configures the debugger for Django.

### How to Use

1. Copy the example files to the `.vscode` directory in your project:

    ```bash
    cp .vscode/settings.json.example .vscode/settings.json
    cp .vscode/launch.json.example .vscode/launch.json
    ```

2. Adjust the settings to fit your environment.

### Example Settings

The following example configuration enables Ruff as the default formatter, organizes imports, and configures pytest for testing:

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
- **Docker** - Runs dependencies like PostgreSQL in isolated containers.
  [Documentation](https://docs.docker.com/) | [GitHub](https://github.com/docker)
- **Pre-commit hooks** - Runs code checks automatically before commits.
  [Documentation](https://pre-commit.com/) | [GitHub](https://github.com/pre-commit/pre-commit)

### CI/CD Tools

- **GitHub Actions** - Automates workflows for testing, linting, and CI/CD.
  [Documentation](https://docs.github.com/en/actions) | [GitHub](https://github.com/features/actions)

### Database Tools

- **PostgreSQL** - The default database for all projects.
  [Documentation](https://www.postgresql.org/docs/) | [GitHub](https://github.com/postgres/postgres)

### Key Python Libraries

- **psycopg** - Modern PostgreSQL database driver for Django with support for asynchronous operations.
  [Documentation](https://www.psycopg.org/psycopg3/docs/) | [GitHub](https://github.com/psycopg/psycopg)
- **python-decouple** - Manages environment variables securely.
  [Documentation](https://github.com/henriquebastos/python-decouple) | [GitHub](https://github.com/henriquebastos/python-decouple)

## ⚙️ Configuration

### Python-decouple

This template uses **Python-decouple** to manage environment variables. Create a `.env` file based on the provided `env.example` and update it with your specific settings (e.g., database credentials, secrets, etc.).

#### Example `.env` file

```plaintext
SECRET_KEY=your-secret-key
DEBUG=True
ALLOWED_HOSTS=localhost,127.0.0.1

# Django database configuration
DB_ENGINE=django.db.backends.postgresql
DB_NAME=mydatabase
DB_USER=myuser
DB_PASSWORD=mypassword
DB_HOST=localhost  # Use 'localhost' for local development or 'db' if using Docker Compose
DB_PORT=5432

# Docker PostgreSQL configuration
POSTGRES_DB=mydatabase
POSTGRES_USER=myuser
POSTGRES_PASSWORD=mypassword
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
