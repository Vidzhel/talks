## Програмна реалізація

Note:
<img height="60%" src="slides/05-tech-stack/note.jpg">
+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.3" -->

### За архітектурний принцип обрано <span data-id="text" >мікросервіси</span>

<img height="60%" src="slides/05-tech-stack/microservices.jpg" data-id="image">

Note:
<img height="60%" src="slides/05-tech-stack/note.jpg">
+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.3" -->

### За архітектурний принцип обрано <span data-id="text" class="red">мікросервіси</span>
<img height="0" src="slides/05-tech-stack/microservices.jpg" data-id="image">

### ... Чому?

Note:
<img height="60%" src="slides/05-tech-stack/note.jpg">
+++

### Як і завжди є трейдофи

<img src="slides/05-tech-stack/microservice-tradeoffs-fowler.jpg">

Note:
<img height="60%" src="slides/05-tech-stack/note.jpg">
+++

- Складність в налагоджені інфраструктури
- Проблема налагодження комунікації (залежності сервісів один від одного)
- Простота в розширені та масштабуванні

Note:
<img height="60%" src="slides/05-tech-stack/note.jpg">
+++

### Golang для написання серверної частини

- Простий у вивчені
- Підходить для написання легковісних сервісів

Note:
<img height="60%" src="slides/05-tech-stack/note.jpg">
+++

<img src="slides/05-tech-stack/protobuf_logo.png">

## [Protobuf](https://developers.google.com/protocol-buffers/docs/proto3) по HTTP для обміну інформацією

Рефлексія типу повідомлень | Генерація DTOs для будь-якої мови | Перспектива переходу на gRPC

Note:
<img height="60%" src="slides/05-tech-stack/note.jpg">
+++

### Структура кожного запиту

```protobuf[|2-5|7-16]
message GetUsersRequest {
  int32 pageSize = 1;
  common.PageToken pageToken = 2;
  optional OrderBy orderBy = 3;
  repeated FilterBy filterBy = 4;

  message OrderBy {
    UserField field = 1;
    common.OrderDirection orderDirection = 2;
  }

  message FilterBy {
    UserField field = 1;
    common.FilterOperation operation = 2;
    repeated google.protobuf.Any value = 3;
  }
}
```

Note:
<img height="60%" src="slides/05-tech-stack/note.jpg">
+++

```protobuf[|2-5|7-10]
message GetUsersResponse {
  oneof result_or_error {
    OperationResult result = 1;
    common.Error error = 2;
  }

  message OperationResult {
    repeated User users = 1;
    common.ListInfo listInfo = 2;
  }
}
```

Note:
<img height="60%" src="slides/05-tech-stack/note.jpg">
+++

### React для фронтенду

Note:
<img height="60%" src="slides/05-tech-stack/note.jpg">
+++
<!-- .slide: data-auto-animate  -->

### А тепер найцікавіше з цієї частини

Note:
<img height="60%" src="slides/05-tech-stack/note.jpg">
+++
<!-- .slide: data-auto-animate  -->

### А тепер найцікавіше з цієї частини <!-- .element: class="red" -->
## Як нам перевіряти коректність документів?

На етапі, коли користувач заповнює форму так і на сервері, наприклад при створенні документа чи оновленні реєстру

Note:
<img height="60%" src="slides/05-tech-stack/note.jpg">
+++

<img src="slides/05-tech-stack/json_schema_logo.png">

## [JSON Schema](https://json-schema.org/draft/2020-12/json-schema-validation.html)

Специфікація для JSON структур | Валідація

Note:
<img height="60%" src="slides/05-tech-stack/note.jpg">
+++

<img src="slides/05-tech-stack/json_schema_example.jpg">

Note:
<img height="60%" src="slides/05-tech-stack/note.jpg">
+++

### Генерування форми з `JSON Schema`

<img src="slides/05-tech-stack/json_schema_form.jpg">

https://rjsf-team.github.io/react-jsonschema-form/

Note:
<img height="60%" src="slides/05-tech-stack/note.jpg">
+++

### В перспективі `JSON Schema`

- Додавання нових гнучких правил для валідації [(залежить від реалізації специфікації)](https://ajv.js.org/keywords.html)
- Можливість створення окремого сервісу для валідації, що буде перевикористовувати правила на сервері

Note:
<img height="60%" src="slides/05-tech-stack/note.jpg">
