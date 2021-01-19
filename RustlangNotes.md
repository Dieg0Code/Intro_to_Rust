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
// Aquí no daria error
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

Es importante saber que en **Rust** cada referencia tiene un **lifetime annotation** incluso si no especificamos una nosotros sigue existiendo en el compilador, eso es lo que se conoce como **lifetime inference** o **lifetime allusion**. **Allusion** no prefiere una inferencia completa, si Rust aplica deterministicamente aplica esta regla (**lifetime allusion**) pero aun asi sigue habiendo una ambigüedad en el **lifetime** actual entonces debemos declararlo explícitamente.

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