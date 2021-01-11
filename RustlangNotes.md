# Notas Rustlang

Rust es un lenguaje de programación compilado, de propósito general y multiparadigma el cual fue desarrollado por Mozilla. Ha sido diseñado para ser un lenguaje seguro, concurrente y practico. Es un lenguaje multiparadigma que soporta programación funcional pura, por procedimientos, imperativa y orientada a objetos.

Rust no tiene garbage collector, en vez de eso usa algo llamado ownership.

El compilador de Rust se llama rustc, el manejador de paquetes de rust es llamado cargo.

Para crear un nuevo proyecto usamos el comando:

```bash
cargo new `proyect_name` --bin
```

Hello world en Rust:

```rust
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

```rust
let x = 5;
x = 2; // ERROR!
```

Las variables en **Rust** son inmutables por defecto, no pueden ser reasignadas a menos que se indique explícitamente.

Para hacer que una variable sea mutable:

```rust
let mut x = 5; // se utiliza la palabra reservada "mut"
```

Rust tiene inferencia de tipos, al declarar

```rust
let x = 5;
```

infiere el tipo de la variable x. Pero también podemos declarar el tipo de una variable explícitamente.

```rust
let x: u32 = 5;
```

Esto es importante debido a como Rust maneja la memoria.

## Datos primitivos

### integers

```rust
// i8, u8, i16, u16, i23, u32, i64, u64
```

Esto esta basado en la cantidad de bytes que el numero esta ocupando en memoria **i8 son 8 bits, i16 son 16 bits etc.**

También están los llamados **size numbers**

```rust
//isize, usize
```

La cantidad de memoria que ocupen estos números estará basada en la computadora en la que se este corriendo. Por ejemplo en una computadora de arquitectura de 64 bits sera por defecto 64 bits pero en una computadora de arquitectura de 32 bits por defecto serán 32 bits.

### floats

```rust
//f32, f64
//32 bits floats, 64 bits floats
//64 bits for double precicion, 32 for single precicion
```

### operadores matematicos

```rust
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

```rust
let c: char = 'c';
```

### tuplas

```rust
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

```rust
let a = [1, 2, 3, 4, 5, 6, 7, 8];
//para acceder a los valores de la array
let a1 = a[0]; //return 1
// cuando se define una array la longitud de esta debe permanecer fija incluso si es una array mutable
```