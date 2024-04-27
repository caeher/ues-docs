---
title: Laboratorios
description: Laboratorios
---

## Laboratorio 1

Ejercicio: Escriba un programa en ensa,blador que realice el cálculo del cuadrado y el cubo de un numero entreo ingresado por el usuario.
  Al final debe mostrar al usuario un mensaje descriptivo que incluya el número ingresado y el resultado de los cálculos. Los mensajes de entrada y salida deben ser
  claros y aportar toda la información posible al usuario (nombres de variables, fórmulas de cálculos realizados, entre otros).
 

```asm
/*
  Archivo: eh18010_lab1.s
  
  UNIVERSIDAD DE EL SALVADOR
  FACULTAD DE INGENIERIA Y ARQUITECTURA
  Autor: Cristian Antonio Escalante Hernandez
  Carnet: EH18010

  Indentación: 4 espacios

  Formulas matematicas
  cuadrado = lado * lado
  cubo = lado * lado * lado

  msg{n} -> mensaje{número}
  len_msg{n} -> ancho del mensaje
  fmt{n} -> formato{número}
 */

    .data

msg1:   .asciz  "**** Programa que calcula el cuadrado y el cubo de un número ingresado. ****\n" // 
len_msg1=.-msg1  // ancho del msg1

msg2:   .asciz  "Ingrese un número: "
len_msg2=.-msg2 // ancho del msg2

msg3:   .asciz  "\nEl número ingresado es: %d\nEl cuadrado de este número es: %d\nEl cubo de este número es: %d\n"

numero: .word 0 // variable para almacenar el numero ingresado
fmt1:   .asciz  "%d" // variable para formato de cadenas

    .text // Sección de código
    .global main // etiqueta de la función principal

main: // etiqueta de la función principal
    PUSH {LR} // Guardar el valor de LR en la pila

    @ Salida en pantalla del primer mensaje
    MOV R7, #4 // Syscall para imprimir 
    MOV R0, #0 // Monitor = 0
    LDR R1, =msg1 // cargar la direccion de R1 = &msg1
    LDR R2, =len_msg1 // cargar la direccion de R1 = &len_msg1
    SVC 0 // Llamada al sistema

    @ Salida en pantalla del segundo mensaje
    MOV R7, #4 // Syscall para imprimir
    MOV R0, #0 // Monitor = 0
    LDR R1, =msg2 // cargar la direccion de R1 = &msg1
    LDR R2, =len_msg2 // cargar la direccion de R1 = &len_msg1
    SVC 0 // Llamada al sistema

    @ Lectura del número usando scanf
    LDR R0, =fmt1 // Cargar en R0 = &fmt1
    LDR R1, =numero // Cargar en R1 = &numero
    BL scanf // Llamada a la función scanf

    @ Calculo del cuadrado y del cubo
    LDR R1, =numero // Cargar en el registro R1 = &numero
    LDR R1, [R1] // Cargar en R1 = *R1
    MUL R2, R1, R1 // Calculo del cuadrado R2 = R1 * R1
    MUL R3, R2, R1 // Calculo del cubo R3 = R2 * R1 = (R1 * R1) * R1

    @ Salida en pantalla del tercer mensaje
    LDR R0, =msg3 // Cargar en R0 = &msg3
    BL printf // Llamada a la función printf

    POP {PC} // Restaurar el valor de LR en la pila y salto a la dirección de retorno

exit: // Etiqueta
    MOV R7, #1  // Syscall para terminar el programa
    SWI 0 // Llamada al sistema

```

## Laboratorio 2

No existe

## Laboratorio 3

Pregunta 1.

  Cree un programa en ensamblador ARM, que deteermine si una figura es un Cuadrado o
  un Rectángulo a partir de dos de sus lados, se debe de solicitar al usuario 2 numeros positivos (b y h)
  que representaran la base y la altura respectivamente.
  Recordar que en un Cuadrado a = b, debe de imprimir el valor de b, h y el tipo de figura.

  El programa debe de validar que los números ingresados sean positivos.
  
```asm
/*
  Archivo: eh18010_lab1.s
  
  UNIVERSIDAD DE EL SALVADOR
  FACULTAD DE INGENIERIA Y ARQUITECTURA
  Autor: Cristian Antonio Escalante Hernandez
  Carnet: EH18010

  Indentación: 4 espacios

 */

    .data
@ Definición de variables
msg_validacion: .asciz "El número ingresado debe ser positivo mayor que 0.\n"

msg1: .asciz "El valor de la base(b) es %d y de la altura(h) es %d\n"
msg2:   .asciz  "**** Programa que identifica el tipo de figura entre (cuadrado y rectángulo) a partir de la base y la altura. ****\n" 
cuadrado: .asciz "La figura es un cuadrado\n"
rectangulo: .asciz "La figura es un rectángulo\n"

msg_base: .asciz "Ingresar el valor de la base(b): "
msg_altura: .asciz "Ingresar el valor de la altura(h): "


fmt1:   .asciz  "%d" @ variable para formato de cadenas
md: .asciz "%d\n"
altura: .word 0 @ Variable donde se va a guardar la altura
base: .word 0  @ Variable donde se va a guardar la base

    .text @ Sección de código
    .global main @ etiqueta de la función principal

main: @ etiqueta de la función principal

    LDR R0, =msg2 @ Cargar en R0 la dir de msg2 R0 = &msg2 (primer parametro para printf)
    BL printf @ llamar a la funcion printf

    _pedir_base:
        LDR R0, =msg_base @ Cargar en R0 la dir de msg_base R0 = &msg_base
        BL printf @llamar a la funcion printf
        LDR R0, =fmt1 @ Cargar en R0 la dir de fmt1 R0 = &fmt1 (primer parametro de scanf)
        LDR R1, =base @ Cargar en R1 la dir de base R1 = &base (Segundo parametro scanf (Variable donde se almacena))
        BL scanf @ llamar a la funcion scanf

        LDR R1, =base @ R1 = &base
        LDR R1, [R1] @ R1 = *base

        CMP R1, #1 
        BLT _error_base @ if (R1 < #1 ) Ejecurar _error_base

    _pedir_altura:
        LDR R0, =msg_altura @ Cargar en R0 la dir de msg_altura R0 = &msg_altura
        BL printf @ Imprimir
        LDR R0, =fmt1 @ Cargar en R0 la dir de fmt1 R0 = &fmt1 (primer parametro de scanf, formato de cadena)
        LDR R1, =altura @ Cargar en R1 la dir de altura R1 = &altura (Segundo parametro scanf (Variable donde se almacena))
        BL scanf @ llamar a la funcion scanf

        LDR R1, =altura @ R1 = &altura
        LDR R1, [R1] @ R1 = *altura

        CMP R1, #1
        BLT _error_altura @ if (R1 < #1 ) Ejecurar _error_altura

    LDR R1, =base @ R1 = &base
    LDR R1, [R1] @ R1 = *base

    LDR R2, =altura @ R1 = &altura
    LDR R2, [R2] @ R1 = *altura

    LDR R0, =msg1 @ Cargar en R0 la dir de msg1 R0 = &msg1
    BL printf @ llamar a la funcion printf

    LDR R1, =base @ R1 = &base
    LDR R1, [R1] @ R1 = *base

    LDR R2, =altura @ R1 = &altura
    LDR R2, [R2] @ R1 = *altura
    
    CMP R1, R2

    BEQ _cuadrado @ if (R1 == R2) es cuadrado
    BNE _rectangulo @ if (R1 != R2) es rectángulo

    B exit
_cuadrado:
    LDR R0, =cuadrado
    BL printf
    B exit

_rectangulo:
    LDR R0, =rectangulo
    BL printf
    B exit

exit: @ Etiqueta
    MOV R7, #1  @ Syscall para terminar el programa
    SWI 0 @ Llamada al sistema

_error_base:
    LDR R0, =msg_validacion  @ Cargar en R0 la dir de msg_validacion R0 = &msg_validacion
    BL printf @ llamar a la funcion printf
    B _pedir_base @ salto a _pedir_base

_error_altura:
    LDR R0, =msg_validacion @ Cargar en R0 la dir de msg_validacion R0 = &msg_validacion
    BL printf @ llamar a la funcion printf
    B _pedir_altura @ salto a _pedir_altura

```

## Laboratorio 4

  Escriba una función que reciba como parametros un arreglo de números enteros y un número a buscar, 
  devolverá la dirección de memoria en que se encuentra el elemento buscado o 0 en caso de no encontrarlo

```asm
/*
  Archivo: laboratorio4.s
  
  UNIVERSIDAD DE EL SALVADOR
  FACULTAD DE INGENIERIA Y ARQUITECTURA
  Autor: Cristian Antonio Escalante Hernandez
  Carnet: EH18010

 */
  .include "in_array.s"

    .data

arreglo: .word 3, -4, 1, -4, 6, 5

numero: .word 6
fmt: .asciz "%d\n"
msg1: .asciz "El número %d no se encuentra en el arreglo.\n"
msg2: .asciz "El número se encontró en la dirección de memoria %p y contiene el valor de %d\n"

    .text @ Sección de código
    .global main @ etiqueta de la función principal

main: @ etiqueta de la función principal

  PUSH {LR}

  LDR R0, =arreglo
  LDR R1, =numero 
  BL buscar_numero

  CMP R0, #0
  BEQ no_encontrado

  imprimir:
    MOV R1, R0
    MOV R2, R0
    LDR R0, =msg2
    BL printf
    B exit
  no_encontrado:
    LDR R0, =msg1 
    LDR R1, =numero
    LDR R1, [R1]
    BL printf
    
  exit:
    POP {PC}
    MOV R7, #1
    SWI 0

```