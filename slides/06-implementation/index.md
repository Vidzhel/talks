## Перша версія

<img src="slides/06-implementation/v1.jpg">

Note:
<img width="100%" src="slides/06-implementation/v1_note.jpg">
+++

#### Рефлексія <!-- .element: class="grey" -->

### Помилки <!-- .element: class="red" -->

- Зв'язаність сервісів
- Перевикористання моделей одного сервісу іншими
- Чисто SQL підхід до роботи з базою (міграції та самі SQL запити)
- Писання стабів для тестування вручну, замість використання генератора моків

Note:
<img width="100%" src="slides/06-implementation/v1_note.jpg">
+++

#### Рефлексія <!-- .element: class="grey" -->

### Успішні рішення <!-- .element: class="green" -->

Note:
<img width="100%" src="slides/06-implementation/v1_note.jpg">
+++

### SDK проєкт для спільного коду

<img height="500px" src="slides/06-implementation/service_sdk.jpg">

Note:
<img width="100%" src="slides/06-implementation/v1_note.jpg">
+++

### Hexagonal architecture - допомогла в рефакторингах

Note:
<img width="100%" src="slides/06-implementation/v1_note.jpg">
+++
<!-- .slide: class="aside" -->

1. Інтерфейси репозиторіїв, що відповідають за збереження та отримання агрегатів
2. Самі сутності та агрегати
3. Сервіси, що використовують репозиторії та маніпулюють сутностями
4. Реалізація всіх інтерфейсів з інфраструктурними залежностями
5. Місце підключення усіх залежностей, контроллерів, запуску додатка

<img height="400px" src="slides/06-implementation/hexagonal_architecture.jpg">

Note:
<img width="100%" src="slides/06-implementation/v1_note.jpg">
+++

### CI/CD з допомогою `docker`, `docker-compose` та `github actions`

<img src="slides/06-implementation/github_actions.jpg">
<img src="slides/06-implementation/releases.jpg">

Note:
<img width="100%" src="slides/06-implementation/v1_note.jpg">
+++

## Друга версія

Note:
<img height="60%" src="slides/06-implementation/v2_note.jpg">
+++

### Розв'язання проблеми з залежностями... <!-- .element: class="green" -->

Особливо добре проглядається на більшій кількості сервісів

<img src="slides/06-implementation/dependency_hell.jpg">

Приклад

Note:
<img height="60%" src="slides/06-implementation/v2_note.jpg">
+++

### шляхом використання event-driven підходу <!-- .element: class="green" -->

<div class="aside">

- Не плутати з `event-sourcing`, що дає багато переваг, але значно поскладнює розробку, якщо немає досвіду
- Protobuf over Rabbit Mq
- Івенти зберігаються локально, після чого відправляються cron джобою що запускається кожні декілька секунд

<img src="slides/06-implementation/v2.jpg" width="500px">

</div>

Note:
<img height="60%" src="slides/06-implementation/v2_note.jpg">
+++

#### Покращена робота з БД <!-- .element: class="green" -->
### Forward only підхід мігрування </br> з автоматичним накатуванням при старті

<img src="slides/06-implementation/migrations.jpg">

- Знаємо яка версія
- Автоматично виконуємо міграцію

Note:
<img height="60%" src="slides/06-implementation/v2_note.jpg">
+++

#### Покращена робота з БД <!-- .element: class="green" -->
### Not a simple ~~ORM~~ [__Query builder__](https://github.com/volatiletech/sqlboiler) 🤩!

<div class="aside">

- Scaffold схеми БД. Ніяких більше проблем зі змінами в коді після міграцій
- Гнучкість query builder по зрівнянню з топорнісю ORM, особливо для golang (вони не полюбляють таке)
- Type safety

<img src="slides/06-implementation/query_builder.jpg">

</div>

Note:
<img height="60%" src="slides/06-implementation/v2_note.jpg">

