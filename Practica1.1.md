## Ejercicios Practica 1 EC
### Ejercicio 1
Desarrollar un programa en ensamblador del 8086 que a partir de una cadena numérica, que viene definida por 4 dígitos (cada dígito va ocupar una posición de memoria y con valores 0 ó 1), introducida por teclado, calcule, almacene en memoria y muestre en pantalla el valor decimal equivalente de la cadena según los siguientes sistemas de representación: Binario natural sin signo y Complemento a 1. Mostrar un esquema de los mapas de direcciones del sistema computador, indicando sus partes claramente diferenciadas.

```asm
.model small

.data
    cadena db 5,0,0,0,0,0,0
    peso db 8, 4, 2, 1
 
.code
    mov ax, seg cadena ;inicializa el valor del segmento de datos
    mov ds, ax ; segmentacion
   
    mov dx, offset cadena ;captura de cadena de caracteres en variable cadena
    mov ah, 0ah
    int 21h
   
    sub cadena [2],48  
    sub cadena [3],48
    sub cadena [4],48
    sub cadena [5],48    
   
    mov al, 0
    mov bl, cadena[5]
    sub al, bl
    mul peso[3]  
    mov al, cadena[4]
    mul peso[2]  
    sub bl,al
    mov al, cadena[3]
    mul peso[1]
    sub bl,al
    mov al, peso[0]
    dec al
    mul cadena[2]
    add al,bl
   
    cmp cadena[2],0
    je pos
    jne nega
   
pos:    
    
    mov bl,10  
    div bl
    
    mov cl, al ; en cl guardamos el cociente
    add cl, 48
    mov ch, ah ; en ch guardamos el resto
    add ch,48

    mov al, 03h;inicializacion segmento extra
    mov ah, 00h
    int 10h
    
    mov ax, 0B800h
    mov es,ax
    mov dl, 0 ; inicializacion registro indexacion
    mov ah, 00001111b ;formato del caracter
    mov al, cl; //caracter a escribir
    mov es:[DI],AX ;escritura del caracter
    mov al, ch; ;caracter a escribir
    mov es: [DI+2], ax; escritura del carácter

    mov ah, 00h
    int 16h
   



nega:  
    mov bl,10  
    div bl
    mov cl, al ; en cl guardamos el cociente
    add cl, 48
    mov ch, ah ; en ch guardamos el resto
    add ch,48

    mov al, 03h;inicializacion segmento extra
    mov ah, 00h
    int 10h
    
    mov ax, 0B800h
    mov es,ax
    mov dl, 0 ; inicializacion registro indexacion
    mov ah, 00001111b ;formato del carácter
    mov al, 45  ; signo menos en ascii
    mov es:[DI],AX
    mov al, cl; //caracter a escribir
    mov es: [DI+2],ax ;escritura del caracter
    mov al, ch; ;caracter a escribir
    mov es: [DI+4], ax; escritura del carácter

    mov ah, 00h
    int 16h


    
   
    
	cmp cadena[2],0
        je posi
        jne negat

posi:
	mov al, cadena[5]
	mul peso[3]  
	mov bl, al
	mov al, cadena[4]
        mul peso[2]  
	add bl,al
	mov al, cadena[3]	
	mul peso[1]
	add al,bl
	
	mov bl,10  
    div bl
	mov cl, al ; en cl guardamos el cociente
	add cl, 48
   	mov ch, ah ; en ch guardamos el resto
    	add ch,48

    	mov al, 03h;inicializacion segmento extra
    	mov ah, 00h
    	int 10h
    
    	mov ax, 0B800h
    	mov es,ax
   	mov dl, 0 ; inicializacion registro indexacion
    	mov ah, 00001111b ;formato del caracter
    	mov al, cl; //caracter a escribir
    	mov es: [DI],ax ;escritura del caracter
    	mov al, ch; ;caracter a escribir
    	mov es: [DI+2], ax; escritura del carácter

    	mov ah, 00h
    	int 16h

		
        


    
negat:
	mov al,0
	mov bl, cadena[5]
	sub al,bl
	mul peso[3]  
	mov bl, al
	mov al, cadena[4]
        mul peso[2]  
	sub bl,al
	mov al, cadena[3]	
	mul peso[1]
	sub bl,al	
	mov al, peso[0]
	mul cadena[2]
	add al,bl
	mov bl, 10 

    	div bl
    	mov cl, al ; en cl guardamos el cociente
    	add cl, 48
    	mov ch, ah ; en ch guardamos el resto
    	add ch,48

    	mov al, 03h;inicializacion segmento extra
    	mov ah, 00h
    	int 10h
    
    	mov ax, 0B800h
    	mov es,ax
    	mov dl, 0 ; inicializacion registro indexacion
    	mov ah, 00001111b ;formato del carácter
    	mov al,45
    	mov es: [DI],ax
    	mov al, cl; //caracter a escribir
    	mov es: [DI+2],ax ;escritura del caracter
    	mov al, ch; ;caracter a escribir
    	mov es: [DI+4], ax; escritura del carácter

    mov ah, 00h
    int 16h


    mov ah, 00h
    int 16h
   
    fin:
        mov ah, 4ch
        int 21h
    end
```
