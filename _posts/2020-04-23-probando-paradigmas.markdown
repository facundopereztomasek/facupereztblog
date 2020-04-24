---
layout: post
title:  "Probando paradigmas"
date:   2020-04-23 19:58:17 -0300
categories: programación
---
Buenas noches,

Ultimamente estoy metiéndole mano a Haskell, porque una de las materias de la carrera de ciencias de la computación,
taller de álgebra, va a usar a Haskell para (supongo yo, todavía no la cursé) resolver problemas.

Y se me ocurrió la idea de comparar un lenguaje de paradigma funcional con uno procedural.
Elegí Haskell y C.

## Lo que hice

La verdad no tiene mucho sentido lo que hice, tengo una lista de libros (que es una mezcla de libros con películas).
La idea era ver:
  - qué implicaba definir esa lista de libros
  - cómo las podía filtrar
  - cómo mostrarlo en pantalla


### Código en C

```c++
#include <stdio.h>

struct Libro
{
  int id;
  float precio;
  char nombre[50];
};

int main() {
  struct Libro libros[2] = {
    { 0, 18.50, "El señor de los anillos"},
    { 1, 14.0, "Matrix"},
  };

  int i;
  int n_libros = sizeof(libros) / sizeof(libros[0]);

  for (i = 0; i < n_libros; i++) {
    printf("\nLibro %s, vale %f", libros[i].nombre, libros[i].precio);
  }

  printf("\n\nLibros con precio mayor a 15.00:\n");

  struct Libro librosFiltrados[0];
  int n_librosFiltrados = 0;

  for (i = 0; i < n_libros; i++) {
    if (libros[i].precio >= 15.00) {
      librosFiltrados[n_librosFiltrados] = libros[i];
      n_librosFiltrados += 1;
    }
  }

  for (i = 0; i < n_librosFiltrados; i++) {
    printf("\nLibro %s, vale %f", librosFiltrados[i].nombre, librosFiltrados[i].precio);
  }

  return 0;
}
```

### Código en Haskell

```hs
data Libro = Libro {
  id :: Int,
  precio :: Float,
  nombre :: String
}

libros = [
    Libro 0 18.50 "El señor de los anillos",
    Libro 1 14.50 "Matrix"
  ]

librosFiltrados = filter (\libro -> precio libro >= 15) libros

main = do
  mapM_ (\libro -> putStrLn ("Libro " ++ nombre libro ++ ", vale " ++ show (precio libro) )) libros
  putStrLn "\nLibros con precio mayor a 15.00:\n"
  mapM_ (\libro -> putStrLn ("Libro " ++ nombre libro ++ ", vale " ++ show (precio libro) )) librosFiltrados
```

### Descargo
No se enojen los profesionales de C ni los de Haskell. Aunque a lo mejor con un enemigo en común se amigan entre paradigmas.
No tengo idea de las buenas prácticas de cada lenguaje.

### Lo que pude ver
Fue que:
- Se nota la diferencia entre decirle a la compu cómo hacer algo (procedural) y qué es algo (funcional).
- También se nota la diferencia en el tamaño del código.
- En teoría hay diferencia entre la velocidad de uno y otro siendo el procedural más rápido. Te lo dejo a vos para que lo corrobores.
- El paradigma funcional, al ser el programa una composición de funciones (funciones aplicadas a otras aplicadas a otras....), es un poco molesto a la hora de imprimir cosas en pantalla. No se si es que todavía hay algo que no entiendo o si realmente hay un tema ahí.
- En procedural tuve que definir la dimensión de las cosas, cosa que no tuve que hacerlo en funcional.
- No esta en el código, pero cuando quise modificar el nombre de un libro en C tuve que hacer un strcpy en lugar de mandar directamente el "=". Eso medio que me hinchó las pelotas.
- No termino de entender Mónadas, que aparentemente es un patrón de diseño del paradigma funcional. Por lo tanto para que me funcione el asunto tuve que hacer un mapM_, que transforma el resultado del map en mónada y además, como tiene guión bajo, no devuelve algo que no entendí, pero funcionó al pelo.
- El temita de castear un Float a String con show me pareció medio raro, espero que haya otra forma.
- No me fijé pero a lo mejor hay una forma de hacer print interpolando en Haskell, aunque no se como eso encajaría dentro de una función.
- Bueno me acabo de fijar y parece que hay un package que se llama string-interpolate que debe parsear la jodita.

### Conclusiones
Sería bueno poder tener una base de experimentación e hipótesis más sólidas antes de escribir una entrada del blog, pero es lo que hay.
Gana el funcional, es más divertido.

Facu