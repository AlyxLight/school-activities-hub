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
subjectId
classId
dateFrom
dateTo
```

Пример:

```http
GET /activities?subjectId=uuid&classId=uuid&dateFrom=2026-09-01&dateTo=2026-09-30
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
