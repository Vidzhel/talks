## Performance

Notes:

- Не найважливіша штука
- Четабельність важливіше
- Ми б не писали на node js

+++

### Вказівники

```golang[1-12|14-21]
type person struct {
 name string
}

// Pass a copy
func rename(p person) {
 p.name = "test"
}

p := person{"Richard"}
rename(p) // <- copy and pass
fmt.Println(p.name) // Richard

// pass by reference
func rename(p *person) {
 p.name = "test"
}

p := person{"Richard"}
rename(&p) // no copying
fmt.Println(p)
```

Notes:

- Вказівники звертають увагу на зміну

+++

### Стек

```golang[1-12|14-21]
type person struct {
 name string
}

// stack allocated
func rename(p person) {
 p.name = "test"
}

p := person{"Richard"} // <- stack allocated
rename(p)
fmt.Println(p.name) // Richard

// pass by reference
func rename(p *person) {
 p.name = "test"
}

// heap allocated
p := person{"Richard"}
rename(&p) // no copying
fmt.Println(p)
```

Notes:
- тільки вказівники - не швидше, бо GC

+++

### Горутини


```golang[1-15|17-34]
// Receive 2 channels
// First to get next number in the sequence
// Second to finish
func fibonacci(c, quit chan int) {
	x, y := 0, 1
	for {
		select {
		case c <- x:
			x, y = y, x+y
		case <-quit:
			fmt.Println("quit")
			return
		}
	}
}

func main() {
	c := make(chan int)
	quit := make(chan int)
	
	// Run in another goroutine
	// Generate first 10 numbers than finish the function
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-c)
		}
		quit <- 0
	}()
	
	// Run generator
	// App won't finish until quit is called
	// and fibonacci is returned
	fibonacci(c, quit)
}
```

[Run example in playground](https://go.dev/tour/concurrency/5)

Notes:

- Абстракція над потоками
- Наявні мютекси, атомік операції

+++

### <span class="red">Rust</span> без надоптимізації не дає приросту...

<img src="/slides/05-performance/node_rust_go_comparison.png">

[Відео](https://www.youtube.com/watch?v=Z0GX2mTUtfo)

Notes:

- In/out затримки

+++

## ...скільки він додає до <span class="red">складності</span>

<img src="/slides/05-performance/rust_go_thread.jpg">

[Відео](https://www.youtube.com/watch?v=Z0GX2mTUtfo)

Notes:

- Rust складно писати асинхронний код
