# Funciones

- Las funciones de declaran con la palabra reservada **func** seguido del nombre de la funcion **()** y las llaves que encierran el dominio de la funcion.
    ```go
    package main

    import "fmt"

    func <nombre-de-funcion> () {}
    func main() {}
    ```
- La funcion puede estar abajo o arriba de la funcion principal **(func main())**.
- En las funciones toda variable creada o mandada hacia esta se queda dentro de ese **scope**.
- Asi como en otros lenguajes las funciones pueden ser pasadas como valor a una variable
    ```go
    package main

    import (
        "fmt"
        "math"
    )

    func main() {
        hypot := func(x, y float64) float64 {
            return math.Sqrt(x*x + y*y)
        }

        fmt.Println(hypot(3, 4))
    }
    ```

## Funciones con parametros
- A las funciones les podemos pasar parametros, los cuales deben ser establecidos junto con la funcion.
    ```go
    package main

    import "fmt"

    func saludo(nombre string) {
        fmt.Printf("Hola %s", string)
    }

    func main() {
        saludo("Cristian")
    }
    ```

## Funciones que retornan valores
- **OJO**: cuando retornamos valores podemos ponerle nombres a los valores que devolveremos y con esto las estariamos **inicializando**, asi que cuando tengas que retornar podrias solo escribir **return** y te devolveria las variables que especificaste automaticamente.
```go
package main

import (
    "fmt"
    "errors"
)

func main(){
    valor := devuelveValor()
    fmt.Println(valor)
}

func devuelveValor(val int) (resultado int){
    resultado = 124
    return
}
```

- Para retornar debemos usar la palabra reservada **return**
- Debemos decir de dato queremos devolver como se vera a continuacion
    ```go
    package main

    import "fmt"

    func main() {
        sum := suma(3,5)
        fmt.Println(sum)
    }
    func suma (x int, y int) int {
        return x + y
    }

    ```
- Tambien podemos recibir un tipo y devolver otro completamente diferente
    ```go
    func esMenor(edad uint8) bool {
        if edad < 20 {
            return true
        }else {
            return false
        }
    }
    ```


## Funciones que retornan multiples valores

- Por lo cual devuelve un slice []**tipo-de-dato**. Y tambien cuando asignes variables a retornar no olvidar encerrar los valores entre **parentesis ()**.
    ```go
    package main

    import "fmt"

    func main() {
        num, ok = isPositive(12)
    }

    func isPositive(integer int)  (int, bool) {
        if integer > 0 {
            return integer,true
        }
    }
    ```
- Como en otros lenguajes aceptan listas y diccionarios. Go acepta pasar slices

## Closures

- Es posible crear funciones dentro de otras funciones o que retornen otras funciones.
    ```go
    func main() {
        add := func (x, y int) int {
            return x + y
        }
        fmt.Println(add(12,61))
    }
    ```

## Recursion

- Y una funcion tambien se puede llamar a si misma
    ```go
    func main() {
        num := fact(5)
        fmt.Println(num)
    }

    func fact(x int) int {
        if x == 0 {
            return 1
        }
        else {
            return x*fact(x-1)
        }
    }
    ```



## Defer (Aplazamiento)

- Go tiene una declaracion especial llamada **defer** la cual **administra** una llamada de funcion **(function call)** para correr luego de que la funcion se completa. (Como en js cuando se hacen funcion asincronas). **ejemplo en defer.go**
    ```go
    package main

    import "fmt"

    func first() {
    fmt.Println("1st")
    }

    func second() {
        fmt.Println("2nd")
    }

    func main() {
        defer second()
        first()
    }
    ```
- Ese programa imprime **1st** seguido de **2nd**. Basicamente, **defer** mueve la llamada de **second** al final de la funcion.
    ```go
    func main() {
        first()
        second()
    }
    ```
- **defer** se suele usar cuando queremos que los recursos se liberen en cierta manera. Por ejemplo cuando abrimos un archivo, nosotros queremos que se cierre siempre y cuando se acabe el programa. **con defer**
    ```go
    func main() {
        f, _ := os.Open(filename)
        defer f.close
    }
    ```
- Esto tiene 3 ventajas:
    - Mantiene nuestro **Close call** cercana a nuestra **Open call** para que sea facil de entender.
    - Si nuestra funcion retorna multiples valores, **Close** correra despues de que estas han sido devueltas.
    - Las funciones diferidasse ejecutan incluso si se produce un **panic** en el tiempo de ejecucion.

## Panic and Recover

- La funcion **PANIC** causa un **runtime error**. Y nosotros podemos manejarla de la manera que queramos con la funcion preconstruida **RECOVER**. **Recover** detiene el **panico** y retorna el valor que fue pasado al **panic()**. Con un ejemplo como este.
    ```go
    package main

    import "fmt"

    func main() {
        panic("PANIC")
        str := recover() // this will never happen
        fmt.Println(str)
    }
    ```
    - Devuelve
        ```console
        panic: PANIC

        goroutine 1 [running]:
        main.main()
	    /home/cris19/Studying/Golang/from-cero-to-hero/04_funciones/panic.go:6 +0x39
        exit status 2
        ```
- Podemos ver como actua la funcion **panic**.
- Pero tambien hay una linea **str := recover()** y porque no se ejecuto ?. Porque la funcion panic inmediatamente detiene la ejecucion de la funcion principal **(func main)**
    ```go
    package main

    import "fmt"

    func main() {
        defer func() {
        str := recover()
        fmt.Println(str)
        }()
        panic("PANIC")
    }
    ```
## Funciones Anonimas

- Las **funciones anonimas** en **GO** som un **tipo de dato**.
- Mantiene disponible el valor de sus variables para la siguiente ejecucion de si misma. Y una funcion anonima puede tambien **como es un tipo de dato** asignarse a una variable. Incluso puede ser autoejecutada tal como otros lenguajes de programacion.
    ```go
    package main

    import "fmt"

    func main(){
        func (){
            fmt.Println("YO me llamo a mi mismo")
        }()

        imprimir := func (){
            fmt.Println("Me imprimo yo solito")
        }

        imprimir() // devuelve el msj
        fmt.Printf("%T\n", imprimir)
    }
    ```

- El tipo de dato que devuelve es **func()**.

## Funcion que retorna una funcion anonima

- Aqui un ejemplo
    ```go
    package main

    import "fmt"

    func main(){
        sec1 := sec()
        // sec1 es una funcion
        fmt.Println(sec1()) // devuelve 1
        fmt.Println(sec1()) // devuelve 2
        fmt.Println(sec1()) // devuelve 3
        fmt.Println(sec1()) // devuelve 4
        fmt.Println(sec1()) // devuelve 5
        fmt.Println(sec1()) // devuelve 6
        // y se puede ver que la varaiable "x" dentro de la funcion aun existe y aumenta
    }

    // la funcion sec retorna func() que retorna un int
    func sec() (func() int ) {
        var x int
        return func() int {
            x++
            return x
        }
    }
    ```
