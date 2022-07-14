## Conventions

+++

### Код в одному модулі

<img style="max-height: 30%" src="/slides/06-conventions/one_module_code.jpg">

+++

### Код в одному файлі

<img style="max-width: 80%; width: 80%" src="/slides/06-conventions/long_file.jpg">

Notes:

- щось типу ~241 тис рядків sqlite

+++

### Context

```golang[1-13|15-45]
// Context as the first argument
func doSomething(ctx context.Context) {
  // Usually that way only some request specific info is passed
  // e.g. user token, transaction...
  // using it as DI container is too much
	anotherCtx := context.WithValue(ctx, "myKey", "anotherValue")
	doAnother(anotherCtx)
}

// Read context value
func doAnother(ctx context.Context) {
	fmt.Printf("doAnother: myKey's value is %s\n", ctx.Value("myKey"))
}

func doSomething(ctx context.Context) {
	ctx, cancelCtx := context.WithCancel(ctx)
	
	printCh := make(chan int)
	go doAnother(ctx, printCh)

	for num := 1; num <= 3; num++ {
		printCh <- num
	}

	cancelCtx()

	time.Sleep(100 * time.Millisecond)

	fmt.Printf("doSomething: finished\n")
}

func doAnother(ctx context.Context, printCh <-chan int) {
	for {
		select {
		case <-ctx.Done():
			if err := ctx.Err(); err != nil {
				fmt.Printf("doAnother err: %s\n", err)
			}
			fmt.Printf("doAnother: finished\n")
			return
		case num := <-printCh:
			fmt.Printf("doAnother: %d\n", num)
		}
	}
}
```

Notes:

- Перший аргумент
- Для передачі значень
- Тільки специфічних для запита

+++

### Дуже сильно опирається на коментарі

<img src="/slides/06-conventions/comments.jpg">

Notes:
- Плагін, що нагадує змінити коментарі при зміні коду біля нього
