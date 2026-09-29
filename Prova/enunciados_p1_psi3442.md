# PSI-3442 — Primeira Prova de Projeto de Sistemas Embarcados

## Questão 1 — Introdução e aspectos gerais de sistemas embarcados e ciberfísicos

Considere a evolução recente dos sistemas embarcados conectados e sua integração com processos físicos. Responda de forma objetiva e relacione os conceitos entre si.

### a) (0,20 ponto)
Defina sistema embarcado e identifique três características que o diferenciam de um computador de propósito geral.

### b) (0,25 ponto)
Defina sistema ciberfísico (CPS) e explique a função das interfaces entre os domínios computacional e físico.

### c) (0,25 ponto)
Defina AIoT e explique como a incorporação de Inteligência Artificial modifica a arquitetura de um sistema IoT convencional.

### d) (0,30 ponto)
Defina Edge Computing e discuta duas vantagens e duas limitações de executar processamento na borda em comparação com a nuvem, considerando latência, energia, conectividade e privacidade.

---

## Questão 2 — Model-Based Design

O desenvolvimento de sistemas embarcados complexos exige mecanismos que permitam especificar, analisar, simular e validar o sistema antes de sua implementação final.

### a) (0,25 ponto)
Explique o conceito de Model-Based Design (MBD) e sua relação com o ciclo de desenvolvimento de sistemas embarcados.

### b) (0,25 ponto)
Descreva um fluxo MBD contendo, no mínimo: requisitos, modelagem, simulação, refinamento, implementação e verificação/validação.

### c) (0,25 ponto)
Explique a diferença entre Model-in-the-Loop (MIL), Software-in-the-Loop (SIL) e Hardware-in-the-Loop (HIL).

### d) (0,25 ponto)
Usando o multirotor DJI F450 como exemplo, indique um subsistema que poderia ser desenvolvido por MBD e descreva quais modelos, entradas, saídas e critérios de validação seriam utilizados.

---

## Questão 3 — Modelagem Discreta

Um controlador embarcado observa o mundo físico em instantes discretos e atualiza suas saídas periodicamente.

### a) (0,20 ponto)
Explique a diferença entre um modelo de tempo contínuo e um modelo de tempo discreto. Dê dois exemplos de grandezas contínuas e dois exemplos de variáveis discretas em um multirotor.

### b) (0,25 ponto)
Explique o significado do período de amostragem $T_s$ e da frequência de amostragem $f_s = 1/T_s$. Discuta como sua escolha afeta desempenho, estabilidade, carga computacional e consumo de energia.

### c) (0,30 ponto)
Considere $dx(t)/dt = u(t)$. Utilizando a aproximação de Euler para frente, obtenha a equação de diferenças que permite calcular $x[k+1]$ a partir de $x[k]$, $u[k]$ e $T_s$. Explique o significado físico de cada termo.

### d) (0,25 ponto)
Represente em um diagrama a cadeia:

**grandeza física → sensor → condicionamento/conversão → amostragem → algoritmo embarcado → saída → atuador → sistema físico**

identificando os domínios contínuo e discreto.

---

## Questão 4 — Modelagem da Dinâmica Física

Considere o problema de estabilização de atitude de um multirotor.

### a) (0,30 ponto)
Apresente as principais grandezas físicas envolvidas na dinâmica de atitude (por exemplo, orientação, velocidades angulares, torques e empuxos) e indique as entradas e saídas de um modelo simplificado.

### b) (0,35 ponto)
Escreva e explique um modelo matemático simplificado para a dinâmica rotacional do veículo. Podem ser utilizadas equações diferenciais, diagramas ou representação em espaço de estados, desde que as hipóteses adotadas sejam explicitadas.

### c) (0,20 ponto)
Discuta pelo menos duas hipóteses simplificadoras do modelo e explique em quais situações elas podem deixar de ser válidas.

### d) (0,15 ponto)
Explique como o modelo físico se relaciona com o controlador implementado no computador de voo, sem repetir a discretização solicitada na Questão 3.

---

## Questão 5 — Máquinas de Estado

Modele a missão abaixo por meio de uma máquina de estados finita de tempo discreto.

Um multirotor permanece em sua base de carregamento até atingir 100% de bateria. Em seguida, decola e visita sequencialmente três coordenadas GPS:

- $A=(X_a,Y_a,Z_a)$
- $B=(X_b,Y_b,Z_b)$
- $C=(X_c,Y_c,Z_c)$

permanecendo 3 minutos em cada uma. Após C, retorna à base e encerra a missão. Se, em qualquer momento de voo, a carga atingir um limiar crítico definido em 20%, deve abortar a missão e retornar imediatamente à base.

### a) (0,25 ponto)
Identifique os estados necessários e descreva a função de cada um.

### b) (0,35 ponto)
Desenhe a máquina de estados indicando eventos, condições de guarda e transições.

### c) (0,20 ponto)
Indique quais variáveis devem ser mantidas pelo sistema para representar tempo, posição, estado da missão e condição da bateria.

### d) (0,20 ponto)
Explique como trataria pelo menos uma situação excepcional, como perda de GPS, falha de comunicação ou impossibilidade de retorno à base.

---

## Questão 6 — Sensores e Atuadores

Considere a integração de um sensor LiDAR compacto ao multirotor utilizado nas aulas.

### a) (0,25 ponto)
Explique o princípio de funcionamento de um LiDAR do tipo time-of-flight e identifique as principais grandezas medidas e fontes de erro.

### b) (0,25 ponto)
A partir de um modelo comercial adequado para drones (por exemplo, Benewake TF-Luna/TFmini-S ou equivalente), consulte o manual/datasheet e apresente: faixa de medição, resolução/precisão, taxa de atualização, tensão de alimentação e interface digital.

### c) (0,30 ponto)
Desenhe um diagrama de blocos mostrando a interface elétrica e lógica entre o sensor e o computador de voo, incluindo alimentação, interface de comunicação, driver e camada de software que disponibiliza a medida à aplicação.

### d) (0,20 ponto)
Explique como calibração, ruído, taxa de amostragem, latência e possíveis falhas do sensor podem afetar o comportamento do sistema embarcado e indique pelo menos uma estratégia de mitigação.

---

## Questão 7 — Tópicos em programação de embarcados

Em sistemas embarcados, a organização do software e da memória influencia desempenho, consumo de recursos e previsibilidade temporal.

### a) (0,25 ponto)
Explique os conceitos de localidade temporal, espacial e sequencial e dê um exemplo de cada um em um programa embarcado.

### b) (0,25 ponto)
Explique a função de registradores, cache, RAM e memória não volátil (Flash) em uma hierarquia de memória.

### c) (0,25 ponto)
Explique como cache, interrupções, DMA e escalonamento de tarefas podem introduzir variações no tempo de execução de uma rotina crítica.

### d) (0,25 ponto)
Proponha duas estratégias de programação/arquitetura de software para aumentar o determinismo temporal de um sistema embarcado de controle de voo. Justifique.

---

## Questão 8 — Decomposição Funcional e Diagrama de Blocos

Realize a decomposição funcional de um drone baseado na plataforma DJI F450, mantendo rastreabilidade entre funções, interfaces e componentes físicos.

### a) (0,20 ponto)
**Nível 0:** represente o sistema como uma caixa-preta, identificando entradas, saídas, energia, indicadores, sensores externos, rádio controle, estação de solo e as interfaces com o sistema físico e com o usuário.

### b) (0,25 ponto)
**Nível 1:** decomponha o drone em macroblocos funcionais (por exemplo: energia, sensoriamento, processamento/controle, comunicação, atuação e estrutura), evidenciando os fluxos entre eles.

### c) (0,30 ponto)
**Nível 2:** escolha dois macroblocos e detalhe suas funções internas, interfaces, sinais e dependências.

### d) (0,25 ponto)
**Nível 3/síntese:** para um dos blocos detalhados, faça o mapeamento das funções para componentes concretos. A solução deve ser consistente com uma lista de materiais (BOM) e com as interfaces definidas nos níveis anteriores.

---

## Questão 9 — Determinismo em sistemas embarcados

Confiabilidade em sistemas embarcados críticos depende de arquitetura, redundância, detecção de falhas, software e processos de validação.

### a) (0,25 ponto)
Defina confiabilidade, disponibilidade, segurança funcional (*safety*) e tolerância a falhas, diferenciando os conceitos.

### b) (0,30 ponto)
Compare duas controladoras de voo reais compatíveis com PX4 e/ou ArduPilot quanto a recursos de redundância e robustez (sensores redundantes, alimentação, barramentos, watchdogs ou outros mecanismos). Utilize documentação técnica dos fabricantes.

### c) (0,25 ponto)
Explique como redundância pode aumentar a confiabilidade, mas também introduzir complexidade e modos de falha comuns. Inclua o conceito de diversidade quando pertinente.

### d) (0,20 ponto)
Discuta criticamente o uso de hardware e software open source em sistemas embarcados críticos, apresentando pelo menos uma vantagem, um risco e uma medida de mitigação baseada em processo de engenharia/verificação.

---

## Questão 10 — Projeto de sistemas embarcados orientado por Inteligência Artificial

Utilize uma ferramenta de IA generativa para auxiliar no projeto conceitual de um drone destinado à inspeção externa da integridade física de aeronaves em aeroportos. A IA deve ser tratada como ferramenta de engenharia, e não como autoridade final de projeto.

### a) (0,20 ponto)
Formule o problema de engenharia, requisitos e restrições fornecidos à ferramenta de IA. Anexe os principais prompts e respostas utilizados.

### b) (0,25 ponto)
Apresente a arquitetura proposta pela IA, incluindo tipo de aeronave, sensores, computador/controladora de voo, software, comunicação, energia/autonomia, atuação e estratégias de redundância.

### c) (0,30 ponto)
Realize uma verificação crítica da proposta: identifique pelo menos dois aspectos tecnicamente adequados e dois aspectos que necessitam correção ou validação adicional. Para cada análise, relacione conceitos estudados na disciplina e fontes técnicas independentes.

### d) (0,25 ponto)
Explique quais evidências, testes, simulações, revisões humanas e etapas de V&V seriam necessárias antes que um engenheiro pudesse assumir responsabilidade técnica pelo projeto. Discuta também como deve ser documentado o uso da IA para garantir rastreabilidade das decisões de engenharia.
