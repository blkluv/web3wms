# Участие в проекте

Спасибо за интерес к проекту системы управления складом и отслеживания оборудования! Этот документ содержит рекомендации по участию в разработке проекта.

## 📋 Содержание

- [Кодекс поведения](#кодекс-поведения)
- [Как сообщить об ошибке](#как-сообщить-об-ошибке)
- [Как предложить улучшение](#как-предложить-улучшение)
- [Процесс разработки](#процесс-разработки)
- [Стандарты кода](#стандарты-кода)
- [Настройка среды разработки](#настройка-среды-разработки)
- [Pull Request процесс](#pull-request-процесс)

## 🤝 Кодекс поведения

Участвуя в этом проекте, вы соглашаетесь следовать нашему кодексу поведения:

- Используйте дружелюбный
- Уважайте различные точки зрения и опыт
- Принимайте конструктивную критику
- Сосредотачивайтесь на том, что лучше для сообщества
- Проявляйте эмпатию к другим участникам

## 🐛 Как сообщить об ошибке

### Перед созданием issue

1. **Проверьте существующие issues** - возможно, проблема уже известна
2. **Убедитесь, что это действительно ошибка** - проблема воспроизводится стабильно
3. **Проверьте документацию** - возможно, это ожидаемое поведение

### Создание bug report

Используйте следующий шаблон для создания отчета об ошибке:

```markdown
## Описание ошибки

Краткое описание того, что произошло.

## Шаги для воспроизведения

1. Перейти на страницу '...'
2. Нажать на кнопку '...'
3. Прокрутить вниз до '...'
4. Увидеть ошибку

## Ожидаемое поведение

Что должно было произойти.

## Фактическое поведение

Что произошло на самом деле.

## Скриншоты

Если применимо, добавьте скриншоты для объяснения проблемы.

## Окружение

- ОС: [например, Windows 10, macOS 12.1, Ubuntu 20.04]
- Браузер: [например, Chrome 96, Firefox 95, Safari 15]
- Версия проекта: [например, v1.0.0]
- Docker версия: [например, 20.10.12]

## Дополнительная информация

Любая другая информация о проблеме.

## Логи
```

Вставьте соответствующие логи здесь

```

```

## 💡 Как предложить улучшение

### Feature Request

Используйте следующий шаблон для предложения новой функциональности:

```markdown
## Описание функциональности

Ясное и краткое описание того, что вы хотите добавить.

## Мотивация

Объясните, зачем нужна эта функциональность. Какую проблему она решает?

## Детальное описание

Подробное описание того, как должна работать функциональность.

## Возможные альтернативы

Краткое описание альтернативных решений или функций, которые вы рассматривали.

## Дополнительный контекст

Любой другой контекст или скриншоты о запросе функциональности.
```

## 🔄 Процесс разработки

### Жизненный цикл issue

1. **Создание** - Issue создается с соответствующими метками
2. **Триаж** - Мейнтейнеры оценивают и назначают приоритет
3. **Назначение** - Issue назначается разработчику
4. **Разработка** - Создается feature branch для работы
5. **Тестирование** - Изменения тестируются локально
6. **Pull Request** - Создается PR с описанием изменений
7. **Code Review** - Мейнтейнеры проверяют код
8. **Merge** - После одобрения изменения мержатся в main

### Ветвление (Branching)

Мы используем следующую стратегию ветвления:

- `main` - стабильная ветка для production
- `develop` - ветка для разработки
- `feature/название-функции` - ветки для новых функций
- `bugfix/описание-ошибки` - ветки для исправления ошибок
- `hotfix/критическое-исправление` - ветки для критических исправлений

### Naming Conventions

#### Branches

```
feature/user-authentication
feature/warehouse-management
bugfix/login-form-validation
hotfix/security-vulnerability
```

#### Commits

Используйте формат [Conventional Commits](https://www.conventionalcommits.org/):

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

**Типы коммитов:**

- `feat` - новая функциональность
- `fix` - исправление ошибки
- `docs` - изменения в документации
- `style` - форматирование, отсутствующие точки с запятой и т.д.
- `refactor` - рефакторинг кода
- `test` - добавление тестов
- `chore` - обновление задач сборки, конфигураций и т.д.

## 📝 Стандарты кода

### Go (Backend Services)

```go
// ✅ Правильно
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

**Стандарты:**

- Используйте `gofmt` для форматирования
- Следуйте [Effective Go](https://golang.org/doc/effective_go.html)
- Добавляйте godoc комментарии для публичных функций
- Используйте context.Context для cancellation
- Обрабатывайте все ошибки
- Используйте meaningful переменные и функции

### TypeScript/React (Frontend)

```typescript
// ✅ Правильно
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

**Стандарты:**

- Используйте TypeScript строго (no `any`)
- Следуйте React hooks правилам
- Используйте функциональные компоненты
- Добавляйте prop types через TypeScript interfaces
- Используйте meaningful названия компонентов
- Следуйте Prettier конфигурации

### Database (MongoDB)

```javascript
// ✅ Правильно - структура коллекции
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

**Стандарты:**

- Используйте snake_case для полей
- Всегда включайте `created_at` и `updated_at`
- Добавляйте подходящие индексы
- Используйте валидацию схем
- Нормализуйте данные где это уместно

## 🛠️ Настройка среды разработки

### Требования

- **Go 1.21+** для backend разработки
- **Node.js 18+** для frontend и tracking service
- **Docker & Docker Compose** для локального окружения
- **MongoDB Compass** (опционально) для работы с БД
- **VS Code** или другая IDE с Go и TypeScript поддержкой

### Рекомендуемые VS Code расширения

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

### Настройка проекта

1. **Клонирование репозитория**

   ```bash
   git clone https://github.com/yourusername/warehouse-management-system.git
   cd warehouse-management-system
   ```

2. **Установка зависимостей Frontend**

   ```bash
   cd frontend
   npm install
   ```

3. **Установка зависимостей Go модулей**

   ```bash
   cd auth-service && go mod tidy
   cd ../warehouse-service && go mod tidy
   cd ../notification-service && go mod tidy
   ```

4. **Установка зависимостей Tracking Service**

   ```bash
   cd tracking-service-express
   npm install
   ```

5. **Запуск в режиме разработки**

   ```bash
   # Запуск инфраструктурных сервисов
   docker-compose up -d mongo rabbitmq ethereum-node

   # Запуск сервисов в development режиме
   # (каждый в отдельном терминале)
   cd auth-service && go run *.go
   cd warehouse-service && go run *.go
   cd notification-service && go run *.go
   cd tracking-service-express && npm run dev
   cd frontend && npm run dev
   ```

### Переменные окружения для разработки

Создайте `.env.local` файлы для каждого сервиса:

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

## 🔍 Pull Request процесс

### Создание Pull Request

1. **Создайте feature branch**

   ```bash
   git checkout -b feature/amazing-new-feature
   ```

2. **Внесите изменения и протестируйте**

   ```bash
   # Ваши изменения...
   git add .
   git commit -m "feat(warehouse): add inventory alerts system"
   ```

3. **Убедитесь, что код проходит линтеры**

   ```bash
   # Go
   go fmt ./...
   go vet ./...
   golangci-lint run

   # Frontend
   npm run lint
   npm run type-check
   ```

4. **Запустите тесты**

   ```bash
   # Go тесты
   go test ./...

   # Frontend тесты
   npm test
   ```

5. **Push и создайте PR**
   ```bash
   git push origin feature/amazing-new-feature
   ```

### Шаблон Pull Request

```markdown
## Описание

Краткое описание изменений в этом PR.

## Тип изменения

- [ ] Bug fix (не ломающее изменение, которое исправляет проблему)
- [ ] New feature (не ломающее изменение, которое добавляет функциональность)
- [ ] Breaking change (изменение, которое может сломать существующую функциональность)
- [ ] Documentation update (изменения в документации)

## Как было протестировано?

Опишите тесты, которые вы выполнили для проверки изменений.

## Чеклист

- [ ] Мой код следует стандартам стиля этого проекта
- [ ] Я провел самопроверку своего кода
- [ ] Я прокомментировал сложные участки кода
- [ ] Я обновил документацию (если необходимо)
- [ ] Мои изменения не создают новых предупреждений
- [ ] Я добавил тесты, которые подтверждают работу моих изменений
- [ ] Новые и существующие unit тесты проходят локально

## Скриншоты (если применимо)

Добавьте скриншоты изменений в UI.

## Связанные Issues

Closes #123
```

### Code Review Guidelines

**Для авторов PR:**

- Делайте PR небольшими и сфокусированными
- Пишите четкие commit сообщения
- Добавляйте тесты для новой функциональности
- Обновляйте документацию при необходимости
- Отвечайте на комментарии ревьюеров

**Для ревьюеров:**

- Проверяйте логику и архитектуру
- Убедитесь в наличии тестов
- Проверьте производительность и безопасность
- Предлагайте конструктивную обратную связь
- Одобряйте PR только после полной проверки

## 🧪 Тестирование

### Unit тесты

```go
// Go пример
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
// TypeScript пример
describe("UserProfile component", () => {
  it("should render user information correctly", () => {
    const user = mockUser();
    render(<UserProfile user={user} onUpdate={jest.fn()} />);

    expect(screen.getByText(user.username)).toBeInTheDocument();
    expect(screen.getByText(user.email)).toBeInTheDocument();
  });
});
```

### Integration тесты

```bash
# API тесты
cd tracking-service-express/scripts
./run-api-test.sh

# E2E тесты (если реализованы)
npm run test:e2e
```

## 📚 Дополнительные ресурсы

- [Go Style Guide](https://github.com/golang/go/wiki/CodeReviewComments)
- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)
- [MongoDB Best Practices](https://docs.mongodb.com/manual/administration/production-notes/)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)

## ❓ Вопросы?

Если у вас есть вопросы о процессе участия:

- 📧 Email: oglenyaboss@icloud.com
- 📱 Telegram: @ll_ogl

Спасибо за ваш вклад в проект! 🚀
