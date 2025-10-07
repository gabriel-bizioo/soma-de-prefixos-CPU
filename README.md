#  Soma Paralela De Prefixos Com CPU

## Objetivo
1) Obter um algoritmo que utiliza multiplas threads para operar
a soma de prefixos em um vetor de entrada

2) Usar Pool de threads na implementação do algoritmo para economizar a 
reinicialização de threads

3) Medir o tempo médio e vazão de operações do algoritmo

4) Comparar o algoritmo criado com a implementação sem pool de threads

## Redução
O algoritmo de redução funciona de forma relativamente simples. Inicialmente, o vetor 
__partialSum__ de tamanho igual ao numero de threads e valores iguais à 0. Ao entrar no algoritmo,
cada thread executa somas de prefixos locais na sua <faixa local>, determinada pelo tamanho do vetor
e o número de threads. Ao terminar a soma local, cada thread armazena o valor final da soma em 
__partialSum[tid]__ (onde __tid__ é o indice lógico da thread) e espera em uma barreira até que todas as demais threads tenham
terminado a soma.

Após terminadas todas as somas locais, cada thread obterá o valor global do total da soma dos elementos
que vem antes da sua faixa local. Esse valor é obtido ao somar todos os valores de __prefixSum__ que vêm antes
de __prefixSum[tid]__. Esse valor será adicionado aos valores da faixa acumulativamente, resultando assim na soma correta.

## Pool de Threads
A ideia por trás do pool de threads é reutilizar
threads chamadas previamente para diminuir a quantidade de operações 
de inicialização de threads e ganhar tempo

A implementação é feita com um loop infinito e duas barreiras. Dentro do loop infinito,
está a primeira barreira, seguido do código para redução paralela (que contém uma outra barreira, não relacionada),
seguido da última barreira.

Uma vez que todas as threads acabam suas operações, ao invés de serem deletadas,
apenas a thread 0 "escapa" da função, e as demais param na primeira barreira
e esperam até que haja outra chamada para a função feita na thread 0. Uma vez que a
thread 0 entra novamente na função, a barreira é aberta e todas as threads estão prontas para
executar suas operações sem necessidade de serem reinicializadas. A última barreira serve para evitar 
problemas de sincronização em casos onde a thread 0 retorna a função muito rapidamente.


## Experimentos
Para medir a performance do algoritmo e o escalonamento da vazão de acordo com
o número de threads, foi utilizado um script que roda o programa 10 vezes por numero de threads, executando 
com apenas 1 até 8 threads, totalizando 80 execuções.


## Resultados

### Algoritmo "Original"
| Número de threads | Tempo médio | Vazão |
|-----|:-----:|-----|

### Algoritmo com pool de threads
| Número de threads | Tempo médio | Vazão | Ganho |
|-----|:-----:|:---:|-----|





