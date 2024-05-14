---
title: Guía 3
description: Buscar titulo
---

# Guia 3 Soluciones


```asm
/**
  * Created by Cristian Antonio Escalante Hernandez
  * on 19/04/2023.
  */

.data // Seccion de datos

.text // Seccion del codigo
.global _start // definir el punto de entrada

_start: // etiqueta
  MOV R0, #78 // Movel el numero 78 en el registro R0
  MOV R7, #1 // Codigo de llamada al sistema, 1 para terminar el progrma
  SWI 0 // Ejecutar la llamada al sistema, equivalente a SVC

```