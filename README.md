# PC Builder

Полнофункциональное веб-приложение для построения и сохранения конфигураций компьютеров. Приложение позволяет пользователям просматривать каталог компонентов, создавать собственные сборки ПК и управлять ними.

## 🌟 Особенности

- **👤 Аутентификация пользователей** - Регистрация и вход с защитой JWT
- **🖥️ Каталог компонентов** - Просмотр доступных компонентов компьютера
- **🛠️ Конструктор сборок** - Создание собственных конфигураций ПК
- **💾 Сохранение сборок** - Сохранение и управление созданными сборками
- **👨‍💼 Панель администратора** - Управление пользователями, компонентами и сборками
- **📋 Профиль пользователя** - Просмотр и редактирование профиля
- **🔒 Безопасность** - Авторизация на основе JWT, хеширование паролей
- **📱 Отзывчивый дизайн** - Оптимизация для мобильных и десктопных устройств

## 🛠️ Технологический стек

### Frontend
- **React 18** - JavaScript библиотека для создания пользовательских интерфейсов
- **TypeScript** - Типизированный JavaScript
- **Vite** - Быстрый build tool и dev server
- **React Router DOM** - Маршрутизация между страницами
- **Tailwind CSS** - Утилитарный CSS фреймворк
- **Axios** - HTTP клиент для API запросов
- **React Hook Form** - Управление формами
- **React Toastify** - Уведомления пользователю
- **Lucide React** - SVG иконки
- **js-cookie** - Работа с cookies

### Backend
- **Node.js** - Среда выполнения JavaScript
- **Express.js** - Web фреймворк для Node.js
- **MySQL** - Реляционная база данных
- **JWT (jsonwebtoken)** - Аутентификация и авторизация
- **bcryptjs** - Хеширование паролей
- **multer** - Загрузка файлов
- **CORS** - Кроссдоменные запросы
- **dotenv** - Переменные окружения

## 📦 Установка

### Предварительные требования
- Node.js v14+ и npm/yarn
- MySQL сервер
- Git

### Шаги установки

1. **Клонируйте репозиторий**
```bash
git clone <repository-url>
cd pc-builder-main
```

2. **Установите зависимости**
```bash
npm install
```

3. **Настройте переменные окружения**

Создайте файл `.env` в корневой папке проекта:
```
VITE_API_BASE_URL=http://localhost:3001/api
PORT=3001
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=pc_builder
JWT_SECRET=your_jwt_secret_key
NODE_ENV=development
```

4. **Создайте базу данных MySQL**

Выполните SQL команды для создания необходимых таблиц:
```sql
-- Создание БД
CREATE DATABASE pc_builder;
USE pc_builder;

-- Создание таблицы пользователей
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(255) UNIQUE NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  role ENUM('user', 'admin') DEFAULT 'user',
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Создание таблицы компонентов
CREATE TABLE components (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  category VARCHAR(100) NOT NULL,
  description TEXT,
  price DECIMAL(10, 2) NOT NULL,
  image VARCHAR(255),
  specifications JSON,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Создание таблицы сборок
CREATE TABLE builds (
  id INT AUTO_INCREMENT PRIMARY KEY,
  userId INT NOT NULL,
  name VARCHAR(255) NOT NULL,
  description TEXT,
  components JSON,
  totalPrice DECIMAL(10, 2),
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  FOREIGN KEY (userId) REFERENCES users(id) ON DELETE CASCADE
);
```

## 🚀 Использование

### Запуск в режиме разработки

Запуск только frontend:
```bash
npm run dev
```

Запуск только backend:
```bash
npm run server
```

Запуск frontend и backend одновременно:
```bash
npm run dev:full
```

Frontend будет доступен по адресу `http://localhost:5173`
Backend будет доступен по адресу `http://localhost:3001`

### Сборка для продакшена

```bash
npm run build
```

### Проверка кода с ESLint

```bash
npm run lint
```

### Предпросмотр собранного приложения

```bash
npm run preview
```

## 📁 Структура проекта

```
pc-builder-main/
├── src/                          # Frontend исходный код
│   ├── components/
│   │   └── layout/              # Компоненты макета
│   │       ├── AdminLayout.tsx
│   │       ├── AdminSidebar.tsx
│   │       ├── Header.tsx
│   │       └── Footer.tsx
│   ├── pages/                   # Страницы приложения
│   │   ├── HomePage.tsx
│   │   ├── CatalogPage.tsx
│   │   ├── BuilderPage.tsx
│   │   ├── ComponentDetailPage.tsx
│   │   ├── SavedBuildsPage.tsx
│   │   ├── ProfilePage.tsx
│   │   ├── LoginPage.tsx
│   │   ├── RegisterPage.tsx
│   │   ├── AboutPage.tsx
│   │   ├── NotFoundPage.tsx
│   │   └── admin/               # Страницы администратора
│   │       ├── Dashboard.tsx
│   │       ├── Users.tsx
│   │       ├── Components.tsx
│   │       └── Builds.tsx
│   ├── context/
│   │   └── AuthContext.tsx      # Контекст аутентификации
│   ├── App.tsx                  # Главный компонент приложения
│   ├── main.tsx                 # Точка входа
│   ├── config.ts                # Конфигурация
│   └── index.css                # Глобальные стили
├── server/
│   └── index.js                 # Backend сервер Express
├── public/                       # Статические файлы
├── uploads/                      # Загруженные файлы
├── vite.config.ts               # Конфигурация Vite
├── tailwind.config.js           # Конфигурация Tailwind CSS
├── tsconfig.json                # Конфигурация TypeScript
├── package.json                 # Зависимости проекта
└── README.md                    # Этот файл
```

## 📖 API Endpoints

### Аутентификация
- `POST /api/auth/register` - Регистрация нового пользователя
- `POST /api/auth/login` - Вход пользователя
- `POST /api/auth/logout` - Выход пользователя

### Компоненты
- `GET /api/components` - Получить список всех компонентов
- `GET /api/components/:id` - Получить детали компонента
- `POST /api/components` - Создать новый компонент (только для админа)
- `PUT /api/components/:id` - Обновить компонент (только для админа)
- `DELETE /api/components/:id` - Удалить компонент (только для админа)

### Сборки
- `GET /api/builds` - Получить сборки пользователя
- `GET /api/builds/:id` - Получить детали сборки
- `POST /api/builds` - Создать новую сборку
- `PUT /api/builds/:id` - Обновить сборку
- `DELETE /api/builds/:id` - Удалить сборку

### Пользователи
- `GET /api/users/profile` - Получить профиль текущего пользователя
- `PUT /api/users/profile` - Обновить профиль
- `GET /api/users` - Получить список пользователей (только для админа)
- `DELETE /api/users/:id` - Удалить пользователя (только для админа)

## 🔐 Аутентификация

Приложение использует JWT (JSON Web Tokens) для аутентификации:

1. При регистрации пароль хешируется с помощью bcryptjs
2. При входе проверяется пароль и выдается JWT токен
3. JWT токен сохраняется в cookies браузера
4. Для защищенных маршрутов проверяется наличие и валидность токена

## 🤝 Вклад в проект

Вклады приветствуются! Пожалуйста:

1. Создайте fork репозитория
2. Создайте ветку для вашей функции (`git checkout -b feature/AmazingFeature`)
3. Сделайте commit ваших изменений (`git commit -m 'Add some AmazingFeature'`)
4. Push в ветку (`git push origin feature/AmazingFeature`)
5. Откройте Pull Request

## 📝 Лицензия

Этот проект распространяется под лицензией MIT. Подробнее см. файл [LICENSE](LICENSE).

## 👨‍💻 Автор

[Ваше имя/организация]

## 📧 Контакты

Если у вас есть вопросы или предложения, пожалуйста, откройте issue или свяжитесь через [ваш контактный адрес].

## ✨ Благодарности

Спасибо всем, кто способствовал развитию этого проекта!

---

**Версия:** 0.1.0  
**Последнее обновление:** 2026
