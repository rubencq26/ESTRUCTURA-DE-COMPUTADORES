## Ejercicios Practica 1 EC
### Ejercicio 1
Desarrollar un programa en ensamblador del 8086 que a partir de una cadena numérica, que viene definida por 4 dígitos (cada dígito va ocupar una posición de memoria y con valores 0 ó 1), introducida por teclado, calcule, almacene en memoria y muestre en pantalla el valor decimal equivalente de la cadena según los siguientes sistemas de representación: Binario natural sin signo y Complemento a 1. Mostrar un esquema de los mapas de direcciones del sistema computador, indicando sus partes claramente diferenciadas.
```asm
.model small

.data
    
    cadena db 5,0,0,0,0,0,0 ;(4 bits + intro),  n real de 
    ;caracteres ,tecleados + dato1 + dato2 + dato3 + dato4 + CR 
    
    peso db 8, 4, 2, 1 ;Cadena de pesos 
 
 .code
    mov ax, seg cadena ;inicializa el valor del segmento de datos
    mov ds, ax ;segmentacion

    
    mov dx, offset cadena ; captura de caracteres en variable cadena
    mov ah, 0ah
    int 21h
                 
                 
    sub cadena[2],48 ;restar 48 para que devuelva el numero en 1 o 0 en codigo ASCII
    sub cadena[3],48
    sub cadena[4],48
    sub cadena[5],48
    
    mov al,0          ;operamos para calcular los valores en binario natural
    mov bl,cadena[5]  ;multiplicamos cada bit positivo por su peso y los sumamos
    add al,bl
    mul peso[3]
    mov bl, al
    mov al, cadena[4]
    mul peso[2]
    add bl, al
    mov al,cadena[3]
    mul peso[1]
    add bl, al
    mov al, cadena[2]
    mul peso[0]
    add al,bl         ;resultado en AL
    
    mov dh, al;guardamos el valor de al para luego
    
    mov bl,10  ;dividimos entre 10 para sacar los dos digitos
    div bl
    mov cl, al ;guardamos el cociente     
    add cl,48
    mov ch, ah ;guardamos el resto  
    add ch,48
    
    mov al, 03h;inicializacion segmento extra
    mov ah, 00h
    int 10h
    
    mov ax, 0B800h
    mov es,ax
    mov dl, 0 ;inicializacion registro indexacion
    mov ah, 00001111b ;formato del caracter
    mov al, cl;caracter a escribir
    mov es:[DI],AX ;escritura del caracter
    mov al, ch ;caracter a escribir
    mov es: [DI+2], ax; escritura del carácter

    mov ah, 00h ;pausamos el programa hasta recibir intro
    int 16h
    
    cmp cadena[2],0 ;comparamos el bit de mayor peso para saber si es negativo o positivo
    je pos
    jne nega ;ruptura de secuencia condicional

pos:    

    mov al, 03h;inicializacion segmento extra
    mov ah, 00h
    int 10h
    
    mov ax, 0B800h
    mov es,ax
    mov dl, 0 ; inicializacion registro indexacion
    mov ah, 00001111b ;formato del caracter 
    mov al, 43 ; signo mas en ascii
    mov es:[DI], AX
    mov al, cl ; carcter a escribir
    mov es:[DI + 2], ax ; escritura del caracter
    mov al, ch ; caracter a escribir
    mov es:[DI + 4], ax; escritura del caracter

    mov ah, 00h
    int 16h    ;pausamos el programa hasta recibir intro
    
    jmp fin ;ruptura de secuencia incondicional
    
nega:
    ; Calcular complemento a uno  
    mov ax, 0 
    mov bl, 15
    sub bl, dh ; dh tiene el valor original
    mov cx, 0 ;inicializamos en 0 para que devuelva 0 en la salida 
    
    add ch,48 ;sumamos 48 para el ASCII
    add bl,48
   

    ; Inicializacion para imprimir negativo
    mov al, 03h
    mov ah, 00h
    int 10h

    mov ax, 0B800h
    mov es, ax
    mov dl, 0 ; inicializacion registro indexacion
    mov ah, 00001111b ; formato del caracter
    mov al, 45 ; signo menos en ascii
    mov es:[DI], AX
    mov al, ch; caracter a escribir
    mov es:[DI + 2], ax ; escritura del caracter
    mov al, bl ; caracter a escribir
    mov es:[DI + 4], ax; escritura del caracter
    
    mov ah, 00h
    int 16h    ;pausamos el programa hasta recibir intro
  
    fin:
        mov ah, 4ch
        int 21h
    end
```


### Ejercicio 3 
Desarrollar un programa en ensamblador del 8086 que a partir de una cadena numérica, que viene definida por 4 dígitos (cada dígito va ocupar una posición de memoria y con valores 0 ó 1), introducida por teclado, calcule, almacene en memoria y muestre en pantalla el valor decimal equivalente de la cadena según los siguientes sistemas de representación: Complemento a 1 y Complemento a 2. Mostrar un esquema de los mapas de direcciones del sistema computador, indicando sus partes claramente diferenciadas. 

```asm
.model small

.data
    
    cadena db 5,0,0,0,0,0,0 ;(4 bits + intro),  n real de 
    ;caracteres ,tecleados + dato1 + dato2 + dato3 + dato4 + CR 
    
    peso db 8, 4, 2, 1 ;Cadena de pesos 
 
 .code
    mov ax, seg cadena ;inicializa el valor del segmento de datos
    mov ds, ax ;segmentacion

    
    mov dx, offset cadena ; captura de caracteres en variable cadena
    mov ah, 0ah
    int 21h
                 
                 
    sub cadena[2],48 ;restar 48 para que devuelva el numero en 1 o 0 en codigo ASCII
    sub cadena[3],48
    sub cadena[4],48
    sub cadena[5],48
    
    mov al,0
    mov bl,cadena[5]
    add al,bl
    mul peso[3]
    mov bl, al
    mov al, cadena[4]
    mul peso[2]
    add bl, al
    mov al,cadena[3]
    mul peso[1]
    add bl, al
    mov al, cadena[2]
    mul peso[0]
    add al,bl
    
    mov dh, al;guardamos el valor de al para luego
    
    
    

    
    cmp cadena[2],0 ;comparamos el bit de mayor peso para saber si es negativo o positivo
    je pos
    jne nega

pos:
    mov bl,10
    div bl
    mov cl, al ;guardamos el cociente     
    add cl,48
    mov ch, ah ; guardamos el resto  
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
    
    jmp fin
    
nega:
    ; Calcular complemento a uno  
    mov ax, 0 
    mov bl, 15
    sub bl, dh ; dh tiene el valor original
    mov cx, 0 ;inicializamos en 0 para que devuelva 0 en la salida 
    
    add ch,48 ;sumamos 48 para el ASCII
    add bl,48
   

    ; Inicializacion para imprimir negativo
    mov al, 03h
    mov ah, 00h
    int 10h

    mov ax, 0B800h
    mov es, ax
    mov dl, 0 ; inicialización registro indexación
    mov ah, 00001111b ; formato del carácter
    mov al, 45 ; signo menos en ascii
    mov es:[DI], AX
    mov al, ch; carcter a escribir
    mov es:[DI + 2], ax ; escritura del caracter
    mov al, bl ; caracter a escribir
    mov es:[DI + 4], ax; escritura del caracter
   
    mov ah,00h
    int 16h
    
    mov al,1
    add bl,al
    
        ; Inicializacion para imprimir negativo
    mov al, 03h
    mov ah, 00h
    int 10h

    mov ax, 0B800h
    mov es, ax
    mov dl, 0 ; inicialización registro indexación
    mov ah, 00001111b ; formato del carácter
    mov al, 45 ; signo menos en ascii
    mov es:[DI], AX
    mov al, ch; carcter a escribir
    mov es:[DI + 2], ax ; escritura del caracter
    mov al, bl ; caracter a escribir
    mov es:[DI + 4], ax; escritura del caracter
  
    fin:
        mov ah, 4ch
        int 21h
    end
```
