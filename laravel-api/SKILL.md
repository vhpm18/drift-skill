---
name: laravel-api
description: Build production-grade Laravel REST APIs using opinionated architecture patterns. Stateless design, versioned endpoints, invokable controllers, Form Request DTOs, Action classes, JWT authentication, and PSR-12 code quality. Use when building, scaffolding, or reviewing Laravel APIs.
license: MIT
compatibility: PHP 8.2+, Laravel 11+
metadata:
  category: api
  time: 6h
  source: laravel-api-skill
  triggers:
    - "build a Laravel API"
    - "create Laravel endpoints"
    - "add API authentication"
    - "review Laravel API code"
    - "refactor Laravel API"
    - "improve Laravel code quality"
---

# Laravel API – Steve's Architecture

Build Laravel REST APIs with clean, stateless, resource-scoped architecture. This skill provides a reusable implementation guide so AI agents generate production-quality Laravel API code.

## Quick Start

When the user asks for a Laravel API, follow this workflow:

1. **Understand requirements** – Resources, operations, authentication needs
2. **Initialize project structure** – Routing, remove frontend bloat
3. **Build first resource** – Full CRUD (Model → Payload → Form Request → Action → Controller)
4. **Add authentication** – JWT via PHP Open Source Saver
5. **Iterate on remaining resources** – Reuse the same pattern

## When to Use This Skill

- **New REST API projects** – Scaffolding a Laravel API from scratch with versioning and auth
- **Adding resources** – Creating new CRUD endpoints (tasks, projects, users) following the same pattern
- **API versioning** – Introducing v2 without breaking existing clients; Sunset headers for deprecation
- **Authentication** – Adding JWT (e.g. PHP Open Source Saver) for stateless API auth
- **Code review / refactor** – Reviewing or refactoring Laravel API code for PSR-12, types, and architecture
- **Validation & DTOs** – Implementing Form Requests with `payload()` returning typed DTOs
- **Query building** – Filtering, sorting, includes with Spatie Query Builder

## Core Concepts

### 1. Stateless by Design

- No hidden dependencies in models or services
- Explicit data flow through DTOs (Payloads)
- Query building over implicit scopes
- `Model::shouldBeStrict()` to catch N+1 early

### 2. Boundary-First Approach

- **HTTP layer**: Controllers (invokable), Form Requests, Response classes
- **Business logic**: Actions (single-purpose) or Services (only when needed)
- **Data layer**: Models (data access only), DTOs for transfer

Form Requests validate and expose a `payload()` that returns a DTO. Controllers coordinate Request → Action → Response.

### 3. Resource-Scoped Organization

- Routes: `routes/api/{resource}.php` (e.g. `tasks.php`, `projects.php`)
- Controllers: `App\Http\Controllers\{Resource}\V1\{Operation}Controller.php`
- Requests/Payloads/Actions: namespaced by resource and version where applicable

### 4. Version Discipline

- Versioning via namespaces (V1, V2)
- HTTP Sunset headers for deprecated versions
- Keep old versions working; avoid breaking existing clients

### 5. Code Quality (PSR-12 & Laravel)

- Preserve functionality: refactors change *how*, never *what*
- Explicit over implicit; type declarations; `declare(strict_types=1)`
- Prefer `match` over nested ternaries; extract complex conditions into named methods

## Project Structure

```
routes/api/
  routes.php              # Main entry; version grouping
  tasks.php               # Task routes, all versions
  projects.php            # Project routes, all versions

app/Http/
  Controllers/{Resource}/V1/
    StoreController.php   # Invokable only
    IndexController.php
    ShowController.php
  Requests/{Resource}/V1/
    StoreTaskRequest.php  # Validation + payload()
  Payloads/{Resource}/
    StoreTaskPayload.php  # DTOs with toArray()
  Responses/
    JsonDataResponse.php  # Implements Responsable
    JsonErrorResponse.php
  Middleware/
    HttpSunset.php

app/Actions/{Resource}/
  CreateTask.php          # Single-purpose business logic

app/Services/              # Only when logic too complex for Actions

app/Models/
  Task.php                 # HasUlids, data access only
```

## PHP Implementation

### Step 1: Model (ULIDs, strict types)

```php
<?php

declare(strict_types=1);

namespace App\Models;

use Illuminate\Database\Eloquent\Concerns\HasUlids;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\BelongsTo;

final class Task extends Model
{
    use HasFactory;
    use HasUlids;

    protected $fillable = [
        'title',
        'description',
        'status',
        'project_id',
    ];

    protected $casts = [
        'created_at' => 'datetime',
        'updated_at' => 'datetime',
    ];

    public function project(): BelongsTo
    {
        return $this->belongsTo(Project::class);
    }
}
```

### Step 2: Routes

Create `routes/api/{resource}.php`:

```php
use App\Http\Controllers\Tasks\V1;

Route::middleware(['auth:api'])->group(function () {
    Route::get('/tasks', V1\IndexController::class);
    Route::post('/tasks', V1\StoreController::class);
    Route::get('/tasks/{task}', V1\ShowController::class);
    Route::patch('/tasks/{task}', V1\UpdateController::class);
    Route::delete('/tasks/{task}', V1\DestroyController::class);
});
```

Include in `routes/api/routes.php`:

```php
Route::prefix('v1')->group(function () {
    require __DIR__ . '/tasks.php';
});
```

### Step 3: DTO (Payload)

`app/Http/Payloads/{Resource}/{Operation}Payload.php`:

```php
<?php

declare(strict_types=1);

namespace App\Http\Payloads\Tasks;

final readonly class StoreTaskPayload
{
    public function __construct(
        public string $title,
        public ?string $description,
        public string $status,
        public string $projectId,
    ) {}

    public function toArray(): array
    {
        return [
            'title' => $this->title,
            'description' => $this->description,
            'status' => $this->status,
            'project_id' => $this->projectId,
        ];
    }
}
```

### Step 4: Form Request

`app/Http/Requests/{Resource}/V1/{Operation}Request.php`:

```php
<?php

declare(strict_types=1);

namespace App\Http\Requests\Tasks\V1;

use App\Http\Payloads\Tasks\StoreTaskPayload;
use Illuminate\Foundation\Http\FormRequest;
use Illuminate\Validation\Rule;

final class StoreTaskRequest extends FormRequest
{
    public function authorize(): bool
    {
        return true;
    }

    public function rules(): array
    {
        return [
            'title' => ['required', 'string', 'max:255'],
            'description' => ['nullable', 'string', 'max:1000'],
            'status' => ['required', Rule::in(['pending', 'in_progress', 'completed'])],
            'project_id' => ['required', 'string', 'exists:projects,id'],
        ];
    }

    public function payload(): StoreTaskPayload
    {
        return new StoreTaskPayload(
            title: $this->string('title')->toString(),
            description: $this->string('description')->toString(),
            status: $this->string('status')->toString(),
            projectId: $this->string('project_id')->toString(),
        );
    }
}
```

### Step 5: Action

`app/Actions/{Resource}/{Operation}.php`:

```php
<?php

declare(strict_types=1);

namespace App\Actions\Tasks;

use App\Http\Payloads\Tasks\StoreTaskPayload;
use App\Models\Task;

final readonly class CreateTask
{
    public function handle(StoreTaskPayload $payload): Task
    {
        return Task::create($payload->toArray());
    }
}
```

### Step 6: Controller (invokable)

`app/Http/Controllers/{Resource}/V1/{Operation}Controller.php`:

```php
<?php

declare(strict_types=1);

namespace App\Http\Controllers\Tasks\V1;

use App\Actions\Tasks\CreateTask;
use App\Http\Requests\Tasks\V1\StoreTaskRequest;
use App\Http\Responses\JsonDataResponse;
use Illuminate\Http\JsonResponse;

final readonly class StoreController
{
    public function __construct(
        private CreateTask $createTask,
    ) {}

    public function __invoke(StoreTaskRequest $request): JsonResponse
    {
        $task = $this->createTask->handle(
            payload: $request->payload(),
        );

        return new JsonDataResponse(
            data: $task,
            status: 201,
        );
    }
}
```

## Response Format

**Success:**

```json
{
    "data": {...},
    "meta": {...}
}
```

**Error (RFC 7807 Problem+JSON):**

```json
{
    "type": "about:blank",
    "title": "Validation Failed",
    "status": 422,
    "detail": "The given data was invalid",
    "errors": {...}
}
```

## Query Building (Spatie)

```php
use Spatie\QueryBuilder\QueryBuilder;

$tasks = QueryBuilder::for(Task::class)
    ->allowedFilters(['status', 'priority'])
    ->allowedSorts(['created_at', 'due_date'])
    ->allowedIncludes(['project', 'assignee'])
    ->paginate();
```

## Versioning & Sunset

When adding V2:

1. Create `App\Http\Controllers\Tasks\V2\` (and related Request/Payload if changed).
2. Add V2 route group in `routes/api/tasks.php`.
3. Add Sunset middleware to V1:

```php
Route::middleware(['auth:api', 'http.sunset:2025-12-31'])->group(function () {
    // V1 routes
});
```

## Authentication (JWT)

```bash
composer require php-open-source-saver/jwt-auth
php artisan vendor:publish --provider="PHPOpenSourceSaver\JWTAuth\Providers\LaravelServiceProvider"
php artisan jwt:secret
```

`config/auth.php`:

```php
'guards' => [
    'api' => [
        'driver' => 'jwt',
        'provider' => 'users',
    ],
],
```

## Essential Setup

`app/Providers/AppServiceProvider.php`:

```php
use Illuminate\Database\Eloquent\Model;

public function boot(): void
{
    Model::shouldBeStrict();
}
```

Register HttpSunset in `app/Http/Kernel.php`:

```php
protected $middlewareAliases = [
    'http.sunset' => \App\Http\Middleware\HttpSunset::class,
];
```

## Code Review & Refactoring

- Preserve functionality; add return/parameter types; use `match` instead of nested ternaries.
- Extract complex conditions into named methods; keep PSR-12 and `declare(strict_types=1)`.

**Match over nested ternary:**

```php
// ❌ Avoid
$status = $task->completed_at ? ($task->verified ? 'verified' : 'completed') : ($task->started_at ? 'in_progress' : 'pending');

// ✅ Prefer
$status = match (true) {
    $task->completed_at && $task->verified => 'verified',
    $task->completed_at => 'completed',
    $task->started_at => 'in_progress',
    default => 'pending',
};
```

## Anti-Patterns to Avoid

- Auto-increment IDs (use ULIDs)
- Business logic in models
- Multiple actions per controller (use one invokable per controller)
- Accessing request data directly in controllers/actions (use Form Request + payload())
- Hidden query scopes (prefer explicit Query Builder / allowedFilters)
- Service classes when an Action is enough
- Breaking changes without versioning
- Inconsistent response shapes
- Nested ternaries; missing type declarations; overly clever code

## References

| File | Purpose |
|------|---------|
| `references/architecture.md` | Principles, structure, component patterns |
| `references/code-examples.md` | Full examples for Model, Request, Payload, Action, Controller, Response |
| `references/code-quality.md` | PSR-12, refactoring, match expressions, type safety |

## Assets (Templates)

Scaffolding templates in `assets/templates/`:

| Template | Use |
|----------|-----|
| `Model.php` | New Eloquent model with HasUlids |
| `Payload.php` | DTO with constructor + `toArray()` |
| `FormRequest.php` | Validation + `payload()` returning DTO |
| `Action.php` | Single-purpose action with `handle(Payload): Model` |
| `Controller.php` | Invokable controller; Request → Action → JsonDataResponse |

Replace placeholders: `{Resource}`, `{Operation}`, `{Request}`, `{Payload}`, `{Action}`, `{Controller}`, `{Model}`.

---

*This skill aligns with the [Drift Skills](https://github.com/dadbodgeoff/drift/wiki/Skills) format: reusable implementation guides so AI agents produce production-grade Laravel API code.*
