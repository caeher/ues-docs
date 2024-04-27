---
title: Guía 6
description: Buscar titulo
---

# Guia 6 Soluciones

## Clasificador de número par/impar

```asm
.data

msj0:   .asciz "Ingrese el numero positivo: \n"
msj1:   .asciz "Es par\n"
msj2:   .asciz "Es impar\n"
msj4:   .asciz "%d \n"

fmt:    .asciz "%d"
num: .word 0
.text
.global main
main:
PUSH {LR}

while:
    LDR R0, =msj0
    BL printf

    LDR R0, =fmt
    LDR R1, =num
    BL scanf

    LDR R0, =num
    LDR R0, [R0]

    CMP R0, #1
    BLT while

//comparacion
LDR R0, =num
LDR R0,[R0]

and R2, R0, #1
CMP R2, #1

BLEQ impar

par:
    LDR R0, =msj1
    BL printf
    BL exit

impar:
    LDR R0, =msj2
    BL printf
    BL exit

POP {PC}
exit:
MOV R7, #1
SWI 0

```