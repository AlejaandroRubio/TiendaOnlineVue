# Archivo de Respuesta

## 1. ¿Cómo observar cambios en una propiedad anidada en un objeto reactivo?

En Vue 3, cuando utilizamos reactive(), este crea un objeto proxy que permite detectar cambios en sus propiedades de nivel superior. Sin embargo, Vue no detecta automáticamente cambios en propiedades anidadas dentro de un objeto reactivo. Esto ocurre porque Vue solo rastrea el acceso directo a las propiedades de nivel superior en reactive(), pero no observa automáticamente cambios dentro de objetos internos o anidados.

Para detectar estos cambios, debemos utilizar watch() con la opción { deep: true }. Esta opción le dice a Vue que revise todas las propiedades internas del objeto y reaccione a cualquier modificación dentro de él.

Por ejemplo, si tenemos un objeto reactivo con información de un usuario, como su nombre y detalles personales, y queremos observar cambios dentro de detalles, lo haríamos así:

```ts
import { reactive, watch } from "vue";

const usuario = reactive({
  nombre: "Carlos",
  detalles: {
    edad: 30,
    direccion: "Calle 123",
  },
});
// Watch para detectar cambios en cualquier propiedad anidada dentro de `usuario`
watch(
  usuario,
  (nuevoValor) => {
    console.log("Cambio detectado en usuario:", nuevoValor);
  },
  { deep: true }
);
```

Ahora, si cambiamos usuario.detalles.edad = 31, la función watch() se activará y mostrará el mensaje en la consola. Sin { deep: true }, Vue no detectaría estos cambios en detalles.

## 2. ¿Cómo funciona watch() en reactive()?

watch() es una función que nos permite ejecutar código en respuesta a cambios en una variable reactiva. Funciona observando un valor y ejecutando una función cuando ese valor cambia. En el caso de reactive(), watch() no puede observar el objeto completo a menos que usemos { deep: true }. Sin embargo, podemos observar una propiedad específica usando una función que la retorne.

Si queremos observar solo una propiedad específica dentro de un objeto reactivo, lo hacemos así:

```ts
import { reactive, watch } from "vue";

const producto = reactive({
  nombre: "Laptop",
  precio: 1000,
  stock: 10,
});

// Se observa solo el precio del producto
watch(
  () => producto.precio,
  (nuevoValor, viejoValor) => {
    console.log(`El precio cambió de ${viejoValor} a ${nuevoValor}`);
  }
);
```

Aquí, watch() se activará únicamente cuando el precio cambie. Por ejemplo, si ejecutamos producto.precio = 1200, se imprimirá en la consola "El precio cambió de 1000 a 1200".

Si en cambio queremos observar cualquier cambio dentro del objeto, utilizamos { deep: true }:

```ts
watch(
  producto,
  (nuevoValor) => {
    console.log("Cambio detectado en el producto:", nuevoValor);
  },
  { deep: true }
);
```

Esto detectará cualquier cambio dentro de producto, ya sea que se modifique nombre, precio o stock.

Usamos watch() en lugar de computed cuando necesitamos ejecutar acciones en respuesta a cambios en los datos, como hacer una petición a una API cuando una propiedad cambia.

## 3. ¿Cómo hacer que watch() detecte cambios en stock dentro de un array de productos?

Cuando trabajamos con arrays de objetos reactivos, Vue no detecta automáticamente los cambios dentro de los objetos internos. Aunque podemos modificar las propiedades de los elementos dentro del array, Vue solo reaccionará a estos cambios si usamos watch() con { deep: true }.

Imaginemos que tenemos un array de productos donde cada producto tiene nombre, precio, stock y disponible. Queremos asegurarnos de que, cuando stock llegue a 0, la propiedad disponible cambie automáticamente a false.

Podemos hacer esto con watch(), observando todo el array con { deep: true }:

```ts
import { reactive, watch } from "vue";

const productos = reactive([
  { nombre: "Laptop", precio: 1000, stock: 5, disponible: true },
  { nombre: "Mouse", precio: 50, stock: 0, disponible: false },
  { nombre: "Teclado", precio: 80, stock: 3, disponible: true },
]);

// Detectar cambios en cualquier propiedad dentro del array de productos
watch(
  productos,
  (nuevosProductos) => {
    nuevosProductos.forEach((producto) => {
      producto.disponible = producto.stock > 0;
    });
  },
  { deep: true }
);
```

Con este código, si reducimos productos[0].stock a 0, su disponible cambiará automáticamente a false.

Si no usamos { deep: true }, Vue solo detectaría cambios cuando se reemplace el array completo, por ejemplo, al hacer productos = [...productos], pero no cuando se modifique una propiedad dentro de un objeto dentro del array.
