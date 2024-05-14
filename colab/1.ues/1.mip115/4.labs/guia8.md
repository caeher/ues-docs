---
title: Guía 8
description: Buscar titulo
---

# Guia 8 Soluciones

## Practica de include

### factorial.s

```asm
  .data

msg: .asciz "El factorial de %d es %d\n"

.text

.global main 

main:

    PUSH {lr}
    MOV R0, #0
    MOV R4, R0
    BL factorial

    MOV R2, R0
    MOV R1, R4  
    LDR R0, =msg 
    BL printf 

    POP {pc}

_exit: 
    MOV R7, #1
    SWi 0

factorial.s

.global factorial

factorial:
    MOV R1, #1
    _loop:
        CMP R0, #0 
        BEQ return
        MUL R1, R0, R1 
        SUB R0,R0,#1 
        BAL _loop 

    return:
        MOV R0, R1 
        BX lr
```

### Utilizar factorial.s

```asm
include "factorial.s"

    .data

msg: .asciz "El factorial de %d es %d\n"

    .text

    .global main

main:

  PUSH {LR}
  MOV R0, #0
  MOV R4, R0
  BL factorial

  MOV R2, R0
  MOV R1, R4
  LDR R0, =msg
  BL printf

  POP {PC}

_exit:
  MOV R7, #1
  SWI 0
```