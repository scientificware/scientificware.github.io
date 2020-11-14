# Types entiers

3 Systèmes de numération :
- binaire, sous Java le nombre binaire 10001101 est saisi avec la notation 0b10001101.
- décimale, 
- et hexadécimale, sous Java le nombre hexadécimal ff est saisi avec la notation 0xff.

Les différents types entiers se distinguent par la taille de leur représentation.
- `byte` : (1 octet ) entiers compris entre -128 et +127 (-2^7 et 2^7-1)
- `short` : (2 octets ) entiers compris entre -32 768 et +32 767 (-2\15 et 2^15-1)
- `int` : (4 octets ) entiers compris entre -2 147 483 648 et +2 147 483 647 (-2^31 et 2^31-1)
- `long` : (8 octets ) entiers compris entre -9 223 372 036 854 775 808 et +9 223 372 036 854 775 807 (-2^63 et 2^63-1)

Attention, ne pas confondre la valeur décimale, hexadécimale ou binaire avec l'objet qu'il permet de représenter. Cela peut-être le cas si l'objet à représenter est justement un nombre. Examinons par exemple le codage de entiers relatifs. Mais nous pouvons aussi coder des entiers naturels (bien que ce ne soit pas une construction de base dans Java).

La notation binaire permet de coder des entiers relatifs. Par exemple si le nombre de bits disponibles permet de coder ou d'ordonnés 2N objets,
- les nombres ⟦-N;-1⟧ sont codés à partir de 2N-1 à rebours avec -1 associé à 2N-1,
- et les nombres ⟦0;N-1⟧ sont codés normalement, avec naturellement 0 associé à 0.

Petits passages délicats entre les différents types.
- Sur 8 bits, c'est à dire au format `byte`, `-115` est codé par **10001101** en binaire. Le bit de gauche codant le signe : `0` pour `+` et `1` pour `-`.
- Sur 16 bits, c'est dire au format `short`, `-115` est codé par 11111111 **10001101** en binaire. Le bit de gauche codant le signe : `0` pour `+` et `1` pour `-`. pour obtenir l'écriture 16 bits à partir de l'écriture 8 bits, il suffit de compléter l'écriture littérale 8 bits à gauche avec huit 1.
- Sur 32 bits, c'est à dire au format `int`, `-115` est codé par 11111111 11111111 11111111 **10001101** en binaire. Comme précédemment, on complète l'écriture 8 bits à gauche avec vingt-quatre 1.


Petits tests avec `jshell` :
```
jshell> int b = 0b10001101
b ==> 141
jshell>  byte c = (byte)0b10001101
c ==> -115
jshell> short d = (short)0b10001101
d ==> 141
jshell> short e = (short)0b1111111110001101
e ==> -115
jshell> int f = (int)0b11111111111111111111111110001101
f ==> -115
```

Pour rétablir l'identité entre code et entier naturel, on peut faire un `&` bitwise sur le nombre de bit que l'on souhaite. Voir [Opérateurs Bit à Bit et Logiques](https://docs.oracle.com/javase/specs/jls/se15/jls15.pdf#%5B%7B%22num%22%3A6687%2C%22gen%22%3A0%7D%2C%7B%22name%22%3A%22XYZ%22%7D%2C72%2C251%2Cnull%5D).
```
jshell> int g = (int)0b11111111111111111111111110001101 & 0b11111111
g ==> 141
jshell> Integer.toBinaryString(141)
$79 ==> "10001101"
```