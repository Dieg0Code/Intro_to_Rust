# Rust

`Rust` es un lenguaje de programación compilado multiparadigma, de alto nivel, de propósito general diseñado para eficiencia y seguridad, especialmente en cuanto a concurrencia. `Rust` es sintácticamente similar a `C++` pero puede garantizar seguridad en la memoria mediante el uso de un *borrow checker* para validar las referencias. `Rust` logra seguridad en la memoria sin un *garbage colector*.

Este lenguaje es de [código abierto](https://github.com/rust-lang) y fue creado inicialmente por **Graydon Hoare** en **Mozilla**.

Puedes encontrar más información sobre el lenguaje en [**Rust's website**](https://www.rust-lang.org/).

Este lenguaje es muy similar a `C++`, se podría decir que apunta a lo mismo, con la diferencia de que `Rust` es mucho mas moderno, no solo en su sintaxis y forma de manejar la concurrencia, sino también en su manejo de memoria, en el manejo de errores, viene con herramientas para testing de base, tiene un gestor de paquetes integrado llamado `cargo` al mas puro estilo **npm** y muchas otras cosas que permiten programar al mismo nivel que podemos llegar con `C++` pero con mucha mas fluidez y con muchos menos errores.

Para instalar `Rust` en tu equipo recomiendo ir directamente a la página oficial de [Rust](https://www.rust-lang.org/tools/install) y seguir las instrucciones.

Ahora, para ir más rápido:

Sistemas basados en `Linux`:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Para windows debes ir por fuerza a la página oficial y descargar el archivo `rustup-init.exe` (32 o 64) y ejecutarlo.

La instalación de `Rust` trae 3 paquetes principales:

- `Cargo`: Es el gestor de paquetes de Rust, si ya has trabajado con `npm`, es casi lo mismo.
- `Rustup`: Es el gestor de versiones de Rust. Rust tiene una peculiaridad en cuanto a sus versiones, este cuenta con 3 versiones principales, `stable`, `beta` y `nightly`, siendo la version `stable` la mas usada, la version `nightly` trae nuevas funcionalidades y mejoras pero podría ser inestable, aunque aun asi es usada por algunos frameworks y librerias como en el caso de [Rocket](https://rocket.rs/) por ejemplo.
- `rustc`: Es el compilador de Rust, es el que se encarga de compilar el código de Rust.

- `Cargo` el gestor de paquetes cuenta con una página web [crates.io](https://cargo.io/) donde puedes encontrar todos los paquetes que hay disponibles.

- Otra página util es [what Rust is it?](https://www.whatrustisit.com/) en donde encontraras información sobre las versiones actuales de cada rama del lenguaje.

Otra característica muy interesante de `Rust` es que trae su documentación integrada para que podamos consultarla de manera offline. Para ello debemos usar el siguiente comando:

```bash
rustup doc
```

Lo que hace este comando es abrir una nueva ventana con la documentación de Rust, esta trae varios libros los cuales podemos consultar.

La documentación trae `"The Book"` el cual es una guía completa del lenguaje y en mi opinión es la mejor forma de comenzar a aprenderlo.

Con este comando podemos abrir directamente el libro de Rust.

```bash
rustup doc --book
```

Otro comando importante es:

```bash
rustup doc --cargo
```

Este comando nos abre el `libro de Cargo`.

Para ver mas comandos que tiene que ver con documentación puedes usar:

```bash
rustup doc --help
```

*nota:* `rustup doc` no funcionaria en [`WSL`](https://docs.microsoft.com/en-us/windows/wsl/install-win10), ya que se necesita un navegador web para abrir la página.

## ¿Que hace a Rust tan seguro?

`Rust` busca frenar varias vulnerabilidades desde el momento en el que empiezas a escribir código, simplemente no compilará si hay algún error de sintaxis o algún bug relacionado con un mal manejo de la memoria, tiene un *borrow checker* el cual se encarga de validar las referencias de memoria, si no se encuentra una referencia de memoria que se esta usando, `Rust` arrojará un error.

Protege la memoria por diseño, se asegura de que el programador use los recursos correctamente y no se dejen referencias de memoria sin usar(sin importar bajo que condiciones se ejecute el programa). Con esto se evitan el [**70% de los bugs de seguridad**](https://www.ordenadores-y-portatiles.com/bug/) que se pueden producir en programación.

Rust advertirá y prevendrá:

- [Buffer overflow](https://www.welivesecurity.com/la-es/2014/11/05/como-funcionan-buffer-overflow/).
- [Use after free](https://vulncat.fortify.com/es/detail?id=desc.controlflow.cpp.use_after_free).
- [Double-free](https://vulncat.fortify.com/es/detail?id=desc.controlflow.cpp.double_free).
- [Null pointer dereference](https://stackoverflow.com/questions/4007268/what-exactly-is-meant-by-de-referencing-a-null-pointer).
- [Using uninitialized memory](https://pvs-studio.com/en/blog/terms/0079/#:~:text=Use%20of%20uninitialized%20memory%20means,a%20so%20called%20%22heisenbug%22.).

Si comparamos un trozo de código `Rust` con uno `C++`, se vuelve evidente que `Rust` es seguro por diseño:

```c++
#include <iostream>
#include <string.h>
int main(void) {
    char a[3] = "12";
    char b[4] = "123";
    strcpy(a, b); // Buffer overflow porque 'a' no tiene memoria suficiente para copiar 'b' en ella. std::cout << a << "; " << b << std::endl;
}
```

Vs.

```rust
pub fn main() {
    let mut a:[char; 2] = ['1', '2'];
    let b:[char; 3] = ['1', '2', '3'];
    a.copy_from_slice(&b); // Buffer overflow porque 'a' no tiene memoria suficiente para copiar 'b' en ella. println!("{}; {}", a, b);
}
```

![Buffer Overflow Rust](./img/bufferOverflowRust.png)

`Rust` nos devuelve un error, y detiene la ejecución del programa al llegar a la función `copy_from_slice`, esto lo detecta `Rust` al chequear el tiempo de ejecución, no en la compilación.

`Rust` le enseña al programador la forma correcta de como deberíamos escribirlo si queremos evitar bugs de seguridad relacionados con el uso de la memoria.

A medida que se va trabajando con el lenguaje podemos darnos cuenta de que `Rust` de cierta manera se enseña a si mismo. Ya sea con la documentación que trae por default o con el mismo compilador el cual no solo nos indica los errores de sintaxis, sino que también nos da tips sobre como solucionar dichos errores, los logs que devuelve son muy claros y nos ayudan a entender que hacemos mal.

### Razones para usar Rust

- `Seguridad de tipos`: El compilador nos asegura que ninguna operación será aplicada a una variable del tipo incorrecto.
- `Seguridad de memoria`: Todas las referencias siempre apuntarán a una dirección de memoria válida.
- `Sin condiciones de carrera`: El sistema de *ownership* nos garantiza que múltiples partes del programa no puedan modificar el mismo valor al mismo tiempo.
- `Abstracciones a costo cero`: Rust nos permite usar conceptos de alto nivel (iteraciones, interfaces, enums, programación funcional, etc) con un costo nulo o mínimo en rendimiento.
- `Runtime mínimo`: Rust tiene un runtime mínimo y lo más optimizado posible, similar a `C` y `C++`.
- `Usado en Webassembly`: [WebAssembly](https://rustwasm.github.io/), abreviado wasm, es un formato de código binario portable, para la ejecución íntegra en navegador de scripts de lado del cliente. Se trata de un lenguaje de bajo nivel, diseñado inicialmente como formato destino en la compilación desde C y C++, aunque se puede usar en otros lenguajes como `Rust` o Go. En pocas palabras permite ejecutar lenguajes de alto rendimiento en el navegador de forma nativa.
- [`Gran potencial para el desarrollo de videojuegos`](https://arewegameyet.rs/): Cuando buscamos opciones que potencien el rendimiento de un videojuegos, Rust es una opción mas que viable.
- `Desarrollo web`: A pesar de ser un lenguaje relativamente nuevo ya posee [formas](https://www.arewewebyet.org/) de usarse en el desarrollo web, tanto para el backend como para el frontend (usando wasm 😮).

## ¿Que es Cargo?

`Cargo` es el package manager (gestor de paquetes) de Rust, como `npm`, `yarn` en JS o `pip` en Python. Este package manager nos ayuda a compilar el código, descargar dependencias, compilar dependencias, etc.

Para checar la version de Cargo que tenemos instalada:

```bash
cargo --version
```

Una de las utilidades de `cargo` es que nos ayuda a armar la estructura de nuestro proyecto para que no tengamos que crearla a mano.

Para crear un nuevo proyecto usando `cargo`:

```bash
cargo new nombre-del-proyecto
```

Este comando nos crea una carpeta con el nombre del proyecto, dentro un archivo `Cargo.toml` y una carpeta `src` con un archivo `main.rs` que contiene el código principal.

### El archivo `Cargo.toml`

Este es el archivo que indica la configuración del proyecto. Indica las dependencias que necesita, las versiones de las mismas, el nombre del proyecto, la versión del proyecto, etc.

Ej:

```toml
[package]
name = "nombre-del-proyecto"
version = "0.1.0"
authors = ["Mi nombre"]
edition = "2018"

[dependencies]
```

No tenemos que reinventar la rueda cada vez que comenzamos un nuevo proyecto, podemos usar paquetes que han echos otros desarrolladores para incluir diferentes funcionalidades en la aplicación.

En Rust los paquetes de dependencias se llaman `crates` (en español seria como cajón, de esos que suelen estar en los puertos).

En vez de ir compilando los archivos manualmente, podemos usar `cargo` para hacerlo automáticamente.

```bash
cargo build
```

Este comando compila todo el proyecto y deja el binario resultante en la carpeta `target`.

Existe otro comando que compila y luego ejecuta el binario:

```bash
cargo run
```

Otro comando muy util es:

```bash
cargo check
```

El cual comprueba si el código es correcto para ser compilado, pero no lo compila. Es mucho más rápido que usar `cargo build` o `cargo run`.

Existe una variación del comando `cargo build`:

```bash	
cargo build --release
```

Este comando compila el proyecto en modo `release`, aplicando optimizaciones para que el binario sea más eficiente. Este comando es muy útil cuando trabajamos con el proyecto en producción, pero no es necesario para el desarrollo ya que hace que la compilación sea más lenta.

Cuando usamos `--release` se crea otra carpeta dentro de `target` llamada `release` que contiene el binario compilado con las optimizaciones.

## ¿Que IDE usar?

En la sección [tools](https://www.rust-lang.org/tools) de la página web de Rust encontramos una lista de herramientas que nos ayudan a programar con el lenguaje. Para esto tenemos que instalar las herramientas correspondientes, en mi caso el que yo uso y recomiendo es [Vscode](https://code.visualstudio.com/), con el plugin [Rust analyzer](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer), recomiendo ese plugin por sobre otros ya que este nos provee *inlay hints*, estos son marcadores especiales que aparecen en el editor, los cuales nos muestran información adicional sobre el código que estamos escribiendo.

![inlay hints](./img/inlay-hints.png)

En este caso nos da información sobre el tipo de dato de las variables, nos da información adicional sobre los parámetros en las funciones, etc. Muy útil.

### Para debugear

Para sistemas mac/linux y windows (x86 only):

- [CodeLLDB](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb)

Para windows:

- [C/C++ extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)

Las extensiones de debuger son muy útiles, ya que mediante *breakpoints* podemos detener la ejecución del código en lineas específicas, y así ver el estado de la ejecución hasta ese punto.

### Otras extensiones útiles

- [TOML Language Support](https://marketplace.visualstudio.com/items?itemName=be5invis.toml) (soporte para archivos TOML)

- [crates](https://marketplace.visualstudio.com/items?itemName=serayuzgur.crates) (soporte para dependencias de Rust)

## Antes de empezar

Es importante que también conozcamos un par de herramientas más que nos provee `Rust` para las buenas prácticas de programación.

### Rustfmt

[`Rustfmt`](https://github.com/rust-lang/rustfmt) formatea automáticamente el código, haciéndolo más fácil de leer, escribir y mantener. Y lo más importante: nunca más tendremos que debatir si es mejor usar espacios o tabulaciones.

Una vez instalado. Para usarlo debemos ejecutar el siguiente comando en la carpeta del proyecto:

```bash
cargo fmt
```

### Clippy

[`Clippy`](https://github.com/rust-lang/rust-clippy) ayuda a los desarrolladores de todos los niveles de experiencia a escribir código más limpio, mas idiomatico, y a reforzar los estándares.

Una vez instalado. Para usarlo debemos ejecutar el siguiente comando en la carpeta del proyecto:

```bash
cargo clippy
```

Y nos dará una lista de sugerencias de código que podemos aplicar para mejorar la calidad y legibilidad de este.

### Cargo Doc

`cargo doc` nos permite generar una documentación de nuestro proyecto de forma automática.

Podemos usarlo localmente y ver lo que genera con el siguiente comando:

```bash
cargo doc --open
```

También está de forma [online](https://docs.rs/) con la documentación de todos los *crates* públicos.