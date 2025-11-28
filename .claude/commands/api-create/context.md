# API Creation Context

## API Requirements
- All endpoints MUST support internationalization with `lang` and `market` parameters
- Use Laravel resource controllers and form requests for validation
- Implement proper error handling with localized messages
- Include comprehensive API documentation

## Internationalization Parameters
- `lang` (required): Language code (ES, EN)
- `market` (required): Market code (Mexico, Spain, US, UK)
- Cache keys MUST include language and market for proper data separation

## API Standards
- RESTful design principles
- Consistent response formats
- Proper HTTP status codes
- Input validation using Laravel Form Requests
- Rate limiting and authentication via Laravel Sanctum

## Example Endpoint Structure
```php
Route::post('/api/v1/research', [ProductResearchController::class, 'store'])
    ->middleware('auth:sanctum')
    ->name('research.store');
```

## Related Contexts
- See `../contexts/internationalization.md` for detailed lang/market specifications
- See `../contexts/code-quality.md` for validation and error handling standards