
# Функциональные требования:
1. Управление пользователями:
   - Регистрация новых пользователей.
   - Аутентификация и авторизация пользователей.
   - Возможность смены пароля и персональных данных пользователя.

2. Управление покупателями (Buyer):
   - Создание новых записей о покупателях.
   - Редактирование информации о покупателях.
   - Удаление записей о покупателях.
   - Связь с пользователями (CustomUser).

3. Управление типами недвижимости (EstateType):
   - Создание новых типов недвижимости.
   - Редактирование информации о типах недвижимости.
   - Удаление типов недвижимости.

4. Управление владельцами (Owner):
   - Создание новых записей о владельцах.
   - Редактирование информации о владельцах.
   - Удаление записей о владельцах.
   - Связь с пользователями (CustomUser).

5. Управление сотрудниками (Employee):
   - Создание новых записей о сотрудниках.
   - Редактирование информации о сотрудниках.
   - Удаление записей о сотрудниках.
   - Связь с пользователями (CustomUser).

6. Управление недвижимостью (Estate):
   - Создание новых записей о недвижимости.
   - Редактирование информации о недвижимости.
   - Удаление записей о недвижимости.
   - Связь с владельцами, покупателями и сотрудниками.
   - Учет стоимости и характеристик недвижимости.

7. Управление сделками (Deal):
   - Создание новых записей о сделках.
   - Редактирование информации о сделках.
   - Удаление записей о сделках.
   - Связь с недвижимостью, покупателями и сотрудниками.
   - Учет стоимости и даты сделок.

8. Управление постами (Post):
   - Создание новых постов.
   - Редактирование и удаление существующих постов.
   - Публикация и снятие с публикации постов.
   - Учет статуса постов (Draft, Published).

9. Управление FAQ:
   - Создание новых вопросов и ответов.
   - Редактирование и удаление существующих вопросов и ответов.
   - Учет даты добавления вопросов.

10. Управление вакансиями (JobVacancy):
    - Создание новых вакансий.
    - Редактирование и удаление существующих вакансий.
    - Учет местоположения и заработной платы.

11. Управление отзывами (Review):
    - Создание новых отзывов.
    - Редактирование и удаление существующих отзывов.
    - Учет рейтинга и даты отзывов.

12. Управление промокодами (PromoCode):
    - Создание новых промокодов.
    - Редактирование и удаление существующих промокодов.
    - Учет активности и дат создания промокодов.

# Сущности

1. "CustomUser" 
    - "id" BIGINT NOT NULL, PK
    - "username" VARCHAR(150) NOT NULL
    - "password" VARCHAR(128) NOT NULL
    - "first_name" VARCHAR(30) NOT NULL
    - "last_name" VARCHAR(150) NOT NULL
    - "email" VARCHAR(254) NOT NULL
    - "date_joined" TIMESTAMP WITH TIME ZONE NOT NULL
    - "date_of_birth" DATE
    - "timezone" VARCHAR(50) NOT NULL
    - "is_staff" BOOLEAN NOT NULL
    - "is_active" BOOLEAN NOT NULL
    - "is_superuser" BOOLEAN NOT NULL

2. "Buyer" 
    - "id" BIGINT NOT NULL, PK
    - "phone_number" VARCHAR(255) NOT NULL
    - "user_id" BIGINT NOT NULL, FK -> CustomUser
    - "full_name" VARCHAR(255) NOT NULL
    - "email" VARCHAR(255) NOT NULL
    - "created_at" TIMESTAMP NOT NULL
    - "updated_at" TIMESTAMP NOT NULL

3. "EstateType" 
    - "id" BIGINT NOT NULL, PK
    - "type_estate" VARCHAR(255) NOT NULL

4. "Owner" 
    - "id" BIGINT NOT NULL, PK
    - "user_id" BIGINT NOT NULL, FK -> CustomUser
    - "full_name" VARCHAR(255) NOT NULL
    - "phone_number" VARCHAR(255) NOT NULL
    - "email" VARCHAR(255) NOT NULL
    - "created_at" TIMESTAMP NOT NULL
    - "updated_at" TIMESTAMP NOT NULL

5. "Employee" 
    - "id" BIGINT NOT NULL, PK
    - "user_id" BIGINT NOT NULL, FK -> CustomUser
    - "full_name" VARCHAR(255) NOT NULL
    - "phone_number" VARCHAR(255) NOT NULL
    - "email" VARCHAR(255) NOT NULL
    - "deal_count" INTEGER NOT NULL
    - "created_at" TIMESTAMP NOT NULL
    - "updated_at" TIMESTAMP NOT NULL
    - "photo" VARCHAR(100)
    - "responsibilities" VARCHAR(255) NOT NULL

6. "Estate" 
    - "id" BIGINT NOT NULL, PK
    - "name" VARCHAR(255) NOT NULL
    - "description" TEXT NOT NULL
    - "address" VARCHAR(255) NOT NULL
    - "creation_date" DATE NOT NULL
    - "square" FLOAT NOT NULL
    - "number_rooms" INTEGER NOT NULL
    - "ceiling_height" FLOAT NOT NULL
    - "owner_id" BIGINT NOT NULL, FK -> Owner
    - "responsible_employee_id" BIGINT NOT NULL, FK -> Employee
    - "estate_type_id" BIGINT NOT NULL, FK -> EstateType
    - "cost" FLOAT
    - "created_at" TIMESTAMP NOT NULL
    - "updated_at" TIMESTAMP NOT NULL

7. "Deal" 
    - "id" BIGINT NOT NULL, PK
    - "estate_id" BIGINT NOT NULL, FK -> Estate
    - "buyer_id" BIGINT NOT NULL, FK -> Buyer
    - "employee_id" BIGINT NOT NULL, FK -> Employee
    - "cost" FLOAT NOT NULL
    - "deal_date" DATE NOT NULL
    - "deal_date_end" TIMESTAMP NOT NULL
    - "created_at" TIMESTAMP NOT NULL
    - "updated_at" TIMESTAMP NOT NULL

8. "Post" 
    - "id" BIGINT NOT NULL, PK
    - "title" VARCHAR(250) NOT NULL
    - "slug" VARCHAR(255) NOT NULL
    - "body" TEXT NOT NULL
    - "publish" TIMESTAMP WITH TIME ZONE NOT NULL
    - "created" TIMESTAMP NOT NULL
    - "updated" TIMESTAMP NOT NULL
    - "author_id" BIGINT NOT NULL, FK -> CustomUser
    - "photo" VARCHAR(100)
    - "status" VARCHAR(2) NOT NULL

9. "FAQ" 
    - "id" BIGINT NOT NULL, PK
    - "question" VARCHAR(255) NOT NULL
    - "answer" TEXT NOT NULL
    - "date_added" TIMESTAMP NOT NULL

10. "JobVacancy" 
    - "id" BIGINT NOT NULL, PK
    - "title" VARCHAR(255) NOT NULL
    - "description" TEXT NOT NULL
    - "location" VARCHAR(100) NOT NULL
    - "salary" DECIMAL(10, 2)
    - "published_date" TIMESTAMP NOT NULL

11. "Review" 
    - "id" BIGINT NOT NULL, PK
    - "user_id" BIGINT NOT NULL, FK -> CustomUser
    - "name" VARCHAR(255) NOT NULL
    - "rating" INTEGER NOT NULL
    - "text" TEXT NOT NULL
    - "date" TIMESTAMP NOT NULL
 
12. "PromoCode" 
    - "id" BIGINT NOT NULL, PK
    - "code" VARCHAR(20) NOT NULL
    - "description" TEXT NOT NULL
    - "is_active" BOOLEAN NOT NULL
    - "created_date" TIMESTAMP NOT NULL
    - "archived_date" TIMESTAMP
