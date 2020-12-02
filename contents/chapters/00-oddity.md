# Números Inteiros Ímpares 

O seguinte método serve para determinar quando um número fornecido é um número ímpar. Será que este método vai funcionar?

```java
public static boolean isOdd(int i) {
	return i % 2 == 1;
}
```

### Solução 

Um número ímpar pode ser definido por um número inteiro que é divisível por 2 com resto 1. A expressão ` i % 2` computa o resto da divisão por 2, então é de se imaginar que esse método funcionará. Infelizmente não! Ele retorna valores incorretos um quarto das vezes em que é executado.

Mas por que um quarto? Porque metade dos números inteiros são negativos, e nosso método `isOdd` falha para todos os números ímpares inteiros negativos. Ele retorna `false` quando é invocado com qualquer valor negativo, mesmo sendo ímpar.

Isto é uma consequência da definição do operador de resto do Java `%`, que foi definido para satisfazer a seguinte definição para todos os valores inteiros `a` e todos os valores diferentes de zero `b`:

```java
(a / b) * b + (a % b) == a
```

Em outras palavras, se você dividir `a` por `b`. multiplicar o resultado por `b`, e adicionar o resto, você estará de volta a estaca zero [JLS 15.17.3](https://docs.oracle.com/javase/specs/jls/se7/html/jls-15.html#jls-15.17.3). Esta definição faz total sentido, mas em combinação com o operador de `truncating` do Java [JLS 15.17.2](https://docs.oracle.com/javase/specs/jls/se7/html/jls-15.html#jls-15.17.2), isso implica que **quando o resto de uma operação retorna um valor diferente de zero, ela tem o mesmo sinal que seu operando à esquerda.**  
Nosso método `isOdd` e a definição do termo ìmpar na qual ele é baseado assume que todos os restos são positivos. Embora essa divisão faça sentido para alguns tipos de divisão, a operação de resto do Java é por correspondentemente compatível com sua operação de divisão inteira, que descarta a parte fracionária de seu resultado.

Quando `i` é um número inteiro negativo, `i % 2` é igual a -1 ao invés de 1, então o método `isOdd` incorretamente retorna `false`. Para prevenir este tipo de comportamento surpresa, **teste isso seus métodos se comportam corretamente quando passados ​​valores negativos, zero e positivos para cada parâmetro numérico.** O problema é facilmente corrigido quando se compara `i % 2` à `0` ao invés de `1`, e revertendo o senso da comparação: 

```java
public static boolean isOdd(int i){
	return i % 2 != 0;
}
```

Também é possível usar uma versão mais performática com o operador *bitwise* `AND` - `&` - no lugar do operador de resto `%`: 

```java
public static boolean isOdd(int i){
	return (i & 1) != 0;
}
```

Esta segunda versão é executada mais rapidamente do que a primeira, dependendo da plataforma e da máquina virtual que você estiver usando.

Em resumo, pense nos sinais dos operandos e do resultado sempre que você usa o operador de resto. O comportamento deste operador é óbvio quando seu operandos são não negativos, mas não é tão óbvio quando um ou ambos os operandos são negativos.




