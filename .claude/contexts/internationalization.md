# Internationalization Support

## Supported Markets
The API supports multiple language and market combinations:
- **Spanish Markets**: `lang=ES&market=Mexico`, `lang=ES&market=Spain`
- **English Markets**: `lang=EN&market=US`, `lang=EN&market=UK`

## Database Design
- Single tables with `lang` and `market` columns for efficient querying
- Indexed by language and market for optimal performance
- Cache keys include language and market for proper data separation
- Examples:
  ```
  products: id, name, description, price, currency, lang, market
  reviews: id, product_id, rating, review_text, lang, market
  competitors: id, product_id, competitor_name, price, lang, market
  ```

## API Parameters
The API will support versioning.
All endpoints accept:
- `lang` (required): Language code (ES, EN)
- `market` (required): Market/country code (Mexico, Spain, US, UK)

Example: `/api/research?product=smartphone&lang=ES&market=Mexico`

## Related Contexts
- See `architecture.md` for Redis caching and DB architecture details