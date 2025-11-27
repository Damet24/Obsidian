
Es un principio de diseño en C++ (y algunos otros lenguajes con destructores deterministas) que indica que los recursos (como memoria, archivos, sockets, etc.) deben ser adquiridos y liberados automáticamente como parte del ciclo de vida de un objeto.

En otras palabras:
- **Adquieres el recurso en el constructor.**
- **Lo liberas en el destructor.**

De esta manera, se garantiza que los recursos se liberen correctamente, incluso si ocurre una excepción, ya que los destructores se llaman automáticamente cuando un objeto sale de su ámbito. Esto hace que RAII sea una técnica muy poderosa para escribir código seguro y libre de fugas de recursos.


Es un nombre realmente terrible para un concepto increíblemente poderoso, y quizás una de las cosas número uno que los desarrolladores de C++ extrañan cuando cambian a otros lenguajes. Ha habido un pequeño movimiento para intentar renombrar este concepto como _Scope-Bound Resource Management_ (Gestión de Recursos Ligada al Ámbito), aunque parece que todavía no ha tenido mucha aceptación.

Cuando decimos "recurso", no solo nos referimos a la memoria: pueden ser manejadores de archivos, sockets de red, manejadores de bases de datos, objetos GDI... En resumen, cosas de las que tenemos una cantidad finita y por tanto necesitamos poder controlar su uso. El aspecto de estar "ligado al ámbito" significa que la vida del objeto está atada al ámbito de una variable, por lo que cuando la variable sale de ese ámbito, el destructor liberará el recurso. Una propiedad muy útil de esto es que mejora considerablemente la seguridad ante excepciones. Por ejemplo, comparemos esto:

```cpp
RawResourceHandle* handle = createNewResource();
handle->performInvalidOperation();  // Ups, lanza una excepción
...
deleteResource(handle); // oh no, nunca se llama, así que el recurso se pierde (fuga de recursos)
```

Con la versión que usa RAII:

```cpp
class ManagedResourceHandle {
public:
   ManagedResourceHandle(RawResourceHandle* rawHandle_) : rawHandle(rawHandle_) {};
   ~ManagedResourceHandle() { delete rawHandle; }
   ... // se omiten operadores como operator*, etc.
private:
   RawResourceHandle* rawHandle;
};

ManagedResourceHandle handle(createNewResource());
handle->performInvalidOperation();
```

En este último caso, cuando se lanza la excepción y se desenrolla la pila, las variables locales se destruyen, lo que asegura que nuestro recurso se libere adecuadamente y no se pierda.