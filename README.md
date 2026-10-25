# Actividad 2: Apuntadores aplicados al recorrido de tokens en C

* **Matrícula:** 10240040
* **Nombre:** VANESSA RODRIGUEZ MARTINEZ
* **Grupo:** 10SC151
* **Asignatura:** Programación de Sistemas
* **Fecha:** 25 de octubre de 2026

---

## 1. Objetivo de la Práctica
Desarrollar y comprender el uso de apuntadores, paso por referencia, operadores de desreferencia y cursores en C11 mediante el recorrido seguro, modificación y búsqueda en un flujo de tokens representado en estructuras, sin realizar análisis léxico ni sintáctico.

---

## 2.
> **Nota:** Los tokens fueron escritos manualmente; el programa no realiza análisis léxico ni sintáctico.

---

## 3. Resumen de Operadores y Conceptos Clave

| Operador / Concepto | Explicación Breve |
| :--- | :--- |
| **`&`** | Operador de dirección. Obtiene la posición de memoria de una variable. |
| **`*`** | Operador de desreferencia / Indirección. Accede o modifica el valor ubicado en la dirección apuntada. |
| **`->`** | Operador de miembro a través de apuntador. Equivale a `(*p).campo` para acceder a una estructura. |
| **`const`** | Calificador de tipo que garantiza que los datos apuntados son de solo lectura y no serán modificados. |
| **`NULL`** | Constante de apuntador nulo (dirección 0) que representa la ausencia de dirección válida o finalización. |
| **`Token **`** | Apuntador doble. Permite a una función recibir la dirección de un cursor y modificar la posición original del llamador. |

---

## 4. Comandos de Compilación y Ejecución

```bash
# Compilación estricta
gcc -std=c11 -Wall -Wextra -Wpedantic src/punteros_tokens.c -o punteros_tokens

# Ejecución normal y guardado de salida
./punteros_tokens > evidencias/salida-final.txt

# Compilación y ejecución con Sanitizadores (AddressSanitizer y UBSan)
gcc -std=c11 -Wall -Wextra -Wpedantic -fsanitize=address,undefined -g src/punteros_tokens.c -o punteros_tokens_san
./punteros_tokens_san > evidencias/sanitizadores.txt 2>&1

## 5. Respuestas a las Preguntas de Razonamiento
Checkpoints 1 y 2: ¿qué diferencia existe entre p_linea, *p_linea y &linea_actual, por qué p_linea = 2 sería incorrecto y por qué avanzar_linea puede modificar la variable original?
&linea_actual obtiene la dirección, p_linea la almacena y *p_linea accede al entero. Asignar p_linea = 2 sería incorrecto porque cambiaría la dirección del apuntador en lugar del valor. Modifica la variable original al recibir su dirección mediante un parámetro de tipo int *.
Checkpoint 2: ¿por qué avanzar_linea debe comprobar NULL antes de utilizar *linea?
NULL indica que el apuntador no señala un objeto válido en memoria. Comprobarlo antes de usar *linea evita un fallo de seguridad o Segmentation fault al intentar desreferenciar una dirección inexistente.
Checkpoint 3: ¿qué función cumple \0, qué expresa const char* y por qué no debe modificarse un literal de cadena?
El carácter \0 actúa como señal de alto que marca el final de una cadena. const char * expresa que la función observará los datos de solo lectura sin modificarlos. Los literales no deben alterarse para evitar comportamientos indefinidos.
Checkpoint 4: ¿cuándo se utiliza punto, cuándo se utiliza -> y qué impide const Token * dentro de mostrar_token?
El punto (.) se usa con estructuras directas y la flecha (->) con apuntadores a estructura. const Token * impide que la función modifique los datos del token, garantizando la seguridad de solo lectura.
Checkpoint 5: ¿cómo calcula la cantidad sizeof flujo / sizeof flujo[0] y por qué fin puede compararse, pero no desreferenciarse?
Divide los bytes totales del arreglo entre los bytes de un solo elemento para obtener la cantidad de registros. fin señala el espacio posterior al último elemento; puede compararse para los límites, pero no desreferenciarse al no ser un token válido.
Checkpoint 6: ¿por qué contar_tipo puede mover una copia de inicio, mientras avanzar_cursor necesita const Token **? Explica &cursor, *cursor, ((*cursor)++) y el uso de (void).
contar_tipo solo lee datos, mientras avanzar_cursor modifica el cursor original usando un apuntador doble (const Token **). &cursor da la dirección de la variable, *cursor accede al original, ((*cursor)++) desplaza su posición y (void) descarta el valor devuelto.
Checkpoints 7 y 8: ¿qué debes comprobar antes de utilizar el resultado de buscar_primero y por qué corregir_lineas recibe Token* en lugar de const Token *?
Se debe comprobar que el resultado no sea NULL antes de acceder a sus campos para evitar fallos de memoria. corregir_lineas recibe un apuntador mutable (Token *) porque su objetivo explícito es modificar el campo de la línea.
Cierre: ¿cómo se relaciona el cursor con el consumo de tokens de un analizador sintáctico y por qué esta práctica no construye un analizador léxico ni sintáctico?
El cursor simula cómo un analizador sintáctico conserva la posición de lectura sobre el flujo de tokens. No construye un analizador real porque no procesa código fuente, expresiones regulares ni gramáticas, limitándose al manejo seguro de apuntadores.
## 6. Resultados de las Cinco Pruebas
Prueba 1 (Compilación estricta): Éxito. GCC compila src/punteros_tokens.c sin errores ni advertencias con -std=c11 -Wall -Wextra -Wpedantic.
Prueba 2 (Ejecución normal): Éxito. Genera evidencias/salida-final.txt
Prueba 3 (Casos frontera): Éxito. avance_nulo=0, vacio=0, nulo=0, avance_en_fin=0 e intervalo_vacio=0 retornan los valores esperados de forma segura sin accesos indebidos.
Prueba 4 (Sanitizadores): Éxito. evidencias/sanitizadores.txt finaliza con estado_sanitizadores=0 confirmando la ausencia de fugas, desbordamientos o comportamientos indefinidos.
Prueba 5 (Regresión): Éxito. Recompilación y verificación completa confirmando consistencia total en los resultados y estados de salida en 0.
## 7. Repositorio GitHub
https://github.com/Vanessa16-dh/actividad02-apuntadores-tokens.git

