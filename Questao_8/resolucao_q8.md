## Questão 8 — Decomposição Funcional e Diagrama de Blocos

Realize a decomposição funcional de um drone baseado na plataforma DJI F450, mantendo rastreabilidade entre funções, interfaces e componentes físicos.

### a) (0,20 ponto)
**Nível 0:** represente o sistema como uma caixa-preta, identificando entradas, saídas, energia, indicadores, sensores externos, rádio controle, estação de solo e as interfaces com o sistema físico e com o usuário.

**Resposta:**

No Nível 0, o drone é tratado como uma caixa-preta, sem detalhamento de seus subsistemas internos. As entradas incluem a energia fornecida pela bateria, comandos do rádio controle e da estação de solo, além das medições provenientes dos sensores externos. Como saídas, o sistema produz atuação física por meio do movimento do veículo, telemetria para a estação de solo e informações de estado para o usuário.

A interface com o sistema físico é bidirecional: o ambiente influencia o drone por meio de grandezas como posição, movimento, obstáculos e perturbações, enquanto o drone atua sobre o ambiente por meio do empuxo e deslocamento. A interface com o usuário ocorre principalmente pelo rádio controle, estação de solo e indicadores de estado.

[Diagrama - Nivel 0](diagrama_8a.md)



### b) (0,25 ponto)
**Nível 1:** decomponha o drone em macroblocos funcionais (por exemplo: energia, sensoriamento, processamento/controle, comunicação, atuação e estrutura), evidenciando os fluxos entre eles.

**Resposta:**

No Nível 1, o drone é decomposto em seis macroblocos funcionais. O bloco de energia fornece alimentação aos sensores, à controladora, aos módulos de comunicação e potência aos atuadores. O bloco de sensoriamento mede o estado do veículo e do ambiente e envia essas informações ao bloco de processamento e controle, que executa os algoritmos de navegação e estabilização.

O bloco de comunicação recebe comandos do piloto ou da estação de solo e envia telemetria e informações de estado. O bloco de atuação recebe os comandos calculados pela controladora e os converte em forças e torques por meio dos ESCs, motores e hélices. Já a estrutura fornece suporte físico e integração mecânica entre os componentes.

O fluxo principal do sistema pode ser resumido como:

sensores → processamento/controle → atuadores → sistema físico → sensores, formando a malha de realimentação do drone.

[Diagrama - Nivel 1](diagrama_8b.md)


### c) (0,30 ponto)
**Nível 2:** escolha dois macroblocos e detalhe suas funções internas, interfaces, sinais e dependências.

**Resposta:**

Os dois macroblocos escolhidos para detalhamento foram o de Sensoriamento e o de Processamento e Controle.

- Sensoriamento: responsável por adquirir informações sobre o estado do drone e sobre o ambiente. Entre suas funções estão medir acelerações e velocidades angulares por meio da IMU, estimar altitude por sensoriamento barométrico, obter posição e velocidade por GPS e medir distâncias ao solo ou a obstáculos com o LiDAR. Esses sinais passam por etapas de aquisição, validação e, quando necessário, fusão de sensores, resultando em uma estimativa do estado do veículo utilizada pelos algoritmos de controle.
As principais entradas desse bloco são grandezas físicas do ambiente e do movimento do drone. As saídas incluem dados como aceleração, velocidades angulares, posição, altitude e distância, que são enviados ao bloco de processamento e controle. Sua operação depende da correta alimentação dos sensores, das interfaces de comunicação e da qualidade das medições obtidas.

[Diagrama Nível 2 - Sensoriamento](diagrama_8c1.md)

- Processamento e Controle: recebe as informações provenientes do sensoriamento e os comandos do piloto ou da estação de solo. A partir desses dados, realiza a estimativa e validação do estado do veículo, gerencia a navegação e a missão e executa os algoritmos de controle de voo, como controle de atitude, altitude e posição.
Os comandos calculados são então distribuídos aos atuadores, convertendo referências de voo em sinais para os motores. O bloco também pode executar funções de supervisão e tratamento de falhas, alterando o comportamento do sistema quando detecta condições anormais.
Suas principais entradas são o estado estimado do veículo, comandos do piloto e referências de missão. As principais saídas são comandos de atuação, informações de telemetria e sinais associados a estratégias de contingência. Esse bloco depende diretamente do sensoriamento para fechar a malha de controle e do sistema de comunicação para receber comandos e transmitir o estado da missão.

[Diagrama Nível 2 - Processamento e Controle](diagrama_8c2.md)

### d) (0,25 ponto)
**Nível 3/síntese:** para um dos blocos detalhados, faça o mapeamento das funções para componentes concretos. A solução deve ser consistente com uma lista de materiais (BOM) e com as interfaces definidas nos níveis anteriores.

**Resposta:**

No Nível 3, o macrobloco de Processamento e Controle é mapeado para a Holybro Pixhawk 6X, que concentra as funções de estimativa de estado, navegação, controle de voo, alocação de comandos e supervisão/failsafe.

As informações utilizadas por esse bloco são fornecidas pelo macrobloco de Sensoriamento, composto por sensores inerciais e barométricos integrados à controladora, além de sensores externos como GPS e TF-Luna.  

A saída do processamento é encaminhada ao macrobloco de Atuação, no qual os comandos são enviados aos quatro ESCs, responsáveis por controlar os motores e hélices do F450.

Dessa forma, o diagrama preserva a rastreabilidade definida nos níveis anteriores:
Sensoriamento → Processamento e Controle → Atuação

