# Notas Rustlang

Rust es un lenguaje de programación compilado, de propósito general y multiparadigma el cual fue desarrollado por Mozilla. Ha sido diseñado para ser un lenguaje seguro, concurrente y practico. Es un lenguaje multiparadigma que soporta programación funcional pura, por procedimientos, imperativa y orientada a objetos.

Rust no tiene garbage collector, en vez de eso usa algo llamado ownership.

El compilador de Rust se llama rustc, el manejador de paquetes de Rust es llamado cargo.

Para crear un nuevo proyecto usamos el comando:

```bash
cargo new `proyect_name` --bin
```

Hello world en Rust:

```Rust
fn main() {
    println!("Hello, world!");
}
```

Para ejecutar el programa se usa el comando:

```bash
cargo run
```

El comando **cargo run** compila el proyecto y luego lo ejecuta.

Variables en Rust:

```Rust
let x = 5;
x = 2; // ERROR!
```

Las variables en **Rust** son inmutables por defecto, no pueden ser reasignadas a menos que se indique explícitamente.

Para hacer que una variable sea mutable:

```Rust
let mut x = 5; // se utiliza la palabra reservada "mut"
```

Rust tiene inferencia de tipos, al declarar

```Rust
let x = 5;
```

infiere el tipo de la variable x. Pero también podemos declarar el tipo de una variable explícitamente.

```Rust
let x: u32 = 5;
```

Esto es importante debido a como Rust maneja la memoria.

## Datos primitivos

### integers

```Rust
// i8, u8, i16, u16, i23, u32, i64, u64
```

Esto esta basado en la cantidad de bytes que el numero esta ocupando en memoria **i8 son 8 bits, i16 son 16 bits etc.**

También están los llamados **size numbers**

```Rust
//isize, usize
```

La cantidad de memoria que ocupen estos números estará basada en la computadora en la que se este corriendo. Por ejemplo en una computadora de arquitectura de 64 bits sera por defecto 64 bits pero en una computadora de arquitectura de 32 bits por defecto serán 32 bits.

### floats

```Rust
//f32, f64
//32 bits floats, 64 bits floats
//64 bits for double precicion, 32 for single precicion
```

### operadores matematicos

```Rust
let a = 1 + 20;
let s = 30 - 20;
let m = 5 * 10;
let d = 4 / 6;
let r = 49 % 2;
```

### booleanos

```rust
// bool: true/false
```

### caracteres

```Rust
let c: char = 'c';
```

### tuplas

```Rust
//collection of data
let t: (i32, f64, char) = (42, 6.12, 'j');
// no es necesario que sean del mismo tipo
// pueden ser tan grandes como quieras
let (z, y, x) = t; // asignara los valores de la tupla t a z, y, x respectivamente

// si usaramos
let (_, _, x) = t;
//ignorara los primeros dos valores y asignara la 'j' a x

//acceso rapido a valores de una tupla
t.0, t.1, t.2 // return 42, 6.12, 'j'
```

### arrays

```Rust
let a = [1, 2, 3, 4, 5, 6, 7, 8];
//para acceder a los valores de la array
let a1 = a[0]; //return 1
// cuando se define una array la longitud de esta debe permanecer fija incluso si es una array mutable
```

----------------------------------------------------

En Rust podemos poner una tupla dentro de otra tupla

Ej:

```Rust
let t = (1, 'a', false);
let f = (2, t);

//podemos imprimir valores de una tupla asi:
println!("{} {} {}", t.0, t.1, t.2); //return 1, 'a', false

//para imprimir el valor de f.1 el cual es la tupla t debe hacerse de la siguiente manera
println!("{:?}", f.1);
//incluso ahora podríamos llamar a la tupla f completa
println!("{:?}", f); // return (2, (1, 'a', false))
// :? es llamado debug flag

// tambien podemos darle formato al imprimir una tupla de la siguiente manera:
println!("{:#?}", f.1);
/*
    return:
    (
        2,
        (
            1,
            'a',
            false
        )
    )
*/

let t = (1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13);

println!("{:?}", t); //ERROR
//si la tupla es muy larga no funciona

//también podemos crear tuplas vaciás

let t = ();

//Las funciones que no retornan nada técnicamente están retornando un tupla vaciá
```

----------------------------------------------------

podemos dar a las arrays tipos

```Rust
let xs: [i32; 5] = [4,5,6,7,8]; // esto estamos diciendo que la array esta llena con datos de tipo i32 y que tiene una longitud de 5

//tambien podemos acceder a los datos dentro de una array
println!("{}", xs[0]); // return 4
//tambien podemos acceder a la longitud de la array
println!("{} {}", xs[0], xs.len());// return 4 and 5
```

```Rust
use std::mem; //library
fn main() {
    let xs: [i32; 5] = [4,5,6,7,8];
    println!("{} {} {}", xs[0], xs.len(), mem::size_of_val(&xs)); //return 4 5 20
    //lo que hace la biblioteca "mem" es decirnos cuanto espacio ocupa la array en memoria

    //podemos tomar un slice de una array de la siguiente manera
    let ys = &xs[2..4]; //return 6 and 7
    // desde el valor 2 hasta el valor 4 excluyendo este ultimo

    println!("{} {}", ys[0], ys[1]);

    println!("{:?} {:?}", ys, xs); // return [6,7] [4,5,6,7,8]
}
```

## Strings

```Rust
//En Rust no es lo mismo declarar un string de esta manera:
let s = "String"; // slice of string, type str
//que declararla de esta otra manera
let ss = String::from("String!"); // type String

//las string no son técnicamente tipos literales en Rust, en vez de eso son mas como arrays o tuplas en el sentido de que están compuestas por diferentes caracteres

//podemos convertir la variable s en una string de la siguiente manera

let s = "String".to_string();

//podemos tomar un slice de una string de la siguiente manera
let  slice = &ss[0..4]; 
println!("{}", slice); //return Stri

//tambien podemos concatenar strings
let h = String::from("Hello, ");
let w = String::from("World!");
let s = h + &w;
println!("{}", s);// return Hello, World!
```