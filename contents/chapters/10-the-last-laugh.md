# Quem Ri Por Último...

Considere o seguinte programa: 

```java
public class LastLaugh{
	public static void main(String[] args){
		System.out.println("H" + "a");
		System.out.println('H' + 'a');
	}
}
```
Agora me diga, o que ele vai imprimir? Se você é como a maioria das pessoas, você talvez tenha pensado em: "Haha". Parece como se concatenasse **H** a **a** de duas maneiras diferentes, mas as aparências enganam. Se você executou o exemplo, você descobriu que ele imprime **Ha169**. Agora, por que ele faria uma coisa dessas?
Como esperado a primeira chamada a `System.out.println` imprime "**Ha**": Os argumentos passados são `"H" + "a"`, o que é uma concatenação normal. Até aqui nada de novo! A segunda chamada a `System.out.println` é outra história. Os argumentos passados na segunda chamada são `'H' + 'a'`. O problema é que aquele **'H'** e aquele **'a'** são `chars` literais. Por conta dos dois não serem do tipo `String`, o operador de adição `+` executa uma adição ao invés de uma concatenação normal. 
O compilador avalia a expressão `'H' + 'a'` promovendo cada um dos valores do tipo `char` para valores inteiros do tipo primitivo `int`, ou seja, a literal **'H'** se torna **72** e no caso **'a'** se torna **97**. Então a expressão `'H' + 'a'` é igual a `72 + 97`, ou **169**.

Então como concatenar `chars`? Você pode optar por usar as bibliotecas do próprio Java como a classe `StringBuffer` por exemplo:

```java 
StringBuffer sb = new StringBuffer();
sb.append(’H’);
sb.append(’a’);
System.out.println(sb);
```
Isso funciona mas é quase uma agressão aos olhos de tão feio. Para evitar toda essa verbosidade você pode forçar o operador de adição `+` a executar uma concatenação normal ao invés de uma adição de inteiros passando pelo menos um dos operandos como uma `String`. O idioma para escrever isso seria: 

```java 
System.out.println("" + 'H' + 'a');
```

Contudo ainda assim fica meio confuso entender essa abordagem deselegante. Talvez a melhor maneira seja usar outro recurso da plataforma introduzido no Java 5 chamado `printf` passando especificadores de formato `%c` como em: 

```java 
System.out.printf("%c%c", ’H’, ’a’);
```

Em resumo, use o operador de concatenação de string com cuidado. **O operador + executa concatenação de string se e somente se pelo menos um de seus operandos for de tipo String**; caso contrário, executa a adição. Se nenhum dos valores for concatenados são strings, você tem várias opções: 

1. prefixar a `string` vazia; 
2. converter o primeiro valor para uma `string` explicitamente, usando `String.valueOf`; 
3. usar uma `StringBuffer`; 
4. ou use o recurso `printf`.



