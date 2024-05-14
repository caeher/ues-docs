---
title: Guía 9
description: Buscar titulo
---

# Guia 9 Soluciones

## Indexación sin actualizar

```asm
/* desripción: programa para mostrar el manejo de arreglos 
Modo de indexacion sin actualizacion
*/
        .data
a:              .skip 40 // Reservar 40 bytes para un arreglo de 10 enteros 
fmt:          .asciz     "%d\n"

                 .text 
                 .global main
main:
         PUSH    {lr}
         LDR      R4, =a      //Dirrección base del arreglo 
         MOV     R5,#0      // inicializar indice del arreglo 

ciclo: 
        CMP R5,#10        // Comparar el indice con el limite (10 enteros)
        BEQ  finCiclo       //Si el contador del indice ya llego a 10, _salir
        @ ADD R6, R4,R5, LSL #2    
        /*R6=R4(R5*4)
                                                     calcular la dirreción del elemento del arreglo a partir de la 
                                                     y el indice
                                                     */
        MUL R7,R5,R5                         //Calcular el cuadrado del numero 
        //STR  R7,[R6]                             //Guarda el valor en la dirrección calculada 
        STR R7, [R4,+R5, LSL #2]
                                                  // en la linea 19
        ADD  R5,#1                             // Aumenta el contador del indice 
        B       ciclo                                // Regresa al inicio del ciclo
finCiclo:
            MOV R5,#0
cicloImpresion: 
            LDR   R0,=fmt
            CMP R5,#10
            BEQ finCicloImpresion
            @ ADD R6,R4,R5,LSL #2              //Calcula la dirrección del elemento del
                                                                 // del arreglo, tal como se hizo en la linea 19
            @ LDR R1,[R6]                               // Carga el valor del elemento del arreglo
            LDR R1, [R4, +R5, LSL #2]
            BL printf 
            ADD R5, #1                                 // aumenta el contador del indice 	
            B   cicloImpresion                       // volver al inicio del ciclo
 finCicloImpresion:
              POP {pc}

fin:          
              MOV R7,#1 
              SWI 0


```

## Indexación con actualización

```asm
/* desripción: programa para mostrar el manejo de arreglos 
Modo de indexacion con actualizacion
*/
        .data
a:              .skip 40 // Reservar 40 bytes para un arreglo de 10 enteros 
fmt:          .asciz     "%d\n"

                 .text 
                 .global main
main:
         PUSH    {lr}
         LDR      R4, =a      //Dirrección base del arreglo 
         MOV     R5,#0      // inicializar indice del arreglo 

ciclo: 
        CMP R5,#10        // Comparar el indice con el limite (10 enteros)
        BEQ  finCiclo       //Si el contador del indice ya llego a 10, _salir
        @ ADD R6, R4,R5, LSL #2    
        /*R6=R4(R5*4)
                                                     calcular la dirreción del elemento del arreglo a partir de la 
                                                     y el indice
                                                     */
        MUL R7,R5,R5                         //Calcular el cuadrado del numero 
        //STR  R7,[R6]                             //Guarda el valor en la dirrección calculada 
        @ STR R7, [R4,+R5, LSL #2]
        STR R7, [R4], #+4
        ADD  R5,#1                             // Aumenta el contador del indice 
        B       ciclo                                // Regresa al inicio del ciclo
finCiclo:
        MOV R5,#0
        LDR R4, =a                                          // en la linea 19
cicloImpresion: 
            LDR   R0,=fmt
            CMP R5,#10
            BEQ finCicloImpresion
            @ ADD R6,R4,R5,LSL #2              //Calcula la dirrección del elemento del
                                                                 // del arreglo, tal como se hizo en la linea 19
            @ LDR R1,[R6]                               // Carga el valor del elemento del arreglo
            @ LDR R1, [R4, +R5, LSL #2]
            LDR R1, [R4], #+4
            BL printf 
            ADD R5, #1                                 // aumenta el contador del indice 	
            B   cicloImpresion                       // volver al inicio del ciclo
 finCicloImpresion:
              POP {pc}

fin:          
              MOV R7,#1 
              SWI 0

```

## Manejo de arreglos

```asm
/* desripción: programa para mostrar el manejo de arreglos 
*/
        .data
a:              .skip 40 // Reservar 40 bytes para un arreglo de 10 enteros 
fmt:          .asciz     "%d\n"

                 .text 
                 .global main
main:
         PUSH    {lr}
         LDR      R4, =a      //Dirrección base del arreglo 
         MOV     R5,#0      // inicializar indice del arreglo 

ciclo: 
        CMP R5,#10        // Comparar el indice con el limite (10 enteros)
        BEQ  finCiclo       //Si el contador del indice ya llego a 10, _salir
        ADD R6, R4,R5, LSL #2    /*R6=R4(R5*4)
                                                     calcular la dirreción del elemento del arreglo a partir de la 
                                                     y el indice
                                                     */
        MUL R7,R5,R5                         //Calcular el cuadrado del numero 
        STR  R7,[R6]                             //Guarda el valor en la dirrección calculada 
                                                   // en la linea 19
        ADD  R5,#1                             // Aumenta el contador del indice 
        B       ciclo                                // Regresa al inicio del ciclo
finCiclo:
            MOV R5,#0
cicloImpresion: 
            LDR   R0,=fmt
            CMP R5,#10
            BEQ finCicloImpresion
            ADD R6,R4,R5,LSL #2              //Calcula la dirrección del elemento del
                                                                 // del arreglo, tal como se hizo en la linea 19
            LDR R1,[R6]                               // Carga el valor del elemento del arreglo
            BL printf 
            ADD R5, #1                                 // aumenta el contador del indice 	
            B   cicloImpresion                       // volver al inicio del ciclo
 finCicloImpresion:
              POP {pc}

fin:          
              MOV R7,#1 
              SWI 0


```