# Code Quality Guidelines

- **Simplicity**: Code must be simple and easy to read
- **Best Practices**: Follow conventions and good practices (SOLID, DRY, YAGNI, etc.)
- **Readability**: Prioritize code clarity over cleverness
- **Consistency**: Follow existing patterns and conventions in the codebase
- **Maintainability**: Code and infrastructure must be maintainable and reproducible
- **No Hardcoded Variables**: NEVER use hardcoded values for variables that could change (URLs, API endpoints, keys, timeouts, limits, etc.). All configurable values MUST be defined in .env file (located in project root) and accessed via config() or env() functions
- **No Magic Strings**: Avoid magic strings and numbers in code. Use named constants, enums, or configuration values instead. String literals used in multiple places or with semantic meaning should be defined as constants (e.g., `private const SECTION_PRODUCT_INFO = 'product_info'`)
- **Test-Driven Development**: When possible, feature and unit tests should be created before finishing any feature or important modification to ensure code reliability and maintainability

## Related Contexts
- See `testing.md` for detailed testing guidelines and database setup