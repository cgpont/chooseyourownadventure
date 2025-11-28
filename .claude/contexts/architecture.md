# System Architecture

## Architecture Components
- **Backend**: Laravel 12 API
- **Database**: PostgreSQL (main) + Redis (cache)
- **AI**: OpenAI API + Claude (switchable)
- **Auth**: Laravel Sanctum
- **Containerization**: Docker (nginx on port 80, PostgreSQL, Redis)
- **Internationalization**: Language and market-specific data storage and caching