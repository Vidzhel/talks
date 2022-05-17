## Обробка помилок

+++

### 2 шляхи

```golang[1-13|15-36]
// A toy implementation of cube root using Newton's method.
func CubeRoot(x float64) float64 {
    z := x/3   // Arbitrary initial value
    for i := 0; i < 1e6; i++ {
        prevz := z
        z -= (z*z*z-x) / (3*z*z)
        if veryClose(z, prevz) {
            return z
        }
    }
    // A million iterations has not converged; something is wrong.
    panic(fmt.Sprintf("CubeRoot(%g) did not converge", x))
}

// PathError records an error and the operation and
// file path that caused it.
type PathError struct {
    Op string    // "open", "unlink", etc.
    Path string  // The associated file.
    Err error    // Returned by the system call.
}

func (e *PathError) Error() string {
    return e.Op + " " + e.Path + ": " + e.Err.Error()
}
for try := 0; try < 2; try++ {
    file, err = os.Create(filename)
    if err == nil {
        return
    }
    if e, ok := err.(*os.PathError); ok && e.Err == syscall.ENOSPC {
        deleteTempFiles()  // Recover some space.
        continue
    }
    return
}
```

Notes:

- Все дуже погано, треба припинити роботу програми
- Будуть бити по руках, особливо якщо бібліотека
- Panic можна перехватити

- Усвідомлюєш що тобі потрібно обробити помилку
- Ексепшени ти не бачиш, крім Java
- Ф-ціональне програмування

+++

### В результаті

```golang[1-4|6-18]
// Just ignore 
resA, _ := doA()
resB, _ := doB()
resC, _ := doC()

// Exhausted by errors
resA, err := doA()
if err != nil {
  return err
}
resB, err := doB()
if err != nil {
  return err
}
resC, err := doC()
if err != nil {
  return err
}
```