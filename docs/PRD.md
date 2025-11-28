# Product Requirements Document (PRD)
## Choose Your Own Adventure API

## Overview

A RESTful API for creating and playing AI-powered Choose Your Own Adventure games. The system generates dynamic story content using AI (OpenAI), allowing players to make choices that affect the narrative direction.

## User Stories

### As a Player
- I want to start a new adventure with a specific theme and difficulty level
- I want to read the current story segment
- I want to see available choices at each decision point
- I want to make a choice and see the story continue based on my decision
- I want my game progress to be saved automatically
- I want to reach different endings based on my choices
- I want to restart or play multiple adventures simultaneously

### As a Developer (Future)
- I want to easily swap AI providers (OpenAI, Anthropic, local models)
- I want comprehensive API documentation
- I want reliable test coverage for confident refactoring
- I want to extend the system with new features easily

## Core Functionality

### Adventure Creation
- Create new adventures with:
  - Title and description
  - Theme (fantasy, sci-fi, mystery, horror, etc.)
  - Difficulty level (easy, medium, hard)
- AI automatically generates the starting story node with initial choices
- Adventures have a status (draft, active, completed)

### Story Generation
- AI generates story content based on:
  - Adventure theme and difficulty
  - Previous story context (last 3 nodes)
  - Choice made by player
- Each story node includes:
  - Narrative text (2-3 paragraphs)
  - 2-4 choices for the player
  - Type indicator (start, decision, ending)

### Game Play
- Players can:
  - Browse available adventures
  - Start a new game session
  - Read current story node
  - Select a choice to progress
  - Reach an ending node
- Progress is tracked per session (no login required)

## API Endpoints

### Adventures

#### List Adventures
```
GET /api/v1/adventures
Query Parameters:
  - difficulty: easy|medium|hard (optional)
  - per_page: integer (optional, default: 15)

Response: 200 OK
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "title": "The Lost Temple",
      "description": "An archaeological mystery in the jungle",
      "theme": "mystery",
      "difficulty": "medium",
      "status": "active",
      "total_nodes": 12,
      "created_at": "2024-01-15T10:00:00Z",
      "links": {
        "self": "/api/v1/adventures/550e8400...",
        "start": "/api/v1/adventures/550e8400.../start"
      }
    }
  ],
  "meta": {
    "current_page": 1,
    "per_page": 15,
    "total": 25
  }
}
```

#### Get Adventure Details
```
GET /api/v1/adventures/{uuid}

Response: 200 OK
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "title": "The Lost Temple",
    "description": "An archaeological mystery in the jungle",
    "theme": "mystery",
    "difficulty": "medium",
    "status": "active",
    "total_nodes": 12,
    "start_node": {
      "uuid": "660e8400...",
      "content": "You stand at the entrance of an ancient temple...",
      "type": "start",
      "choices": [
        {
          "uuid": "770e8400...",
          "text": "Enter the temple carefully",
          "order": 0
        },
        {
          "uuid": "880e8400...",
          "text": "Search the perimeter first",
          "order": 1
        }
      ]
    },
    "created_at": "2024-01-15T10:00:00Z"
  }
}

Response: 404 Not Found
{
  "message": "Adventure not found"
}
```

#### Create Adventure
```
POST /api/v1/adventures
Content-Type: application/json

Body:
{
  "title": "The Lost Temple",
  "description": "An archaeological mystery in the jungle",
  "theme": "mystery",
  "difficulty": "medium"
}

Response: 201 Created
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "title": "The Lost Temple",
    "description": "An archaeological mystery in the jungle",
    "theme": "mystery",
    "difficulty": "medium",
    "status": "active",
    "start_node": {
      "uuid": "660e8400...",
      "content": "You stand at the entrance...",
      "type": "start",
      "choices": [...]
    },
    "created_at": "2024-01-15T10:00:00Z"
  }
}

Response: 422 Unprocessable Entity
{
  "message": "The given data was invalid.",
  "errors": {
    "title": ["The title field is required."],
    "difficulty": ["The selected difficulty is invalid."]
  }
}

Response: 500 Internal Server Error
{
  "message": "Failed to generate adventure",
  "error": "AI provider connection failed"
}
```

### Game Progress

#### Start Adventure
```
POST /api/v1/adventures/{uuid}/start

Response: 201 Created
{
  "data": {
    "session_id": "def50200...",
    "adventure": {
      "uuid": "550e8400...",
      "title": "The Lost Temple"
    },
    "current_node": {
      "uuid": "660e8400...",
      "content": "You stand at the entrance...",
      "type": "start",
      "is_ending": false,
      "choices": [
        {
          "uuid": "770e8400...",
          "text": "Enter the temple carefully",
          "order": 0
        }
      ]
    },
    "is_completed": false,
    "started_at": "2024-01-15T11:30:00Z"
  },
  "message": "Adventure started successfully"
}
```

#### Make Choice
```
POST /api/v1/adventures/{uuid}/choice
Content-Type: application/json

Body:
{
  "choice_uuid": "770e8400-e29b-41d4-a716-446655440001"
}

Response: 200 OK
{
  "data": {
    "current_node": {
      "uuid": "990e8400...",
      "content": "You cautiously step inside the temple...",
      "type": "decision",
      "is_ending": false,
      "choices": [...]
    },
    "is_completed": false
  }
}

Response: 400 Bad Request
{
  "message": "Invalid choice for current node"
}

Response: 404 Not Found
{
  "message": "No active session found"
}
```

#### Get Current Progress
```
GET /api/v1/adventures/{uuid}/progress

Response: 200 OK
{
  "data": {
    "session_id": "def50200...",
    "adventure": {
      "uuid": "550e8400...",
      "title": "The Lost Temple"
    },
    "current_node": {
      "uuid": "990e8400...",
      "content": "You cautiously step inside...",
      "type": "decision",
      "is_ending": false,
      "choices": [...]
    },
    "is_completed": false,
    "started_at": "2024-01-15T11:30:00Z"
  }
}
```

### Health Check
```
GET /api/v1/health

Response: 200 OK
{
  "status": "healthy",
  "timestamp": "2024-01-15T12:00:00Z"
}
```

## Implementation Tasks

These numbered tasks can be used to create GitHub issues for tracking project progress.

### Phase 1: Foundation (Tasks 1-4)

**1. Setup Docker Environment**
- Create `docker-compose.yml` with services: PHP 8.2-FPM, Nginx, MySQL 8, Redis
- Create `docker/php/Dockerfile` with PHP extensions (pdo_mysql, redis, zip)
- Create `docker/nginx/default.conf` for Laravel routing
- Create `docker/mysql/my.cnf` for MySQL configuration
- Configure volume mounts for hot-reload
- Test: `docker-compose up -d` runs all services without errors

**2. Install Laravel 12 and Dependencies**
- Run `composer create-project laravel/laravel:^12.0 .`
- Install dev dependencies: `laravel/pint`, `phpunit/phpunit`
- Configure `.env.example` with all required variables
- Test: `php artisan --version` shows Laravel 12.x

**3. Create Project Directory Structure**
- Create `app/Contracts/` for interfaces
- Create `app/DataTransferObjects/` with subdirectories (AI, Adventure, StoryNode)
- Create `app/Enums/` for PHP enums
- Create `app/Exceptions/` for custom exceptions
- Create `app/Repositories/` for repository implementations
- Create `app/Services/` with `AI/` subdirectory
- Create `app/Traits/` for reusable traits
- Create `config/ai.php` and `config/adventure.php`

**4. Configure Environment Files**
- Update `.env.example` with Docker service names
- Add AI provider configuration variables
- Add database connection settings
- Add Redis configuration
- Document all environment variables in README

### Phase 2: Database & Models (Tasks 5-8)

**5. Create Database Migrations**
- Create `create_adventures_table` migration (uuid PK, enums, indexes)
- Create `create_story_nodes_table` migration (self-referencing FK, JSON metadata)
- Create `create_choices_table` migration (links to next nodes)
- Create `create_user_progress_table` migration (session tracking)
- Test: All migrations run without errors

**6. Create Eloquent Models with Relationships**
- Create `Adventure` model with UUID trait, relationships, scopes, accessors
- Create `StoryNode` model with self-referencing relationships
- Create `Choice` model with node relationships
- Create `UserProgress` model with session tracking
- Create `HasUuid` trait for UUID primary keys
- Test: All relationships load correctly

**7. Create Factories and Seeders**
- Create `AdventureFactory` with states (active, draft, withStartNode)
- Create `StoryNodeFactory` with node types
- Create `ChoiceFactory`
- Create `UserProgressFactory`
- Create `SampleAdventureSeeder` for testing
- Test: Factories generate valid data

**8. Create PHP Enums**
- Create `AdventureStatus` enum (draft, active, completed)
- Create `NodeType` enum (start, decision, ending)
- Create `DifficultyLevel` enum (easy, medium, hard)
- Test: Enums cast correctly in models

### Phase 3: Repository Pattern (Tasks 9-13)

**9. Define Repository Interfaces**
- Create `AdventureRepositoryInterface` with CRUD methods
- Create `StoryNodeRepositoryInterface` with query methods
- Create `GameProgressRepositoryInterface` with session methods
- Document interface methods with PHPDoc

**10. Implement Adventure Repository**
- Create `AdventureRepository` implementing interface
- Implement `findByUuid()`, `getActive()`, `create()`, `update()`, `delete()`
- Implement `getWithNodes()` with eager loading
- Use pagination for list methods
- Throw `AdventureNotFoundException` when not found

**11. Implement StoryNode Repository**
- Create `StoryNodeRepository` implementing interface
- Implement `create()`, `findWithChoices()`, `getByAdventure()`
- Use eager loading to prevent N+1 queries
- Handle parent-child relationships

**12. Implement GameProgress Repository**
- Create `GameProgressRepository` implementing interface
- Implement `create()`, `findActiveSession()`, `updateProgress()`
- Handle session isolation
- Query by session_id and adventure_uuid

**13. Create Repository Service Provider**
- Create `RepositoryServiceProvider`
- Bind all repository interfaces to implementations
- Register provider in `config/app.php`
- Test: Interfaces resolve to implementations

### Phase 4: AI Integration (Strategy Pattern) (Tasks 14-19)

**14. Create AI Provider Interface**
- Create `AIProviderInterface` with methods:
  - `generateStoryNode(StoryGenerationRequest): StoryGenerationResponse`
  - `generateChoices(string $content, int $count): array`
  - `testConnection(): bool`

**15. Create AI DTOs**
- Create `StoryGenerationRequest` (readonly DTO with toPrompt() method)
- Create `StoryGenerationResponse` (readonly DTO)
- Add factory methods (fromArray)
- Test: DTOs are immutable and type-safe

**16. Implement OpenAI Provider**
- Create `OpenAIProvider` implementing interface
- Use Laravel HTTP client for API calls
- Implement chat completions endpoint
- Parse AI responses into DTOs
- Handle errors with `AIProviderException`
- Create system prompt for story generation
- Test: Can generate story content

**17. Implement Mock AI Provider**
- Create `MockAIProvider` implementing interface
- Return predictable responses for testing
- No external API calls
- Test: Returns valid responses

**18. Create AI Service Provider**
- Create `AIServiceProvider`
- Bind interface based on `config('ai.default_provider')`
- Support: 'openai', 'mock'
- Use match expression for provider selection

**19. Create AI Configuration File**
- Create `config/ai.php`
- Add `default_provider` setting
- Add OpenAI configuration (api_key, model, max_tokens)
- Document configuration options

### Phase 5: Service Layer (Tasks 20-24)

**20. Implement Adventure Service**
- Create `AdventureService` with constructor injection
- Implement `createAdventure()` with DB transactions
- Implement `listAdventures()` with filtering
- Implement `getAdventureDetails()` with eager loading
- Orchestrate repository + AI provider
- Test: Service methods work correctly

**21. Implement Story Generation Service**
- Create `StoryGenerationService`
- Implement `generateNextNode()` to create nodes via AI
- Implement `buildStoryContext()` to walk parent nodes
- Implement `determineNodeType()` logic for endings
- Link choices to newly generated nodes
- Use DB transactions

**22. Implement Game Progress Service**
- Create `GameProgressService`
- Implement `startAdventure()` to initialize progress
- Implement `makeChoice()` to progress story (triggers AI)
- Implement `getProgress()` to fetch session state
- Validate choices belong to current node
- Handle session isolation

**23. Create Custom Exceptions**
- Create `AIProviderException`
- Create `AdventureNotFoundException`
- Create `InvalidChoiceException`
- Create `InvalidProgressException`
- Extend base Laravel exceptions

**24. Create DTOs for Services**
- Create `AdventureData` DTO
- Create `CreateAdventureData` DTO
- Create `StoryNodeData` DTO
- Create `GameProgressData` DTO
- Add fromModel() factory methods

### Phase 6: API Layer (Tasks 25-30)

**25. Create Form Requests**
- Create `CreateAdventureRequest` with validation rules
- Create `MakeChoiceRequest` with validation
- Add authorization logic (return true for now)
- Add custom error messages
- Add `toDTO()` methods

**26. Create API Resources**
- Create `AdventureResource` with transformation logic
- Create `AdventureCollection` for paginated results
- Create `StoryNodeResource` with choices
- Create `ChoiceResource`
- Create `GameProgressResource`
- Add HATEOAS links
- Use `whenLoaded()` for conditional fields

**27. Implement Adventure Controller**
- Create `AdventureController` in `app/Http/Controllers/Api/`
- Implement `index()` for listing adventures
- Implement `store()` for creating adventures
- Implement `show()` for adventure details
- Inject `AdventureService` via constructor
- Return proper HTTP status codes
- Handle exceptions

**28. Implement Game Progress Controller**
- Create `GameProgressController` in `app/Http/Controllers/Api/`
- Implement `start()` to begin adventure
- Implement `makeChoice()` to progress story
- Implement `show()` to get current progress
- Extract session_id from request
- Inject `GameProgressService` via constructor

**29. Define API Routes**
- Add routes to `routes/api.php`
- Prefix all routes with `/api/v1`
- Use resource routes for adventures (index, show, store)
- Add nested routes for game progress
- Add health check route
- Name all routes

**30. Add Error Handling**
- Catch domain exceptions in controllers
- Return JSON error responses
- Use appropriate HTTP status codes
- Log AI provider failures
- Return validation errors in consistent format

### Phase 7: Testing (Tasks 31-37)

**31. Configure Test Environment**
- Update `phpunit.xml` to use SQLite in-memory
- Add `RefreshDatabase` trait to TestCase
- Configure test environment variables
- Set AI provider to 'mock' in tests

**32. Write Adventure API Feature Tests**
- Test adventure creation with AI integration
- Test listing adventures with filters
- Test getting adventure details
- Test validation errors (422)
- Test 404 for missing adventures
- Assert database state changes
- Assert API response structure

**33. Write Game Progress API Feature Tests**
- Test starting adventure
- Test making choices (story progression)
- Test invalid choices (400)
- Test completing adventure (ending node)
- Test session isolation
- Test missing session (404)

**34. Write Service Unit Tests**
- Test `AdventureService` with mocked repositories and AI
- Test `StoryGenerationService` context building
- Test `GameProgressService` validation logic
- Use Mockery for mocking
- Test transaction rollback on errors
- Test exception handling

**35. Write Repository Unit Tests**
- Test `AdventureRepository` CRUD operations
- Test eager loading in repositories
- Test pagination
- Test exception throwing
- Use RefreshDatabase trait

**36. Write Model Tests**
- Test model relationships (adventure->storyNodes, etc.)
- Test query scopes (active(), ofType())
- Test accessor methods (isEnding())
- Test enum casting
- Test UUID generation

**37. Run Coverage Report**
- Run `php artisan test --coverage`
- Ensure >80% overall coverage
- Ensure >90% service coverage
- Ensure >85% repository coverage
- Fix any uncovered critical paths

### Phase 8: Documentation (Tasks 38-42)

**38. Update CLAUDE.md**
- Document all architecture decisions
- Add learning objectives mapped to code
- Add SOLID principles examples with file references
- Add interview discussion points
- Document out of scope features

**39. Create Comprehensive README.md**
- Add quick start guide with Docker commands
- Document prerequisites
- Add setup instructions step-by-step
- Document how to run tests
- Add architecture overview
- Link to API documentation
- Add troubleshooting section

**40. Add Inline Code Documentation**
- Add PHPDoc blocks to all public methods
- Document complex business logic
- Add architecture pattern annotations as comments
- Document learning objectives in comments

**41. Create API Documentation**
- Create Postman collection with all endpoints OR
- Create OpenAPI/Swagger spec
- Include example requests and responses
- Document error response formats
- Add authentication notes (none required)

**42. Update PRD.md**
- Finalize all user stories
- Document all API endpoints with examples
- Add database schema ERD
- List all 47 implementation tasks
- Add non-functional requirements

### Phase 9: DevEx & Polish (Tasks 43-47)

**43. Configure Laravel Pint**
- Create `pint.json` configuration
- Use PSR-12 preset
- Run `./vendor/bin/pint` to format all code
- Add Pint to development workflow

**44. Create .editorconfig**
- Add .editorconfig for consistent formatting
- Set indentation, line endings, charset
- Configure for PHP, Blade, JSON, YAML

**45. Create .gitignore**
- Add Laravel defaults
- Add Docker-specific ignores
- Add IDE-specific ignores (.idea, .vscode)
- Add environment files (.env)

**46. Create Sample Adventure Seeder**
- Create pre-populated adventure with full story tree
- Include multiple branches and endings
- Demonstrate all node types
- Make it playable for testing

**47. Manual End-to-End Verification**
- Test with Postman or curl
- Create adventure → Start game → Make choices → Reach ending
- Test error scenarios
- Test with real OpenAI API
- Test with MockAIProvider
- Verify all endpoints work

## Non-Functional Requirements

### Performance
- API response time < 2 seconds for AI generation
- Database queries optimized (no N+1 queries)
- Proper indexing on foreign keys and query columns
- Pagination for list endpoints

### Code Quality
- PSR-12 compliant code style
- >80% test coverage
- No linting errors
- Type hints on all methods
- Meaningful variable and method names

### Security
- Input validation on all endpoints
- SQL injection prevention (use Eloquent)
- XSS prevention (API only, no HTML output)
- Rate limiting considerations (not implemented, but documented)
- Environment variables for secrets

### Reliability
- Database transactions for atomicity
- Proper error handling and logging
- Graceful AI provider failure handling
- Database migrations are reversible
- No data loss on errors

### Maintainability
- SOLID principles followed
- Design patterns clearly implemented
- Comprehensive documentation
- Consistent naming conventions
- Separation of concerns

### Scalability Considerations (For Discussion)
- UUID primary keys (distributed-friendly)
- Stateless API (session in Redis)
- Repository pattern (can swap data sources)
- Strategy pattern (can add AI providers)
- Dockerized (easy to scale horizontally)

## Future Enhancements (Out of Scope)

### Phase 10+ (Post-Interview)
- User authentication with Laravel Sanctum
- Adventure ownership and permissions
- Public adventure library with ratings
- Story branching visualization (graph UI)
- Analytics dashboard (popular paths, choices)
- Multiple AI providers (Anthropic Claude, local models)
- Caching layer for frequently accessed adventures
- WebSocket support for real-time multiplayer
- Adventure templates and themes
- Export adventures to PDF/ebook format
- Mobile app (React Native)
- Social features (sharing, comments)
- Achievement system
- Story editor UI for manual creation
- A/B testing different story branches
- Rate limiting and API throttling
- API documentation UI (Swagger/Redoc)
- CI/CD pipeline with GitHub Actions
- Monitoring and observability (logs, metrics)
- Multi-language support (i18n)

## Success Metrics

### Technical Metrics
- All 47 tasks completed
- >80% test coverage achieved
- All tests passing
- Docker environment works on fresh machine
- API fully functional end-to-end
- Zero critical bugs

### Learning Metrics
- Can explain each SOLID principle with code example
- Can discuss architecture decisions and trade-offs
- Can demonstrate all design patterns implemented
- Can walk through test strategy
- Can explain Laravel concepts used
- Interview-ready confidence level

## Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| OpenAI API issues | Medium | High | Use MockAIProvider, implement retry logic |
| Time overruns | Medium | High | Prioritize core features, defer polish tasks |
| Docker problems | Low | High | Provide troubleshooting docs, test on multiple OS |
| Database design issues | Low | Medium | Review schema early, use migrations for changes |
| Test failures | Medium | Medium | Use in-memory SQLite, mock external dependencies |
| Scope creep | High | High | Strictly enforce "out of scope" list |

## Appendix

### Technology Choices Rationale

**Laravel 12**: Latest version, showcases modern PHP knowledge, excellent for APIs

**MySQL 8**: Industry standard, demonstrates SQL knowledge, supports JSON columns

**Docker**: Environment consistency, easy setup, demonstrates DevOps knowledge

**OpenAI**: Industry-leading AI, easy to integrate, well-documented API

**PHPUnit**: Laravel default, comprehensive testing features, industry standard

**Repository Pattern**: Demonstrates clean architecture, testability, SOLID principles

**Strategy Pattern**: Shows understanding of behavioral patterns, extensibility

### Glossary

- **DTO**: Data Transfer Object - Immutable object for transferring data between layers
- **Strategy Pattern**: Behavioral design pattern for swappable algorithms
- **Repository Pattern**: Structural pattern for abstracting data access
- **SOLID**: Five principles of object-oriented design (SRP, OCP, LSP, ISP, DIP)
- **N+1 Query**: Performance problem where queries increase linearly with data
- **Eager Loading**: Loading related data upfront to prevent N+1 queries
- **UUID**: Universally Unique Identifier for distributed systems
- **Mock**: Test double that returns pre-defined responses
- **Feature Test**: Test that covers full application flow
- **Unit Test**: Test that covers single class in isolation
