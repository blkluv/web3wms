# Contributing

Thank you for your interest in the warehouse management and equipment tracking system! This document contains guidelines for contributing to the project.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How to Report a Bug](#how-to-report-a-bug)
- [How to Suggest an Improvement](#how-to-suggest-an-improvement)
- [Development Process](#development-process)
- [Code Standards](#code-standards)
- [Setting Up Development Environment](#setting-up-development-environment)
- [Pull Request Process](#pull-request-process)

## 🤝 Code of Conduct

By participating in this project, you agree to abide by our code of conduct:

- Be friendly and patient
- Respect different points of view and experiences
- Accept constructive criticism
- Focus on what is best for the community
- Show empathy towards other members

## 🐛 How to Report a Bug

### Before Creating an Issue

1. **Check existing issues** - the problem might already be known
2. **Ensure it is actually a bug** - the problem reproduces consistently
3. **Check documentation** - it might be expected behavior

### Creating a Bug Report

Use the following template to create a bug report:

```markdown
## Bug Description

A brief description of what happened.

## Steps to Reproduce

1. Go to page '...'
2. Click on button '...'
3. Scroll down to '...'
4. See error

## Expected Behavior

What should have happened.

## Actual Behavior

What actually happened.

## Screenshots

If applicable, add screenshots to explain the problem.

## Environment

- OS: [e.g., Windows 10, macOS 12.1, Ubuntu 20.04]
- Browser: [e.g., Chrome 96, Firefox 95, Safari 15]
- Project Version: [e.g., v1.0.0]
- Docker Version: [e.g., 20.10.12]

## Additional Information

Any other information about the problem.

## Logs

Paste relevant logs here
```

## 💡 How to Suggest an Improvement

### Feature Request

Use the following template to suggest new functionality:

```markdown
## Feature Description

A clear and concise description of what you want to add.

## Motivation

Explain why this functionality is needed. What problem does it solve?

## Detailed Description

Detailed description of how the functionality should work.

## Possible Alternatives

Brief description of alternative solutions or features you considered.

## Additional Context

Any other context or screenshots about the feature request.
```

## 🔄 Development Process

### Issue Lifecycle

1. **Creation** - Issue is created with appropriate labels
2. **Triage** - Maintainers evaluate and prioritize
3. **Assignment** - Issue is assigned to a developer
4. **Development** - Feature branch is created for work
5. **Testing** - Changes are tested locally
6. **Pull Request** - PR is created with description of changes
7. **Code Review** - Maintainers review the code
8. **Merge** - After approval, changes are merged into main

### Branching Strategy

We use the following branching strategy:

- `main` - stable branch for production
- `develop` - development branch
- `feature/feature-name` - branches for new features
- `bugfix/bug-description` - branches for bug fixes
- `hotfix/critical-fix` - branches for critical fixes

### Naming Conventions

#### Branches

```
feature/user-authentication
feature/warehouse-management
bugfix/login-form-validation
hotfix/security-vulnerability
```

#### Commits

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
type(scope): description

feat(auth): add JWT token refresh mechanism
fix(warehouse): resolve inventory calculation bug
docs(readme): update installation instructions
style(frontend): improve component styling
refactor(api): simplify error handling logic
test(tracking): add unit tests for equipment service
chore(deps): update dependencies to latest versions
```

**Commit Types:**

- `feat` - new functionality
- `fix` - bug fix
- `docs` - documentation changes
- `style` - formatting, missing semicolons, etc.
- `refactor` - code refactoring
- `test` - adding tests
- `chore` - updating build tasks, configurations, etc.

## 📝 Code Standards

### Go (Backend Services)

```go
// ✅ Correct
type User struct {
    ID        primitive.ObjectID `bson:"_id,omitempty" json:"id"`
    Username  string             `bson:"username" json:"username" validate:"required,min=3,max=50"`
    Email     string             `bson:"email" json:"email" validate:"required,email"`
    CreatedAt time.Time          `bson:"created_at" json:"created_at"`
}

func (s *UserService) CreateUser(ctx context.Context, user *User) error {
    if err := s.validate.Struct(user); err != nil {
        return fmt.Errorf("validation failed: %w", err)
    }

    user.CreatedAt = time.Now()
    _, err := s.collection.InsertOne(ctx, user)
    return err
}
```

**Standards:**

- Use `gofmt` for formatting
- Follow [Effective Go](https://golang.org/doc/effective_go.html)
- Add godoc comments for public functions
- Use context.Context for cancellation
- Handle all errors
- Use meaningful variables and functions

### TypeScript/React (Frontend)

```typescript
// ✅ Correct
interface User {
  id: string;
  username: string;
  email: string;
  role: UserRole;
  createdAt: Date;
}

const UserProfile: React.FC<UserProfileProps> = ({ user, onUpdate }) => {
  const [isEditing, setIsEditing] = useState(false);

  const handleSubmit = useCallback(
    async (data: UserUpdateData) => {
      try {
        await updateUser(user.id, data);
        onUpdate();
        setIsEditing(false);
      } catch (error) {
        console.error("Failed to update user:", error);
      }
    },
    [user.id, onUpdate]
  );

  return <Card>{/* Component JSX */}</Card>;
};
```

**Standards:**

- Use TypeScript strictly (no `any`)
- Follow React hooks rules
- Use functional components
- Add prop types via TypeScript interfaces
- Use meaningful component names
- Follow Prettier configuration

### Database (MongoDB)

```javascript
// ✅ Correct - collection structure
{
  "_id": ObjectId("..."),
  "username": "john_doe",
  "email": "john@example.com",
  "role": "warehouse_manager",
  "is_active": true,
  "created_at": ISODate("2024-01-01T00:00:00Z"),
  "updated_at": ISODate("2024-01-01T00:00:00Z")
}
```

**Standards:**

- Use snake_case for fields
- Always include `created_at` and `updated_at`
- Add appropriate indexes
- Use schema validation
- Normalize data where appropriate

## 🛠️ Setting Up Development Environment

### Requirements

- **Go 1.21+** for backend development
- **Node.js 18+** for frontend and tracking service
- **Docker & Docker Compose** for local environment
- **MongoDB Compass** (optional) for DB management
- **VS Code** or another IDE with Go and TypeScript support

### Recommended VS Code Extensions

```json
{
  "recommendations": [
    "golang.go",
    "ms-vscode.vscode-typescript-next",
    "bradlc.vscode-tailwindcss",
    "esbenp.prettier-vscode",
    "ms-vscode.vscode-docker",
    "ms-vscode.vscode-mongo"
  ]
}
```

### Project Setup

1. **Clone the repository**

   ```bash
   git clone https://github.com/yourusername/warehouse-management-system.git
   cd warehouse-management-system
   ```

2. **Install Frontend Dependencies**

   ```bash
   cd frontend
   npm install
   ```

3. **Install Go Module Dependencies**

   ```bash
   cd auth-service && go mod tidy
   cd ../warehouse-service && go mod tidy
   cd ../notification-service && go mod tidy
   ```

4. **Install Tracking Service Dependencies**

   ```bash
   cd tracking-service-express
   npm install
   ```

5. **Start in Development Mode**

   ```bash
   # Start infrastructure services
   docker-compose up -d mongo rabbitmq ethereum-node

   # Start services in development mode
   # (each in a separate terminal)
   cd auth-service && go run *.go
   cd warehouse-service && go run *.go
   cd notification-service && go run *.go
   cd tracking-service-express && npm run dev
   cd frontend && npm run dev
   ```

### Environment Variables for Development

Create `.env.local` files for each service:

```bash
# auth-service/.env.local
MONGO_URI=mongodb://localhost:27017/warehouse_auth
JWT_SECRET=dev_jwt_secret_key
PORT=8000

# warehouse-service/.env.local
MONGO_URI=mongodb://localhost:27017/warehouse_inventory
PORT=8001
AUTH_SERVICE_URL=http://localhost:8000

# tracking-service-express/.env.local
MONGO_URI=mongodb://localhost:27017/warehouse_tracking
PORT=8002
ETH_NODE_URL=http://localhost:8545

# frontend/.env.local
NEXT_PUBLIC_API_AUTH_URL=http://localhost:8000
NEXT_PUBLIC_API_WAREHOUSE_URL=http://localhost:8001
NEXT_PUBLIC_API_TRACKING_URL=http://localhost:8002
```

## 🔍 Pull Request Process

### Creating a Pull Request

1. **Create a feature branch**

   ```bash
   git checkout -b feature/amazing-new-feature
   ```

2. **Make changes and test**

   ```bash
   # Your changes...
   git add .
   git commit -m "feat(warehouse): add inventory alerts system"
   ```

3. **Ensure code passes linters**

   ```bash
   # Go
   go fmt ./...
   go vet ./...
   golangci-lint run

   # Frontend
   npm run lint
   npm run type-check
   ```

4. **Run tests**

   ```bash
   # Go tests
   go test ./...

   # Frontend tests
   npm test
   ```

5. **Push and create PR**
   ```bash
   git push origin feature/amazing-new-feature
   ```

### Pull Request Template

```markdown
## Description

Brief description of changes in this PR.

## Change Type

- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update

## How Has This Been Tested?

Describe the tests that you ran to verify your changes.

## Checklist

- [ ] My code follows the style guidelines of this project
- [ ] I have performed a self-review of my own code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have updated the documentation (if applicable)
- [ ] My changes generate no new warnings
- [ ] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally

## Screenshots (if applicable)

Add screenshots of UI changes.

## Related Issues

Closes #123
```

### Code Review Guidelines

**For PR Authors:**

- Keep PRs small and focused
- Write clear commit messages
- Add tests for new functionality
- Update documentation if needed
- Respond to reviewer comments

**For Reviewers:**

- Check logic and architecture
- Ensure tests are present
- Check performance and security
- Offer constructive feedback
- Approve PR only after full check

## 🧪 Testing

### Unit Tests

```go
// Go example
func TestCreateUser(t *testing.T) {
    service := NewUserService(mockDB)

    user := &User{
        Username: "testuser",
        Email:    "test@example.com",
    }

    err := service.CreateUser(context.Background(), user)
    assert.NoError(t, err)
    assert.NotEmpty(t, user.ID)
}
```

```typescript
// TypeScript example
describe("UserProfile component", () => {
  it("should render user information correctly", () => {
    const user = mockUser();
    render(<UserProfile user={user} onUpdate={jest.fn()} />);

    expect(screen.getByText(user.username)).toBeInTheDocument();
    expect(screen.getByText(user.email)).toBeInTheDocument();
  });
});
```

### Integration Tests

```bash
# API tests
cd tracking-service-express/scripts
./run-api-test.sh

# E2E tests (if implemented)
npm run test:e2e
```

## 📚 Additional Resources

- [Go Style Guide](https://github.com/golang/go/wiki/CodeReviewComments)
- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)
- [MongoDB Best Practices](https://docs.mongodb.com/manual/administration/production-notes/)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)

## ❓ Questions?

If you have questions about the contribution process:

- 📧 Email: oglenyaboss@icloud.com
- 📱 Telegram: @ll_ogl

Thank you for contributing to the project! 🚀
