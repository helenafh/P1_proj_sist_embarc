## Questão 7 — Tópicos em programação de embarcados

Em sistemas embarcados, a organização do software e da memória influencia desempenho, consumo de recursos e previsibilidade temporal.

### a) (0,25 ponto)
Explique os conceitos de localidade temporal, espacial e sequencial e dê um exemplo de cada um em um programa embarcado.

**Resposta:**

O princípio da localidade descreve a tendência de programas acessarem repetidamente um conjunto relativamente pequeno de posições de memória em determinados intervalos de execução. Esse comportamento é explorado pela hierarquia de memória para reduzir o tempo médio de acesso aos dados e instruções.

1. Localidade temporal: ocorre quando uma mesma posição de memória, dado ou instrução é acessada novamente em um curto intervalo de tempo.
Exemplo: uma variável acumuladora utilizada repetidamente dentro de um laço ou uma rotina de controle PID executada periodicamente.

2. Localidade espacial: ocorre quando, após o acesso a uma posição de memória, posições próximas tendem a ser acessadas em seguida.
Exemplo: o acesso aos campos consecutivos de uma struct que armazena leituras de sensores, como aceleração nos eixos \(x\), \(y\) e \(z\).

3. Localidade sequencial: é um caso particular da localidade espacial em que os acessos ocorrem de forma ordenada no espaço de memória.
Exemplo: percorrer sequencialmente os elementos de um vetor de amostras de ADC, como buffer[0], buffer[1], buffer[2], etc.




### b) (0,25 ponto)
Explique a função de registradores, cache, RAM e memória não volátil (Flash) em uma hierarquia de memória.

**Resposta:**

Em uma hierarquia de memória, diferentes níveis oferecem vantagens e desvantagens em relação a velocidade, capacidade, custo e persistência dos dados. Algumas considerações sobre cada um:

- Registradores: são pequenas áreas de armazenamento localizadas dentro do processador e usadas diretamente pelas instruções para manter operandos, endereços e resultados temporários. Possuem acesso muito rápido, porém capacidade extremamente limitada.

- Cache: é uma memória rápida intermediária entre o processador e a memória principal. Armazena temporariamente dados e instruções acessados com maior frequência ou recentemente, explorando os princípios de localidade para reduzir o tempo médio de acesso à memória.

- RAM: é a memória principal de trabalho do sistema, utilizada para armazenar variáveis, pilha, heap e outros dados necessários durante a execução. Possui maior capacidade que registradores e cache, porém é mais lenta. É uma memória volátil, ou seja, perde seu conteúdo quando a alimentação é removida.

- Memória não volátil (Flash): mantém os dados mesmo sem alimentação. Em sistemas embarcados, é normalmente utilizada para armazenar o firmware, constantes e, em alguns casos, parâmetros de configuração persistentes. Em geral, apresenta menor velocidade de escrita e um número limitado de ciclos de gravação em comparação com a RAM.

Assim, a hierarquia busca combinar os diferentes tipos de memória de forma que dados e instruções de uso mais imediato permaneçam nos níveis mais rápidos, enquanto informações de maior volume ou persistentes sejam mantidas em níveis de maior capacidade.




### c) (0,25 ponto)
Explique como cache, interrupções, DMA e escalonamento de tarefas podem introduzir variações no tempo de execução de uma rotina crítica.

**Resposta:**

Em sistemas embarcados, alguns mecanismos de hardware e software podem introduzir variações no tempo de execução de uma rotina crítica, reduzindo a previsibilidade temporal do sistema.

- Cache: quando os dados ou instruções necessários já estão presentes no cache (cache hit), o acesso é rápido. Quando não estão (cache miss), é necessário buscar a informação em níveis mais lentos da memória, aumentando o tempo de execução. Assim, a mesma rotina pode apresentar durações diferentes dependendo do estado da cache.

- Interrupções: uma rotina crítica pode ser temporariamente interrompida pela execução de uma rotina de serviço de interrupção (ISR). O atraso depende do instante em que a interrupção ocorre, de sua prioridade e do tempo necessário para tratá-la, introduzindo variações no tempo de execução da rotina original.

- DMA (Direct Memory Access): o DMA permite transferências de dados entre periféricos e memória sem participação contínua da CPU, reduzindo sua carga. Entretanto, as transferências DMA podem competir com a CPU pelo acesso à memória ou ao barramento, causando atrasos variáveis em acessos realizados pela rotina crítica.

- Escalonamento de tarefas: em sistemas com múltiplas tarefas ou RTOS, uma rotina pode ser preemptada (interrompida temporariamente para que outra tarefa com maior prioridade possa ser executada) ou atrasada enquanto aguarda recursos compartilhados. O tempo de resposta passa, portanto, a depender do estado do escalonador, das prioridades e da carga do sistema.

Esses efeitos podem causar jitter, isto é, variações no instante de início ou na duração de execução de uma rotina. Em sistemas de controle de voo, essa variabilidade pode afetar a regularidade da malha de controle e, consequentemente, o comportamento do sistema.



### d) (0,25 ponto)
Proponha duas estratégias de programação/arquitetura de software para aumentar o determinismo temporal de um sistema embarcado de controle de voo. Justifique.

**Resposta:**

A partir do que foi discutido nos itens anteriores, duas possíveis estratégias para aumentar o determinismo temporal são:

1. Execução periódica com prioridades fixas: organizar as tarefas críticas de controle para executar em períodos bem definidos, utilizando um escalonamento de tempo real com prioridades fixas. A malha de controle de voo pode receber prioridade elevada e um período de execução conhecido, reduzindo atrasos causados por outras tarefas menos críticas. Dessa forma, torna-se mais previsível o instante em que cada cálculo de controle é iniciado e concluído.

2. Redução de operações com tempo de execução imprevisível: em rotinas críticas, devem ser evitadas operações como alocação dinâmica de memória, chamadas bloqueantes e laços cujo número de iterações não possui limite conhecido. É preferível utilizar memória alocada estaticamente, algoritmos com tempo de execução limitado e seções críticas curtas. Isso reduz a variação no tempo de execução e facilita a estimativa do pior caso de execução (Worst-Case Execution Time — WCET).

