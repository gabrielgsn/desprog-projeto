
# Algoritimo Knuth-Morris-Pratt


## O que é o Algoritimo Knuth-Morris-Pratt


A busca por strings ou padrões em strings é um problema comum em computação, como encontrar uma 
 frase em um documento ou um trecho de DNA em uma sequência genética.
 O algoritmo Knuth-Morris-Pratt (KMP) resolve esse problema de forma eficiente, encontrando 
 todas as ocorrências de uma substring dentro de uma string maior.
 


 O KMP é conhecido por sua eficiência em comparação com algoritmos de força bruta, pois evita comparações
 desnecessárias ao usar informações sobre os padrões já verificados.
 


## Como isso pode ser aplicado na vida real


O algoritmo KMP é amplamente usado para encontrar padrões em textos longos de forma rápida e eficiente. 
 Um exemplo prático é na bioengenharia, onde cientistas buscam sequências específicas de DNA 
 (como genes ou mutações) em cadeias genéticas com milhões de bases.
 


Um exemplo atual disso é a recriação dos lobos terriveis feita pela empresa Colossal Biosciences, 
 que por meio de uma tecnoligia da bioengenharia aprimorada chamada CRISPR conseguiram realizar este feito.

 


![](Direwolf.jpeg)
Esse método consiste basicamente em edição genômica, ou seja, identificação de alguns fragmentos 
 na cadeia de DNA de uma espécie e alteração de um valor possível para outro, como trocar um “A” 
 por um “T” ou um “C” por um “G”.
 No caso dessa espécie, o DNA de lobos comuns foi alterado com base em um fóssil que estava 
 preservado e somente 14 alterações em 20 genes precisaram ser feitas para que a sequência 
 genética ficasse idêntica ao dos Lobos Terríveis.
 


Neste caso o algoritmo KMP seria uma potencial ferramenta utilizada para encontrar os padrões 
 de DNA desejados, levando em conta sua eficiente analise de padrẽes em strings grandes, como o DNA.


## Algoritimo ingênuo


Antes de estudarmos o algoritmo KMP, vamos entender como funciona o principio de análise 
 de padrões de strings utilizando o algoritmo ingênuo e como o algoritmo KMP resolve o principal problema dele.


Assim como o KMP, o algoritimo ingênuo também é utilizado para procurar padroẽs em strings. 
 Neste caso, o algoritimo ingênuo realiza esta busca utilizando uma método de "brute force", 
 ou seja, ele testa todas as combinações possiveis para encontrar padrões.




??? Exercício


Com o contexto de como o algoritimo funciona, pense quais argumentos o algoritmo ingênuo deve 
 receber para seu funcionemento: `void algoritimo_ingenuo(???){...}`.




::: Gabarito

Como você deve ter imaginado, ele não precisa receber muitas coisas, apenas as 2 strings e seus tamanhos:


`void algoritimo_ingênuo(char string[], char substring[], int n, int m){...}`



:::

???


Agora vamos desenvolver mais este código e tentar montar a estrutura do loop principal do algoritimo:



``` c

void algoritimo_ingenuo(char string[], char substring[], int n, int m){
    for(int i = 0; i < n - m; i++){
        // loop para percorrer a string
    }
}

```



??? Exercício


Completando um pouco mais o código, chegamos dentro do loop de comparação das strings:



``` c

void algoritimo_ingenuo(char string[], char substring[], int n, int m){
    for(int i = 0; i < n - m; i++){
        for (int j = 0; j < m; j++) {
            //loop de comparação
        }
    }
}

```

Usando o código fornecido, tente desenvolver o conteúdo deste loop para que ele continue realizando as 
 comparações de caractéres das strings enquanto elas estiverem iguais.




::: Gabarito


``` c

void algoritimo_ingenuo(char string[], char substring[], int n, int m){
    for (int i = 0; i <= n - m; i++) {
        for (int j = 0; j < m; j++) {
            if (string[i + j] != substring[j]){
                break;
            }
        }
    }
}

```


:::
???


Com a comparação de caractéres feita dentro do loop, apenas precisamos complemetar o codigo para 
 que ele nos devolva os indices onde o padrão foi encontrado:



``` c

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

Vamos ver uma simples demonstração de como ele funciona por meio da animação:


:ingenuo_01


??? Exercício


Agora que estamos familiarizados com o funcionamento deste algoritimo, tente estimar qual vai ser a complexidade dele no seu pior caso.




::: Gabarito

Como o algoritmo ingênuo compara cada posição da string principal, e em cada posição tenta casar a substring. Se a string principal tem tamanho *n* e a substring tem tamanho *m*, a complexidade no pior caso é \(O(nm) \).



:::
???

Agora vamos tentar com uma string e substring maiores:


:ingenuo_02

Como pode ser visto nas animações, existe um problema bem aparente neste algoritimo, a sua redundância nas comparações. Quando uma incompatibilidade (mismatch) ocorre, o algoritmo simplesmente avança para a próxima posição na string principal e reinicia a comparação da substring do início, revisitando caracteres que já foram analisados.

Por exemplo, se uma parte da substring já foi confirmada como compatível, o algoritmo ingênuo não aproveita essa informação, resultando em comparações desnecessárias. Essa abordagem leva a uma complexidade de \(O(nm)\) no pior caso, tornando-o inviável para strings longas.

##  O KMP


Como superar as limitações do algoritmo ingênuo? A ideia central do KMP é pré-processar o padrão para criar uma estrutura que permita "pular" comparações redundantes, otimizando a busca pelo padrão ao longo da string .

Agora vamos olhar para uma versão de alto nível do código do KMP:

``` c

void kmp(char string[],int n, char substring[], int m){
    enquanto i for menor que n, continue o processamento {
        se string[i] for igual a substring[j]{
            avance contador i e j
        }
        se j for igual a m{
            sabemos que o padrao foi encontrado porque o tamanho maximo da substring foi atingido
            zere j para continuar a busca
        }
        se i for menor que n e string[i] for diferente de substring[j]{
            se j for igual a 0{
                avanca i mantendo j em 0
            }caso contrario,{
                reinicia j, mas mantem o valor de i para nao perder o progresso
            }
        }
    }
}
            
```



??? Exercício


A partir dessa descricao de alto nivel do KMP, tente escrever como seu codigo em C ficaria



``` c

void kmp(char string[], int n, char substring[], int m){

    ...
    
}
        
```



::: Gabarito


``` c

void kmp(char* string, int n, char* substring, int m) {
    // passo 1: inicialize os indices
    int i = 0; // indice para o string
    int j = 0; // indice para a substring
    
    // passo 2: inicie um laco na string
    while (i < n) {
        // passo 3: compare caracteres
        if (string[i] == substring[j]) {
            // passo 4: se coincidem
            i++;
            j++;
        }
        
        // passo 5: se o padrao foi encontrado
        if (j == m) {
            printf("padrao encontrado na posicao %d\n", i - j);
            j = 0; // reinicia j para buscar outras ocorrencias
        }
        // passo 6: se ha um mismatch
        else if (i < n && string[i] != substring[j]) {
            // passo 6a: se j eh zero
            if (j == 0) {
                i++; // avanca i, mantendo j em 0
            }
            // passo 6b: Se j eh maior que zero
            else {
                j = 0; // reinicia j, mas mantem i
            }
        }
    }
}
        
```


:::
???


Essa abordagem é mais eficiente que a busca ingênua, pois evita retroceder no texto, mas ainda realiza 
 comparações redundantes, já que reinicia j para 0 em cada mismatch. Sua complexidade no pior caso pode 
 se aproximar de O(n * m), onde n é o tamanho do texto e m é o tamanho do padrão.


## Retirando a Redundância: O Vetor de LPS


O vetor Longest Proper Prefix which is also Suffix (LPS) serve é o Coração do algoritimo KMP. A Abordagem que ele adota de indentificar
 o padrão é, como o nome já sugere, entender até que ponto do comprimento o prefixo é igual ao sufixo de uma string. Essa padronização é 
 utilizada para direcionar a eficiência do algoritimo que, ao invés de retroceder, agora, só volta até a parte onde ainda não temos padrões.
 Mas como eu vou entender padrões comparando prefixos com sufixos? É aí que a mágica vem... 
 




??? Exercício


Suponhas as seguintes sequências de caracteres


1.ABCABCABCABC


2.ABABACABABACABABA


3.ABCABDABCABEABCABDABCABEABCABD


Qual o Padrão textual presente em cada uma delas?




::: Gabarito

1.ABC - Note que aqui a sequência é simples, só temos que ver quando o caractere A se repete novamente


2.ABABA - Aqui, já vemos um grau de ruptura, com a letra "c" entre alguns padrões


3.ABCABDABCABE - Já temos padrões grandes e difíceis de se indentificar visualmente, com grandes quebras claras



:::
???

Quando tentamos encontrar padrões em uma sequência, o nosso pensamento segue três etapas naturais. Primeiro, reconhecemos pequenas repetições locais, comparando trechos que já vimos com o que estamos lendo agora. Depois, procuramos uma regularidade, tentando identificar se essas repetições seguem um tamanho ou ritmo constante. Por fim, condensamos essas repetições em uma ideia única para economizar esforço, enxergando, por exemplo, "ABC" repetido, em vez de cada letra separada. Esse raciocínio leva naturalmente a buscar partes do início (prefixos) que também aparecem no final (sufixos), pois eles já foram confirmados e podem ser reutilizados.Logo, o LPS sintetiza esse raciocínio em uma lógica intuitiva.
 



``` c

void lps(char padrao[], int m, int* lps){
                
    inicie o comprimento do maior prefixo próprio e sufixo em 0, ou seja, aqui você está olhando para o maior padrão
                
    defina lps[0] como 0, porque um único caractere não tem prefixo nem sufixo
            
    inicie o contador i em 1
            
        enquanto i for menor que m, continue o processamento {
            se padrao[i] for igual a padrao[comprimento] {
                incremente o comprimento
                atribua lps[i] como o novo comprimento
                avance i
            }
            caso contrário {
                se comprimento for diferente de 0 {
                    atualize o comprimento para o valor de lps[comprimento - 1]
                    não avance i ainda, pois vamos tentar casar um prefixo menor
                }
                se comprimento for igual a 0 {
                    defina lps[i] como 0
                    avance i
                }
            }
        }
    }
            
```



??? Exercício


A partir dessa descricao de alto nivel do LPS, tente escrever como seu codigo em C ficaria



``` c

        void lps(char padrao[], int m, int* lps){
            
            ...

        }
        
```



::: Gabarito


``` c

void lps(char padrao[], int m, int lps[]){
    
    // Passo 1: Inicializa comprimento como 0 e o primeiro caractere do vetor também, já que ele nao tem prefixo e nem sufixo
    int comprimento = 0;
    lps[0] = 0;

    // Passo 3: Começa a análise do segundo caractere
    int i = 1;

    // Passo 4: Percorre o padrão até o final
    while (i < m) {

        // Passo 5: Se os caracteres combinam, atualiza comprimento e LPS
        
        if (padrao[i] == padrao[comprimento]) {
            comprimento++;
            lps[i] = comprimento;
            i++;
        }

        // Passo 6: Se os caracteres não combinam
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
???
## Otimizando o KMP

Utilizando o LPS como uma função auxiliar para o KMP, podemos otimizar ele muito, de maneira a reduzir a redundância ao extremo. Vamos olhar como fica o código do KMP agora:

``` c

void kmp(char string[], char substring[], int n, int m) {
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
        } else if (i < n && string[i] != substring[j]) {
            if (j != 0) {
                j = lps[j - 1];
            } else {
                i++;
            }
        }
    }
}
        
```


??? Exercício


A partir do código fornecido, tente estimar a nova complexidade do algoritmo KMP.



::: Gabarito

Agora que estamos usando o lps, o KMP alcança uma complexidade de \(O(n + m)\). Isso ocorre porque a construção do vetor LPS, que pré-processa o padrão, é realizada em \(O(m)\), e a busca na string principal é feita em \(O(n)\), sem retrocessos desnecessários.

:::
???

Vamos olhar agora para uma animação, demonstrando o funcionamento desta versão do KMP:

:KMP