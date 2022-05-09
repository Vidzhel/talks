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

<img src="/slides/05-implementation/microservice-tradeoffs-fowler.jpg">

+++

- Складність в налагоджені інфраструктури
- Проблема налагодження комунікації (залежності сервісів один від одного)
- Простота в розширені та масштабуванні

+++

### Golang для написання серверної частини

- Простий у вивчені
- Підходить для написання легковісних сервісів

+++

<img src="/slides/05-implementation/protobuf_logo.png">

## Protobuf по HTTP для обміну інформацією

Рефлексія типу повідомлень | Генерація DTOs для будь-якої мови | Перспектива переходу на gRPC

+++

```js
let user = {
    name: "John",
    age: 30,
    "likes birds": true  // имя свойства из нескольких слов должно быть в кавычках
};
```

+++

```protobuf
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
