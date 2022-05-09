## Програмна реалізація

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.3" -->

### За архітектурний принцип обрано <span data-id="text" >мікросервіси</span>

+++
<!-- .slide: data-auto-animate data-auto-animate-duration="0.3" -->

### За архітектурний принцип обрано <span data-id="text" class="red">мікросервіси</span>

### ... Чому?

+++

### Як і завжди є трейдофи

<img src="/slides/05-tech-stack/microservice-tradeoffs-fowler.jpg">

+++

- Складність в налагоджені інфраструктури
- Проблема налагодження комунікації (залежності сервісів один від одного)
- Простота в розширені та масштабуванні

+++

### Golang для написання серверної частини

- Простий у вивчені
- Підходить для написання легковісних сервісів

+++

<img src="/slides/05-tech-stack/protobuf_logo.png">

## [Protobuf](https://developers.google.com/protocol-buffers/docs/proto3) по HTTP для обміну інформацією

Рефлексія типу повідомлень | Генерація DTOs для будь-якої мови | Перспектива переходу на gRPC

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

+++

### React для фронтенду

+++
<!-- .slide: data-auto-animate  -->

### А тепер найцікавіше з цієї частини

+++
<!-- .slide: data-auto-animate  -->

### А тепер найцікавіше з цієї частини <!-- .element: class="red" -->
## Як нам перевіряти коректність документів?

На етапі, коли користувач заповнює форму так і на сервері, наприклад при створенні документа чи оновленні реєстру

+++

<img src="/slides/05-tech-stack/json_schema_logo.png">

## [JSON Schema](https://json-schema.org/draft/2020-12/json-schema-validation.html)

Специфікація для JSON структур | Валідація

+++

<img src="/slides/05-tech-stack/json_schema_example.jpg">

+++

### Генерування форми з `JSON Schema`

<img src="/slides/05-tech-stack/json_schema_form.jpg">

https://rjsf-team.github.io/react-jsonschema-form/

+++

### В перспективі `JSON Schema`

- Додавання нових гнучких правил для валідації [(залежить від реалізації специфікації)](https://ajv.js.org/keywords.html)
- Можливість створення окремого сервісу для валідації, що буде перевикористовувати правила на сервері