# REST API YaMDb

### Описание

Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles). 
Произведения делятся на категории: «Книги», «Фильмы», «Музыка». 
Список категорий (Category) может быть расширен администратором (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»). 
Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку. 
В каждой категории есть произведения: книги, фильмы или музыка. 
Например, в категории «Книги» могут быть произведения «Винни-Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха. 
Произведению может быть присвоен жанр (Genre) из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор. 
Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы (Review) и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). 
На одно произведение пользователь может оставить только один отзыв.

### Технологии
Python 3
Django REST Framework 
PostgreSQL
Simple-JWT
(токены для авторизации и регистрации)
Docker
 
### Документация по API
_Документация по API будет доступна после установки, по адресу /redoc._


## Install
Предварительно установить Docker:

<a href="https://docs.docker.com/engine/install/"></a>


### Запуск проекта

- Клонировать репозиторий

```
git clone git@github.com:a-ignatov/infra_sp2.git
```

- Запускаем сервисы 


```
cd infra_sp2/infra
```
```
docker-compose up -d --build
```

- Создаём структуру приложения в DB:

```
docker-compose exec web python manage.py makemigrations reviews
```

```
docker-compose exec web python manage.py migrate
```

- Добавляем статику:

```
docker-compose exec web python manage.py collectstatic --no-input
```

- Добавляем тестовые данные в DB и superuser:

```
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py loaddata fixtures.json
```
## Использование
После выполнения всех команд выше, станет доступна административная часть и ваш API готов к приёму запросов.

- Админка
```
localhost/admin
```

## Ресурсы API YaMDb

**AUTH**: 
Авторизация
```
http://127.0.0.1:8000/api/v1/auth/signup/
```
### Алгоритм регистрации пользователей
Пользователь отправляет POST-запрос на добавление нового пользователя с параметрами email и username на эндпоинт /api/v1/auth/signup/.
YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на адрес email.
Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит token . 

**USERS**: 
Пользователи
```
http://127.0.0.1:8000/api/v1/users/
```
 Создание пользователя и получение информации о всех пользователях. Доступны запросы **Get, Post**

**TITLES**: 
Произведения, к которым пишут отзывы (определённый фильм, книга или песенка).
```
http://127.0.0.1:8000/api/v1/titles/
```
Работа со статьями , доступны запросы **Get, Post, Patch и Del**

**CATEGORIES**: 
Категории (типы) произведений ("Фильмы", "Книги", "Музыка").
```
http://127.0.0.1:8000/api/v1/categories/
```
Работа с категориями, доступны запросы **Get, Post и Del**

**GENRES**: 
Жанры произведений. Одно произведение может быть привязано к нескольким жанрам.
```
http://127.0.0.1:8000/api/v1/genres/
```
Работа с жанрами, доступны запросы **Get, Post и Del**

**REVIEWS**: 
Отзывы на произведения. Отзыв привязан к определённому произведению.
```
http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/
```
Работа с отзывами , доступны запросы **Get, Post, Patch и Del**

**COMMENTS**: 
Комментарии к отзывам. Комментарий привязан к определённому отзыву.
```
http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/
```
Работа с комментариями , доступны запросы **Get, Post, Patch и Del**