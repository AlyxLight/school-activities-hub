# API Requirements

Для MVP нам нужны следующие сущности:

* Пользователь
* Занятие
* Запись на занятие
* Уведомление

Но наружу будем отдавать только то, что реально нужно клиентскому приложению.

---

# Операции ученика

## Получить список занятий

### Метод

```http
GET /activities
```

### Назначение

Получение списка опубликованных занятий.

### Фильтры

```text
subject
class
date
```

Пример:

```http
GET /activities?subject=english&class=9
```

---

## Получить карточку занятия

```http
GET /activities/{activityId}
```

Назначение:

Получение подробной информации о занятии.

---

## Записаться на занятие

```http
POST /activities/{activityId}/enrollments
```

Назначение:

Создание записи ученика на занятие.

---

## Отменить запись

```http
DELETE /enrollments/{enrollmentId}
```

Назначение:

Отмена существующей записи.

---

## Получить мои занятия

```http
GET /students/me/enrollments
```

Назначение:

Получение списка занятий текущего ученика.

---

# Операции учителя

## Создать занятие

```http
POST /activities
```

Назначение:

Создание нового занятия.

---

## Изменить занятие

```http
PUT /activities/{activityId}
```

Назначение:

Редактирование занятия.

---

## Отменить занятие

```http
PATCH /activities/{activityId}/cancel
```

Назначение:

Перевод занятия в статус Cancelled.

---

## Получить список участников

```http
GET /activities/{activityId}/participants
```

Назначение:

Получение списка записавшихся учеников.

---

# Минимальный набор DTO

---

## Activity

```json
{
  "id": "uuid",
  "title": "Консультация по ОГЭ",
  "subject": "Английский язык",
  "class": "9Б",
  "date": "2026-09-15",
  "startTime": "15:30",
  "endTime": "16:15",
  "participantLimit": 15,
  "availablePlaces": 4,
  "status": "Published"
}
```

---

## Enrollment

```json
{
  "id": "uuid",
  "activityId": "uuid",
  "studentId": "uuid",
  "status": "Active",
  "enrolledAt": "2026-09-12T18:20:00"
}
```

---
