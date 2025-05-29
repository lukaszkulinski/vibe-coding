# tech-stack.mdc

## Core Technologies
- PHP 8.4
- Symfony 7.3 (LTS)
- Doctrine ORM
- MySQL (latest stable)
- Composer (dependency management)

## Tooling & Development Environment
- Docker (for local development and production deployment)
- Git & GitHub (version control)
- PHPUnit (unit, functional, and integration testing)
- PHPStan (static analysis)
- PHP-CS-Fixer (code style and formatting)
- Monolog (logging)
- Sentry (error and exception tracking)
- NelmioApiDocBundle (API documentation)

## DevOps / CI/CD
- GitHub Actions (CI/CD pipeline for testing, code quality checks, migrations, and deployment)
- Docker Compose (service orchestration for local development and CI)

## API & Documentation
- RESTful backend only (no frontend in this project)
- API documented via NelmioApiDocBundle

## Error Handling & Monitoring
- Monolog for structured logging
- Sentry for error reporting and production exception monitoring

## Health Monitoring
- Simple custom `/health` endpoint or LiipMonitorBundle (optional, for automated environment health checks and integration with orchestration tools)

## Caching
- No application-level cache (Redis/Memcached) used by default.
- Can be introduced later if performance bottlenecks are identified for specific use-cases (e.g., frequently read static reference data).
- **Security-sensitive data (payment info, card tokens) MUST NOT be cached.**

## Deployment
- Automated deployment via GitHub Actions (build, test, migration, deploy pipeline)
- Docker used for both build and run environments

## Supported Environments
- Linux (production servers, Docker containers)
- Linux/Mac/Windows (developer machines, via Docker)

