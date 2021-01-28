# Notas Rustlang

##### *fuente original [aquí](https://www.youtube.com/playlist?list=PLJbE2Yu2zumDF6BX6_RdPisRVHgzV02NW)* 🦀🦀🦀

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

    fn area(&self) -> u32 { //self automáticamente hará referencia al objeto al que esta vinculado el método
        self.width * self.height
    }

     //Podemos poner los prints en una funcion:
    fn show(&self) {
        println!("{}x{} with area: {}", self.width, self.height, self.area());
    }
}

//Podemos crear otra implementación para tener ahi las funciones relacionadas.
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

    //writer
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "({}, {}) and Area: {}", self.width, self.height, self.area());
    }
}
```

## Control Flow, Conditionals and Patters Matching

Los operadores condicionales en Rust son:

```Rust
//igual a:
==
//no igual a:
!=
//mayor que:
>
//menor que:
<
//menor o igual que:
<=
//mayor o igual que:
>=
```

### if statement

```Rust
fn main() {
    fn () -> ()

    let n = 2;

    if < 5 {
        println!("true");
    } else {
        println!("false");
    }

    //tambien podemos crear varios else:

    let m = 6;

    if m % 4 == 0 {
        println!("m is divisible by 4");
    } else if m % 3 == 0 {
        println!("m is divisible by 3");
    } else if m % 2 == 0 {
        println!("m is divisible by 2");
    } else {
        println!("m is not divisible by 4, 3, or 2");
    }

    //tambien podemos usar if-else statements en bindings
    //ya que if-else statements son expresiones en Rust
    let c = true;

    //las expresiones if-else siempre deben resolver en un booleano
    let n = if c { //aquí no habría problema ya que c es una variable booleana 
        50
    } else {
        76
    };

    println!("n : {}", n);

    //También hay que saber que si la expresión if-else va a retornar algo deben ambas ser del mismo tipo
    //Ej:

    let c = false;

    let n = if c {
        50
    } else {
        "String" //ERROR! if-else have incompatible types
    }
}
```

### loops

```Rust
fn main() {
    //loop infinito
    loop {
        println!("infinite");
    }

    //tambien podemos usar loops que no son infinitos
    let mut c = 0;

    loop {
        println!("finite");
        c += 1;

        if c >= 10 {
            break;
        }
    }

    //podemos poner labels en los loops

    'a: loop {
        println!("loop a");
        'b: loop {
            println!("loop b");
            'c: loop {
                println!("loop c");

                //lo interesante de los labels es que podemos romper un loop en especifico con "break"
                break

                /*
                    lo que pasaría aquí es que comenzaría corriendo el loop a
                    luego pasaría al loop b, luego al c, en el loop c se saldría
                    y volvería al loop b, luego al c, luego al b y luego al c y asi
                    infinitamente.
                    
                    pero si ponemos:

                    break 'b

                    dentro del loop c. volvería de vuelta al loop a de nuevo, luego al b, luego al c, luego al a, al
                    b, al c, etc
                */

                //También tenemos la keyword "continue":

                if true {
                    //lo que pasaría es que llegaría a 'c' se detendría, evaluaría si es verdadero y continuaría
                    continue
                } else {
                    break
                }
                //se repetiría a,b,c infinitamente
            }
        }
    }

    //También podemos usar loop statements en bindings
    let x = loop {
        break 10; //asignaría el numero 10 a la variable 'x'
    };

    println!("x: {}", x); //return 'x: 10'


    //Tenemos tambien el loop while:
    let mut n = 10;

    while n != 0 {
        println!("{}!", n);

        n = n - 1;
    }

    //For loop:
    let a  = vec![10, 20, 30, 40, 50];
    //iteraría por cada elemento del vector 'a'
    for i in a {
        println!("i: {}", i);
    }

    //podemos hacer que el loop for itere N cantidad de veces:
    for i in 1..101 { //iteraría 100 veces, ya que se están usando dos puntos (1..101) "exclusive range"
        println!("i: {}", i);
    }

}
```

### match statement

```Rust
fn main() {
    let x = 5;

    //es similar al switch de otros lenguajes
    match x {
        1 => println!("one"),
        2 => println!("two"),
        3 => println!("three"),
        4 => println!("four"),
        5 => println!("five"),
        _ => println!("something else"), //debemos tener al menos un case para todos los integers
        //por lo que se usa un "general case _" para todos los demás integers
    }

    let n = 15;
    match n {
        1 => println!("One!"),
        2 | 3 | 5 | 7 | 11 => println!("This is a prime"), //múltiples cases en una linea
        13...19 => println!("A teen"),
        _ => println!("Ain't special"), //case para todos los demás números
    }

    let pair = (0, -2); //tupla
    match pair {
        (0, y) => println!("y: {}", y), //match the tuple based in one value of the tuple | y toma el valor '-2'
        (x, 0) => println!("x: {}", x), //si el segundo valor de la tupla fuera 0 se ejecutaría esta linea
        _      => println!("No match"),
    }

    let pair = (5, -5);
    match pair {
        //toman los dos valores de la tupla y luego usan el if statement
        (x, y) if x == y => println!("Equal"),
        (x, y) if x + y == 0 => println!("Equal zero"), //se ejecutaría esta linea
        (x, _) if x % 2 == 0 => println!("X is even"),
        _ => println!("No match"),
    }

    let p = 5;
    match p {
        //evalua si el numero '5' esta en el rango 
        n @ 1 ... 12 => println!("n: {}", n), // '5' es asignado a 'n'
        n @ 13 ... 19 => println!("n: {}", n),
        _ => println!("no match"),
    }

    //tambien podemos usar las match statements como expresions
    let p = 5;

    let n = match p {
        n @ 1...12 => n,
        n @ 13...19 => n,
        _ => 0,
    };

    println!("n: {}", n);
}
```

## Enums and Options

```Rust
// Enumerations es otra manera de crear tipos personalizados en Rust, son similares a las "structs"
// pero nos permiten hacer múltiples diferentes variaciones en un tipo

//esta notación sirve para que no salten los warnings por variables que están declaradas pero no usadas
#![allow(dead_code)]

#[derive(Debug)]
enum Direction {
    Up(Point),
    Down(Point),
    Left(Point),
    Right(Point),
}

#[derive(Debug)]
enum Keys {
    UpKey(String),
    DownKey(String),
    LeftKey(String),
    RightKey(String),
}

impl Direction {
    fn match_direction(&self) -> Keys {
        match *self {
            Direction::Up(_) => Keys::UpKey(String::from("Pressed w")),
            Direction::Down(_) => Keys::DownKey(String::from("Pressed s")),
            Direction::Left(_) => Keys::LeftKey(String::from("Pressed a")),
            Direction::Right(_) => Keys::RightKey(String::from("Pressed d")),
        }
    }
}

//Digamos que queremos sacar el valor de la "String" del tipo "Key"
impl Keys {
    fn destruct(&self) -> &String {
        match *self {
            Keys::UpKey(ref s) => s,
            Keys::DownKey(ref s) => s,
            Keys::LeftKey(ref s) => s,
            Keys::RightKey(ref s) => s,
        }
    }
}

#[derive(Debug)]
struct Point {
    x: i32,
    y: i32,
}

enum Shape {
    Rectangle {width: u32, height: u32},
    Square(u32),
    Circle(f64),
}

impl Shape {
    fn area(&self) -> f64 {
        match *self {
            //Podría considerarse como polimorfismo 
            Shape::Rectangle {width, height} => (width * height) as f64,
            Shape::Square(ref s) => (s * s) as f64,// los casteos a f64 son porque la función retorna un f64
            Shape::Circle(ref r) => 3.14 * (r * r),
        }
    }
}

fn main() {
    let r = Shape::Rectangle{width: 10, height: 70};
    let s = Shape::Square(10);
    let c = Shape::Circle(4.5);

    let ar = r.area();
    println!("{}", ar);

    let aq = s.area();
    println!("{}", aq);

    let ac = c.area();
    println!("{}", ac);

    let u = Direction::Up(Point {x: 0, y: 1});
    let k = u.match_direction();
    let x = k.destruct();

    println!("{:?}", k);// return UpKey("Pressed w")
    println!("{:?}", x);// Pressed w

    //ref keyword
    let u = 10;
    let v = &u;// v es igual a una referencia a la variable u
    let ref z = u;// esto seria equivalente a la linea anterior

    if z == v {
        println!("they are equals");
    }


}
```

### options

```Rust
//Rust no tiene el concepto de null en vez de eso tiene algo llamado "options"
enum Option<T> {
    Some(T),
    None,
    //si pasamos algo a este "enum", este decidirá si lo que pasamos tiene algo dentro o no
}

//checa si se esta dividiendo por 0
fn division(x: f64, y: f64) -> Option<f64> {
    if y == 0.0 {
        None
    } else {
        Some(x / y)// sino es asi ejecuta la division
    }
}

fn main() {
    let res = division(5.0, 7.0);
    match res {
        Some(x) => println!("{:.10}", x),// ":.10" determina la cantidad de decimales que se quieren mostrar
        None => println!("cannot divide by 0"),
    }

    //Si tuviésemos un valor el cual potencialmente podría ser nulo deberíamos usar un "Option"
}
```

## Vectors, HashMaps, Casting, If-Let, While-Let, and the Result Enum

```Rust
fn main() {
    //vector, los vectors son arrays que pueden cambiar de tamaño(resizable arrays)
    let x = vec![1,2,3,4];
    // un vector esta representado por tres piezas de data, un pointer a la data, la capacidad(cuanta memoria ocupa)
    // y su longitud

    //podemos crear vectores con el método new:
    let mut v = Vec::new();

    //pone un nuevo valor en el vector
    v.push(5);
    v.push(6);
    v.push(7);
    v.push(8);

    for i in &v {
        println!("{}", i);
    }

    // Los vectores solo pueden tener solo un tipo de valor dentro de ellos

    println!("{:?} {} {}", &v, v.len(), v.capacity()); //return '[5,6,7,8] 4 4'
    // pero si ponemos otro valor dentro del vector
    v.push(10);
    // el valor que retornará será '[5,6,7,8,10] 5 8'
    // esto es porque la capacidad va creciendo en múltiplos de 4 (4,8,16,32,etc)

    // pop the last value of the vector
    println!("{:?}", v.pop()); //return "Some(10)"

    // tipos de datos en un vector
    let mut v: Vec<i32> = Vec::new();

    // we can also embed a enum inside of a vector
    #[derive(Debug)]
    enum Example {
        Float(f64),
        Int(i32),
        Text(String),
    }

    fn main() {
        let r = vec![
            Example::Int(142),
            Example::Float(12.32),
            Example::Text(String::from("string")),
        ];

        println!("{:?}", &r);// return '[Int(142), Float(12.32), Text("string")]' 
    }
}
```

### HashMaps

```Rust

// The type HashMaps store a mapping of keys map to a value
// en python los HashMaps vendrían siendo los diccionarios
use std::collections::HashMap;

fn main() {
    let mut hm = HashMap::new();

    // al igual que con los vectores los tipos de datos almacenados deben ser consistentes
    hm.insert(String::from("random"), 12); // key type String, value type int
    hm.insert(String::from("string"), 49); // si pusiese un float aquí daría error

    for (k, v) in &hm {
        println!("{}: {}", k, v);
    } // return 'random: 12'
      //        'String: 49'

    // podemos usar la función get para ganar acceso a un elemento dentro del HashMap
    match hm.get(&String::from("random")) { // lo que retorna la función get es un Option por eso debemos usar un match
        Some(&n) => println!("{}", n), // return '12'
        _ => println!("no match"),
    }

    // podemos usar la función remove para eliminar no solo la key sino tambien el value
    hm.remove(&String::from("string")); // la key esta siempre asociada al value, por eso se eliminan los dos

}
```

### If-Let

```Rust
fn main() {

    let s = Some('c');

    // forma verbosa
    match s {
        Some(i) => println!("{}", i),
        _ => {}
    }

    // forma menos verbosa
    if let Some(i) = s {
        println!("{}", i);
    }
}
```

### While-Let

```Rust

    // forma verbosa
    let mut s = Some(0);

    loop {
        match s {
            Some(i) => if i > 19 {
                println!("Quit");
                s = None;
            } else {
                println!("{}", i);
                s = Some(i + 2);
            },
            _ => {
                break;
            }
        }
    }

    //forma menos verbosa de hacer lo anterior
    while let Some(i) = s {
        if i > 19 {
            println!("Quit");
            s = None;
        } else {
            println!("{}", i);
            s = Some(i + 2);
        }
    }

```

### Casting

```Rust
    // Rust provee algo llamado conversion de datos no implícita (non implicit type conversion) o coercion
    // entre datos primitivos en ves de conversion explicita (explicit conversion) o casting

    let f = 24.4321_f32;
    let i = f as u8;
    let c = i as char;

    println!("{} {} {}", f, i, c);

    // si queremos convertir un int en un char solo podemos hacerlo desde el 0 hasta el 255
    println!("{}", 255 as char);
```

### Result Enum

```Rust

// Por lo general es usados para checar si hay errores
enum Result<T, E> {
    Ok(T),
    Err(E),
}

// Ej de Result Enum
use std::fs::File;

fn main() {
    let f = File::open("test.txt");

    let f = match => {
        Ok(file) => file,
        Err(error) => {
            panic!("There was a problem opening the file: {:?}", error)
        },
    };
}
```

## Traits and Generic Types

```Rust
// Son una forma de definir un comportamiento compartido entre múltiples diferentes sets de datos
// los "Traits" son muy similares a lo que vendría siendo las "interfaces" en otros lenguajes
// nos permiten definir como una función debería verse, tambien nos permiten implementar esa función en varios tipos de datos diferentes.

trait Shape {
    fn area(&self) -> u32;
}

struct Rectangle {
    x: u32,
    y: u32,
}

struct Circle {
    radius: f64,
}

impl Shape for Rectangle {
    fn area(&self) -> u32 {
        self.x * self.y
    }
}

impl Shape for Circle {
    fn area(&self) -> u32 {
        (3.141 * self.radius * self .radius) as u32
    }
}

fn main() {
    let c = Circle {radius: 100.132};
    let r = Rectangle {x: 30, y: 20};
    println!("{} {}", c.area(), r.area());
}

// tambien tenemos las "Dirive annotations" las cuales nos permiten implementar varios "traits" diferentes
// el compilador es capaz de proveer implementaciones básicas para "traits" específicos.

#[derive(Debug, Clone, Copy)]
struct A(i32);
struct B(i32);

// "Clone" es interesante porque nos da un poco mas de control sobre como las variables funcionan dentro
// del sistema de "Ownership"

fn main() {
    let a = A(32);
    let b = B(12.13);
    let c = a;
    println!("{:?}", a); //daría error en esta linea porque y 'a' ya no referencia a 'A(32);'
    // para solucionar el error podemos escribir a.clone() ya que estamos usando la "annotation" 'Clone'
    // quedaría asi:
    let c = a.clone();// asi clonamos la variable 'a' para que siga haciendo referencia a 'A(32);'
    // aunque tambien podemos hacerlo de otra manera con la anotación 'Copy'
    // lo que significa esta anotación es que cada vez que una función o un variable pida prestado un dato
    //  este sera automáticamente copiado
    // asi ya no es necesario usar:
    let c = a.clone();
    // sino que simplemente podemos escribir:
    let a = A(32);
    let c = a;
    println!("{:?}", a);// y no nos dará ningún error

    // tambien tenemos diferentes 'traits' como:
    // #[derive(Eq, PartialEq, PartialOrd, Ord)] 
}
```

Podemos usar los "traits" para sobrecargar operadores:

```Rust
 use std::ops;

    struct A;
    struct B;
    #[derive(Debug)]
    struct AB;
    #[derive(Debug)]
    struct BA;

    impl ops::Add<B> for A {
        type Output = AB;

        fn add(self, _rhs: B) -> {
            AB
        }
    }

    impl ops::Add<A> for B {
        type Output = BA;

        fn add(self, _rhs: A) -> BA {
            BA
        }
    }

    fn main() {
        println!("{:?}", A + B);
        println!("{:?}", B + A);
        // si intentase sumar 'A' con 'A' o 'B' con 'B' me daría error porque no implemente esas sumas
        // esto puede ser util cuando queremos sobrecargar los operadores básicos
    }
```

Drop:

```Rust
// Drop nos permite especificar al compilador cuando queremos que dropee un valor de la memoria
struct A {
    a: String,
}

impl Drop for A {
    fn drop(&mut self) {
        println!("dropped {}", self.a)
    }
}

// lo interesante de Drop es que sera llamado automáticamente cuando una variable sea dropeada
// incluso si nosotros no lo usamos explícitamente sera llamado

fn main() {
    let a = A{a: String::from("A")};
    {
        let b = A{a: String::from("B")};
        {
            let c = A{a: String::from("C")};

            println!("leaving inner scope 2");
        }
        println!("leaving inner scope 1");
    }
    drop(a); // especifica que queremos dropear 'a' antes de que termine el programa
    println!("program ending");
}
```

Iterator:

```Rust
struct Fib {
    c: u32,
    n: u32,
}

// Se usa para implementar 'iteradores' en colecciones como arrays, etc.
impl Iterator for Fib {
    type Item = u32;

    fn next(&mut self) -> Option<u32> {
        let n = self.c + self.n;
        self.c = self.n;
        self.n = n;

        Some(self.c)
    }
}

// tenemos varios métodos que podemos usar en nuestros 'iteradores'

fn fib() -> Fib {
    Fib{c: 1, n: 1}
}

fn main() {
    for j in fib().take(10) {
        println!("{}", j);
    }

    for j in fib().skip(14).take(10) {// skip, evita los primeros 14 elementos de la colección, luego toma los primeros 10 y itera sobre ellos
        println!("{}", j);
    }

    let mut f = fib();

    println!("{:?}", f.next());
    println!("{:?}", f.next());
    println!("{:?}", f.next());
    println!("{:?}", f.next());
    println!("{:?}", f.next()); 
}
```

Generics

```Rust
// lo que permiten los generics es generalizar el tipo de dato de una 'struct' especifica
struct Square<T> {
    x: T,
}

fn main() {
    let s = Square{x: 10};
    let s = Square{x: 1.0};
    let s = Square{x: "Hello"};
    let s = Square{x: 'c'};
}
// los 'generics' son muy útiles cuando queremos reducir los duplicados en el código
```

Podemos usar "generics" dentro de funciones:

```Rust
use std::fmt;
fn p<T: fmt::Debug>(x: T) { // esta función podría imprimir cualquier tipo de dato
    println!("{:?}", x);
}

// tambien podemos usar 'generics' dentro de 'impls'
struct A<T> {
    x: T,
}

impl <T> A<T> { // el 'generic' esta escrito dos veces aquí porque 'A<T>' hace referencia al 'struct' y la <T> es el 'generic' de este bloque
    fn item(&self) -> &T {
        &self.x
    }
}

// podemos usar 'generics' para definir 'patterns' para 'structs' (aguante el Espanglish)
struct A<U, V> { //debido a que hay dos 'generics' las variables 'x', 'y' pueden tener diferentes tipos de datos
    x: U,
    y: V,
}

struct B<V> {// pero a  quí ya que hay solo un 'generic' ambos deben tener el mismo tipo de dato, cualquier tipo, pero ambas el mismo
    x: V,
    y: V,
}

fn main() {
    p(10);
    p(String::from("String"));

    let a = A{x: "Hello"};

    a.item();
}
```

```Rust
use fmt::ops::Mul;

trait Shape<T> {
    fn area(&self) -> T;
}

struct Rectangle <T: Mul> {
    x: T,
    y: T,
}
/* 
impl <T: Copy> Shape<T> for Rectangle<T>
    where T: Mul<Output = T>, {
        fn area(&self) -> T {
            self.x * self.y
        }
    
} 
*/

//otra forma de escribir la 'impl' anterior es:
impl <T:Mul<Output = T> + Copy> Shape<T> for Rectangle<T>
{
    fn area(&self) -> T {
        self.x * self.y
    }
}

fn main() {
    let r = Rectangle {x: 10, y: 20};
    let r2 = Rectangle {x: 10.10, y: 20.31};

    println!("{} {}", r.area(), r2.area());
}
```

## Closures, the Box Pointer and Iterators

Un pointer es una variable que en vez de contener un dato, contiene una dirección en memoria, en esencia un pointer apunta hacia algún otro dato.
Los "smart pointers" por otro lado son estructuras de datos que actúan como pointers pero tienen metadata adicional. El "smart pointer" que vamos a ver es "the box smart pointer", es el mas util que tiene Rust. Este nos permite asignar datos al ["Heap"](https://codingornot.com/diferencias-entre-heap-y-stack) cada dato primitivo en Rust esta asignado automáticamente al ["Stack"](https://codingornot.com/diferencias-entre-heap-y-stack) pero si lo envolvemos/usamos una "box" estará asignado al **Heap**.

```Rust
// Box
fn main() {
    let b = Box::new(10);
    println!("b = {}", b);
}
```

El mejor ejemplo que podemos usar para explicar el tipo "Box" es implementar algo que es llamado **consList**. **consList** es una estructura de datos que viene del lenguaje de programación **lisp**, en **lisp** hay una función llamada **cons** (viene de contruct) la cual podemos llamar para crear una nueva lista, lo que es esta **consList** es una llamada recursiva al operador **cons** una y otra ves hasta que crea una lista.

Podemos modelar ese comportamiento asi:

```Rust
enum List {
    Cons(i32, Box::new<List>), // si no usamos el 'Box' dará error porque almacenara 'List' en la 'Stack'/pila 
    End,
}

use List::Cons;
use List::End;

fn main() {
    let l = Cons(1, Box::new(Cons(2, Box::new(Cons(3, Box::new(End)))))); //esto crearía una lista
}
```

```Rust
fn main() {
    let y = 4;
    let x = &y;
    let z = Box:new(y);

    if *x == *z {
        println!("true");// se imprime "true"
    }
}
```

"Box" se vuelve muy util cuando usamos **Closures**

### Closures

"Closures" son funciones anónimas las cuales puedes guardar en una variable y pasarla como argumento a otra función, puedes incluso tener una función que crea una "Closure"

```Rust
//fn f(i: i32) -> i32 {i + 1} asi es como se crearía una función normalmente

fn main() {
    //podemos crear una función anónima usando el siguiente formato
    let f = |i| i + 1;
    let x = 10;
    println!("{}", f(x));

    // podemos crear closures que no toman valores
    let p = || println!("this is a closure");
    p(); // return 'this is a closure'
}
```

Las "closures" son heredadamente flexibles, haremos lo que la funcionalidad requiera para hacer que la **closure** funcione sin "annotations". Esto nos permite capturar, adaptar flexiblemente al caso de uso, a veces mover y a veces prestar variables. Las "closures" pueden capturar variables en varias formas, pero tienen una preferencia por capturar variables por referencia.

```Rust
fn main() {
    let mut c = 0;// el scope de 'c' esta dentro de la función 'main'

    let mut inc = || { // esta función toma prestada la variable 'c' del scope de fuera
        c += 1;
        println!("incremented by 1: {}", c);
    };

    inc();
    inc();
    inc();
}
```

```Rust
fn run<F>(f: F)
where F: Fn() {// F debe ser una función(Fn())
    f();
}

fn add3<F>(f: F) ->
where F: Fn(i32) -> {
    f(3)
}

// podemos usar closures dentro de structs
struct A<F: Fn(i32) -> i32 > { // Fn() indica que el generic debe ser tratado como una función
    f: F
}

fn main() {
    let p = || println!("hello from run function!");
    run(p);

    let x = |i| i * 10;

    //println!("3 * 10 {}", add3(x));

    let a = A {
        f: x, // con esto podemos guardar la closure 'x' dentro de la struct 'A'
    };
} 
```

```Rust
fn run<F>(f: F)
where F: Fn() {
    f();
}

fn pr() {
    println!("this is a normal function!");
}

fn main() {
    let p = || println!("hello from run function")
    run(p);
    run(pr);
}
```

Podemos tambien usar **closures** como parámetros de output. Esto en teoria es problemático porque se supone que Rust solo devuelve datos concretos no genéricos. Las closures son por definición tipos anónimos por lo que retornar una **closure** es solo posible usando el **smart pointer** **"Box"**.

```Rust
fn create() -> Box<Fn()> {
    Box::new(move || println!("this is a closure in a box!!")) // move lo que dice es que todos los valores dentro
    // de la closure deben ser referenciados por valor en vez de por referencia
    // tan pronto como salgamos de la función todos los valores creados dentro de esta morirán en el scope 
    // y no serán pasados al scope de la función 'main'
    // al usar la keyword 'move' prevenimos que eso pase 

fn main() {
    let x = create();
    x();
}
```

Las **closures** son especialmente útiles cuando trabajamos con iteradores

```Rust
fn main() {
    let v = vec![1,2,3];

    println!("v {}", v.iter().any(|&x| x != 2));// itera sobre el vector luego checka si hay algún elemento que sea diferente de 2
    //return v true, ya que hay 2 elementos en el vector que son diferentes a 2
}
```

```Rust
fn main() {
    let v = vec![1,2,3];

    for i in v.iter() {
        println!("{}", i);
    }
}
```

```Rust
trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;
}

fn main() {
    let v = vec![1,2,3];
    v.iter().next();
}
```

```Rust
fn is_even(n: u32) -> bool {
    n % 2 == 0
}

fn main() {
    let top = 10000;
    let mut c = 0;

    // en Rust los iteradores son llamados "lasy iterators" esto quiere decir que podemos crear un loop
    // como este el cual tiende a infinito y aun asi no romperá el programa
    // El compilador no resolverá cosas hasta que lo necesita
    for n in 0.. {
        let x = n * n;

        if x >= top {
            break;
        } else if is_even(x) {
            c += x;
        }
    }
    println!("{}", c);
}
```

Podemos crear el mismo loop de una forma mas funcional usando **closures**:

```Rust
fn is_even(n: u32) -> bool {
    n % 2 == 0
}

fn main() {
    let s: u32 =
    (0..).map(|n| n*n)// mapea la closure
    .take_while(|&n| n < 10000)
    .filter(|&n| is_even(n))// filtra los valores usando la función 'is_even()'
    .fold(0, |s, i| s + 1);// ejecuta la closure y suma todos los valores

    println!("{}", s);
    // esto es equivalente al bloque anterior, solo que aquí usamos funciones de orden superior y closures
    // es la forma funcional de hacerlo.
}
```

## Modules and Lifetimes

Para ver los módulos debemos crear una biblioteca, para hacer eso escribimos en la terminal `cargo new 'proyect_name'`.

Ej:

```Rust
mod A {
    pub fn a() {}
    pub fn b() {}
    pub mod B {
        pub fn a() {}
        pub fn b() {}
    }
}

mod C {
    fn a() {}
    fn b() {}
}

fn test() {
    // Para usar la función 'b' dentro el modulo 'A'
    A::b();
    // sin embargo nos dará problemas
    // para solucionarlo debemos agregar la keyword 'pub' a la función b ya que por ahora es privada

    // Para acceder al modulo 'B' dentro del modulo 'A'
    A::B::a();
}
```

Podemos separar todos los módulos en diferentes archivos por ejemplo podríamos tener:

- el archivo `lib.rs` en donde tendríamos lo siguiente:

```Rust
mod A;
mod C;

fn test() {
    A::b();
    A::B::a();
}
```

- el archivo `C.rs` en donde tendríamos:

```Rust
pub fn a() {}
pub fn b() {}
```

- Una carpeta llamada `A` dentro de la cual tendríamos el archivo `B.rs` y el archivo `mod.rs`:

```Rust
// B.rs
pub fn a() {}
pub fn b() {}
```

```Rust
// mod.rs
pub mod B;
pub fn a() {}
pub fn b() {}

// este archivo actuaria como el modulo 'A'
```

Como 'C' no tiene sub módulos solo llamamos al archivo `C.rs`, pero si un modulo tiene sub módulos debemos poner la declaración de ese modulo dentro de una carpeta con el nombre del modulo ('A' por ejemplo) y luego dentro de la carpeta un archivo llamado `mod.rs`

```Rust
pub mod a {
    pub mod b {
        pub mod c {
            pub mod d {
                pub fn e() {}
            }
        }
    }
}
// cuando tenemos una jerarquía de módulos de este tipo podemos usar la keyword 'use' para llamar directamente a una función
//que este dentro de la siguiente manera
use a::b::c::d;

// al hacer eso podemos llamar a la función de una manera mas simple

// si tenemos un enum podemos acceder a el desde la función main asi:
enum Ex {
    A,
    B,
    C,
}

use Ex::{A, C} // tambien podemos solo usar un * para llamar a todos los tipos dentro del 'enum'

fn main() {
    d::e();
    A
    C
}
```

Para llamar a una librería externa:

```Rust
extern crate name;
```

## Lifetimes

Cada referencia en **Rust** tiene un **Lifetime**, Lifetime en esencia es el scope en el cual una referencia es valida. En Rust la mayoría de **Lifetimes references** son implícitas o son inferidas de la misma forma en la que los tipos pueden ser inferidos, tambien podemos usar **annotations** para definir **Lifetimes** manualmente.

```Rust
fn main() {
    let x;
    {
        let y = 10;
        x = &y;
    } // aquí daría error porque al tratar de imprimir 'x' fuera del scope se trata de hacer referencia a 'y'
      // pero 'y' muere en el scope en el que es declarada

    println!("{}", x);
}
```

La parte del compilador que lidia con **Lifetimes** es llamado . **Borrow checker** determina si todos los prestamos de variables son validos o no, en el caso anterior el préstamo `x = &y` no es valido para el scope en el que se declaro.

Para que sea valido debemos reescribirlo de esta manera:

```Rust
fn main() {
    let y = 10;
    let x;

    x = &y;

    println!("{}", x);
}
```

```Rust
fn pr(x: &str, y: &str) -> &str { // nos daría un error aquí (missing lifetime specifier)
                                  // el problema es que el tipo que se retorno es creado dentro del scope
    if x.len() == y.len() {       // de la función, no podemos garantizar que el borrow (-> &str) va a existir
        x                         // el tiempo suficiente para ser retornado por la función main
    } else {
        y
    }
}

fn main() {

}
```

Para solucionarlo:

```Rust
// debemos poner un Lifetime specifier
fn pr<'a>(x: &'a str, y: &'a str) -> &'a str{ // 'a es el lifetime specifier 
    if x.len() == y.len() {
        x
    } else {
        y
    }
}

// la razone de porque se va el error es porque estamos diciendo que 'a' y 'b' tienen el mismo lifetime
// que el output str, lo que significa que debería existir hasta que la función main termine
fn main() {
    let a = "a string";
    let b = "b string";

    let c = pr(a, b);

    println!("{}", c);
}
```

La forma en la que especificamos **Lifetimes** depende de lo que la función este haciendo, si cambiamos la implementación de la función para que retorne el primer argumento en vez de uno de los otros entonces no tendríamos que especificar el **lifetime** o el segundo parámetro ('y'):

```Rust
// Aquí no daría error
fn pr<'a>(x: &'a str, y: &str) -> &'a str {
    x
}
```

Cuando retornamos una referencia desde una función el **lifetime** del return necesita coincidir con el **lifetime** de uno de los argumentos. Si la referencia que se retorna no refiere a uno de los argumentos la única otra posibilidad es que haga referencia a un valor creado en esta función lo cual seria una referencia peligrosa.

También podemos usar referencia dentro de **structs**.

```Rust
// ya que la struct tiene almacena una referencia necesita tener una lifetime annotation
struct A<'a> {
    x: &'a str,
}

fn main() {
    let a = A{x: "Hello"};
}
```

Si tenemos 2 **lifetimes** dentro del **struct** podemos tener 2 **lifetimes** independientes

```Rust
struct A<'a, 'b> {
    x: &'a str,
    y: &'b str,
}

fn main() {
    let a = A{x: "Hello", y: "There"};
}
```

Es importante saber que en **Rust** cada referencia tiene un **lifetime annotation** incluso si no especificamos una nosotros sigue existiendo en el compilador, eso es lo que se conoce como **lifetime inference** o **lifetime allusion**. **Allusion** no prefiere una inferencia completa, si Rust aplica deterministicamente esta regla (**lifetime allusion**) pero aun asi sigue habiendo una ambigüedad en el **lifetime** actual entonces debemos declararlo explícitamente.

El compilador aplica **allusion** basado en unas pocas reglas, si tenemos una función con **una** referencia entonces tendrá exactamente **un** parámetro

```Rust
struct A<'a, 'b> {
    x: &'a str,
    x: &'b str,
}

fn ab<'a, 'b>(x: &'a i32, y: &'b i32) {}
/* 

fn a<'a>(s: &'a str) -> &'a str {
    s
} 

Colocar lifetime parameters en la funciona anterior es redundante porque al ser uno solo el compilador lo inferirá por nosotros
*/
// la forma correcta seria simplemente
fn a(s: &str) -> &str {
    s
}

fn main() {
    let a = A{x: "Hello", y: "There"};
}
```

Hay múltiples **input lifetime parameters** hay uno que es una referencia a si mismo o a un *mutable self* entonces el **lifetime** de *self* es asignado a todos los parámetros de output.
Asique basados en la ultima regla sabemos que podemos asignar **lifetimes** a métodos.

```Rust
struct A<'a> {
    x: &'a str,
}

impl <'a> A<'a> {
    fn slf(&self) -> &str {
        self.x
    }
}

// Básicamente muchas de las formas en las que escribimos lifetimes en Rust son muy similares a como
// escribimos 'generics' en Rust

fn main() {
    let a = A{x: "Hello"};
}
```

Hay un lifetime mas el cual es importante, el **static lifetime**, este es un **lifetime** especial el cual estará durante toda la duración del programa:

```Rust
fn main() {
    let s: &'static str = "The String";
    // hay que evitar usar este lifetime a menos que sea necesario usarlo ya que hará que el programa sea mas lento
    // si es que hay varios de estos
    // si no encontramos la solución a una referencia potencialmente peligros entonces deberíamos usar el 
    // 'static lifetime'
}
```

## Macros and Metaprogramming

Rust provee un sistema con varios **macros** bastante poderosos lo cual nos permite hacer **metaprogramacion**. Los **macros** actúan como funciones excepto que estas usan un bang al final `!`, por ejemplo `println!()` y `vec![]` son ambos **macros**

```Rust
fn main() {
    println!("");
    vec![]
}
```

Cualquier función que veas que tiene un signo de exclamación al final es un **macro**. En vez de nosotros generar una función las **macros** están de base en el lenguaje.

Para crear una **macros**:

```Rust
macro_rules! a_macro {// usamos la macro 'macro_rules!' y luego ponemos el nombre de la macro ("a_macro")
    () => (
        println!("This is a macro");// body de la macro
    )
}

fn main() {
    a_macro!();//return "This is a macro"
}
```

Típicamente se usan las macros cuando no quieres repetirte. Muchas veces dentro de un programa encontraremos que estamos repitiendo varias veces alguna funcionalidad entonces tal vez quieras crear una macro para hacerte el trabajo un poco mas fácil, tambien puedes usar las macros para crear **domain specific languages** **(DSL)**

Los argumentos de una macro deben ser precedidos por un signo de dolar `$`

```Rust
macro_rules! x_and_y {
    (x => $e:expr) => (println!("X: {}", $e));//argumento 'e' con el 'designator' expr(expretion)
    (y => $e:expr) => (println!("Y: {}", $e));
}

macro_rules! build_fn { // esta macro crea una función que cuando se ejecuta imprime el println!()
    ($func_name:ident) => (
        fn $func_name() {

            println!("You called {:?}()",
                    stringify!($func_name));
        }
    )
}

macro_rules! print_ex {
    ($e:expr) => ( // toma una expresión 'e' y luego imprime esa expresión
        println!("{:?} = {:?}",
                stringify!($e),
                $e);
    )
}

fn main() {
    x_and_y!(x => 10);
    x_and_y!(y => 20 + 30);

    build_fn!(Hi_there);
    Hi_there();// return You called "Hi_there"()
    
    print_ex!({ // podemos pasar este bloque completo a la macro
        let y = 20;
        let z = 30;
        z + y + 10 * 3 * 100
    }); // return "{let y = 20; let z = 30; z + y + 10 * 3 * 100}" = 3050
}
```

Las macros tambien pueden ser sobrecargadas:

```Rust
macro_rules! exame {
    // si lo invocamos con un 'and' entre 'l' y 'r' ejecutara este bloque
    ($l:expr; and $r:expr) => ( // este bloque es similar a un match statement
        println!("{:?} and {:?} is {:?}",
                stringify!($l),
                stringify!($r),
                $l && $r) // en esencia el bloque se reduce a esta linea
    );

    // si lo invocamos con un 'or' entre 'l' y 'r' ejecutara este bloque
    ($l:expr; or $r:expr) => ( // este bloque tambien es similar aun match statement
        println!("{:?} or {:?} is {:?}",
                stringify!($l),
                stringify!($r),
                $l || $ r) // <-
    );
}

fn main() {
    exame!(1 == 1; and 2 == 1 + 1); // return: "1 == 1" and "2 == 1 + 1" is true
    exame!(true; or false);// return: "true" or "false" is  true
}
```

List comprehension macro:

```Rust
macro_rules! compr {
    ($id1: ident | $id2: ident <- [$start: expr ; $end: expr], $cond: expr) => {
        {
            let mut vec = Vec::new();

            for num in $start..$end + 1 {
                if $cond(num) {
                    vec.push(num);
                }
            }

            vec
        }
    };
}

fn even(x: i32) -> bool {
    x % 2 == 0
}

fn odd(x: i32) -> bool {
    x % 2 != 0
}

fn main() {
    let evens = compr![x | x <- [1;10], even];
    println!("{:?}", evens); // return: [2, 4, 6, 8, 10]

    let odds = compr![y | y <- [1;10], odd];
    println!("{:?}", odds); // return: [1, 3, 5, 7, 9]
}
```

```Rust
use std::collections::HashMap;

macro_rules! new_map {
    ($($key: expr => $val: expr),*) => {    // esta expresión se puede repetir desde cero hasta infinitas veces
        {                                  // eso es lo que representa el signo '$' al principio junto con el signo
            let mut map = HashMap::new();  // '*' al final

            $(
                map.insert($key, $key);// se repite la misma cantidad de veces que la expresión de arriba.
            )*  // Podemos usar un signo '+' en vez de un '*' para indicar para indicar que la macro debe aparecer
                // al menos una vez y se puede repetir las veces que quiera, al igual que el '*', lo que el
                // asterisco indica es que si la expresión aparece 0 o mas veces, pero si debe aparecer al menos una
            map // vez usamos es signo mas
        }
    };
}

fn main() {
    let m = new_map! {
        "one" => 1,
        "two" => 2,
        "three" => 3
    }; // el HashMap que retorna es: {"one": 1, "two": 2, "three": 3}
}
```

Domain specific language type macro example:

```Rust
macro_rules! calc {
    (eval $e: expr) => {{ // aquí creamos la keyword "eval"
        {
            let val: usize = $e;// forzamos a todas la expresiones que pasen por aquí a ser de tipo Integers
            println!("{} = {}", stringify!{$e}, val);
        }
    }};

    (eval $e:expr, $(eval $es: expr), +) =>  {
        {
            calc! {eval $e}
            calc! { $(eval $es),+}
        }
    };
}

fn main() {
    calc! {
        eval 4 * 5,
        eval 4 + 10,
        eval (10 * 3) - 20
    };  // return: 4 * 5 = 20
}       // 4 + 10 = 14
        // (10 * 3) - 20 = 10     
```

## Error Handling

Cualquier programa que escribas va a tener en algún momento algún tipo de bug o error. Rust divide sus errores en dos grupos **recuperable** e **irrecuperable**, los errores **recuperables** son errores que cuando ocurren el programa no crashea por completo, tan solo sigue corriendo y tal vez puede llegar a causar algún efecto secundario innecesario. Por otro lado un error **irrecuperable** causa que el programa falle, estos errores podrían hacer que el programa pare por completo.

### errores irrecuperables

```Rust
fn main() {
    let v = vec![1,2];

    v[50];// esto causaría un panic error
}
```

```Rust
fn exit(x: i32) {
    if x == 0 {
        panic!("we got a 0");// en Rust esta la macro panic! la cual fuerza un panic error en el programa
    }
    println!("things are fine!");
}

fn main() {
    exit(1);
    exit(0);
}
```

No todos los errores son irrecuperables. La mayoría no son lo suficientemente serios como para que el programa pare completamente. Usamos estas dos abstracciones para ayudarnos con estos tipos errores.

```Rust
enum Result<T, E>{
    Ok(T),
    Err(E)
}

enum Option<T>{
    Some(T),
    None,
}
```

Podemos reescribir la función "exit" anterior con un "Option"

```Rust
fn exit(x: Option<i32>) {
    match x {
        Some(0) => panic!("We got a 0"),
        Some(x) => println!("We got a {} things are fine", x),
        None => println!("we got nothing"),
    }
}

fn main() {
    exit(Some(1));
    exit(Some(10));
    exit(None);
    exit(Some(0));
}
```

Las "Options" son bastante útiles cuando queremos lidiar con los errores.

Results:

```Rust
use std::fs::File;

fn main() {
    let f = File::open("text.txt");

    let f = match f  {
        Ok(file) => file,
        Err(ref error) if error.kind() == ErrorKind::NotFound => {
            match File::create("text.txt") {
                Ok(fc) => fc,
                Err(e) => {
                    panic!(
                        "could not create file: {:?}",
                        e
                    )
                },
            }
        },
        Err(error) => {
            panic!(
                "could not open the file: {:?}",
                error
            )
        },
    };
}
```

Este tipo pattern matching es muy util pero al mismo tiempo es muy verboso, por eso en Rust hay implementadas funciones que hacen exactamente los mismo que el bloque anterior.

```Rust
fn main() {
    let f = File::open("text.txt").unwrap();// mostrara la data del result
    let f = File::open("text.txt").expect("Could not open file");// si obtenemos un error automáticamente
}                                                               // hará panic! y devolverá el mensaje que declaramos
```

```Rust
use std::io::ErrorKind;

use std::io;
use std::io::Read;
use std::fs::File;

fn read_file() -> Result<String, io::Error> {// propagating error
    let f = File::open("text.txt");

    let mut f = match f {
        Ok(file) => file,
        Err(e) => return Err(e),
    };

    let mut s = String::new();
    match f.read_to_string(&mut s) {
        Ok(_) => Ok(s),
        Err(e) => Err(e),  
    }
}

fn main() {

}
```

Podemos crear una version menos verbosa de la función anterior, ya que Rust provee un operador especifico para hacer mas fácil el usar el error propagating pattern

```Rust
use std::io::ErrorKind;

use std::io;
use std::io::Read;
use std::fs::File;

fn read_file() -> Result<String, io::Error> {
    let mut f = File::open("text.txt")?;
    let mut s = String::new();
    f.read_to_string(&mut s)?;
    Ok(s)
}

// Si obtenemos un ok de vuelta el programa continuara normalmente, si el valor es un error el valor dentro del 
// error sera devuelto por la función.
```

Podemos simplificarla aun mas:

```Rust
use std::io::ErrorKind;

use std::io;
use std::io::Read;
use std::fs::File;

// Los signos '?' solo pueden ser usados dentro de funciones que retornen un 'Result'
fn read_file() -> Result<String, io::Error> {
    let mut s = String::new();
    File::open("text.txt")?.read_to_string(&mut s)?;
    Ok(s)
}

fn main() {

}
```

Los panics son especialmente útiles cuando estamos usando código externo el cual suele estar fuera de nuestro control, ya que por lo general los errores nos son algo que esperamos que pasen.

## Concurrency, Threads, Channels, Mutex and Arc

Hay dos principales tipos de **Threads** en programación, el primero es llamado **OS Thread** y el segundo es llamado **Green Thread**. El `Operating System Thread` lo entrega el mismo sistema operativo, por otro lado el `Green Thread` es una abstracción la cual corren sobre el **Operating System Thread**.
En lenguajes como Go, Erlang o Elixir se usan estos llamados **Green Threads**, estos nos permiten crear muchos mas threads que un lenguaje normal el cual usa solo los **Operating System Threads**, por ejemplo Go podría crear 10 **Green Threads** por cada **Operating System Thread**, aparte esto escala basado en el poder de computo que se esta manejando. **Rust** usa el **Operating System Thread** directamente la razón de esto es en son de tener un menor runtime, es decir una menor cantidad de código incluyendo cada binario luego de compilar.

### Threads

```Rust
use std::thread;

fn main() {
    let mut c = vec![];

    for i in 0..10 {
        c.push(thread::spawn(move || {
            println!("thread number {}", i);
        }));
    } // solo se ejecutan 5 external threads, los otros 5 nunca se imprimen porque el main thread los termina antes
      // de que se ejecuten

    for j in 0..10 {
        println!("main thread");
    }
}
```

El thread spawn method retorna lo que se llama un join handler, cuando llamamos al join method en nuestro join handler forzara al main thread a esperar al thread al cual esta asociado el join handler a finalizar:

```Rust
use std::thread;

fn main() {
    let mut c = vec![];

    for i in 0..10 {
        c.push(thread::spawn(move || {
            println!("thread number {}", i);
        }));
    }

    for j in c {// itera sobre el vector, un thread a la vez
        j.join();// luego añade cada uno de los threads al main thread
    }            // fuerza al main thread a esperar a que todos los otros threads se resuelvan

    // return:
    // thread number 0
    // thread number 1
    // thread number 2
    // thread number 3
    // thread number 4
    // thread number 5
    // thread number 6
    // thread number 9
    // thread number 8
    // thread number 7

    // Rust no da garantía sobre el orden de ejecución de los threads por lo que el thread 0 podría ejecutarse
    // ultimo o el thread 9 ejecutarse primero.
    // En todo caso en la mayoría de las veces no nos importa el orden de ejecución de los threads
```

```Rust
use std::thread;

fn main() {
    let v = vec![1, 2, 3];

    let handle = thread::spawn(move || {// move keyword permite a la closure usar la data de un thread en otro
        println!("vector: {:?}", v);    // en esencia estamos tomando control de la data del main thread 
    });                                 // dentro de la closure.
                                        // 'move' keyword fuerza a la closure a referenciar la data por valor en vez
    handle.join();                      // de por referencia, en ese sentido podemos capturar valores del entorno
}                                       // cuando estamos iniciando nuevos threads.
                                        // Rust de primeras trata de capturar en este caso la variable 'v'
                                        // pero la macro println!() solo necesita una referencia
                                        // asique la closure trata de pedir prestada 'v'
                                        // cuando ponemos 'move' en vez de pedir prestada la variable 'v'
                                        // tomamos 'v' y la ponemos dentro del thread y le damos el control total a 
                                        // este.
                                        // Usamos 'move' para forzar a la closure a tomar control del valor, cuando
                                        // usamos 'move' garantizamos a Rust que el main thread no seguirá usando
                                        // mas este valor. Si luego intentamos interactuar con 'v' fuera de la
                                        // closure no  funcionara ya que 'v' fue removida por completo del scope
                                        // de 'main' y fue puesta dentro del scope de la closure
```

### Channels

```Rust
use std::thread;
use std::sync::mpsc;// Multiple Producer Single Consumer (mpsc)

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || { tx.send(42).unwrap(); });

    println!("got {}", rx.recv().unwrap());
}
```

Los channels son usados para pasar mensajes, los channels se componen por un transmisor `(tx)` y un receptor `(rx)`. El transmisor enviá un mensaje el flujo del sistema y el receptor es por donde sale ese mensaje.
El receptor tiene dos métodos útiles `.recv()` y `.tryrecv()`, `recv()` es un método bloqueante, bloqueara en este caso la ejecución del main thread y esperara el mensaje, por otro lado `tryrecv()` es un método no bloqueante lo usaremos en casos cuando no necesitamos un resultado inmediato, cuando queremos que el thread continue haciendo otras cosas mientras esperamos el mensaje.

```Rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

const NUM_TIMERS: usize = 24;

fn timer(d: usize, tx: mpsc::Sender<usize>) {
    thread::spawn(move || {
        println!("{}: setting timer...", d);
        thread::sleep(Duration::from_secs(d as u64)); // manda el thread a dormir por 'd' segundos
        println!("{}: sent!", d);
        tx.send(d),unwrap();
    });
}

fn main() {
    let (tx, rx) = mpsc::channel();

    for i in 0..NUM_TIMERS {
        timer(i, tx.clone());// clonamos el transmisor múltiples veces
    }

    for v in rx.iter().take(NUM_TIMERS) {
        println!("{}: recived!", v);
    }
}
```

Este ejemplo la idea de **Multiple Producers Single Consumer (mpsc)** tenemos 24 **producers** `(tx)` diferentes y solo tenemos un **consumer** `(rx)`, por lo que la función main esta obteniendo todos los resultados de vuelta desde nuestros múltiples diferentes **threads**.

Los **channels** son bastante poderosos, son una buena manera de enviar data de un **thread** otro.

### Mutex

Rust tiene una segunda abstracción que se usa para comunicar memoria compartida dentro de su modelo de concurrencia, esta es llamada `Mutex`, lo cual viene de **mutual exclussion**. Básicamente un mutex solo permite que un thread tenga acceso a un pedazo de data a un cierto tiempo. Hay dos roles principales por los que esta `Mutex` en `Rust`, primero el thread que quiera tener acceso a `mutex` necesita obtener el lock del mutex, y luego una vez que se finaliza la data, debes desbloquear la data para que entonces los otros threads puedan entonces adquirir la data, podemos pensar a **mutex** como un casillero de almacenamiento donde solo tenemos una llave pero tenemos múltiples personas con acceso a este, cada persona que quiera tener acceso al casillero necesitara tener acceso a la llave, si otra persona quiere acceso entonces tendra que ir donde la persona dueña de la llave, tomar la llave de esta y luego ir al casillero y abrirlo.

Mutex tambien tiene acceso a un **smart pointer**, este es similar a nuestro **Box pointer**. Mas exactamente el método lock de **mutex** retorna un **smart pointer** llamado `mutex arc`, el cual implementa derefen droop, asi que automáticamente dejaremos ir el lock cuando salga del scope:

```Rust
use std::sync::{Mutex, Arc};
use std::thread;

fn main() {
    let c = Arc::new(Mutex::new(0)); // el mutex esta embebido en un Arc, Arc is an 'Atomically Reference Counted'
    let mut hs = vec![];             // types, necesitamos hacer esto porque arcs convierten el tipo
                                     // en tipos primitivos, los cuales son guardados para que estén seguros
    for _ in 0..10 {                 // entre los threads, en esencia estamos convirtiendo este mutex en un tipo
        let c = Arc::clone(&c);      // el cual actuá como un dato primitivo el cual es guardado para ser compartido
        let h = thread::spawn(move || { // entre múltiples threads. 
            let mut num = c.lock().unwrap(); // el thread gana el lock control del mutex, gana el valor dentro
                                             // del mutex, en este caso 0
            *num += 1;
            println!("{}", num);
        });
        hs.push(h);
    }

    for h in hs {

        h.join().unwrap();// join() para que el main thread pare y espere a que todos los otros threads
    }                     // se resuelvan

    println!("Result: {}", *c.lock().unwrap());
}
```

Cada thread se crea y cuando es creado entonces gana acceso al mutex, incrementa el mutex en 1, luego libera su acceso al mutex, luego el siguiente thread gana acceso al mutex lo incrementa en 1, lo libera y asi. Luego después de que todos lo threads se resolvieron por el método `.join()` nuestro main thread gana acceso al mutex y luego lo imprime.

mutex and channel:

```Rust
use std::sync::{mpsc, Arc, Mutex};
use std::thread;

fn is_prime(n: usize) -> bool {// checa si un numero es primo
    (2..n).all(|i| {n % i != 0})
}

fn producer(tx: mpsc::SyncSender<usize>) -> thread::JoinHandle<()> {
    thread::spawn(move || for i in 100_000_000.. {
        tx.send(i).unwrap();
    })
}

fn worker(id: u64, shared_rx: Arc<Mutex<mpsc::Receiver<usize>>>) {
    thread::spawn(move || loop {
        {
            let mut n = 0;
            match shared.rx.lock() {
                Ok(rx) => {
                    match rx.try_recv() {
                        Ok(_n) => {
                            n = _n;
                        },
                        Err(_) => ()
                    }
                },
                Err(_) => ()
            }

            if n != 0 {
                if is_prime(n) {
                    println!("worker {} found a prime: {}", id, n);
                }
            }
        }
    });
}

fn main() {
    let (tx, rx) = mpsc::sync_channel(1024);// establece la cantidad de memoria que se usara (1024)
    let shared_rx = Arc::new(Mutex::new(rx));

    for i in 1..5 {
        worker(i, shared_rx.clone());// spawn 5 workers threads
    }

    producer(tx).join().unwrap();
}
```

## Tests, Attributes, Configuration and Conditional compilation

Cuando creas una nueva library con **cargo** esto es lo que queda dentro de `lib.rs`:

```Rust
// atributo cfg(conditional compilation flag) compilamos condicionalmente este código basado en la
// configuración. Esto significa que todo este bloque solo compilara cuando llamemos a 'test'
#[cfg(test)]
mod test { // creamos un submodulo
    #[test] // le dice al compilador que esta función es parte del test
    fn it_works() {

    }
}

// Los atributos son metadata sobre piezas de código en Rust
```

Esto se genera automáticamente para que nosotros podamos comenzar a hacer TDD (Test Driven Development).

Los proyectos de tipo library no tienen función **main**, asique la forma para poder testear la funcionalidad de esta en vez de ponerla dentro de una función main es llamando a **test**

Podemos correr los test con `cargo test`. Rust construye el test y lo corre en el binario que ejecuta la función que tiene la annotation `#[test]` y luego reporta si la función pasó o no el test.
La suite de test tiene acceso al atributo **test**, tambien tiene varias **macros** que podemos usar y tambien tiene un atributo llamado should_panic el cual podemos usar si queremos

```Rust
#[cfg(test)]
mod test {
    #[test]
    fn item_works() {

    }

    #[test]
    fn check_two() {
        assert!(1 + 1 == 2);// assert! macro es proveída por la std lib, es usada cuando queremos asegurarnos de 
    }                       // que una condición en un test evalúe si es verdadera. Le damos a la macro un argumento
}                           // para que value un booleano, si el valor que resulta es true assert! no hace nada
                            // y el test aprueba pero si es false hará panic! y el test falla
    #[test]
    fn test_three() {
        assert!(3 == 4);// FAILED
    }

    //Para poder estar seguros que el código retorna assertions correctas podemos checar que el código maneje
    //las condiciones de error como esperamos. Por ejemplo si queremos que un test falle podemos añadir otro atributo
    #[test]
    #[should_panic] // le dice al compilador que este test debería fallar
    fn test_four() {// return: OK , ya que esperábamos que fallara
    }
```

```Rust
pub fn add_two(a: i32) -> i32 {
    internal_adder(a, 2)
}

fn internal_adder(a: i32, b: i32) -> i32 {
    a + b
}

pub fn greeting(name: &str) -> String {
    format!("Hello {}", name)
}

fn main() {

}

#[cfg(test)]
mod test {
    use super::*;// trae el código de fuera del modulo al scope del modulo
    #[test]
    fn it_works(){
        assert_eq!(4, internal_adder(2, 2));// compara los argumentos buscando igualdad, si falla imprime los valores
        // tambien esta la macro assert_ne! la cual hace lo apuesto a assert_eq!, es decir busca que no sean iguales
        // si son diferentes pasa el test
    }

    #[test]
    #[should_panic]
    fn another() {
        assert!(true == false);// solo nos dice si tenemos o no un valor falso, no los valores que hicieron que falle 
    }
/* 
    #[test]
    fn greeting_contains_name() {
        let result = greeting("Carol");
        assert!(result.contains("Carol"));// retorna true, pero si ponemos "carol" en vez de "Carol" falla
    } */
    // Podemos modificar esta función para que retorne un mensaje personalizado
    #[test]
    fn greeting_contains_name() {
        let result = greeting("Carol");
        assert!(
            result.contains("Carol"),
            "Greeting did not contain name, value was `{}`", result
        );
    }
}
```

También podemos correr un solo test en especifico con `cargo test greeting_contains_name` por ejemplo

El atributo cfg (conditional compilation flag) en este caso solo compilara el modulo si es que corremos el comando `cargo test`, de no ser asi solo compilara el resto del código, asique no tenemos que preocuparnos del que el modulo de tests relentece la ejecución del programa.

Podemos usar cfg tambien para otras cosas:

```Rust
#[cfg(target_os = "linux")]// si es linux corre esta función
fn are_you_on_linux() {
    println!("running linux!");
}
#[cfg(not(target_os = "linux"))]// si no es linux corre esta
fn are_you_on_linux() {
    println!("not running linux!");
}

fn main() {
    are_you_on_linux();

    println!("check OS again");
    if cfg!(target_os = "linux") {
        println!("definitely linux");
    } else {
        println!("not linux");
    }
}
```

Esta feature es muy util para CLI tools.

otros atributos:

```Rust
#![allow(dead_code)]
fn dead_func() {}

fn main() {
    dead_func();
}
```