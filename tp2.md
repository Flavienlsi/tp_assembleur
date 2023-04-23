# tp2_assembler

1. Comprendre

a)
```
   cases	|   Hex	   |	  Binaire
   x3000            x5020               101000000100000
   x3001	    x2406		10010000000110
   x3002	    x2606		10011000000110
   x3003	    x1002		1000000000010
   x3004	    x16FF		1011011111111
   x3005	    x03FD		1111111101
   x3006	    x3003		11000000000011
   x3007	    xF025		1111000000100101
   x3008            x000A		1010
   x3009   	    x0005		0101
   x300A	    x0009		1001
```

b) 
```
           1 : x3000
	   2 : x3001
	   3 : x3002
	   4 : x3003
	   5 : x3004
	   6 : x3005
	   7 : x3003
	   8 : x3004
           9 : x3005
           10 : x3003
           11 : x3004
           12 : x3005
           13 : x3003
           14 : x3004
           15 : x3005
           16 : x3003
           17 : x3004
           18 : x3005
           19 : x3006
           20 : x3007
          
	  Le compteur ordinal ne prend donc que 20 valeurs dans ce programme car il s'arrête à x3007.
```
	
	c) Ce programme stocke le résultat de la multiplication 10 fois 5 dans la case x300A.


2.

a)
```
	.orig x3000

	;####################### func_max #######################
	;## calcule RESULATm = 0 si VALEUR1m >= VALEUR2m ##
	;## RESULATm = 1 si VALEUR1m < VALEUR2m ##
	;########################################################

	FUNC_MAX ;LABEL alias d'adresse de la première instruction de la fonction f
	
	AND R0, R0, #0 
	ADD R0, R0, #7
	ST R0, VALEUR1m 
	AND R0, R0, #0   
	ADD R0, R0, #5  
	ST R0, VALEUR2m 

	LD R0, VALEUR1m
	LD R1, VALEUR2m

	NOT R1, R1
	ADD R1, R1, #1

	AND R3, R3, #0
	ADD R0, R0, R1
	BRZP POSITIF

	ADD R3, R3, #1
	ST R3, RESULTATm
	BRNZP FIN
```
POSITIF ST R3, RESULTATm
```
	;espace mémoire variables func_autre
	VALEUR1a .fill #25
	VALEUR2a .fill #10
	RESULTATa .blkw #1
FIN
        .END
```


b)
```
	.orig x3000
	;####################### func_autre #####################
	;## Mon autre fonction ##
	;########################################################
FUNC_AUTRE ;LABEL alias d'adresse de la première instruction de la fonction

	LD R0, VALEUR1a
	LD R1, VALEUR2a
	ST R0, VALEUR1m
	ST R1, VALEUR2m
	JSR FUNC_MAX
	LD R0, RESULTATm
    	ST R0, RESULTATa
	HALT ; Arrêter l'execution ici

	;####################### func_max #######################
	;## calcule RESULATm = 0 si VALEUR1m >= VALEUR2m ##
	;## RESULATm = 1 si VALEUR1m < VALEUR2m ##
	;########################################################

FUNC_MAX ;LABEL alias d'adresse de la première instruction de la fonction f


	LD R0, VALEUR1m
	LD R1, VALEUR2m

	NOT R1, R1
	ADD R1, R1, #1

	AND R3, R3, #0
	ADD R0, R0, R1
	BRZP POSITIF

	ADD R3, R3, #1
	ST R3, RESULTATm
	BRNZP FIN_F

POSITIF ST R3, RESULTATm

FIN_F   RET

	VALEUR1m .blkw #1 ; La directive '.blkw n' résèrve n block de mémoire
	VALEUR2m .blkw #1
	RESULTATm .blkw #1


	;espace mémoire variables func_autre
	VALEUR1a .fill #25
	VALEUR2a .fill #10
	RESULTATa .blkw #1

	.END
```

c) a. (l'exo n'est pas fini)

		.orig x3000

		LEA R1, TABLEAU 
		AND R2, R2, #0 


		LD R3, TAILLE_TABLEAU
		NOT R3, R3 
		ADD R3, R3, #1 


BOUCLE_TABLEAU	ADD R4, R2, R3
BRZP FIN

		AND R5, R5, #0
		ADD R5, R2, #0
		NOT R5, R5
		ADD R5, R5, #1
		ST R1, MEMORY

BOUCLE_INTERNE  AND R1, R1, #0

		LD R1, MEMORY
		ADD R5, R5, #1
		BRZP BOUCLE_TABLEAU
		
		LDR R0, R1, #0 
		ADD R1, R1, #1 
		ADD R2, R2, #1 
		BRNZP BOUCLE_TABLEAU


FIN 		HALT


		ADRESSE_TABLEAU_I .blkw #1
		COMPTEUR_PARCOURU_I .blkw #1
		TAILLE_TABLEAU .fill #10
		TABLEAU .fill #5
		.fill #2
		.fill #6
		.fill #1
		.fill #7
		.fill #2
		.fill #8
		.fill #22
		.fill #1
		.fill #12


		TRIE_TABLEAU .blkw #60

		MEMOIRE .blkw #1
		.END


	b)

        

	c)




BONUS)


1)

```
        .orig x3000

        AND R2, R2, #0; Caractère de la chaine qui va être comparé
        AND R3, R3, #0; Compteur d'occurence
        LD R4, CHAR
        LEA R0, LOREM
        
LOOP	LDR R2, R0, #0
        ADD R0, R0, #1
        
	NOT R1, R2
	ADD R1, R1, #1
	ADD R1, R1, R4
	BRNP NO_INCR
        ADD R3, R3,  #1

NO_INCR	ST R0, VERIF
        LDR R6, R0, #0 ; Si LDR est vide on quitte le prog
        BRZ FIN
        BRNZP LOOP

FIN	ST R3, CMPT

	CHAR .STRINGZ "r" ; caractère dont on souhaite compter le nombre d'itératio
	LOREM .STRINGZ "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."
	CMPT .FILL #0 ; espace mémoire pour stocker le résultat
        VERIF .FILL #0 ; verif si on a fini le tableau

	HALT
	.END
```

2)
```
        .orig x3000
        LD R4, CHAR
        LEA R0, LOREM
     
LOOP    AND R2, R2, #0
        LDR R2, R0, #0
        ADD R0, R0, #1

	NOT R1, R2
	ADD R1, R1, #1
	

        AND R2, R2, #0
        LD R2, TMP1

        ADD R1, R1, R4
	BRNP NO_INCR
        ADD R2, R2,  #1


NO_INCR	ST R0, VERIF
        ST R2, TMP1
        AND R2, R2, #0
        LDR R2, R0, #0
        BRZ FIN
        BRNZP LOOP

FIN     AND R2, R2, #0
        LD R2, TMP1
        ST R2, CMPT

	CHAR .STRINGZ "r" ; caractère dont on souhaite compter le nombre d'itératio
	LOREM .STRINGZ "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."
	CMPT .FILL #0 ; espace mémoire pour stocker le résultat
        VERIF .FILL #0 ; verif si on a fini le tableau
        TMP1 .FILL #0 ; variable temporaire pour stocker les registres
	HALT
	.END
```