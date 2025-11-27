### [Memoria y Asignación](https://book.rustlang-es.org/ch04-01-what-is-ownership#memoria-y-asignaci%C3%B3n) 

**¿Qué implica que un literal de cadena sea conocido en tiempo de compilación?**
	implica que el texto se codificará directamente en el ejecutable final.

**¿Cuando se libera la memoria en Rust?**
	Rust devuelve la memoria automáticamente una vez que la variable que la posee sale del contexto de ejecución	
```Rust
    {
        let s = String::from("hola"); // s es valido desde aquí
        // Hacer algo con s
    }                                  // este ámbito termina aquí, 
                                       // s ya no es valido
```

**¿Cuál es la función que ejecuta Rust para liberar memoria cuando una variable sale del contexto de ejecución?**
	Rust llama a una función especial para nosotros llamada [`drop`](https://doc.rust-lang.org/std/ops/trait.Drop.html#tymethod.drop)
	Rust llama a `drop` automáticamente en la llave de cierre.

Note: [[RAII]]

