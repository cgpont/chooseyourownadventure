# Testing Guidelines

- **Use dedicated test database** with RefreshDatabase trait for all tests involving database interactions
- **All tests run against PostgreSQL** for realistic behavior (not SQLite)
- **Tests automatically migrate** and seed fresh database before each test
- **Test database is completely isolated** from development data
- **Run tests with**: `php artisan test` or `vendor/bin/phpunit`
- **Feature tests** should use RefreshDatabase trait when testing database functionality
- **Test database name**: `product_research_test` (configured in phpunit.xml)
- **Docker integration**: Tests work seamlessly with containerized PostgreSQL setup
- **Clean state guarantee**: Each test starts with a fresh, migrated database

## Related Contexts
- See `internationalization.md` for multi-language testing considerations