---
title: Guía 7
description: Buscar titulo
---

# Guia 7 Soluciones

## Factorial de un número

```asm
    .data

msg1: .asciz "El factorial de %d es %d\n"
    .text
    .global main

main:
    @ Descendiente
    MOV R0, #1 // Direccionamiento directo
    MOV R1, #5 
    MOV R2, #2 
    MOV R3, R1 // Direccionamiento por registro

    loop:
        CMP R1, R2
        BLT endloop // if R2 < 2

        MUL R0,  R1 // R0 = R0 x R1
        SUB R1, #1 // R1 = R1 - 1

        B loop // if R2 >= 2, repetir loop

    endloop:
        MOV R1, R3
        MOV R2, R0
        LDR R0, =msg1 // Cargar en R0 la cadena
        BL printf // imprimir

    exit:
        MOV R7, #1
        SWI 0

```

## Divisiones

```asm
  .data 
msg1: .asciz "El divisor es cero\n"
msg2: .asciz "La división de %d entre %d es: %d\n"
msg3:  .asciz "Cociente: %d y residuo: %d\n"

cociente: .word 0
residuo: .word 0

  .text
  .global main
main:
     MOV  R1,#27 //Dividendo
     MOV  R2,#4  //Divisor

     MOV  R3,#0  //inicia el cociente
     MOV  R4,R1  //Colocar el dividendo en R4

div:
     CMP R2, #0
     BEQ divisor_cero
     CMP    R4,R2 //Compara el dividendo y el divisor 
     SUBGE  R4,R4,R2 //si R4 mayor o igual a R2  (R4=R4-R2)
                    //Si el dividendo es mayor o igual 
                    //que el divisor restarle el divisor 
     ADDGE   R3,R3,#1 //si R4 mayor o igual que R2 (R3=R3+1)
                     //aumenta el cociente

     BGE     div      //Si R4  mayor igual R2 repetir el ciclo 
     MOV     R0,R3    // colocar el cociente R0

imprimir:
     @ LDR R8, =cociente
     @ LDR R9, =residuo
     STR R3, [R8, #4]
     STR R4, [R9, #4]
     @ STR R3, [R8]
     @ STR R4, [R9]
     LDR R0, =msg2
     BL printf

     LDR R0, =msg3
     LDR R1, [R8, #4]
     LDR R2, [R9, #4]
     BL printf

exit:
     MOV R7,#1
     SVC 0

divisor_cero: 
     LDR R0, =msg1
     BL printf
     B exit

```