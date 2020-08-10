# api-guidelines

## URI
Универсальный формат
`/collection/item/collection`

> Важно!
> Глубина вложенности строго ограничена до 2 коллекций.
> С увеличение глубины возрастает сложность поддержки и адаптации.
> Необходимо использовать идентификаторы для получения данных в других коллекциях. 
> 
> *Плохо:*
> `/users/1/comments/2/likes`
>  *Хорошо:*
> `/comments/2/likes`

**Примеры:**  
`/users`  – коллекция пользователей  
`/users/1` – элемент коллекции  
`/users/1/comments` – связная коллекция

## Headers
::WIP::

## Методы

#### GET `/collection/item`  
Возвращает представление ресурса.
Возвращает код состояния HTTP 200 (ОК). Если ресурс не найден, метод должен вернуть код 404 (не найдено). В ответе также должны содержаться заголовки с информацией о политике кеширования.

#### GET `/collection?param=value&param1=value1`  
Возвращает коллекцию запрашиваемых ресурсов с учетом параметров запроса.
Возвращает код состояния HTTP 200 (ОК). При отсутствии ресурсов возвращается пустая коллекция.

*Пагинация с limit & offset:*  
`/collection?limit=25&offset=50`  

*Пагинация с курсором:*  
::WIP::  

*Сортировка:*  
`collection?sort=+field&sort=-field`

#### HEAD `/collection/item`  
Возвращает HTTP-заголовки.
::WIP::
> Используется для проверки ресурса без его передачи.

#### POST `/collection`  
- Создает новый ресурс по указанному URI. Текст запроса содержит сведения о новом ресурсе. 
Возвращает код  состояния HTTP 201 (создано).
- Вызывает удаленные процедуры.  Возвращает код состояния HTTP 200 (ОК).

> Важно!
> Неидемпотентный метод. Повторный вызов создает новый ресурс или вызывает новую процедуру.

#### PUT `/collection/item`  
Изменяет ресурс целиком. В запросе должны содержаться **все** поля изменяемого ресурса.
Возвращает код состояния HTTP 204 (нет содержимого).  Если ресурс не найден, метод должен вернуть код 404 (не найдено).

#### PATCH `/collection/item`  
Изменяет ресурс частично. В запросе должны содержаться поля, подлежащие изменению.
Возвращает код состояния HTTP 200 (ОК) и представление измененного ресурса. Если ресурс не найден, метод должен вернуть код 404 (не найдено).

#### DELETE `collection/item`  
Удаляет ресурс.
Возвращает код состояния HTTP 204 (нет содержимого).

## Версионирование
Для более гибкой работы с разными версиями **API** передачи версии происходит с помощью HTTP заголовка `api-version=1_0`

## Ответы сервера с ограничением
```ts
{
  code: HTTPCode,
  data?: any | any[],
  error?: {
    code: ERRORCode,
    message: string
  }
}
```
**HTTPCode** – код состояния HTTP  
**ERRORCode** – уникальный идентификатор ошибки (enum).
