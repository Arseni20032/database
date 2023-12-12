# Функциональные требования

**Администратор (ADMIN):**
- Управление объектами недвижимости
- Управление клиентами
- Управление сотрудниками компании
- Управление сделками 
- Управление новыми вопросами и ответами в FAQ
- Управление новостями
- Управление вакансиями

**Без регистрации (анонимный пользователь):**
- Регистрация / авторизация
- Просмотр категорий объектов и самих объектов
- Просмотр вопросов и ответов
- Просмотр промокодов 
- Просмотр новостей 
- Просмотр вакансий 

**Любой зарегистрированный пользователь:**
- Выход из системы
- Создание отзывов
- Фильтрация данных по цене и категории

**Зарегистрированный пользователь, сотрудник (Employee):**
- Просмотр сделок
- Просмотр покупателей/арендаторов (с которыми есть сделки)
- Просмотр суммы сделок наиболее прибыльного типа объекта
- Просмотр общей суммы и количества сделок

# Сущности

1. **CustomUser**
    - `id` (BIGINT NOT NULL, PK) - Уникальный идентификатор пользователя.
    - `username` (VARCHAR(150) NOT NULL) - Имя пользователя.
    - `password` (VARCHAR(128) NOT NULL) - Хэшированный пароль пользователя.
    - `first_name` (VARCHAR(30) NOT NULL) - Имя пользователя.
    - `last_name` (VARCHAR(150) NOT NULL) - Фамилия пользователя.
    - `email` (VARCHAR(254) NOT NULL) - Электронная почта пользователя.
    - `date_joined` (TIMESTAMP WITH TIME ZONE NOT NULL) - Дата и время регистрации пользователя.
    - `date_of_birth` (DATE) - Дата рождения пользователя (необязательно).
    - `timezone` (VARCHAR(50) NOT NULL) - Часовой пояс пользователя.
    - `is_staff` (BOOLEAN NOT NULL) - Флаг, определяющий, является ли пользователь персоналом.
    - `is_active` (BOOLEAN NOT NULL) - Флаг, указывающий, активен ли пользователь.
    - `is_superuser` (BOOLEAN NOT NULL) - Флаг, указывающий, является ли пользователь суперпользователем.
    - `phone_number` (VARCHAR(255)) - Номер телефона пользователя (необязательно).

2. **Buyer**
    - `id` (BIGINT NOT NULL, PK) - Уникальный идентификатор покупателя.
    - `user_id` (BIGINT NOT NULL, FK -> CustomUser) - Ссылка на пользователя, связанного с этим покупателем.
    - `has_pets` (BOOLEAN) - Флаг, указывающий, есть ли у покупателя домашние животные (необязательно).
    - `has_children` (BOOLEAN) - Флаг, указывающий, есть ли у покупателя дети (необязательно).
    - `created_at` (TIMESTAMP) - Дата и время создания записи.
    - `updated_at` (TIMESTAMP) - Дата и время обновления записи.

3. **EstateType**
    - `id` (BIGINT NOT NULL, PK) - Уникальный идентификатор типа недвижимости.
    - `type_estate` (VARCHAR(255) NOT NULL) - Тип недвижимости, например, "Квартира", "Дом"

4. **Employee**
    - `id` (BIGINT NOT NULL, PK) - Уникальный идентификатор сотрудника.
    - `user_id` (BIGINT NOT NULL, FK -> CustomUser) - Ссылка на пользователя, связанного с этим сотрудником.
    - `full_name` (VARCHAR(255)) - Полное имя сотрудника.
    - `phone_number` (VARCHAR(255)) - Номер телефона сотрудника.
    - `email` (VARCHAR(255)) - Электронная почта сотрудника.
    - `deal_count` (INTEGER NOT NULL) - Количество сделок, совершенных сотрудником.
    - `created_at` (TIMESTAMP) - Дата и время создания записи.
    - `updated_at` (TIMESTAMP) - Дата и время обновления записи.
    - `photo` (VARCHAR(255)) - Фотография сотрудника (путь к файлу).
    - `responsibilities` (VARCHAR(255) NOT NULL) - Ответственность сотрудника, например, "Менеджер по продажам".

5. **Owner**
    - `id` (BIGINT NOT NULL, PK) - Уникальный идентификатор владельца.
    - `user_id` (BIGINT NOT NULL, FK -> CustomUser) - Ссылка на пользователя, связанного с этим владельцем.
    - `created_at` (TIMESTAMP) - Дата и время создания записи.
    - `updated_at` (TIMESTAMP) - Дата и время обновления записи.
    - `bank_account_number` (VARCHAR(34)) - Номер банковского счета владельца.

6. **Estate**
    - `id` (BIGINT NOT NULL, PK) - Уникальный идентификатор недвижимости.
    - `name` (VARCHAR(255)) - Название недвижимости.
    - `description` (TEXT) - Описание недвижимости.
    - `address` (VARCHAR(255)) - Адрес недвижимости.
    - `creation_date` (DATE NOT NULL) - Дата создания недвижимости.
    - `square` (FLOAT NOT NULL) - Площадь недвижимости.
    - `number_rooms` (INTEGER NOT NULL) - Количество комнат.
    - `ceiling_height` (FLOAT NOT NULL) - Высота потолков.
    - `owner_id` (BIGINT NOT NULL, FK -> Owner) - Ссылка на владельца недвижимости.
    - `responsible_employee_id` (BIGINT NOT NULL, FK -> Employee) - Ссылка на сотрудника, ответственного за недвижимость.
    - `estate_type_id` (BIGINT NOT NULL, FK -> EstateType) - Ссылка на тип недвижимости.
    - `cost` (FLOAT) - Стоимость недвижимости (необязательно).
    - `created_at` (TIMESTAMP) - Дата и время создания записи.
    - `updated_at` (TIMESTAMP) - Дата и время обновления записи.

7. **Deal**
    - `id` (BIGINT NOT NULL, PK) - Уникальный идентификатор сделки.
    - `estate_id` (BIGINT NOT NULL, FK -> Estate) - Ссылка на недвижимость, связанную с этой сделкой.
    - `buyer_id` (BIGINT NOT NULL, FK -> Buyer) - Ссылка на покупателя, участвующего в сделке.
    - `owner_id` (BIGINT NOT NULL, FK -> Owner) - Ссылка на владельца недвижимости, участвующего в сделке.
    - `employee_id` (BIGINT NOT NULL, FK -> Employee) - Ссылка на сотрудника, связанного с сделкой.
    - `cost` (FLOAT NOT NULL) - Стоимость сделки.
    - `deal_date` (DATE NOT NULL) - Дата сделки.
    - `deal_date_end` (TIMESTAMP NOT NULL) - Дата и время окончания сделки.
    - `created_at` (TIMESTAMP) - Дата и время создания записи.
    - `updated_at` (TIMESTAMP) - Дата и время обновления записи.

8. **Post**
    - `id` (BIGINT NOT NULL, PK) - Уникальный идентификатор поста.
    - `title` (VARCHAR(250) NOT NULL) - Заголовок поста.
    - `slug` (VARCHAR(255)) - Уникальный слаг (часть URL) для поста.
    - `body` (TEXT NOT NULL) - Текст поста.
    - `publish` (TIMESTAMP WITH TIME ZONE NOT NULL) - Дата и время публикации поста.
    - `created` (TIMESTAMP) - Дата и время создания поста.
    - `updated` (TIMESTAMP) - Дата и время обновления поста.
    - `author_id` (BIGINT NOT NULL, FK -> CustomUser) - Ссылка на автора поста.
    - `photo` (VARCHAR(255)) - Фотография поста (путь к файлу).
    - `status` (VARCHAR(2) NOT NULL) - Статус поста (Draft или Published).

9. **FAQ**
    - `id` (BIGINT NOT NULL, PK) - Уникальный идентификатор часто задаваемого вопроса.
    - `question` (VARCHAR(255) NOT NULL) - Вопрос.
    - `answer` (TEXT NOT NULL) - Ответ на вопрос.
    - `date_added` (TIMESTAMP NOT NULL) - Дата и время добавления вопроса и ответа.

10. **JobVacancy**
    - `id` (BIGINT NOT NULL, PK) - Уникальный идентификатор вакансии.
    - `title` (VARCHAR(255) NOT NULL) - Название вакансии.
    - `description` (TEXT NOT NULL) - Описание вакансии.
    - `location` (VARCHAR(100) NOT NULL) - Местоположение вакансии.
    - `salary` (DECIMAL(10, 2)) - Заработная плата (необязательно).
    - `published_date` (TIMESTAMP NOT NULL) - Дата и время публикации вакансии.

11. **Review**
    - `id` (BIGINT NOT NULL, PK) - Уникальный идентификатор отзыва.
    - `user_id` (BIGINT NOT NULL, FK -> CustomUser) - Ссылка на пользователя, оставившего отзыв.
    - `name` (VARCHAR(255) NOT NULL) - Имя автора отзыва.
    - `rating` (INTEGER NOT NULL) - Рейтинг отзыва (от 1 до 5).
    - `text` (TEXT NOT NULL) - Текст отзыва.
    - `date` (TIMESTAMP NOT NULL) - Дата и время добавления отзыва.

12. **PromoCode**
    - `id` (BIGINT NOT NULL, PK) - Уникальный идентификатор промо-кода.
    - `code` (VARCHAR(20) NOT NULL) - Уникальный код промо-кода.
    - `description` (TEXT NOT NULL) - Описание промо-кода.
    - `is_active` (BOOLEAN NOT NULL) - Флаг, указывающий, активен ли промо-код.
    - `created_date` (TIMESTAMP NOT NULL) - Дата и время создания промо-кода.
    - `archived_date` (TIMESTAMP) - Дата и время архивации промо-к
