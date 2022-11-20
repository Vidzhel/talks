## Dependency management

+++

## Go mod

```golang
// Full path to the project is the name of the project
// go get github.com/user/project_name - use dependency
// go install github.com/user/project_name - compile and install into PATH
module github.com/user/project_name

go 1.18

...
```

Notes:

- Назва проєкту - повний шлях
- Щоб знати звідки тянути залежність
- Зміна місця - змінюється назва

+++

```golang
...
require (
	github.com/gin-gonic/gin v1.7.7 // <- version pinning
	golang.org/x/crypto v0.0.0-20220411220226-7b82a4e95df4 // <- comit pinning
	github.com/stretchr/testify v1.7.1 // <- test dependencies
)

// Transitive dependency
require (
	github.com/caarlos0/env/v6 v6.9.1 // indirect
	...
```

Notes:

- Пінінг версій (тегом, комутом)
- Привіт node, ніяких npm ci
- Немає реєстру, як в npm, є тільки proxy (private repo)
- vendoring
- Транзитивні залежності
- Тестові залежності
