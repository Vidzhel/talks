## Language

+++

### Дуже швидка у вивчені мова

<div class="aside">
  <img src="/slides/01-language/go_tour.jpg">
  <img src="/slides/01-language/go_by_example.jpg">
</div>

Note:

- 2 з половиною концепції
- Пройти вивчення за вечір
- Go Tour, Go by Example

+++

### Порівняємо з Rust, Java

<div class="aside">
  <img src="/slides/01-language/rust_by_example.jpg">
  <img src="/slides/01-language/java_docs.jpg">
</div>


Notes:
- Rust - цілий том коду
- Java - outdated все + спробу знайди, зазвичай вивчається на інших ресурсах
- C# - все добре
- JS - Modern JS Book

+++

### Структури

```golang
// Just a bag with fields, no behaviour
type Cron struct {
	entries   []*Entry
	chain     Chain
	stop      chan struct{}
	...
}
```

Notes:
- Які програмісти, якщо не подивимося код
- C подібно
- Приватні поля

+++

### Білдери

```golang[|16-30]
// No constructors... no problems
func New(opts ...Option) *Cron {
	c := &Cron{
		entries:   nil,
		chain:     NewChain(),
		add:       make(chan *Entry),
		...
	}
	// Modify struct with options
	for _, opt := range opts {
		opt(c)
	}
	return c
}

type Option func(*Cron)

// WithLocation overrides the timezone...
func WithLocation(loc *time.Location) Option {
	return func(c *Cron) {
		c.location = loc
	}
}

// WithSeconds overrides the parser ...
func WithSeconds() Option {
	return WithParser(NewParser(
		Second | Minute | Hour | Dom | Month | Dow | Descriptor,
	))
}
```

Notes:
- Немає конструкторів
- Конструктори обмежені (async constructor JS)
- Тому все одно створюємо білдери

+++

### Методи
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

```golang
// Simple function with receiver, just like `self` in python
func (c *Cron) AddJob(spec string, cmd Job) (EntryID, error) {
	schedule, err := c.parser.Parse(spec)
	if err != nil {
		return 0
	}
	return c.Schedule(schedule, cmd)
}
```
<!-- .element: data-id="code" -->

Notes:
- Мають тип ресіверу (поінтер або ж ні) - як в пітоні self
- Декларація в тому ж пекеджі

+++

### Методи
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" -->

```golang
// Simple function with receiver, just like `self` in python
func (c *Cron) AddJob(spec string, cmd Job) (EntryID, error) {
	schedule, err := c.parser.Parse(spec)
	if err != nil {
		return 0
	}
	return c.Schedule(schedule, cmd)
}

// No overloading? Give more concise name!
func (c *Cron) AddFunc(spec string, cmd func()) EntryID {
  ...
}
```
<!-- .element: data-id="code" -->

Notes:
- Мають тип ресіверу (поінтер або ж ні) - як в пітоні self
- Декларація в тому ж пекеджі

+++

### Аліаси

```golang
type itemsSlice []string

// aliases are types just like structs!
func (a itemsSlice) print() {
   for i, app := range a {
     fmt.Println(i, app)
   }
}
```

Гнучкість типів в інших мовах:
- [Type-Driven API Design in Rust](https://www.youtube.com/watch?v=bnnacleqg6k)
- [Domain Modeling Made Functional (F#)](https://www.youtube.com/watch?v=1pSH8kElmM4)

Notes:

- ДДД Value objects
- Guid за типом сутності
- Спрощує моделювання сутностей
- Не так класно як в TS (гнучкість), Rust,

+++

### ~~Наслідування~~ Композиція
<!-- .slide: data-auto-animate data-auto-animate-duration="0.5" data-auto-animate-restart -->

```golang[1-13|15-23|25-34]
type Base struct {
  b int
}

type Container struct {     // Container is the embedding struct
  // Can also be *Base
  Base                      // Equal to `Base Base`
  c string
}

func (base *Base) GetString() string {
	return base.b
}

func BasePrinter(base *Base) {
	fmt.Println(base.GetString())
}

c := Container{Base: &Base{b: "HI"}}
fmt.Println(c.GetString()) // HI
// Not inheritence!!
// BasePrinter(c) <--- Error cannot use c (variable of type *Container)...
BasePrinter(c.Base) // HI

func (base Base) Name() string {
	return "Base"
}

func (base Container) Name() string {
	return "Container"
}

c := Container{Base: Base{b: "HI"}}
fmt.Println(c.Name()) // Container
```

[Поклацати в Go playground](https://go.dev/play/p/3x4bJ1QNACM)

+++

### Інтерфейси 💪

```golang[1-3|5-19|21-30]
type Shape interface {
    Area() float64
}

type Rectangle struct {
    Width, Height float64
}

type Circle struct {
    Radius float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

func getArea(shape Shape) {
    fmt.Println(shape.Area())
}

r := Rectangle{Width: 7, Height: 8}
c := Circle{Radius: 5}

// Implicit interafces WORK! 
getArea(r)
getArea(c)
```

Notes:

- Implicit 
- Duck typing

+++

### Ф-ції - first class citizens

```golang[1-6|7-11|13-26]
// Anonymous
a := func() {
    fmt.Println("hello world first class function")
}
a()
fmt.Printf("%T", a) // func()

// Fire and forget
func(n string) {
    fmt.Println("Welcome", n)
}("Gophers")

// add - function type-aslias
type add func(a int, b int) int

func (f add) Add() int {
	return f(1, 2)
}

func implementation(a int, b int) int {
	return a + b
}

func main() {
	fmt.Println(add(implementation).Add()) // 3
}

// defer will back you up
func write(fileName string, text string) error {
	file, err := os.Create(fileName)
	defer file.Close()
	if err != nil {
		return err // <- will close here
	}
	_, err = io.WriteString(file, text)
	if err != nil {
		return err // <- Here
	}
	
	return nil // <- And here
}
```

Notes:

- Повертає декілька значень
- Немає значення за замовчуванням

+++

### Теги полів

```golang[1-7|9-25]
// Tags can be retrieved through reflection
type User struct {
	Name          string    `json:"name"` <- tag
	Password      string    `json:"password"`
	PreferredFish []string  `json:"preferredFish"`
	CreatedAt     time.Time `json:"createdAt"`
}

// Have you tried to fit whole app config in one struct? This guy tired
// Actualy like Attributes like ones in C# more...
var opts struct {
	Listen            string   `short:"l" long:"listen" env:"LISTEN" description:"listen on host:port (default: 0.0.0.0:8080/8443 under docker, 127.0.0.1:80/443 without)"`
	MaxSize           string   `short:"m" long:"max" env:"MAX_SIZE" default:"64K" description:"max request size"`
	GzipEnabled       bool     `short:"g" long:"gzip" env:"GZIP" description:"enable gz compression"`
	ProxyHeaders      []string `short:"x" long:"header" description:"outgoing proxy headers to add"` // env HEADER split in code to allow , inside ""
	DropHeaders       []string `long:"drop-header" env:"DROP_HEADERS" description:"incoming headers to drop" env-delim:","`
	AuthBasicHtpasswd string   `long:"basic-htpasswd" env:"BASIC_HTPASSWD" description:"htpasswd file for basic auth"`
	...
```

Notes:

- Крім джейсону, для конфігурації

+++

### Generics💥

```golang
// Custom constraint
type Number interface {
    int64 | float64
}

// SumNumbers sums the values of map m. Its supports both integers
// and floats as map values.
func SumNumbers[K comparable, V Number](m map[K]V) V {
    var s V
    for _, v := range m {
        s += v
    }
    return s
}
```

[Методи не можуть декларувати змінні типи (Go playground)](https://go.dev/play/p/JQSScOOUPXG?v=gotip)</br>
[Немає варіативності типів (Generic type variance)](https://go.dev/play/p/-cTavIeVV4U)

Notes:

- 10 років, а вони тільки уламали розробників на 
- Стабільність, якщо набридло вчити нові фічі

+++

<img src="/slides/01-language/no_generics.jpg">

Notes:

- До дженеріків
- З серйозним лицем писали ось таке

+++

### Billion dollar mistake також тут
<!-- .element: class="red" -->
## Nil - фіча а не баг🤦

<iframe src="https://giphy.com/embed/vyTnNTrs3wqQ0UIvwE" width="480" height="400" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

+++

```golang[1-12|13-20]
type Person struct {
	name string
}

// Nil also can be called
func (p *Person) Name() string {
	if p == nil {
		return "None"
	}

	return p.name
}

func main() {
	defaultInit := Person{name: "John"}
	fmt.Println(defaultInit.Name()) // John

	var empty *Person = nil
	fmt.Println(empty.Name()) // None ALSO WORKS!
}
```

+++

### Але частіше ти бачиш ось це

<img style="max-width: 80%; width: 80%;" src="/slides/01-language/nil_panic.png">

