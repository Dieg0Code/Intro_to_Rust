# Rust

`Rust` es un lenguaje de programación compilado multiparadigma, de alto nivel, de propósito general diseñado para eficiencia y seguridad, especialmente en cuanto a concurrencia. `Rust` es sintácticamente similar a `C++` pero puede garantizar seguridad en la memoria mediante el uso de un *borrow checker* para validar las referencias. `Rust` logra seguridad en la memoria sin un *garbage colector*.

Este lenguaje es de código abierto y fue creado inicialmente por **Graydon Hoare** en **Mozilla**.

Puedes encontrar más información sobre el lenguaje en [**Rust's website**](https://www.rust-lang.org/).

Este lenguaje es muy similar a `C++`, se podría decir que apunta a lo mismo, con la diferencia de que `Rust` es mucho mas moderno, no solo en su sintaxis y forma de manejar la concurrencia, sino también en su manejo de memoria, en el manejo de errores, viene con herramientas para testing de base, tiene un gestor de paquetes integrado llamado `cargo` al mas puro estilo **npm** y muchas otras cosas que permiten programar al mismo nivel que podemos llegar con `C++` pero con mucha mas fluidez y con muchos menos errores.

Para instalar `Rust` en tu equipo recomiendo ir directamente a la página oficial de [Rust](https://www.rust-lang.org/tools/install), y seguir los pasos que te indica el manual.

Ahora para ir más rápido:

Sistemas basados en `Linux`:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Para windows debes ir por fuerza a la página oficial y descargar el archivo `rustup-init.exe` y ejecutarlo.

La instalación de `Rust` trae 3 paquetes principales:

- **Cargo**: Es el gestor de paquetes de Rust, si ya has trabajado con `npm`, es casi lo mismo.
- **Rustup**: Es el gestor de versiones de Rust. Rust tiene una peculiaridad en cuanto a sus versiones, este cuenta con 3 versiones principales, `stable`, `beta` y `nightly`, siendo la version `stable` la mas usada, la version `nightly` trae nuevas funcionalidades y mejoras pero podría ser inestable, aunque aun asi es usada por algunos frameworks y librerias como en el caso de [Rocket](https://rocket.rs/) por ejemplo.
- **rustc**: Es el compilador de Rust, es el que se encarga de compilar el código de Rust.

`Cargo` el gestor de paquetes cuenta con una página web [crates.io](https://cargo.io/) donde puedes encontrar todos los paquetes que hay disponibles.

Otra página util es [what Rust is it?](https://www.whatrustisit.com/) en donde encontraras información sobre las versiones actuales de cada rama del lenguaje.

Otra característica muy interesante de `Rust` es que trae su documentación integrada para que podamos consultarla de manera offline. Para ello debemos usar el siguiente comando:

```bash
rustup doc
```

Lo que hace este comando es abrir una nueva ventana con la documentación de Rust, esta trae varios libros los cuales podemos consultar.

La documentación trae "The Book" el cual es una guía completa del lenguaje y en mi opinión es la mejor forma de comenzar a aprenderlo.

Con este comando podemos abrir directamente el libro de Rust.

```bash
rustup doc --book
```

Otro comando importante es:

```bash
rustup doc --cargo
```

Este comando nos abre el libro de Cargo.

Para ver mas comandos que tiene que ver con documentación puedes usar:

```bash
rustup doc --help
```

*nota:* `rustup doc` no funcionaria en WSL, ya que se necesita un navegador web para abrir la página.

