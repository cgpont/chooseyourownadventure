# Choose Your Own Adventure API - Core Project Context
## Timeline Constraints

- **Total Development Time**: 2 days (16 hours)
- **Interview**: Day 3
- **Focus**: Code quality and architecture over feature completeness

## Technical Requirements

### Stack
- **PHP**: 8.2
- **Framework**: Laravel 12
- **Database**: MySQL 8.0
- **Cache/Sessions**: Redis 7
- **Web Server**: Nginx 1.25
- **Containerization**: Docker & Docker Compose
- **AI Integration**: OpenAI API (swappable)
- **Testing**: PHPUnit (Feature + Unit, >80% coverage)

### Development Practices
- Modern PHP 8.2 features (readonly properties, enums, typed properties)
- PSR-12 code style
- SOLID principles throughout
- Clean architecture (separation of concerns)
- Comprehensive testing
- API-first design (RESTful)

## Core Features

1. **AI-Powered Story Generation**
   - Generate interactive story nodes using OpenAI API
   - Dynamic choice generation based on context
   - Story progression with branching paths

2. **Adventure Management**
   - Create adventures with themes and difficulty levels
   - List and filter adventures
   - View adventure details with story tree

3. **Game Progress Tracking**
   - Session-based progress (no authentication required)
   - Save current position in story
   - Track choices made
   - Detect story completion

## Development Approach
- Keep it simple (MVP)
- One task at a time
- Test each step before moving forward
- Focus on core functionality first
