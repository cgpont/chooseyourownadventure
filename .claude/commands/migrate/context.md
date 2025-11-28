# Database Migration Context

## Migration Commands
- `php artisan make:migration` - Create new migration
- `php artisan migrate` - Run pending migrations
- `php artisan migrate:rollback` - Rollback last batch
- `php artisan migrate:status` - Show migration status

## Internationalization Requirements
- All product-related tables MUST include `lang` and `market` columns
- Create indexes on language and market for optimal performance
- Examples:
  ```sql
  $table->string('lang', 2); // ES, EN
  $table->string('market', 10); // Mexico, Spain, US, UK
  $table->index(['lang', 'market']);
  ```

## Database Design Principles
- Single tables with lang/market columns (not separate tables per language)
- Indexed by language and market for efficient querying
- Support for PostgreSQL full-text search capabilities

## Related Contexts
- See `../contexts/internationalization.md` for complete database design specifications
- See `../contexts/architecture.md` for PostgreSQL and caching architecture details