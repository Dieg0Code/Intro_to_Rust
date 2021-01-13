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

/*las string no son técnicamente tipos literales en 
Rust, en vez de eso son mas como arrays o tuplas en 
el sentido de que están compuestas por diferentes caracteres
*/

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

## Ownership and Borrowing

Si piensas en lenguajes como C++ y C en ellos tienes la habilidad de manejar directa e indirectamente la memoria, estos tienen los punteros y las referencias, pero a la vez hay un poco de peligro en el ser capaz de poder manejar manualmente los punteros y las referencias, si creas dos punteros los cuales apuntan a la misma pieza de data y luego quitas esa referencia puedes causar una corrupción en la memoria, por eso los lenguajes de alto nivel como Java, C#, Python y Ruby usan el conocido "garbage collector" este es en esencia un algoritmo que va por ahi y encuentra todos los espacios libres en memoria y los libera automáticamente. En Rust tenemos algo en el medio, el llamado **"Ownership"**, este sigue 3 reglas.

Cada variable tiene un valor y la variable en si misma es llamada Owner

Ej:

```Rust
let x = 1;//podemos decir que x "Owns" 1
```

Cada pedazo de data puede tener solo un **"Owner"** a la vez

```Rust
//Cuando el owner se sale del scope actual el valor es dropeado(espanglish btw)
let x = 1;
let y = x;

//si creamos un nuevo scope
{
    let a = 10;
}

x + a; //tendríamos problemas con esto porque a no existe en este scope

let s = String::from("String"); // definimos una string
// s owns the string
let y = s; //bind s to y

println!("{}", s); // ERROR use of moved value: `s`
// lo que pasa es que movimos la referencia de s a y
// depues de eso la referencia de s desapareció completamente del scope
//Only one reference can own a piece of data at the time
//Para solucionar esto podemos usar lo que es llamado "Borrowing"
let s = String::from("String");
let y = &s;// y is equals to a reference to s
```

```Rust
fn take(v: Vec<i32>) { //vector of dinamic size
    println!("We took v: {}", v[10] + v[100]);
}
fn main() {
    let mut v = Vec::new();

    for i in 1..1000 { //put the data into the vector
        v.push(i);
    }

    take(v); //transfering the ownership from the main func to the take func
    //never return the ownership back to the main func
    //println!("{}", v[0]);
    println!("Finished");
    //this is what is called moving because we are actually moving the resource from one func to another
}
```

```Rust
//Tambien podemos usar al llamado "coping"
//Ej:
fn cop(a: i32, b: i32) {
    println!("{}", a + b);
} fn() -> ()
fn main() {
    let a = 32;
    let b = 45;

    cop(a,b);//unlike the string values that we haved before, they don't become unallocated in the main function
    //thats beacuse they not actually being moved instead they're being copied

    println!("we have a: {} and b: {}", a, b);
}
```

## Structs, Methods, Functions, Related Functions and the Display/Debug Traits

Si vienes de lenguajes orientados a objetos seguramente encontraras una similitud entre "structs" y los objetos.

```Rust
//Digamos que queremos modelar un objeto que tiene un ancho y un alto

struct Object { //se usaría la keyword struct y la primera letra mayuscula para el tipo
    width: u32,
    height: u32,

}

fn area(obj: &Object) -> u32 {
    obj.width * obj.height // no es estrictamente necesario poner return ya que Rust retornara siempre la ultima linea de una función
}

fn main() {
    let o = Object {
        width: 35,
        height: 55,
    };

    println!("{}x{} with area: {}", o.width, o.height, area(&o));
}
```

```Rust
//Para crear métodos:
impl Object {
    fn area(obj: &Object) -> u32 {
        obj.width * obj.height
    }
}

//Podemos simplificarlo aun mas:
impl Object {

    fn area(&self) -> u32 { //self automáticamente hará referencia al objeto al que esta vinculado
        self.width * self.height
    }

     //Podemos poner los prints en una funcion:
    fn show(&self) {
        println!("{}x{} with area: {}", self.width, self.height, self.area());
    }
}

//Podemos crear otra implementación para tener ahi las funciones relacionados en esta.
impl Object {
    
    //podemos crear lo que es llamado funciones relacionadas:
    fn new(width :u32, height: u32) -> Object {
        Object {
            width,
            height,
        }
    }
}

fn main() {
    let o = Object {
        width: 35,
        height: 55,
    };

    //Para ganar acceso a la función new
    let obj = Object::new(57, 83);//asi podemos usarla como a cualquier otra función

    
    o.show();
    obj.show();

    println!("{:?}", o); //si intentásemos imprimir el objeto con la flag ":?" nos dará un error
    //Para solucionar eso debemos añadir una notación al objeto
    //Ej:
    #[derive(Debug)]
    struct Object {
        width: u32,
        height: u32,
    }
}
//Entonces podríamos imprimir por pantalla sin problemas el objeto
//Sin embargo no podemos imprimir el objeto sin la flag ":?"
println!("{}", o);
println!("{}", obj);

//Para poder imprimir por pantalla el objeto sin la flag ":?" tendríamos que hacer lo siguiente:

use std::fmt;

//luego tendriamos que crear otra "impl":
impl fmt::Display for Object {

    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "({}, {}) and Area: {}", self.width, self.height, self.area());
    }
}
```