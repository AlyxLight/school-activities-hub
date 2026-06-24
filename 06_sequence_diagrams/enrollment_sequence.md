# Sequence Diagram

## Сценарий: запись ученика на занятие

Участники:

* Ученик
* Frontend
* Backend API
* База данных
* Сервис уведомлений

```mermaid
sequenceDiagram
    actor Student as Ученик
    participant Frontend
    participant API as Backend API
    participant DB as База данных
    participant Notify as Сервис уведомлений

    Student->>Frontend: Открывает карточку занятия
    Frontend->>API: GET /activities/{activityId}
    API->>DB: Получить данные занятия
    DB-->>API: Данные занятия
    API-->>Frontend: 200 OK + ActivityDetails
    Frontend-->>Student: Показывает карточку занятия

    Student->>Frontend: Нажимает "Записаться"
    Frontend->>API: POST /activities/{activityId}/enrollments

    API->>DB: Проверить статус занятия
    DB-->>API: status = published

    API->>DB: Проверить свободные места
    DB-->>API: availablePlaces > 0

    API->>DB: Проверить, нет ли активной записи
    DB-->>API: Активной записи нет

    API->>DB: Создать запись на занятие
    DB-->>API: Запись создана

    API->>Notify: Создать уведомление о записи
    Notify-->>API: Уведомление создано

    API-->>Frontend: 201 Created + EnrollmentResponse
    Frontend-->>Student: Показывает подтверждение записи
```

## Альтернативные сценарии

### Нет свободных мест

```mermaid
sequenceDiagram
    actor Student as Ученик
    participant Frontend
    participant API as Backend API
    participant DB as База данных

    Student->>Frontend: Нажимает "Записаться"
    Frontend->>API: POST /activities/{activityId}/enrollments

    API->>DB: Проверить свободные места
    DB-->>API: availablePlaces = 0

    API-->>Frontend: 409 Conflict + NO_AVAILABLE_PLACES
    Frontend-->>Student: Показывает сообщение "Свободных мест нет"
```

### Ученик уже записан

```mermaid
sequenceDiagram
    actor Student as Ученик
    participant Frontend
    participant API as Backend API
    participant DB as База данных

    Student->>Frontend: Нажимает "Записаться"
    Frontend->>API: POST /activities/{activityId}/enrollments

    API->>DB: Проверить активную запись ученика
    DB-->>API: Активная запись уже есть

    API-->>Frontend: 409 Conflict + ALREADY_ENROLLED
    Frontend-->>Student: Показывает сообщение "Вы уже записаны"
```

## Важное пояснение

На диаграмме видно, что перед созданием записи система проверяет три бизнес-условия:

1. занятие опубликовано;
2. есть свободные места;
3. ученик ещё не записан на это занятие.

Это связывает Sequence Diagram с бизнес-правилами API и ER-моделью.

---
