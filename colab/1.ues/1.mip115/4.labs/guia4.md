---
title: Guía 4
description: Buscar titulo
---

# Guia 4 Soluciones

## Arbol

```asm
section .data
    tree db '   X   ', 0
    stump db 'XXXXXXX', 0

section .text
    global _start

_start:
    ; Imprimir la parte superior del árbol
    mov ecx, 7 ; Número de ramas en la parte superior
    mov esi, 0 ; Contador de ramas impresas
    top:
        ; Imprimir espacios en blanco antes de la rama
        mov eax, 3 ; Tamaño de cada rama
        sub eax, ecx ; Calcular el número de espacios en blanco
        shr eax, 1 ; Dividir por dos para centrar la rama
        mov ebx, eax ; Guardar el número de espacios en ebx
        spaces:
            mov eax, 4 ; Función para imprimir caracteres
            mov ebx, 1 ; Descriptor de archivo (stdout)
            mov ecx, ' ' ; Carácter a imprimir
            int 0x80 ; Llamada al sistema
            dec ebx ; Decrementar el contador de espacios
        jnz spaces ; Imprimir todos los espacios necesarios
        ; Imprimir la rama
        mov eax, 4
        mov ebx, 1
        mov ecx, tree
        int 0x80
        ; Imprimir espacios en blanco después de la rama
        mov ebx, eax ; Guardar el tamaño de la rama
        mov eax, 3 ; Tamaño de cada rama
        sub eax, ebx ; Calcular el número de espacios en blanco
        shr eax, 1 ; Dividir por dos para centrar la rama
        mov ebx, eax ; Guardar el número de espacios en ebx
        more_spaces:
            mov eax, 4
            mov ebx, 1
            mov ecx, ' '
            int 0x80
            dec ebx
        jnz more_spaces
        ; Imprimir nueva línea
        mov eax, 4
        mov ebx, 1
        mov ecx, 0x0A
        int 0x80
        ; Incrementar el contador de ramas impresas
        inc esi
        ; Decrementar el número de ramas que quedan por imprimir
        dec ecx
    jnz top ; Imprimir todas las ramas

    ; Imprimir el tronco del árbol
    mov eax, 4
    mov ebx, 1
    mov ecx, stump
    int 0x80

    ; Salir del programa
    mov eax, 1
    xor ebx, ebx
    int 0x80

```

## Prueba de salida y entrada básica

```asm
/*
  Archivo: inout.s
  Descripción: Prueba de salida y entrada básica
  Autor: Cristian Antonio Escalante Hernandez
  Fecha: 2023-03-29
  Notas:
    Se utilizó tabulación de 2 espacios
 */

 /* El inge recomienda usar asciiz */

 // ****************** SECCION DE DATOS

  .data
msj1:       .asciz  "Ingrese su nombre: "
len=.-msj1
nombre:     .skip     100
msj2:       .ascii    "Ingrese un numero entero: \000" // ascii se le tiene que poner fin de cadena
msj3:       .asciz   "Su numbre es: %s\nEl cuadrado de %d es: %d\n: "
fmt1:       .asciz   "%d"
numero:     .word     0

/*
Repasar el ejercicio 3 y 4
*/

 // ****************** SECCION DE CODIGO

  .text
  .globl main

main: 
  PUSH {LR} // Guarda el valor de LR en la pila


  // ******** TRABAJANDO CON SYCALL
  MOV R7, #4
  MOV R0, #0 // 0 = MONITOR
  LDR R1, =msj1
  MOV R2, #20
  SVC 0

  // Leer desde el teclado
  MOV R7, #3 // codigo de la syscall 3 = read
  // Los parametros se comienzan a poner en R0
  MOV R0, #0 // 0 = teclado 
  LDR R1, =nombre
  MOV R2, #100 //
  SVC 0 // hacer la llamada


  // ******** 2) Utilizando funciones de C */
  /* 2.1) mostrar un mensaje utilizando printf
    printf("Ingree un numero entero: ");
    printf(R0);
   */
   LDR R0, =msj2 // Direccion de memoria de msj2
   BL printf // Llamada a la funcion printf

  /*  2.2) Leer un numero entero
    scanf("%d", &numero);;
    scanf(R0, R1);
    */
  LDR R0, =fmt1 // Direccion de memoria de fmt1
  LDR R1, =numero // Direccion de memoria de numero
  BL scanf // Llamada a la funcion scanf
  
  // Calculos
  LDR R2, =numero // Cargo la direccion de memoria de 'numero'
  LDR R2, [R2] // Leo valor; r2 = *r2
  MUL R3, R2, R2 // R2 = R2 * R2

  LDR R0, =msj3
  LDR R1, =nombre
  BL printf

  POP {PC} // Carga el valor de LR en PC y LR

exit:
  MOV R7, #1
  SWI 0

```

## Definiciones basicas de variables y punteros

```asm
/*
  Archivo: intro.s
  Descripcion: Definiciones basicas de variables y punteros
 */

  .data // Seccion para definir variables (puede ir al final del archivo)
// Formato: nombre_variable: tipo valor_inicial

var1: .word 3 // Definir variable de 32 bits (una palabra)
var2: .byte 4 // Definir variable de 8 bits (un byte)
var3: .word 0 

.equ DOS, 2 // Definir una constante

// Valores iniciales en otras bases
varHex: .word 0x1234 // valor inicial hexadecimal (0X)
varBin: .word 0b1010 // valor inicial binario (0b)
varOct: .word 01234 // valor inicial octal (0)

  .text // Seccion para definir codigo (puede ir al principio del archivo)
  .globl main // Indica que la funcion main es global

main: 
  // Punteros forma 1, acceder a la variable a traves de un puntero declarado
  LDR R1, puntero_var1 // Cargar la direccion de memoria en R1, R1 = &var1
  LDR R1, [R1] // Cargar en R1, el valor apuntado por R1, R1 = *R1

  @ Punteros forma 2, acceder a la variable a traves de un puntero declarado
  LDR R2, =var2 // R2 = &var2
  LDR R2, [R2] // R2 = *R2

  LDR R3, puntero_var3 // R3 = &var3
  ADD R0, R1, R2 // R0 = R1 + R2
  STR R0, [R3] // *R3 = R0 Guardar el valor de R0 en la direccion apuntada por R3

  ADD R0, R0, #DOS // Al utilizar una constante el compilador la sustituye por su valor.

  exit: // Etiqueta para salir del programa (return 0;)
    MOV R7, #1 
    SWI 0 // SVC es equivalente a SWI

  puntero_var1: .word var1
  puntero_var2: .word var2
  puntero_var3: .word var3

```

