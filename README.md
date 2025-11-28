# Choose Your Own Adventure API - Project Context

## Purpose

This is a learning-focused project designed to demonstrate **Tech Lead-level Laravel 12 architecture and patterns** for a technical interview. The goal is to build a production-quality API while reinforcing modern PHP and Laravel development practices.

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

## Architecture Decisions

### 1. Repository Pattern
**Why**: Abstracts data access layer, enables testability, demonstrates SOLID principles (Dependency Inversion)

**Where**: All Eloquent model access goes through repository interfaces

**Implementation**:
- `app/Contracts/*RepositoryInterface.php` - Interfaces
- `app/Repositories/*Repository.php` - Concrete implementations
- Bound in `RepositoryServiceProvider`

**Example**:
```php
// Controller depends on interface, not concrete implementation
public function __construct(
    private AdventureRepositoryInterface $adventureRepo
) {}
```

**Interview Points**:
- Makes testing easier (mock repositories)
- Can swap data sources (MySQL → MongoDB, API, etc.)
- Single Responsibility (repository only handles data access)
- Dependency Inversion (depend on abstractions)

### 2. Service Layer
**Why**: Separates business logic from controllers, orchestrates multiple repositories, handles transactions

**Where**: Complex operations involving multiple models, external APIs, or business rules

**Implementation**:
- `app/Services/*Service.php`
- Injected into controllers via dependency injection

**Example**:
```php
// AdventureService orchestrates: Repository + AI Provider + Transactions
public function createAdventure(CreateAdventureData $data): AdventureData
{
    return DB::transaction(function () use ($data) {
        // 1. Create adventure via repository
        // 2. Generate story via AI provider
        // 3. Create story nodes
        // 4. Return DTO
    });
}
```

**Interview Points**:
- Keeps controllers thin (Single Responsibility)
- Reusable business logic
- Transaction boundaries clearly defined
- Testable in isolation with mocks

### 3. Strategy Pattern (AI Providers)
**Why**: Enables swappable AI implementations, demonstrates Open/Closed principle

**Where**: AI integration layer

**Implementation**:
- `app/Contracts/AIProviderInterface.php` - Strategy interface
- `app/Services/AI/OpenAIProvider.php` - OpenAI implementation
- `app/Services/AI/MockAIProvider.php` - Testing implementation
- `AIServiceProvider` binds interface based on configuration

**Example**:
```php
// Easy to add new providers (Anthropic, local models, etc.)
// No code changes required, just new implementation + config

// config/ai.php
'default_provider' => env('AI_PROVIDER', 'openai'),

// AIServiceProvider
$this->app->singleton(AIProviderInterface::class, function ($app) {
    return match(config('ai.default_provider')) {
        'openai' => new OpenAIProvider(...),
        'anthropic' => new AnthropicProvider(...),
        'mock' => new MockAIProvider(),
    };
});
```

**Interview Points**:
- Open/Closed Principle (open for extension, closed for modification)
- Liskov Substitution (any provider can replace another)
- Easy testing (switch to MockAIProvider)
- Future-proof (add providers without changing existing code)

### 4. Data Transfer Objects (DTOs)
**Why**: Type-safe data containers, immutable, clear contracts between layers

**Where**: Data passing between layers (Controller → Service, Service → Repository, AI requests/responses)

**Implementation**:
- `app/DataTransferObjects/*` using PHP 8.2 readonly properties
- Factory methods (fromArray, fromModel)
- Domain-specific transformations

**Example**:
```php
final readonly class CreateAdventureData
{
    public function __construct(
        public string $title,
        public string $description,
        public string $theme,
        public DifficultyLevel $difficulty,
    ) {}

    public static function fromArray(array $data): self
    {
        return new self(
            title: $data['title'],
            description: $data['description'],
            theme: $data['theme'],
            difficulty: DifficultyLevel::from($data['difficulty']),
        );
    }
}
```

**Interview Points**:
- Type safety (compiler catches errors)
- Immutability (readonly prevents accidental changes)
- Self-documenting (clear what data is required)
- No over-fetching (only necessary data)

### 5. API Resources & Form Requests
**Why**: Separates validation and transformation concerns from controllers

**Where**: All API endpoints

**Implementation**:
- `app/Http/Requests/*Request.php` - Validation logic
- `app/Http/Resources/*Resource.php` - Response transformation

**Example**:
```php
// Form Request handles validation
class CreateAdventureRequest extends FormRequest
{
    public function rules(): array
    {
        return [
            'title' => ['required', 'string', 'max:255'],
            'difficulty' => ['required', Rule::enum(DifficultyLevel::class)],
        ];
    }

    public function toDTO(): CreateAdventureData
    {
        return new CreateAdventureData(...$this->validated());
    }
}

// API Resource handles transformation
class AdventureResource extends JsonResource
{
    public function toArray(Request $request): array
    {
        return [
            'uuid' => $this->uuid,
            'title' => $this->title,
            'start_node' => new StoryNodeResource(
                $this->whenLoaded('startNode')
            ),
            'links' => [
                'self' => route('adventures.show', $this->uuid),
            ],
        ];
    }
}
```

**Interview Points**:
- Single Responsibility (each class has one job)
- Reusable validation rules
- Consistent API responses
- Hides internal model structure
- Conditional fields (whenLoaded prevents N+1)

## Database Design

### Schema Overview

```
adventures (UUID PK)
  ├─ story_nodes (UUID PK, self-referencing)
  │   └─ choices (UUID PK, links to next node)
  └─ user_progress (session tracking)
```

### Key Design Decisions

**UUID Primary Keys**:
- **Why**: Distributed system friendly, prevents enumeration attacks, no ID conflicts
- **Trade-off**: Slightly larger storage, no sequential ordering
- **Implementation**: HasUuid trait, uuid column as primary key

**Self-Referencing Story Nodes**:
- **Why**: Enables tree structure for story branching
- **Implementation**: `parent_uuid` foreign key to same table
- **Query**: Use recursive CTEs or eager loading to traverse tree

**JSON Metadata Column**:
- **Why**: Flexible storage for AI context, flags, custom data
- **Trade-off**: Can't index JSON fields easily
- **Use Case**: Store AI generation parameters, story flags

**Session-Based Progress**:
- **Why**: No authentication required, simpler for demo
- **Implementation**: Laravel session ID as tracking key
- **Future**: Easy to add user_id foreign key when auth is added

## SOLID Principles Examples

### Single Responsibility Principle (SRP)
- **AdventureRepository**: Only handles adventure data access
- **AdventureService**: Only handles adventure business logic
- **AdventureController**: Only handles HTTP request/response
- **CreateAdventureRequest**: Only handles validation

### Open/Closed Principle (OCP)
- **AI Providers**: Add new providers without modifying existing code
- **Example**: Add AnthropicProvider by implementing AIProviderInterface

### Liskov Substitution Principle (LSP)
- **AI Providers**: Any AIProviderInterface implementation is interchangeable
- **Repositories**: Any repository implementation can replace another

### Interface Segregation Principle (ISP)
- **Specific Interfaces**: AdventureRepositoryInterface only has adventure-related methods
- **No Fat Interfaces**: Each repository interface is focused

### Dependency Inversion Principle (DIP)
- **Controllers**: Depend on repository interfaces, not concrete classes
- **Services**: Depend on AIProviderInterface, not specific providers
- **Binding**: Service providers bind interfaces to implementations

## Learning Objectives

### Laravel Concepts Demonstrated

1. **Advanced Eloquent**
   - Self-referencing relationships (story tree)
   - JSON casting for metadata
   - Query scopes for reusability
   - Eager loading to prevent N+1
   - UUID primary keys
   - Enum casting (PHP 8.1+)

2. **API Design**
   - RESTful endpoints
   - API Resources for transformation
   - Form Requests for validation
   - Proper HTTP status codes
   - HATEOAS links
   - API versioning (/api/v1)

3. **Testing**
   - Feature tests (end-to-end API testing)
   - Unit tests (isolated service testing)
   - Test doubles (mocking external APIs)
   - Database transactions (RefreshDatabase)
   - Factory patterns and states
   - >80% code coverage

4. **Architecture**
   - Repository Pattern
   - Service Layer
   - Strategy Pattern
   - Dependency Injection
   - Service Providers for bindings
   - Custom configuration files

5. **Modern PHP**
   - PHP 8.2 readonly properties
   - Enums with backed values
   - Constructor property promotion
   - Match expressions
   - Named arguments
   - Typed properties

## Interview Discussion Points

### Architecture Questions

**Q: Why use Repository Pattern instead of using Eloquent directly in controllers?**
A:
- Abstraction: Controllers don't need to know about data source
- Testability: Easy to mock repositories in tests
- Flexibility: Can swap MySQL for MongoDB without changing controllers
- Query Reusability: Common queries defined once in repository
- Single Responsibility: Data access separated from HTTP handling

**Q: How does the Strategy Pattern make AI providers swappable?**
A:
- All providers implement AIProviderInterface
- Service Provider binds interface based on config
- Services depend on interface, not concrete class
- Can switch providers by changing environment variable
- Easy to test with MockAIProvider
- Future providers (Anthropic, local models) just implement interface

**Q: What are the trade-offs of using DTOs vs arrays?**
A:
- **DTOs**: Type safety, IDE autocomplete, immutability, but more boilerplate
- **Arrays**: Flexible, less code, but no type checking, typos not caught
- **Decision**: Use DTOs for external boundaries (API, AI), arrays internally where appropriate

**Q: When should you use database transactions?**
A:
- Multi-model operations that must succeed/fail together
- Creating adventure + story node + choices atomically
- Rollback on AI provider failure
- Prevent partial data in case of errors

**Q: How do you prevent N+1 query problems?**
A:
- Eager loading with `with()` in repositories
- `whenLoaded()` in API resources prevents loading if not needed
- Use `withCount()` for counts instead of loading collections
- Query scopes for common eager loads

### Testing Questions

**Q: What's the difference between Feature and Unit tests?**
A:
- **Feature**: Test entire flow (HTTP → Controller → Service → DB), use real database
- **Unit**: Test single class in isolation, mock all dependencies
- **Example**: Feature test hits API endpoint, Unit test mocks repository in service

**Q: How do you test external API integrations?**
A:
- Create interface (AIProviderInterface)
- Implement MockAIProvider for tests
- Bind mock in test environment
- Test real provider separately (integration test)
- Use HTTP fake for Laravel HTTP client

## Out of Scope

These features are intentionally excluded to maintain focus and meet time constraints:

- **Authentication/Authorization**: No user accounts, token auth, or permissions
- **Frontend**: API-only, no React/Vue UI
- **Caching**: Redis available but not implementing cache layer
- **Real-time**: No WebSockets or Server-Sent Events
- **Admin Panel**: No admin interface for managing adventures
- **Rate Limiting**: No API throttling or rate limits
- **File Uploads**: No image uploads for adventures
- **Story Visualization**: No graph/tree visualization of story branches
- **Social Features**: No sharing, comments, ratings
- **Analytics**: No tracking of popular choices or paths
- **Multi-language**: English only

## Success Criteria

### For Development
- ✅ Docker environment runs without errors
- ✅ All migrations run successfully
- ✅ Test suite passes with >80% coverage
- ✅ Can create adventure via API
- ✅ Can play through full adventure (start → choices → ending)
- ✅ AI provider is swappable via configuration

### For Interview
- ✅ Can explain architecture decisions clearly
- ✅ Can demonstrate SOLID principles in code
- ✅ Can discuss trade-offs of design patterns
- ✅ Can walk through test strategy
- ✅ Can explain Laravel concepts with examples
- ✅ Code is clean, documented, and production-ready
