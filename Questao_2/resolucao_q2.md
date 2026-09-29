## Questão 2 — Model-Based Design

O desenvolvimento de sistemas embarcados complexos exige mecanismos que permitam especificar, analisar, simular e validar o sistema antes de sua implementação final.

### a) (0,25 ponto)
Explique o conceito de Model-Based Design (MBD) e sua relação com o ciclo de desenvolvimento de sistemas embarcados.

**Resposta:**

Model-Based Design (MBD), ou Projeto Baseado em Modelos, é uma metodologia de desenvolvimento em que modelos matemáticos e computacionais são utilizados como elemento central para especificar, projetar, simular e analisar o comportamento de um sistema antes de sua implementação final.

De acordo com a abordagem apresentada na disciplina, o MBD pode ser entendido a partir de três partes principais:

1. Modelagem: criação e refinamento de representações abstratas do sistema, incluindo tanto o processo físico quanto os algoritmos e componentes computacionais.

2. Projeto: definição da arquitetura e da organização do sistema, incluindo hardware, software, interfaces e decomposição funcional.

3. Análise: avaliação das propriedades e do comportamento dos modelos, como desempenho, estabilidade, temporização, consumo de recursos e atendimento aos requisitos.

No desenvolvimento de sistemas embarcados, essa abordagem permite estudar e validar o comportamento do sistema ainda nas etapas iniciais do projeto, antes da implementação completa em hardware. Isso facilita a identificação precoce de erros, reduz retrabalho e permite o refinamento progressivo dos modelos até sua implementação.

Além disso, os modelos servem como uma representação comum entre diferentes áreas de engenharia, como software, eletrônica, controle e mecânica, facilitando a integração entre subsistemas e apoiando as etapas posteriores de verificação e validação.




### b) (0,25 ponto)
Descreva um fluxo MBD contendo, no mínimo: requisitos, modelagem, simulação, refinamento, implementação e verificação/validação.

**Resposta:**

Um fluxo típico de Model-Based Design (MBD) pode ser organizado nas seguintes etapas:

1. Definição de requisitos: são estabelecidos os requisitos do sistema, como comportamento esperado, desempenho, restrições de tempo, consumo de energia, segurança e interfaces.

2. Modelagem: são construídos modelos matemáticos e computacionais que representam o sistema físico, os algoritmos de controle e os principais subsistemas envolvidos.

3. Simulação: os modelos são executados em ambiente virtual para analisar o comportamento do sistema em diferentes condições de operação e verificar se os requisitos estão sendo atendidos.

4. Refinamento: com base nos resultados das simulações, os modelos e decisões de projeto são ajustados, corrigidos ou detalhados progressivamente, reduzindo simplificações e aproximando o modelo da implementação real.

5. Implementação: após o modelo atingir um nível adequado de maturidade, os algoritmos e funções definidos são implementados no hardware e software embarcados, manualmente ou com auxílio de ferramentas de geração automática de código.

6. Verificação e validação: confere se a implementação está de acordo com o modelo e com as especificações do projeto e se o sistema final atende aos requisitos definidos inicialmente.

Esse fluxo é iterativo, de forma que resultados obtidos durante simulação, implementação ou verificação podem exigir o retorno a etapas anteriores para ajustes no modelo, nos requisitos ou no próprio projeto. Abaixo está um diagrama visual do fluxo MBD descrito acima.

[Diagrama do fluxo MBD](diagrama_2b.md)




### c) (0,25 ponto)
Explique a diferença entre Model-in-the-Loop (MIL), Software-in-the-Loop (SIL) e Hardware-in-the-Loop (HIL).

**Resposta:**

MIL, SIL e HIL são etapas progressivas de verificação em um fluxo MBD, diferenciadas principalmente por quais partes do sistema são modelos, software executável ou hardware real.

- MIL (Model-in-the-Loop): tanto o controlador quanto a planta são representados por modelos matemáticos ou computacionais. O objetivo é validar a lógica de controle e o comportamento funcional do sistema ainda em um ambiente totalmente simulado.

- SIL (Software-in-the-Loop): o modelo do controlador é substituído pelo software que será utilizado na implementação, enquanto a planta continua simulada. Essa etapa permite verificar se o comportamento do código permanece consistente com o modelo e identificar efeitos associados à implementação computacional.

- HIL (Hardware-in-the-Loop): o controlador passa a executar no hardware embarcado real, com o firmware destinado ao sistema final, enquanto a planta continua sendo simulada, normalmente em tempo real. Isso permite avaliar aspectos mais próximos da implementação real, como temporização, interfaces de entrada e saída e comportamento do sistema em diferentes condições de operação e falha.

Assim, a progressão aumenta gradualmente o grau de realismo dos testes: parte-se de modelos puramente virtuais, passa-se para o software implementado e, por fim, incorpora-se o hardware real do controlador, mantendo a planta simulada.




### d) (0,25 ponto)
Usando o multirotor DJI F450 como exemplo, indique um subsistema que poderia ser desenvolvido por MBD e descreva quais modelos, entradas, saídas e critérios de validação seriam utilizados.

**Resposta:**

O DJI FlameWheel F450 é uma plataforma multi-rotor do tipo quadricóptero fornecida como um conjunto de peças para o usuário montar. O manual indica que a plataforma pode ser utilizada com sistemas de autopiloto para realizar funções como estabilização e hovering. O kit inclui quatro motores e quatro ESCs (Electronic Speed Controller - Controlador Eletrônico de Velocidade), mas o sistema de controle de voo deve ser instalado separadamente pelo projetista.

Um possível subsistema para desenvolvimento por MBD seria o controle de atitude, responsável por estabilizar a orientação do multirotor em torno dos seus eixos (roll, pitch e yaw).

Para o desenvolvimento seriam utilizados principalmente dois grupos de modelos:

- Modelo da planta, representando a dinâmica rotacional do F450, incluindo a relação entre o empuxo/torque produzido pelos quatro motores e a variação da orientação e das velocidades angulares do veículo.

- Modelo do controlador, responsável por comparar a atitude desejada com a atitude estimada e calcular os comandos necessários para os quatro motores. Também podem ser incorporados modelos simplificados dos sensores e dos atuadores para representar atrasos, ruídos e limitações do sistema real.

As entradas do controlador seriam as referências de atitude ou velocidade angular desejadas e as medidas/estimativas do estado atual do veículo, como ângulos de roll, pitch e yaw e respectivas velocidades angulares.

As saídas seriam os comandos enviados aos quatro ESCs, que controlam a velocidade dos motores. No F450, os ESCs recebem o sinal da controladora e possuem entrada PWM compatível com níveis de 3,3 V ou 5 V, de acordo com o manual.

Como critérios de validação, poderiam ser avaliados a estabilidade da atitude, erro em regime permanente, tempo de resposta, sobressinal, capacidade de rejeitar perturbações e comportamento diante das limitações dos atuadores. O controlador também deveria manter seu funcionamento dentro das características físicas e elétricas da plataforma.

O desenvolvimento poderia avançar progressivamente: utilizando MIL, inicialmente validando os modelos da planta e do controlador; SIL, verificando o comportamento do software de controle contra a planta simulada; e HIL, executando o controlador no hardware embarcado real enquanto a dinâmica do F450 é simulada em tempo real. Dessa forma, parte significativa da validação pode ser realizada antes dos ensaios de voo.
