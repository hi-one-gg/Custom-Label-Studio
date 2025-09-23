# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview
This is a Label Studio instance - an open-source data labeling platform built with Django. It's used for annotating various data types (images, text, audio, video) for machine learning projects. This appears to be a customized version for 국립종자원 (National Seed Office) project.

## Essential Commands

### Development Environment
```bash
# Run development server (Django)
python manage.py runserver
# or
label-studio

# Run migrations
python manage.py migrate

# Create superuser
python manage.py createsuperuser

# Collect static files after CSS/JS changes
python manage.py collectstatic --noinput
```

### Testing
```bash
# Run all tests (uses pytest)
pytest

# Run specific test file
pytest tests/test_annotations.py

# Run tests with coverage
pytest --cov

# Run Tavern API tests
pytest tests/*.tavern.yml

# Run tests in parallel
pytest -n auto
```

### Code Quality
```bash
# Run linter (if Ruff is installed)
ruff check .

# Fix linting issues automatically
ruff check . --fix

# Format code (if Blue formatter is installed)
blue label_studio/
```

## Architecture Overview

### Core Django Application Structure
- **`core/`** - Main Django application with settings, middleware, and base configurations
  - `settings/` - Django settings split into base.py and label_studio.py
  - `management/commands/` - Custom Django management commands
  - `middleware.py` - Custom middleware including session timeout and activity tracking

### Main Applications
- **`projects/`** - Project management and configuration
- **`tasks/`** - Task management for labeling work items
- **`users/`** - User authentication and management
- **`organizations/`** - Multi-organization support
- **`data_manager/`** - Data management interface
- **`data_import/`** - Handles importing various data formats
- **`data_export/`** - Export functionality for labeled data
- **`ml/`** - Machine learning backend integration
- **`ml_models/`** - ML model management
- **`webhooks/`** - Webhook integration for external services
- **`io_storages/`** - Integration with cloud storage (S3, GCS, Azure)

### Key Configuration Files
- **`pytest.ini`** - PyTest configuration with Django settings
- **`feature_flags.json`** - Feature flag configurations (extensive feature toggle system)
- **`.coveragerc`** - Code coverage configuration

### Database
- Default: SQLite for development (`label_studio.sqlite3`)
- Production: PostgreSQL supported
- Database backend controlled by `DJANGO_DB` environment variable

### Important Environment Variables
- `DJANGO_SETTINGS_MODULE` - Set to `core.settings.label_studio`
- `DEBUG` - Enable debug mode
- `DJANGO_DB` - Database backend selection
- `LOG_LEVEL` - Logging level (default: WARNING)
- `SENTRY_DSN` - Error tracking configuration

### Testing Approach
- Unit tests using pytest-django
- API tests using Tavern (YAML-based REST API testing)
- Test fixtures and factories defined in `tests/conftest.py`
- Parallel test execution supported with pytest-xdist

### Frontend Assets
The frontend is pre-built and served from:
- `web/dist/libs/datamanager/` - Data manager UI
- `web/dist/libs/editor/` - Annotation editor
- `web/dist/apps/labelstudio/` - Main application UI
- `core/static/` and `core/static_build/` - Static assets

## Development Tips
- Always run migrations after pulling changes: `python manage.py migrate`
- Use `python manage.py shell` for Django shell with project context
- Test files follow pattern: `test_*.py` or `*.tavern.yml` for API tests
- The project uses Django REST Framework for API endpoints
- Feature flags can be toggled in `feature_flags.json`
- After modifying CSS files in `core/static/css/`, run `python manage.py collectstatic --noinput`
- Static files are served from both `core/static/` (source) and `core/static_build/` (collected)