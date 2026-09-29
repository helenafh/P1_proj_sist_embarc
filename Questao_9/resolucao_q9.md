## Questão 9 — Determinismo em sistemas embarcados

Confiabilidade em sistemas embarcados críticos depende de arquitetura, redundância, detecção de falhas, software e processos de validação.

### a) (0,25 ponto)
Defina confiabilidade, disponibilidade, segurança funcional (*safety*) e tolerância a falhas, diferenciando os conceitos.

**Resposta:**

Em sistemas embarcados críticos, confiabilidade, disponibilidade, segurança funcional e tolerância a falhas são conceitos relacionados, mas com objetivos distintos.

- Confiabilidade (Reliability): é a capacidade ou probabilidade de o sistema executar corretamente sua função durante um intervalo de tempo especificado, sob condições definidas, sem apresentar falhas. Seu foco é a continuidade do serviço correto ao longo do tempo.

- Disponibilidade (Availability): representa a probabilidade ou fração de tempo em que o sistema está operacional e pronto para ser utilizado quando solicitado. Diferentemente da confiabilidade, admite que falhas possam ocorrer, desde que o sistema seja restaurado rapidamente. Uma métrica típica é:
$A =  \frac {MTBF} {MTBF + MTTR}$
onde MTBF é o tempo médio entre falhas e MTTR o tempo médio de reparo ou recuperação.

- Segurança funcional (Safety): refere-se à redução de riscos de danos inaceitáveis a pessoas, equipamentos ou ao ambiente causados por falhas do sistema. O objetivo não é necessariamente manter o funcionamento a qualquer custo, mas garantir que, diante de uma falha, o sistema evolua para uma condição segura, quando necessário.

- Tolerância a falhas (Fault Tolerance): é a capacidade do sistema de continuar prestando seu serviço corretamente, ou em um nível degradado aceitável, mesmo na presença de falhas. Para isso, podem ser utilizados mecanismos como redundância de hardware ou software, memória com ECC (Error-Correcting Code) e estratégias automáticas de recuperação.

Assim, confiabilidade está relacionada a evitar falhas ao longo do tempo, disponibilidade à prontidão do sistema para operar, safety à prevenção de consequências perigosas e tolerância a falhas aos mecanismos que permitem manter ou degradar de forma controlada a operação quando falhas ocorrem.




### b) (0,30 ponto)
Compare duas controladoras de voo reais compatíveis com PX4 e/ou ArduPilot quanto a recursos de redundância e robustez (sensores redundantes, alimentação, barramentos, watchdogs ou outros mecanismos). Utilize documentação técnica dos fabricantes.

**Resposta:**

Foram comparadas duas controladoras de voo compatíveis com PX4: Holybro Pixhawk 6X e CubePilot Cube Orange+. Ambas utilizam redundância de sensores e múltiplas interfaces de comunicação, mas apresentam diferenças na forma como implementam isolamento, alimentação e mecanismos auxiliares de recuperação. Aqui está uma tabela com as principais informações sobre ambas.


| Critério | Holybro Pixhawk 6X | CubePilot Cube Orange+ |
|---|---|---|
| **Processador principal** | STM32H753, Cortex-M7, 480 MHz | STM32H757, Cortex-M7, 400 MHz |
| **IMUs** | 3 IMUs redundantes | 3 IMUs redundantes |
| **Barômetros** | 2 barômetros redundantes | 2 barômetros redundantes |
| **Isolamento / independência dos sensores** | Sensores redundantes em barramentos separados, com domínios isolados e alimentação independente por conjunto de sensores | Duas IMUs possuem isolamento contra vibração e uma terceira IMU fixa atua como referência/backup |
| **Alimentação** | 2 portas de alimentação; arquitetura com alimentação independente dos conjuntos de sensores | Entradas de alimentação redundantes com *failover* automático |
| **Processador auxiliar** | STM32F103 como processador de I/O | STM32F103 como coprocessador de *failsafe* |
| **Barramentos de comunicação** | 2 CAN, SPI, I²C, UART e Ethernet | 2 CAN, SPI, I²C e UART |
| **Recursos adicionais de robustez** | Isolamento de vibração, controle térmico das IMUs e troca para sensor redundante quando uma falha é detectada pelo PX4 | Sistema integrado de backup para recuperação em voo e *manual override*, com processador e alimentação independentes |
| **Compatibilidade** | PX4 e ArduPilot | PX4 |

As duas controladoras utilizam redundância de sensores e múltiplos mecanismos de robustez, mas com ênfases diferentes. A Pixhawk 6X se destaca pela separação dos sensores em barramentos e domínios de alimentação independentes, favorecendo o isolamento de falhas. Já a Cube Orange+ combina redundância de sensores com recursos adicionais de failsafe, como alimentação redundante com failover automático e um co-processador dedicado à recuperação e supervisão.

Assim, a Pixhawk 6X enfatiza principalmente a independência entre os canais de sensoriamento, enquanto a Cube Orange+ apresenta uma arquitetura mais voltada à continuidade de operação e recuperação diante de falhas.


### c) (0,25 ponto)
Explique como redundância pode aumentar a confiabilidade, mas também introduzir complexidade e modos de falha comuns. Inclua o conceito de diversidade quando pertinente.

**Resposta:**

A redundância pode aumentar a confiabilidade de um sistema embarcado porque permite que uma falha em um componente seja detectada, isolada ou compensada por outro elemento equivalente. Isso pode ser feito, por exemplo, com sensores duplicados, fontes de alimentação redundantes ou múltiplos processadores executando funções críticas.

Entretanto, a redundância também aumenta a complexidade da arquitetura, exigindo mecanismos adicionais de comparação, votação, sincronização, diagnóstico e gerenciamento de falhas. Esses próprios mecanismos podem introduzir novos modos de falha.

Além disso, componentes redundantes podem estar sujeitos a falhas de modo comum, quando uma mesma causa afeta simultaneamente vários elementos supostamente independentes. Isso pode ocorrer, por exemplo, quando sensores redundantes compartilham a mesma fonte de alimentação, o mesmo barramento de comunicação, o mesmo algoritmo de software ou estão expostos às mesmas condições ambientais.

Uma forma de reduzir esse risco é utilizar diversidade, isto é, empregar componentes ou implementações diferentes para realizar a mesma função. Pode-se, por exemplo, combinar sensores de tecnologias distintas, fornecedores diferentes ou versões independentes de software. Dessa forma, diminui-se a probabilidade de uma única causa provocar a falha simultânea de todos os elementos redundantes.

Portanto, a redundância pode melhorar significativamente a tolerância a falhas, mas deve ser projetada com independência suficiente entre os canais redundantes e acompanhada de mecanismos adequados de detecção, isolamento e recuperação.




### d) (0,20 ponto)
Discuta criticamente o uso de hardware e software open source em sistemas embarcados críticos, apresentando pelo menos uma vantagem, um risco e uma medida de mitigação baseada em processo de engenharia/verificação.

**Resposta:**


O uso de hardware e software open source em sistemas embarcados críticos pode trazer benefícios importantes, mas exige cuidados adicionais de engenharia.

Uma vantagem é a transparência do projeto: código-fonte, esquemas e documentação podem ser inspecionados, auditados e modificados pela equipe de desenvolvimento, além de se beneficiarem da revisão por uma comunidade ampla. Isso pode acelerar a detecção de defeitos, facilitar a adaptação do sistema e reduzir a dependência de um único fornecedor.

Por outro lado, um risco é assumir que, por ser amplamente utilizado ou publicamente disponível, o projeto já está automaticamente validado para aplicações críticas. Componentes open source podem apresentar bugs, mudanças frequentes, dependências externas ou ausência de evidências formais de desenvolvimento segundo processos de segurança funcional.

Uma medida de mitigação é tratar o componente open source como qualquer outro elemento crítico do projeto: selecionar e congelar versões específicas, manter rastreabilidade das alterações, realizar revisão de código, análise de requisitos, testes unitários e de integração, análise de falhas e campanhas de verificação e validação antes da liberação. Também é importante documentar quais partes do sistema dependem desse componente e quais evidências sustentam sua aceitação.

Assim, o caráter open source pode ser uma vantagem para transparência e auditabilidade, mas não substitui um processo formal de engenharia, verificação e validação.

