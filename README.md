# Django Template

A simple and ready-to-use template for starting new Django projects with modern tools.

## 🚀 Features

- **Django** - The web framework for building modern web applications quickly and easily.
- **UV Astral** - Simplifies dependency management, virtual environments, and builds.
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

## 📦 Installing UV Astral

Before using this template, you need to install **UV Astral**, a modern dependency and environment management tool.

### Installation

Install UV Astral using a package manager:

- **Homebrew** (recommended for macOS/Linux):

    ```bash
    brew install uv
    ```

- **Windows** (via Scoop):

    ```bash
    scoop install uv
    ```

For detailed instructions and troubleshooting, refer to the [UV Astral Documentation](https://docs.astral.sh/uv/).

## Initial Setup for New Projects

1. Use this template on GitHub:

    You can create a new repository using this template by clicking the "Use this template" button on the GitHub page of this repository.

2. Clone this repository:

    ```bash
    git clone https://github.com/matus-kocik/django-template.git
    cd django-template
    ```

After creating a new repository using this template, follow these steps to customize it for your project:

1. **Rename the Project:**
   Update the project name and references throughout the repository:
   - `pyproject.toml`:

     ```toml
     [project]
     name = "your-new-project-name"
     ```

   - `.github/workflows/ci.yml`:
     Replace occurrences of `django-template` with your new project name.

   - `docker-compose.yaml`:
     Update the container names and service names to avoid conflicts with other projects:

     ```yaml
     services:
       postgres:
         container_name: "your-new-project-postgres"
     ```

2. **Update the README:**
   - Replace the title and description at the top of the README file to match your new project's purpose.
   - Remove or adjust template-specific sections that no longer apply.

3. **Check Other References:**
   - Search the repository for any mentions of `django-template` and replace them with your new project's name.
   - This includes code comments, configuration files, and documentation.

4. **Verify CI/CD:**
   - Ensure that the GitHub Actions workflow (`ci.yml`) aligns with your new project setup.

5. **Update `.env.example`:**
   - Customize the `.env.example` file with environment variables specific to your new project.

6. **Run Pre-commit:**
   - Run pre-commit hooks to format and lint your changes:

     ```bash
     uv run pre-commit run --all-files
     ```

### Additional Notes

- This template is designed to streamline the setup of Django projects with modern tools.
- If you encounter any issues, refer to the template's documentation or open an issue on the original repository.

## 🗌 Usage

1. Create a `.env` file:

    Copy the `.env.example` file to `.env` and configure the environment variables:

    ```bash
    cp .env.example .env
    ```

    Update the database credentials and other settings in the `.env` file if necessary.

    **Important:** Ensure that your `.env` file is listed in `.gitignore` to prevent it from being committed to the repository. This file contains sensitive information and should always remain private.

2. Start Docker services for dependencies:

    Use Docker Compose to start the PostgreSQL database:

    ```bash
    docker-compose up -d
    ```

    This will start the PostgreSQL database locally. Ensure that Docker is installed and running on your machine.

3. Install dependencies using UV Astral:

    ```bash
    uv sync
    ```

    **Note:** UV Astral uses the `uv.lock` file to ensure reproducible builds and automatically manages virtual environments for your project. You don’t need to activate it manually.

    If you need to install dependencies using `pip` for other environments, you can generate and use the `requirements.txt` file:

    ```bash
    uv export --output-file requirements.txt --with-hashes
    pip install -r requirements.txt
    ```

    This approach ensures compatibility with tools or environments that rely on `requirements.txt` while maintaining reproducibility through the `uv.lock` file.

4. **Check Python Interpreter and Environment**

    After installing dependencies, verify that the correct Python interpreter is being used.

    To check your Python version:

    ```bash
    python --version
    ```

    UV Astral automatically manages your virtual environment. No manual activation is required.

5. **GitHub Actions Secrets:**

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

6. Generate a new `SECRET_KEY`:

    You can generate a new secret key using Django's built-in functionality. Run the following command in the Django shell:

    ```bash
    uv run python -c 'from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())'
    ```

    This command will generate a new secret key each time it is run. Copy the generated key and update the `SECRET_KEY` entry in your `.env` file:

    ```env
    SECRET_KEY=your-new-secret-key
    ```

    **Note:** Always keep your `SECRET_KEY` safe and never share it publicly. For CI/CD workflows, store it securely in GitHub Actions Secrets.

7. Set up and update pre-commit hooks:

    Pre-commit hooks help maintain code quality by running checks automatically. Follow these steps to set up and update them:

    1. Install pre-commit hooks:

        ```bash
        uv run pre-commit install
        ```

    2. Auto-update to the latest versions of the hooks:

        ```bash
        uv run pre-commit autoupdate
        ```

    3. Run pre-commit hooks on all files to ensure compliance:

        ```bash
        uv run pre-commit run --all-files
        ```

8. Start the Django project:

    The template includes a pre-configured Django project named `src/config`.

9. Create a new app:

    To create a new app, use the `startapp` command and specify the app name and its location within the `src/apps` directory. For example, to create an app named `my_app`:

    ```bash
    mkdir src/apps/my_app
    uv run python src/manage.py startapp my_app src/apps/my_app
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

10. Apply migrations:

    After creating a new app and adding models, apply the migrations to update the database schema:

    ```bash
    uv run python src/manage.py makemigrations
    uv run python src/manage.py migrate
    ```

11. Run the development server:

    Start the Django development server to test your changes:

    ```bash
    uv run python src/manage.py runserver
    ```

    The server will be available at [http://localhost:8000](http://localhost:8000).

12. (Optional) Set up VS Code configuration:

    If you’re using Visual Studio Code, this template provides recommended settings for formatting, linting, and debugging.

    To get started, follow the instructions in the [Optional: VS Code Settings](#optional-vs-code-settings) section below.

13. (Optional) Learn about the CI/CD Workflow:

    See the [Continuous Integration and Deployment (CI/CD)](#continuous-integration-and-deployment-cicd) section for details.

## Continuous Integration and Deployment (CI/CD)

This template uses **GitHub Actions** to automate tasks like testing, linting, and dependency management for every `push` or `pull request` to the `main` branch.

### Workflow Steps

1. **Install Dependencies**
   Installs dependencies using UV Astral.
2. **Run Black**
   Ensures code adheres to consistent formatting.
3. **Run Ruff**
   Ensures code style adherence and identifies linting issues.
   Note: Ruff is now updated to use settings within the `[lint]` section of `pyproject.toml`.
4. **Run isort**
   Verifies that all imports are correctly sorted.
5. **Run Pytest**
   Executes all unit tests.

## 🧪 Running Tests

To run all tests in the project, use:

```bash
uv run pytest
```

Test File Structure

Test files are located in the `tests/` directory. Example:

```plaintext
tests/
├── conftest.py        # Shared fixtures for tests
├── test_sample.py     # Example test file
```

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

## ⚙️ Tools and Dependencies

### Core Tools

- **Python** - >=3.13.0
  [Documentation](https://docs.python.org/) | [GitHub](https://github.com/python/cpython)
- **Django** - >=5.1.5, <6.0.0
  [Documentation](https://docs.djangoproject.com/) | [GitHub](https://github.com/django/django)
- **UV Astral** - Manages dependencies, environments, and builds.
  [Documentation](https://docs.astral.sh/uv/) | [GitHub](https://github.com/astra.sh/uv)

### Development Tools

- **Black** - Formats code automatically.
  [Documentation](https://black.readthedocs.io/) | [GitHub](https://github.com/psf/black)
- **Ruff** - A fast linter for code style and quality.
  [Documentation](https://beta.ruff.rs/docs/) | [GitHub](https://github.com/astral-sh/ruff)
- **isort** - Sorts imports automatically.
  [Documentation](https://pycqa.github.io/isort/) | [GitHub](https://github.com/PyCQA/isort)
- **pytest** - Framework for running unit tests.
  [Documentation](https://docs.pytest.org/) | [GitHub](https://github.com/pytest-dev/pytest)
- **pip-tools** - Manages `.in` and `.txt` files for dependencies.
  [Documentation](https://pip-tools.readthedocs.io/) | [GitHub](https://github.com/jazzband/pip-tools)
- **Docker** - Runs dependencies like PostgreSQL in isolated containers.
  [Documentation](https://docs.docker.com/) | [GitHub](https://github.com/docker)
- **Pre-commit hooks** - Runs code checks automatically before commits.
  [Documentation](https://pre-commit.com/) | [GitHub](https://github.com/pre-commit/pre-commit)

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
