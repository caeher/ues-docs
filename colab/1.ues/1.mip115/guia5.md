---
title: Guía 5
description: Buscar titulo
---

# Guia 5 Soluciones

## area de un cuadrado

```asm
    .data

msg1: .asciz "Ingrese el lado del cuadrado: "
len_msg1=.-msg1

fmt1: .asciz "%d"
lado: .word 0

msg2: .asciz "El lado del cuadrado es %d y su area %d\n"
len_msg2=.-msg2

    .text
    .global main

main: 
    PUSH {LR}

    // pedir numero
    MOV R7, #4 // syscal escritura
    MOV R0, #0 // monitor = 0
    LDR R1, =msg1
    LDR R2, =len_msg1
    @ BL printf

    SVC 0

    // Lectura del numero
    MOV R7, #3 // syscall de lectura
    MOV R0, #0 // monitor = 0
    LDR R0, =fmt1
    LDR R1, =lado
    BL scanf

    @ SVC 0

    @ MOV R7, #4
    @ MOV R0, #0
    @ LDR R1, =msg2
    @ LDR R2, =len_msg2
    @ LDR R3, =lado // R1 = &numero
    @ LDR R3, [R3] // R1 = *R1
    @ MUL R4, R3, R3 

    @ SVC 0

    LDR R0, =msg2
    LDR R1, =lado // R1 = &numero
    LDR R1, [R1] // R1 = *R1
    MUL R2, R1, R1 

    BL printf
    
    POP {PC}

exit:
    MOV R0, #1
    SWI 0

```

## Área de un rectangulo

```asm
/*
  Archivo: area_rectangulo.s
  @descripcion Crear un programa en ensamblador que realice el calculo del area de un rectangulo, debe de solicitar dos numeros enteros (base y altura)

  y debe de mostrar un mensaje con los resultados y los valores de entrada.
 */

.data
  msg1: .asciz "Ingrese la base del rectangulo: "
  msg2: .asciz "Ingrese la altura del rectangulo: "
  msg3: .asciz "El area del rectangulo es: %d"
  msg4: .asciz "La base del rectangulo es: %d"
  msg5: .asciz "La altura del rectangulo es: %d"

  fmt1: .asciz "%d"

  base: .word 0
  altura: .word 0
  .text
  .global main

main:
  PUSH {LR} // Guardar el valor de LR en la pila

  // Imprimir primer mensaje
  MOV R7, #4 // System call para imprimir
  MOV R0, #0 // Salida a monitor = 0
  LDR R1, =msg1 // Mensaje a imprimir
  MOV R2, #100 // Tamaño del mensaje

  SVC 0 // Llamada al sistema (ejecutar syscall)

  // leer el numero de base
  MOV R7, #3 // System call para leer
  LDR R0, =fmt1 // Entrada estandar = 0
  LDR R1, =base // Formato de lectura
  BL scanf // Llamada a la funcion scanf

  LDR R5, =base
  LDR R5, [R5]

  // Imprimir segundo mensaje
  MOV R7, #4 // System call para imprimir
  MOV R0, #0 // Salida a monitor = 0
  LDR R1, =msg2 // Mensaje a imprimir
  MOV R2, #100 // Tamaño del mensaje

  SVC 0 // Llamada al sistema (ejecutar syscall)

  // leer el numero de altura
  MOV R7, #3 // System call para leer
  LDR R0, =fmt1 // Entrada estandar = 0
  LDR R1, =altura // Formato de lectura
  BL scanf // Llamada a la funcion scanf

  LDR R4, =altura
  LDR R4, [R4]
  @ SVC 0 

  // Imprimir cuarto mensaje
  LDR R0, =msg4
  LDR R1, [R4]

  BL printf

  // Imprimir quinto mensaje
  LDR R0, =msg5
  LDR R1, [R5]

  BL printf

  // Imprimir tercer mensaje
  LDR R0, =msg3
  MUL R1, R4, R5

  BL printf

  POP {PC} // Restaurar el valor de LR y regresar al main

exit:
  MOV R7, #1
  SWI 0

```

## Ejercicio 2

```asm
  .data

msg1: .asciz "Ingrese un numero: "
len_msg1=.-msg1

fmt1: .asciz "%d"
numero: .word 0

msg2: .asciz "Doble: %d \nMitad: %d \nCubo: %d \n"

  .text
  .global main
main:

  PUSH {LR}

  MOV R7, #4
  MOV R0, #0
  LDR R1, =msg1
  LDR R2, =len_msg1

  SVC 0

  LDR R0, =fmt1
  LDR R1, =numero 
  BL scanf 

  LDR R0, =msg2
  LDR R1, =numero
  LDR R4, [R1]
  MOV R1, R4, LSL #1
  MOV R2, R4, ASR #1
  MUL R3, R4, R4
  MUL R3, R3, R4
  BL printf

  POP {PC}

exit:
  MOV R7,#1
  SWI 0

```

## Ejercicio 4

```asm
    .data
msg1: .asciz "Ingrese la base del rectangulo: "
len_msg1=.-msg1

msg2: .asciz "Ingrese la altura del rectangulo: "
len_msg2=.-msg2

fmt1: .asciz "%d"
base: .word 0
altura: .word 0

msg3: .asciz "Base: %d\nAltura: %d\nArea: %d\n"

    .text
    .global main

main: 
    PUSH {LR}

    MOV R7, #4
    MOV R0, #0
    LDR R1, =msg1
    LDR R2, =len_msg1

    SVC 0

    LDR R0, =fmt1
    LDR R1, =base 
    BL scanf

    MOV R7, #4
    MOV R0, #0
    LDR R1, =msg2
    LDR R2, =len_msg2

    SVC 0

    LDR R0, =fmt1
    LDR R1, =altura 
    BL scanf

    LDR R0, =msg3
    LDR R1, =base
    LDR R1, [R1]
    LDR R2, =altura 
    LDR R2, [R2]
    MUL R3, R1, R2 

    BL printf

    POP {PC}
exit:
    MOV R7, #1
    SWI 0

```

## Notas

2. Al compilar se agregará el parámetro -g el cual le indica al compilador que mantenga toda la
información de depuración disponible para que gdb pueda trabajar con el código fuente
original.
$ gcc -g -o intro intro.s
3. Iniciar el depurador con el comando gdb
$ gdb intro
Obtendremos una pantalla similar a esta

4. Estamos en el modo interactivo de gdb. En este modo, te comunicas con gdb usando
comandos. Hay un comando de ayuda incorporado llamado help. O puede consultar la
documentación del depurador GNU http://sourceware.org/gdb/current/onlinedocs/gdb/
5. Para empezar la depuración debemos iniciar el programa con el comando start

Esto nos dice que gdb ejecutó nuestro programa hasta la primera instrucción de la función main.
6. Habilitaremos la sección de código fuente y registros. Para esto, escribir el siguiente
comando:
tui reg general
7. Para ejecutar una instrucción a la vez utilizamos el comando stepi. Al realizar el comando
stepi se ejecuta la línea actual y nos muestra la siguiente línea a ejecutar y se actualiza el
valor de PC (program counter). Se puede observar que se ha resaltado la nueva línea a
ejecutar, en este caso la línea 24

8.En la sección de registros podemos ver cómo se van cambiando los valores a medida que
vamos ejecutando el programa. Observe por ejemplo que el contenido de R1 ahora es
0x21024, el cual es la dirección de var1

9. Revisemos la variable var1, para eso utilizaremos el comando print ( o su forma abreviada
p). Para ver la dirección de memoria de una variable se utiliza el operador & (similar a c)

10. Para ver el contenido de la memoria utilizamos el comando x de examine

El resultado se muestra por defecto en hexadecimal, para verlo en otro formato utilizamos
modificadores. La sintaxis del comando es:
x/FMT ADDRESS
F, es el formato de la salida: Algunos de los valores posibles son x(hex), d(decimal), u (unsigned
decimal), t (binary), c (char), s (string).
M, b (byte), h (halfword), w (word), g (giant, 8 bytes).
T, es la cantidad de objetos a mostrar.
Para nuestro caso sabemos que la variable var1 es decimal de tamaño word, el comando podríamos
ponerlo así: x/dw 0x21024

11. Para ejecutar la siguiente línea volvemos a utilizar el comando stepi o su forma abreviada si
12. Se puede agregar puntos de interrupción, con el comando break o b
Por ejemplo para agregar un punto de interrupción en la línea 33 escribimos b 33

Podemos agregar todos los puntos de control que necesitemos. Para ver los puntos colocados
ponemos: info b
Para borrar un punto de interrupción se utiliza el número identificador que aparece del lado
izquierdo y el comando delete o d, en este caso tenemos dos puntos de interrupción
identificados con los números 4 y 5. Se borra el 4 con el comando d 4

13. Para ejecutar el código desde el punto actual hasta el final del programa o hasta el siguiente
punto de interrupción, se utiliza el comando continue o c
14. Para salir usamos el comando quit

