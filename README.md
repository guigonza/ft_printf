# ft_printf (42 School)

Implementación en C de una versión reducida de la función estándar `printf`, creada como proyecto de la cursus de 42 School. El objetivo es replicar el comportamiento de `printf` para un conjunto de especificadores, devolver el número de bytes escritos y asegurar un código limpio, portátil y bien documentado.

## Contenido

- `ft_printf.c` — entrada y lógica principal del parser/dispatcher.
- `ft_printf_utils.c` — funciones auxiliares para imprimir caracteres, strings, números y hexadecimales.
- `ft_printf.h` — prototipos, includes y defines públicos.
- `Makefile` — reglas para compilar, limpiar y comprobar el proyecto.

## Características y alcance

Esta implementación soporta los especificadores más comunes requeridos por la evaluación de 42:

- `%c` : carácter
- `%s` : cadena de caracteres (si es `NULL` imprime `(null)`)
- `%p` : puntero (si es `NULL` imprime `(nil)`; si no, `0x` + valor en hexadecimal)
- `%d` / `%i` : enteros con signo
- `%u` : enteros sin signo
- `%x` / `%X` : enteros en hexadecimal (minúsculas / mayúsculas)
- `%%` : imprime el carácter `%`

Limitaciones conocidas:

- No se implementan flags, ancho de campo, precisión ni length modifiers (`l`, `h`, etc.).
- No existe gestión avanzada de errores en llamadas a `write` (se asume éxito).

## Compilación

El repositorio incluye un `Makefile` con al menos los objetivos básicos estándar. Para compilar:

```bash
make
```

Para limpiar objetos y binarios generados:

```bash
make fclean
```

Para recompilar desde cero:

```bash
make re
```

Si el `Makefile` requiere un nombre de salida específico (por ejemplo `libftprintf.a` o `ft_printf`), ajusta los objetivos según la convención del repositorio.

## Uso básico

Incluye la cabecera y llama a `ft_printf` igual que a `printf`. La función devuelve el número de bytes escritos (int), por ejemplo:

```c
#include "ft_printf.h"

int main(void)
{
	int n;

	n = ft_printf("Hola %s, num: %d, hex: %x\n", "mundo", 42, 255);
	/* n contendrá el número de caracteres escritos */
	(void)n;
	return 0;
}
```

Ejemplo de salida esperada:

```
Hola mundo, num: 42, hex: ff
```

## Contrato de la API

- Entrada: cadena de formato y lista variable de argumentos (igual que `printf`).
- Salida: escribe a `stdout` y devuelve un `int` con el número total de bytes escritos.
- Errores: comportamiento no definido para errores de I/O; el proyecto asume que `write` tiene éxito.

Edge cases cubiertos:

- `NULL` en `%s` imprime `(null)`.
- `NULL` en `%p` imprime `(nil)`.

## Estructura de implementación (resumen técnico)

- `ft_printf` itera sobre la cadena de formato. Cuando encuentra `%` delega a una función de conversión que consume el especificador y los argumentos.
- Se usan funciones auxiliares para escribir caracteres, strings y números, implementadas con `write`.
- Los números se convierten a cadenas mediante recursión o con algoritmos iterativos en base 10/16 según corresponda.
- El contador de bytes escritos se va acumulando y se devuelve al final.

## Recomendaciones de pruebas

- Añade un pequeño `main` de pruebas que cubra cada especificador y casos límite (NULL, 0, valores negativos, valores grandes).
- Comparar salida y valor devuelto con la `printf` estándar para casos permitidos.

Ejemplo de test manual rápido:

```bash
# compilar el binario de prueba desde un archivo test_main.c que llame a ft_printf
cc -Wall -Wextra -Werror ft_printf.c ft_printf_utils.c test_main.c -o test_printf
./test_printf
```

## Estilo y buenas prácticas

- Mantener funciones pequeñas y con responsabilidad única.
- Documentar las funciones en los headers.
- Evitar variables globales y usar `static` para funciones internas cuando sea apropiado.

## Contribución

Si quieres mejorar el proyecto considera:

- Añadir soporte para flags, ancho y precisión (avance incremental y tests).
- Añadir un conjunto de tests automatizados (scripts o un pequeño runner en C/Python).
- Añadir comprobaciones de retorno de `write` y manejo apropiado de errores.

## Licencia y autor

Este README y el código son parte del repositorio privado de práctica del autor; ajusta la sección de licencia si vas a abrir el código públicamente.

---

