# System Architecture

## Architecture Components
- **Backend**: Laravel 12 API
- **Database**: MySQL (main) + Redis (cache)
- **AI**: OpenAI API + Claude (switchable)
- **Auth**: Laravel Sanctum
- **Containerization**: Docker (nginx on port 80, MySQL, Redis)
- **Internationalization**: Language and market-specific data storage and caching