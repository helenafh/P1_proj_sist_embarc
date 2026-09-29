## Questão 1 — Introdução e aspectos gerais de sistemas embarcados e ciberfísicos

Considere a evolução recente dos sistemas embarcados conectados e sua integração com processos físicos. Responda de forma objetiva e relacione os conceitos entre si.

### a) (0,20 ponto)
Defina sistema embarcado e identifique três características que o diferenciam de um computador de propósito geral.

**Resposta:**

Um sistema embarcado é um sistema computacional projetado para executar uma função específica, normalmente como parte de um sistema maior e sujeito a restrições de projeto. Diferentemente de um computador de propósito geral, cujo objetivo é oferecer flexibilidade para executar diferentes tipos de tarefas, um sistema embarcado é, de maneira geral, otimizado para um conjunto limitado de funções.

Três características que diferenciam os sistemas embarcados de computadores de propósito geral são:

1. Função Dedicada: Sistemas embarcados são projetados para executar uma função específica, ou um conjunto de funções específicas, com hardware e software definidos em função dessa aplicação. Já computadores de propósito geral são projetados para executar tarefas e aplicações variadas e arbitrárias instaladas pelo usuário.

2. Restrições de projeto: sistemas embarcados normalmente possuem limitações severas de custo, tamanho e peso, pois são integrados ao produto final e muitas vezes fabricados em grande escala. Em computadores de propósito geral também existem restrições, porém menos severas, havendo maior margem para incorporar recursos computacionais adicionais em favor de flexibilidade funcional e desempenho.

3. Restrição de consumo energético: em muitos sistemas embarcados, especialmente os alimentados por meio de baterias, o consumo de energia é um requisito central do projeto. Por isso, é comum o uso de recursos como redução da frequência de clock, desligamento de periféricos e uso de mecanismos com baixo consumo de energia. Em computadores de propósito geral, embora a eficiência energética também seja relevante, é comum ter uma maior disponibilidade de energia e o desempenho computacional recebe maior prioridade.




### b) (0,25 ponto)
Defina sistema ciberfísico (CPS) e explique a função das interfaces entre os domínios computacional e físico.

**Resposta:**

Um Sistema Ciberfísico (CPS - Cyber-Physical System) é a integração de sistemas computacionais e processos físicos, na qual computadores embarcados e redes monitoram e controlam o comportamento do sistema físico. Essa interação ocorre tipicamente por meio de malhas de realimentação (feedback loops), nas quais o estado do processo físico influencia a computação enquanto as decisões computacionais alteram o comportamento do processo físico.

As Interfaces entre os domínios computacional e físico atuam como uma ponte entre o mundo cibernético, onde são executados algoritmos e manipuladas variáveis computacionais, e o mundo físico, caracterizado por grandezas como posição, temperatura, pressão e aceleração. Essas interfaces são compostas principalmente por:

1. Sensores: Responsáveis por medir as grandezas físicas e convertê-las em sinais que possam ser adquiridos e processados pelo sistema computacional. Dessa forma, fornecem ao controlador informações sobre o estado do sistema físico.

2. Atuadores: Responsáveis por modificar o estado do sistema físico a partir dos comandos gerados pelo domínio computacional. Eles convertem sinais de controle em ações físicas, como força, torque, movimento ou variações de energia.




### c) (0,25 ponto)
Defina AIoT e explique como a incorporação de Inteligência Artificial modifica a arquitetura de um sistema IoT convencional.

**Resposta:**

O termo AIoT (Artificial Intelligence of Things) refere-se à integração entre Inteligência Artificial e Internet das Coisas (IoT). Enquanto um sistema IoT convencional se concentra principalmente em conectar sensores, atuadores e dispositivos à rede para coletar, transmitir e disponibilizar dados, a AIoT incorpora técnicas de Inteligência Artificial, como aprendizado de máquina e inferência, para analisar esses dados e permitir comportamentos mais adaptativos, automatizados e inteligentes. Com isso, o sistema deixa de apenas coletar e transmitir informações e passa também a extrair padrões, realizar inferências e apoiar ou executar decisões com base nos dados obtidos.

A incorporação de Inteligência Artificial modifica a arquitetura de um sistema IoT convencional ao adicionar uma camada de processamento inteligente aos dados coletados. Essa mudança pode aumentar os requisitos de processamento, memória e armazenamento dos dispositivos ou da infraestrutura responsável pela execução dos modelos, podendo inclusive demandar hardware especializado em aplicações mais complexas. Além disso, o fluxo de dados pode ser modificado, pois informações brutas podem ser processadas, filtradas ou transformadas em informações de maior nível antes de serem utilizadas por outros componentes do sistema.




### d) (0,30 ponto)
Defina Edge Computing e discuta duas vantagens e duas limitações de executar processamento na borda em comparação com a nuvem, considerando latência, energia, conectividade e privacidade.

**Resposta:**

Edge Computing (Computação na Borda) é um paradigma de computação distribuída no qual o processamento, o armazenamento e a análise de dados são realizados próximos ao local onde esses dados são gerados, como sensores, atuadores e dispositivos periféricos, reduzindo a dependência de servidores remotos ou de data centers em nuvem.

Em comparação com o processamento em nuvem, a execução na borda apresenta como principal vantagem a redução de latência, pois os dados são processados próximos ao local onde são gerados, evitando o tempo de transmissão até servidores remotos e o retorno da resposta. Isso é especialmente importante em sistemas embarcados que exigem respostas rápidas ou operação em tempo real.

Outra vantagem é a menor dependência de conectividade externa. Como parte do processamento é realizada localmente, o sistema pode continuar executando funções essenciais mesmo quando a conexão com a Internet estiver indisponível ou instável.

Como limitação, o processamento na borda aumenta a demanda por recursos computacionais e energia no dispositivo local. Algoritmos mais complexos podem exigir maior capacidade de processamento, memória e consumo energético, o que é particularmente relevante em sistemas alimentados por bateria.

Além disso, dispositivos de borda podem apresentar maior exposição física, já que frequentemente estão instalados próximos ao ambiente monitorado. Isso pode aumentar a vulnerabilidade a acesso físico não autorizado, adulteração ou extração de dados e chaves armazenadas no dispositivo.

