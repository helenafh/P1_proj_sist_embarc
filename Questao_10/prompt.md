# Prompt — Projeto Conceitual de Drone para Inspeção Externa de Aeronaves

Estou desenvolvendo conceitualmente um sistema embarcado para um drone destinado à **inspeção visual externa da integridade física de aeronaves estacionadas em áreas controladas de aeroportos**.

O objetivo do sistema é realizar uma inspeção **sem contato físico com a aeronave**, coletando imagens de sua superfície externa que possam ser posteriormente utilizadas para identificar e documentar possíveis anomalias visíveis, como danos, deformações ou alterações superficiais.

## Forma de ativação da missão

Considere duas possibilidades para o início da missão:

1. **Ativação manual assistida:** após a aeronave estar estacionada na posição designada, um operador humano seleciona ou confirma a aeronave/plataforma a ser inspecionada e inicia a missão.

2. **Ativação automática:** o sistema recebe dados externos que indiquem que uma aeronave pousou, chegou à posição de estacionamento e está disponível para inspeção, podendo então iniciar automaticamente a missão após verificar as condições necessárias.

Avalie as duas alternativas e indique qual delas é mais adequada para uma primeira versão do sistema, considerando segurança, complexidade, integração com sistemas externos e necessidade de dados adicionais.

Em ambos os casos, o **operador humano deve ser capaz de abortar a missão a qualquer momento**.

## Componentes já estudados

Sempre que tecnicamente adequado, reutilize os seguintes componentes já estudados:

- estrutura quadricóptero **DJI F450**;
- controladora de voo **Holybro Pixhawk 6X**;
- sensores inerciais (**IMUs**) e barômetros presentes na controladora;
- módulo **GPS**;
- sensor LiDAR **Benewake TF-Luna**, utilizado principalmente para medição de distância;
- quatro ESCs;
- quatro motores e hélices;
- bateria LiPo compatível com a plataforma.

Manuais e documentações adicionais sobre esses recursos estão anexadas. Além dos arquivos adicionados, utilize os seguintes links quando necessário:

- https://github.com/PX4/PX4-user_guide/blob/main/tr/flight_controller/cubepilot_cube_orangeplus.md
- https://docs.holybro.com/autopilot/pixhawk-6x


Como a missão envolve inspeção visual, será necessário acrescentar pelo menos um sistema de captura de imagens. **Selecione e justifique um tipo de câmera adequado**, sem assumir que os componentes listados acima são suficientes para realizar toda a missão.

## Requisitos e restrições

Considere os seguintes requisitos:

1. A inspeção deve ocorrer apenas com a aeronave **estacionada** em uma área operacional previamente controlada.
2. A inspeção deve ser realizada **sem contato físico** com a aeronave.
3. O drone deve manter uma **distância segura da superfície da aeronave**.
4. Proponha uma **faixa inicial de distância operacional** entre o drone e a aeronave e justifique tecnicamente a escolha. Deixe claro quais aspectos dessa faixa exigiriam validação experimental.
5. Devem existir mecanismos para reduzir o risco de colisão.
6. O operador humano deve possuir supervisão da missão e capacidade de **abortar ou assumir o controle** quando necessário.
7. Devem ser previstos comportamentos de segurança para:
   - bateria crítica;
   - perda de comunicação;
   - falhas de sensores;
   - leituras inválidas ou inconsistentes;
   - outras condições relevantes identificadas durante o projeto.
8. O sistema deve transmitir **telemetria** para uma estação de solo.
9. As imagens da inspeção devem ser armazenadas ou transmitidas de forma que possam ser associadas ao trecho correspondente da aeronave.
10. Devem ser considerados autonomia de voo, massa, consumo de energia e capacidade de carga da plataforma.
11. Considere as limitações de GPS e demais sensores ao operar próximo a uma grande estrutura metálica.
12. Não considere os componentes propostos automaticamente seguros, certificados ou adequados a uso aeroportuário. Identifique explicitamente as hipóteses que exigem validação experimental, análise de engenharia ou certificação adicional.

## Tarefa

A partir dessas condições, proponha uma **arquitetura conceitual completa do sistema embarcado**.

A resposta deve apresentar:

- tipo e configuração da aeronave;
- avaliação das duas estratégias de ativação da missão e recomendação para uma primeira versão;
- sensores utilizados e função de cada um;
- sistema de captura de imagens;
- faixa proposta de distância segura de operação e sua justificativa;
- controladora de voo e, caso necessário, computador embarcado adicional;
- arquitetura de software;
- estratégia de navegação e controle;
- comunicação com a estação de solo;
- armazenamento e transmissão de dados;
- sistema de energia e estimativa conceitual de autonomia;
- sistema de atuação;
- mecanismos de redundância, detecção de falhas e *failsafe*;
- possibilidade de intervenção e aborto da missão pelo operador;
- principais interfaces entre os subsistemas;
- principais riscos, limitações e hipóteses que ainda precisam ser validados.

Organize a arquitetura em blocos funcionais e explique o fluxo de informações entre:

**sensoriamento → processamento/controle → atuação**

Inclua também os blocos de comunicação e energia.

Para cada decisão importante, apresente brevemente a justificativa técnica.

Quando algum requisito não puder ser atendido adequadamente pelos componentes já definidos, **não force a utilização do componente**. Identifique sua limitação e proponha uma alternativa ou componente complementar.

Esta é uma proposta conceitual para posterior avaliação por um engenheiro. Portanto, diferencie claramente:

- **decisões de projeto**;
- **hipóteses adotadas**;
- **recomendações da IA**;
- **pontos que exigem validação adicional**.