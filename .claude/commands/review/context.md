# Code Review Context

## Review Focus Areas
- Security vulnerabilities and input validation
- Performance implications (database queries, caching)
- Laravel best practices and conventions
- Internationalization support (lang/market parameters)
- Test coverage and quality

## Quality Standards
- Code simplicity and readability
- No hardcoded values (use .env and config())
- Proper error handling and logging
- SOLID principles and maintainable architecture
- Container management compliance (no direct container changes)

## Project-Specific Checks
- Proper lang/market parameter handling
- Redis caching with language/market-specific keys
- PostgreSQL query optimization
- RefreshDatabase trait in database tests
- Conventional commit message format