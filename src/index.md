
# Algoritimo Knuth-Morris-Pratt


## O Problema da Busca por Padrões Textuais 


Em um mundo ditado por grandes volumes de dados, a capacidade de encontrar informações em grandes textos de forma rápida e acurada 
se torna uma habilidade algorítmica cada vez mais necessária, seja para resolver problemas da computação ou para lidar com desafios 
relacionados à biotecnologia e à segurança da informação.

Esse tipo de abordagem pode abranger desde problemas muito comuns — como contar o número de vezes que determinado termo aparece em 
um texto — até desafios mais complexos, como encontrar características únicas dentro de uma sequência de DNA.

À primeira vista, esse problema pode parecer trivial, especialmente quando pensamos em contextos simples e cotidianos. No entanto, 
recentemente, a busca por padrões foi aplicada em um feito antes inimaginável: a recriação dos lobos terríveis, realizada pela empresa 
Colossal Biosciences, por meio de uma tecnologia de bioengenharia avançada chamada CRISPR.

![](Direwolf.jpeg)


O método utilizado consistiu em edição genômica — ou seja, a identificação de fragmentos específicos na cadeia de DNA de uma espécie e 
a alteração de determinadas bases por outras possíveis, como substituir um “A” por um “T” ou um “C” por um “G”.No caso dos lobos terríveis, 
o DNA de lobos comuns foi modificado com base em informações obtidas de um fóssil preservado. Com apenas 14 alterações em 20 genes, os 
cientistas conseguiram reconstruir uma sequência genética idêntica à da espécie extinta.



## Solucionando o problema: Os Algoritimos de Knuth-Morris-Pratt e o algoritimo ingênuo


Para encontrar essas sequências específicas no DNA, é necessário utilizar algoritmos de busca por padrões. O mais simples deles é o algoritmo 
ingênuo, que compara o padrão desejado com cada parte do texto (ou DNA), letra por letra, até encontrar uma correspondência. Já o algoritmo 
Knuth-Morris-Pratt (KMP) melhora esse processo ao evitar comparações repetidas, usando informações do próprio padrão para avançar mais 
rapidamente no texto. Enquanto o método ingênuo pode ser lento em casos grandes, o KMP é muito mais eficiente, especialmente quando lidamos 
com sequências longas e repetitivas — como ocorre em genomas.

Dessa forma, precisamos entender o princípio por trás desses algoritimos para solucionarmos o problema,e, por isso, começaremos com a força
bruta do algoritimo ingênuo.

## Aprofundando no Algoritimo ingênuo


Tendo visto o problema como um todo, vamos entender o princípio básico de busca por padrões, a **"força bruta"**.
Pensemos, primeiro, no princípio mais alto nível de busca por padrões. 

??? Testando para ver se você está acordado

Dado que você quer contar quantas vezes aparece uma sílaba em um texto quaisquer, de quais informações você precisa para realizar a busca? 




::: Gabarito


A Resposta é óbvia: Precisamos apenas do **texto e do padrão buscado**. No caso do algoritimo em C, ele precisa de uma informação a mais: **O Tamanho do texto e do padrão textual buscado**



:::

???


Logo, iniciando a contrução do código em C, começamos com algo como:

``` c

void algoritimo_ingenuo(char string[], char substring[], int n, int m){
    // ???
}


```

??? Exercício Conceitual 1

Antes de inicarmos a construção do loop do código, precisamos exercitar o nosso pensamento. Dessa forma, dado as seguintes sequências de caracteres, identifique o padrão sequencial presente: 

**1.AAABBBAAABBB** <br>
**2.ABBAABBA** <br>
**3.XYYXYYYXYYY** <br> 


::: Gabarito

**Resposta 1:** AAABBB<br> 
**Resposta 2:** ABBA<br>
**Resposta 3:** XYYY<br>

:::

???


Mas o nosso objetivo aqui obviamente não é te fazer saber indentificar padrões, na verdade, é pensar em como traduzir esse 
raciocínio para o algoritimo. 

??? Exercício Conceitual 2

Dessa forma, além de pensar no padrão, quero que pense: A partir de qual linha de raciocínio podemos achar padrões da maneira 
mais acurada e demorada? 


**A)** Observar apenas se os primeiros e últimos caracteres da sequência se repetem, como em ACBA <br>
**B)** Contar quantas vezes cada letra aparece e escolher a que aparece mais <br>
**C)** Testar todas as formas possíveis de dividir a sequência em blocos e ver quais se repetem ao longo dela <br>
**D)** Olhar apenas pares de letras vizinhas, como AB, BC, CD, e tentar construir o padrão a partir deles <br>


::: Gabarito

**C)** Testar todas as formas possíveis de dividir a sequência em blocos e ver quais se repetem ao longo dela <br> 

:::

???

O Algoritimo ingênuo atua justamente nessa abordagem **força bruta**, o qual ele irá: 
- Percorrer todo o texto dado; 
- Comparar o caractere percorrido com o primeiro caractere da substring;
- Caso os caracteres sejam iguais, compara o segundo caractere da substring com o caractere percorrido, e assim sucessivamente;
- Caso o padrão da substring seja interrompido, volta ao caractere percorrido antes de encontrar alguma string da sequência. 

A Partir dessa estrutura, temos um pensamento em alto nível parecido com: 

``` c

void algoritimo_ingenuo(char string[], char substring[], int n, int m){
    // para cada i em (0, 1, 2, ..., n - m)
    //     verifica se a substring ocorre a partir da posição i
    //     para cada j em (0, 1, 2, ..., m - 1)
    //         se string[i + j] ≠ substring[j]
    //             interrompe a verificação (não é uma ocorrência)
    //     se todos os caracteres da substring foram verificados com sucesso
    //         registra a posição i como ocorrência do padrão
}


```

??? Exercício Conceitual 3

Beleza, prepare o papel e o lápis e vamos passar por uma tarefa um pouco mais trabalhosa. Dado a seguinte sequência **AABABBABA**, e o padrão desejado **BBA**,
simule os valores de i e j para cada uma das iterações do **loop interno**.

::: Gabarito

```plaintext
1:   0 0 
2:   1 0
3:   2 0
4:   2 1
5:   3 0
6:   4 0
7:   4 1
8:   4 2
9:   5 1
10:  6 0  
```
Perceba que, como a nossa sequência tem 3 caracteres, e, quando chegamos em i = 2 = m - 1, encontramos a única presença do padrão nesse trecho!

:::

???


Ao ilustrar essas iterações, seria algo como: 

:ingenuo_01


Ok, já vimos que esse algoritimo entrega acurácia, mesmo que com construções demoradas, mas então, qual o problema dele? 


??? Exercício Conceitual 4

A melhor forma de explicar isso é elevando o nível de  dificuldade. Dessa forma, dado a sequência **AAAABAAAAAABBAAB** e o padrão desejado **AAABA**, estime apenas o 
**número de iterações do loop interno**

::: Gabarito

Nessa situação teríamos **38 iterações** do loop interno, isso para um sequência de apenas 16 caracteres, o que já resultaria em um loop interno mais extenso.

:ingenuo_02

:::






??? 

Se para uma sequência de poucos caracteres já parece exaustivo, imagine para casos mais complexos, como sequências de DNA, por exemplo! Isso nos leva para o principal 
problema do algoritimo: sua complexidade no tempo. O que o torna uma alternativa pouco versátil para soluções mais robustas. 

??? Exercício Conceitual 5


Para concretizar essa linha de pensamento, a partir da descrição em alto nível, qual a complexidade do algoritimo ingênuo? 

``` c

void algoritimo_ingenuo(char string[], char substring[], int n, int m){
     para cada i em (0, 1, 2, ..., n - m)
         verifica se a substring ocorre a partir da posição i
         para cada j em (0, 1, 2, ..., m - 1)
             se string[i + j] ≠ substring[j]
                 interrompe a verificação (não é uma ocorrência)
         se todos os caracteres da substring foram verificados com sucesso
             registra a posição i como ocorrência do padrão
}


```

::: Gabarito

Como o algoritmo ingênuo compara cada posição da string principal, e em cada posição tenta casar a substring. Se a string principal tem tamanho **n** e a substring tem tamanho **m**, a complexidade no pior caso é **O(n*m)**.



:::
???



Percebe-se então, um problema bem aparente neste algoritimo, a sua redundância nas comparações. Quando uma incompatibilidade (mismatch) ocorre, o algoritmo simplesmente avança para a próxima posição na string principal e reinicia a comparação da substring do início, revisitando caracteres que já foram analisados.

Aqui esta mais uma animação para reforçar mais ainda esse defeito:

:imgs-kmp-ingenuo-1

Bom, dessa forma, fica claro que existe um problema de eficiência neste algoritimo, o que nos faz pensar, como será que podemos melhorar para que ele seja menos redundante?



## o Algoritimo melhorado: O Algoritimo KMP

Bom, já ficou bem claro que o problema do Ingênuo é que ele demora devido a redundancia de ficar voltando indices quando ocorre um mismatch dentro da mesma string. Foi na busca da solução deste problema que o KMP foi criado, agora, vamos tentar pensar e entender a solução encontrada para este problema. (dica: a resposta é mais simples do que parece)!


??? Exercicio conceitual 6

Como podemos fazer o algoritmo não perder progresso?

**A)** Pulando os caracteres que o algoritmo já sabe que estão corretos, usando informações do padrão.

**B)** Repetindo a comparação desde o início do padrão sempre que houver erro, garantindo que nenhum caractere seja ignorado.

**C)** Adicionando caracteres aleatórios ao padrão para tentar encontrar correspondências mais rapidamente.

**D)**  Ignorando todos os prefixos do padrão após um erro e iniciando a busca diretamente do próximo caractere do texto.

::: Gabarito
**A)** Pulando os caracteres que o algoritmo já sabe que estão corretos, usando informações do padrão
:::

???



Agora ficou bem claro qual é a primeira etapa do KMP: pular "casas" quando ele sabe que não vai ocorrer match. Mas quais casam são essas?
De modo simples: ele pula para casas que sabe que ainda podem dar match, ignorando as que já foram testadas e não funcionam. Explicando melhor, o código procura por um *prefixo* na substring que também seja um *sufixo* na string. Isso significa que, ao encontrar um caractere que não combina, o algoritmo pode usar o conhecimento prévio sobre a substring para avançar mais rapidamente.

Lembrando rapidamente o que são prefixos e sufixos:

- Prefixo: uma sequência de caracteres que aparece no início de uma string.
- Sufixo: uma sequência de caracteres que aparece no final de uma string.

Por exemplo, a palavra "paragrafo" tem como prefixo "para" e como sufixo "grafo". O KMP utiliza essa relação para otimizar a busca, evitando comparações desnecessárias.

Em alto nível, fica algo como: 


``` c

void kmp(char string[], char substring[], int n, int m){
    Inicializa um mecanismo auxiliar baseado no padrão para guiar os saltos

     Começa a percorrer o texto com dois ponteiros: um para o texto e outro para o padrão

     Enquanto ainda houver texto a ser percorrido
         Se os caracteres do texto e do padrão coincidirem
             Avança ambos os ponteiros
             Se o ponteiro do padrão atingir o final
                 Registra a posição como ocorrência
                 Usa o mecanismo auxiliar para ajustar o ponteiro do padrão e continuar a busca
         Se os caracteres forem diferentes
             Se já houve algum progresso no padrão
                 Usa o mecanismo auxiliar para reposicionar o ponteiro do padrão sem voltar no texto
             Caso contrário
                 Apenas avança no texto
}
            
```

Para melhor visualizar isso:

:KMP-1



Dessa forma, fica mais claro como o KMP funciona: ele tenta casar a substring com a string, e quando ocorre uma incompatibilidade, ele não volta para o início da substring. Em vez disso, ele utiliza o conhecimento prévio sobre os prefixos e sufixos para avançar mais rapidamente.

Mas como ele faz isso? A resposta está no vetor LPS (Longest Prefix Suffix), que armazena o comprimento do maior prefixo que também é sufixo para cada posição da substring. Esse vetor é fundamental para otimizar o algoritmo KMP, permitindo que ele avance de forma eficiente sem retroceder desnecessariamente e reduzindo significativamente a sua complexidade.


## O Segredo por trás do KMP: O Vetor LPS

Para cada posição {red}(i) no padrão, o algoritmo KMP calcula {red}(lps[i]), que representa o comprimento do maior **prefixo próprio** da substring {red}(padrao[0...i]) que também é um **sufixo próprio** dessa mesma substring.

Esse valor indica o quanto do padrão já foi reconhecido e pode ser reaproveitado, caso haja falha durante a busca no texto.

Abaixo, mostramos passo a passo a construção do vetor LPS para o padrão {red}(ABABAC).

!!! Importante!

Lembre-se que o prefixo sempre começa da primeira letra e exclui a última, enquanto o sufixo termina na última letra e exclui a primeira.

Exemplo: 

Na palavra INSPER os **prefixos** seriam: I, IN, INS, INSP e INSPE.

Enquanto os **sufixos** seriam: R, ER, PER, SPER, NSPER.



!!!



??? Exemplo da Implementação

Vamos montar o vetor LPS de um texto na prática agora. Tente acertar qual será o maior sufixo que também é prefixo da substring, isto é, {red}(padrão[0...i]), depois complete o vetor com o tamanho deste elemento.

![](LPS/LPS_0.png)

Não temos prefixos ou sufixos em palavras de uma letra, por isso começamos sempre com 0 na primeira posição do vetor.

::: i = 1

![](LPS/LPS_1.png)

Prefixo: "A"      

Sufixo: "B"

Não temos igualdade, então colocamos 0 novamente no vetor.


::: i = 2

![](LPS/LPS_2.png)

Prefixos: "A", "AB"     

Sufixos: "A", "BA"

"A" se repete e tem tamanho 1, então colocamos 1 na próxima casa do vetor.

::: i = 3

![](LPS/LPS_3.png)

Prefixos: "A", **"AB"**, "ABA"   

Sufixos: "B", **"AB"**, "BAB"

LPS[i] = 2

::: i = 4

![](LPS/LPS_4.png)

Prefixos: "A", "AB", **"ABA"**, "ABAB" 

Sufixos: "A", "BA", **"ABA"**, "BABA"

LPS[i] = 3

::: i = 5

![](LPS/LPS_5.png)

Prefixos: "A", "AB", "ABA", "ABAB", "ABABA"

Sufixos: "C", "AC", "BAC", "ABAC", "BABAC"

LPS[i] = 0

:::
???


Agora que você entendeu como atua o LPS, temos que praticar os 2 conceitos principais: A Busca por prefixos que também são sufixos, e a montagem do vetor LPS.

??? Exercício Conceitual 7

Dados as seguintes sequências, identifique os maiores prefixos que também são sufixos, e a quantidade de vezes que eles aparecem: 

1.ABCABCABCABC <br>
2.XYXYYXYXYXYYXY <br>
3.ABACABADABACABA <br>


::: Gabarito

1.ABCABCABC <br>
2.XYXYYXY  <br>
3.ABACABA <br>



:::

???

??? Exercício Conceitual 8

Agora é com você! Dada sequência **XYXXYZYXYXXYXYZ**, monte o vetor LPS. O Princípio é o mesmo do exemplo acima!

::: Gabarito

:LPS_2

:::

???


??? Exercício Conceitual 9

Como a prática nunca é demais, vamos montar o vetor LPS de um texto na prática agora. Tente acertar qual será o maior sufixo que também é prefixo da substring, isto é, {red}(padrão[0...i]), depois complete o vetor com o tamanho deste elemento.

![](LPS/LPS_0.png)

Não temos prefixos ou sufixos em palavras de uma letra, por isso começamos sempre com 0 na primeira posição do vetor.

::: i = 1

![](LPS/LPS_1.png)

Prefixo: "A"      

Sufixo: "B"

Não temos igualdade, então colocamos 0 novamente no vetor.


::: i = 2

![](LPS/LPS_2.png)

Prefixos: "A", "AB"     

Sufixos: "A", "BA"

"A" se repete e tem tamanho 1, então colocamos 1 na próxima casa do vetor.

::: i = 3

![](LPS/LPS_3.png)

Prefixos: "A", **"AB"**, "ABA"   

Sufixos: "B", **"AB"**, "BAB"

LPS[i] = 2

::: i = 4

![](LPS/LPS_4.png)

Prefixos: "A", "AB", **"ABA"**, "ABAB" 

Sufixos: "A", "BA", **"ABA"**, "BABA"

LPS[i] = 3

::: i = 5

![](LPS/LPS_5.png)

Prefixos: "A", "AB", "ABA", "ABAB", "ABABA"

Sufixos: "C", "AC", "BAC", "ABAC", "BABAC"

LPS[i] = 0

:::
???


## O Algoritimo completo de Busca: Entenda o KMP com o LPS


Dessa forma, temos toda a construção em alto nível feita, o código é dado por: 

```c
void kmp(char string[], char substring[], int n, int m){
    int lps[m];
    lps(substring, m, lps);

    int i = 0;
    int j = 0;

    while (i < n) {
        if (string[i] == substring[j]) {
            i++;
            j++;
        }

        if (j == m) {
            printf("Padrão encontrado na posição %d\n", i - j);
            j = lps[j - 1];
        }
        else if (i < n && string[i] != substring[j]) {
            if (j != 0) {
                j = lps[j - 1];
            } else {
                i++;
            }
        }
    }
}


```


Logo, vamos praticar a partir dele, para garantir a fixação dele como um todo 


??? Exercício Conceitual 10


A Partir da sequência **ABXABXAB**, identifique: <br>
- O Vetor LPS inteiro 
- Os valores de **i** e **j**
Em todas iterações do Loop



::: Gabarito

**Passo 1: Vetor LPS**

```
0 1 2 0 1 2 0 0 
```
**Passo 2:Determinação de i e j nas iterações**

```
1: 0 0
2: 1 1 
3: 2 2 
4: 3 0
5: 3 0
6: 4 1 
7: 5 2 
8: 6 0
9: 6 0 
10: 7 1
11: 8 0

```

:::
???


Por fim, vamos comparar o KMP com o Algoritimo ingênuo, para sintetizar todo esse raciocínio:

:KMP

Como puderam ver, o KMP é muito mais eficiente do que o ingenuo, no exemplo dado, ele completou a análise com quase a metade das iterações utilizadas pelo ingenuo.






??? Exercício


A partir do código fornecido, tente estimar a nova complexidade do algoritmo KMP.



::: Gabarito

Agora que estamos usando o lps, o KMP alcança uma complexidade de \(O(n + m)\). Isso ocorre porque a construção do vetor LPS, que pré-processa o padrão, é realizada em \(O(m)\), e a busca na string principal é feita em \(O(n)\), sem retrocessos desnecessários.

:::
???

## Desafios

Os desafios requerem pensar em alto nível a ideia de cada parte do algoritmo, e depois montar seu código em C.

## Desafio 1 — Algoritmo Ingênuo de Busca de Padrão

O algoritmo percorre cada posição da string principal e tenta verificar, caractere a caractere, se a substring aparece a partir dali. Se houver mismatch, ele interrompe a verificação e parte para a próxima posição da string.


:::Ideia

```c
void algoritimo_ingenuo(char string[], char substring[], int n, int m){
    // para cada i em (0, 1, 2, ..., n - m)
    //     verifica se a substring ocorre a partir da posição i
    //     para cada j em (0, 1, 2, ..., m - 1)
    //         se string[i + j] ≠ substring[j]
    //             interrompe a verificação (não é uma ocorrência)
    //     se todos os caracteres da substring foram verificados com sucesso
    //         registra a posição i como ocorrência do padrão

```
:::
 

:::Implementação

```c
void algoritimo_ingenuo(char string[], char substring[], int n, int m){
    for (int i = 0; i <= n - m; i++) {
        for (int j = 0; j < m; j++) {
            if (string[i + j] != substring[j]){
                break;
            }
            if (j == m - 1) {
                printf("Padrão encontrado na posição %d\n", i);
            }
        }
    }
}
```
:::

---

## Desafio 2 — Construção do Vetor LPS

O vetor LPS armazena, para cada posição i da substring, o tamanho do maior prefixo que também é sufixo da substring até a posição i. Esse vetor é utilizado pelo algoritmo KMP para evitar retrocessos desnecessários na busca.

:::Ideia

```c
void lps(char padrao[], int m, int* lps){
    // lps[0] = 0, pois uma letra sozinha não tem prefixo próprio
    // para i de 1 até m-1:
    //     se padrao[i] == padrao[comprimento], temos um prefixo/sufixo maior
    //         atualiza lps[i] com comprimento++
    //     senão:
    //         se comprimento ≠ 0, recua para lps[comprimento - 1]
    //         senão, lps[i] = 0
}
```
:::

:::Implementação

```c
void lps(char padrao[], int m, int* lps){
    lps[0] = 0;
    int comprimento = 0;
    int i = 1;

    while (i < m) {
        if (padrao[i] == padrao[comprimento]) {
            comprimento++;
            lps[i] = comprimento;
            i++;
        }
        else {
            if (comprimento != 0) {
                comprimento = lps[comprimento - 1];
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }
}
```
:::

---

## Desafio 3 — Algoritmo KMP com LPS

O algoritmo KMP utiliza o vetor LPS para evitar repetir comparações em caso de falha parcial. Quando há uma incompatibilidade entre a substring e a string principal, ele usa o LPS para "pular" os caracteres já verificados, garantindo eficiência.

:::Ideia

```c
void kmp(char string[], char substring[], int n, int m){
    // calcula o vetor lps da substring
    // usa dois índices: i para string principal, j para substring
    // se string[i] == substring[j], avança ambos
    // se j == m, encontrou ocorrência — imprime e reseta j para lps[j - 1]
    // se string[i] ≠ substring[j]:
    //     se j ≠ 0, recua j = lps[j - 1]
    //     senão, avança i
}
```
:::

:::Implementação

```c
void kmp(char string[], char substring[], int n, int m){
    int lps[m];
    lps(substring, m, lps);

    int i = 0;
    int j = 0;

    while (i < n) {
        if (string[i] == substring[j]) {
            i++;
            j++;
        }

        if (j == m) {
            printf("Padrão encontrado na posição %d\n", i - j);
            j = lps[j - 1];
        }
        else if (i < n && string[i] != substring[j]) {
            if (j != 0) {
                j = lps[j - 1];
            } else {
                i++;
            }
        }
    }
}
```
:::

## Simulador

Para simular o código do KMP com seus próprios inputs, acesse o [link](implementacao.html).

