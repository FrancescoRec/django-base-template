# Zakat - Django 5 Project

A Django 5 application for Zakat management with PostgreSQL database, Docker support, and production-ready configuration.

## Features

- Django 5.0+ with Python 3.11+
- Separated settings for development and production
- PostgreSQL database with SQLite fallback for development
- Docker and Docker Compose support
- User authentication with login/logout
- Bootstrap-styled templates
- WhiteNoise for static file serving
- Production-ready security settings
- Gunicorn WSGI server for production

## Quick Start (Windows PowerShell)

### Local Development Setup

1. **Clone and setup virtual environment:**
```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

2. **Configure environment variables:**
```powershell
# Copy the example environment file
Copy-Item env.example .env

# Set Django settings module (PowerShell)
$env:DJANGO_SETTINGS_MODULE = "config.settings.local"

# Or permanently set it (requires admin privileges)
setx DJANGO_SETTINGS_MODULE "config.settings.local"
```

3. **Run migrations and start server:**
```powershell
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

4. **Access the application:**
   - Application: http://127.0.0.1:8000
   - Admin: http://127.0.0.1:8000/admin

### Docker Development Setup

1. **Copy environment file:**
```powershell
Copy-Item env.example .env
```

2. **Start with Docker Compose:**
```powershell
docker-compose up --build
```

3. **Access the application:**
   - Application: http://localhost:8000
   - PostgreSQL: localhost:5432

## Project Structure

```
zakat/
├── config/                 # Django project configuration
│   ├── settings/          # Split settings
│   │   ├── __init__.py
│   │   ├── base.py        # Common settings
│   │   ├── local.py       # Development settings
│   │   └── prod.py        # Production settings
│   ├── __init__.py
│   ├── asgi.py
│   ├── urls.py
│   └── wsgi.py
├── core/                   # Main application
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   ├── urls.py
│   └── views.py
├── templates/              # HTML templates
│   ├── base.html
│   ├── core/
│   │   └── home.html
│   └── registration/
│       └── login.html
├── static/                 # Static files (CSS, JS, images)
├── media/                  # User uploaded files
├── staticfiles/            # Collected static files (production)
├── docker-compose.yml      # Docker Compose configuration
├── Dockerfile              # Docker image definition
├── entrypoint.sh           # Docker entrypoint script
├── requirements.txt        # Python dependencies
├── env.example             # Environment variables template
├── .gitignore             # Git ignore rules
├── Makefile               # Common commands
├── manage.py              # Django management script
└── README.md              # This file
```

## Environment Variables

Copy `env.example` to `.env` and configure:

### Required Variables
- `SECRET_KEY`: Django secret key (change in production)
- `DEBUG`: Enable/disable debug mode
- `DB_NAME`, `DB_USER`, `DB_PASSWORD`: Database credentials
- `DB_HOST`, `DB_PORT`: Database connection

### Optional Variables
- `USE_SQLITE`: Use SQLite instead of PostgreSQL (local development)
- `ALLOWED_HOSTS`: Comma-separated list of allowed hosts (production)
- `DJANGO_SUPERUSER_*`: Automatic superuser creation

## Database Configuration

### PostgreSQL (Default)
The project is configured to use PostgreSQL by default. Update the database settings in your `.env` file:

```env
DB_NAME=zakat
DB_USER=postgres
DB_PASSWORD=postgres
DB_HOST=localhost  # or 'db' for Docker
DB_PORT=5432
```

### SQLite (Development Alternative)
For local development, you can use SQLite instead:

```env
USE_SQLITE=true
```

## Available Commands

### Using Makefile
```bash
make install      # Install dependencies
make migrate      # Run database migrations
make superuser    # Create Django superuser
make run          # Start development server
make test         # Run tests
make docker-up    # Start Docker containers
make docker-down  # Stop Docker containers
```

### Using Django Management Commands
```bash
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
python manage.py collectstatic
python manage.py test
```

## Production Deployment

### Environment Setup
1. Set `DJANGO_SETTINGS_MODULE=config.settings.prod`
2. Configure production environment variables
3. Set `DEBUG=False`
4. Configure `ALLOWED_HOSTS`
5. Use strong `SECRET_KEY`

### Static Files
```bash
python manage.py collectstatic --noinput
```

### Security Check
```bash
python manage.py check --deploy
```

### Docker Production
```bash
# Build production image
docker build --target production -t zakat:prod .

# Run with production settings
docker run -e DJANGO_ENV=prod -p 8000:8000 zakat:prod
```

## Security Features

The production settings include:

- Secure cookies (HTTPS only)
- HSTS headers
- Content type nosniff
- XSS protection
- Secure referrer policy
- X-Frame-Options protection
- CSRF protection

## Authentication

- Login URL: `/accounts/login/`
- Logout URL: `/accounts/logout/`
- Home page: `/` (requires authentication)

Default redirects:
- After login: home page
- After logout: login page

## Development

### Adding New Apps
1. Create the app: `python manage.py startapp appname`
2. Add to `INSTALLED_APPS` in `config/settings/base.py`
3. Include URLs in `config/urls.py`

### Database Migrations
```bash
python manage.py makemigrations
python manage.py migrate
```

### Running Tests
```bash
python manage.py test
python manage.py test core  # Test specific app
```

## Troubleshooting

### Common Issues

1. **PostgreSQL connection errors:**
   - Ensure PostgreSQL is running
   - Check database credentials in `.env`
   - For Docker: ensure `db` service is up

2. **Static files not loading:**
   - Run `python manage.py collectstatic`
   - Check `STATIC_URL` and `STATIC_ROOT` settings

3. **Permission denied on entrypoint.sh:**
   ```bash
   chmod +x entrypoint.sh
   ```

4. **Docker build errors:**
   - Check Docker is running
   - Verify Dockerfile syntax
   - Try `docker system prune` to clean up

### Getting Help

- Check Django documentation: https://docs.djangoproject.com/
- Review error logs in the console
- Use Django's built-in error pages in development mode

## License

This project is for educational and development purposes.
