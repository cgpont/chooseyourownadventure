# Testing Context

## Test Commands
- `php artisan test` - Run all tests
- `vendor/bin/phpunit` - Alternative test runner
- `php artisan test --testsuite=Unit` - Unit tests only
- `php artisan test --testsuite=Feature` - Feature tests only

## Database Testing
- Uses dedicated `product_research_test` database
- RefreshDatabase trait ensures clean state between tests
- Tests run against PostgreSQL (not SQLite) for realistic behavior
- Automatic migrations and seeding before each test

## Test Requirements
- All database tests MUST use RefreshDatabase trait
- Tests are completely isolated from development data
- Each test starts with fresh, migrated database
- Docker integration works seamlessly with containerized setup