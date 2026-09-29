## Questão 6 — Sensores e Atuadores

Considere a integração de um sensor LiDAR compacto ao multirotor utilizado nas aulas.

### a) (0,25 ponto)
Explique o princípio de funcionamento de um LiDAR do tipo time-of-flight e identifique as principais grandezas medidas e fontes de erro.

**Resposta:**

Um sensor LiDAR (Light Detection and Ranging) do tipo Time-of-Flight (ToF) mede distâncias por meio da emissão de pulsos de luz e da medição do intervalo de tempo necessário para que o sinal refletido por um objeto retorne ao sensor.

Conhecendo a velocidade da luz c e o tempo de voo medido t, a distância ao alvo pode ser estimada por:

$d = \frac {c \Delta t}2$

O fator 2 aparece porque o pulso percorre o trajeto de ida até o obstáculo e de volta até o fotodetector.

As principais grandezas envolvidas são o tempo de voo e a distância estimada. Entre as principais fontes de erro estão:

- Incertezas na medição do tempo: afetam diretamente a distância calculada.

- Ruído e interferência de luz ambiente: podem dificultar a detecção do pulso refletido.

- Propriedades da superfície do alvo, como baixa refletividade ou reflexão especular: podem atrapalhar a leitura.

- Efeitos de multicaminho: quando a luz retorna ao sensor por trajetórias indiretas.

- Erros de alinhamento e vibração mecânica: especialmente relevantes quando o sensor está instalado em um multi-rotor, pode resultar em imprecisões ou dificuldade na realização da leitura.

- Limitações de faixa, resolução e taxa de amostragem: podem reduzir a precisão ou provocar perda de detecção de objetos pequenos ou em movimento.

Assim, embora o princípio ToF seja simples, o desempenho real do LiDAR depende tanto da eletrônica de medição quanto das condições ópticas, mecânicas e ambientais.




### b) (0,25 ponto)
A partir de um modelo comercial adequado para drones (por exemplo, Benewake TF-Luna/TFmini-S ou equivalente), consulte o manual/datasheet e apresente: faixa de medição, resolução/precisão, taxa de atualização, tensão de alimentação e interface digital.

**Resposta:**

O sensor escolhido foi o Benewake TF-Luna, um LiDAR ToF de curto alcance. A partir do Datasheet, foi possível obter as informações solicitadas:

| Parâmetro | TF-Luna | 
|---|---|
| **Faixa de medição** | 0,2–8 m para alvo com 90% de refletividade; 0,2–2,5 m para alvo com 10% de refletividade | 
| **Precisão** | ±6 cm entre 0,2–3 m; ±2% entre 3–8 m | 
| **Resolução** | 1 cm | 
| **Taxa de atualização** | 1–250 Hz, com 100 Hz como valor padrão |   
| **Tensão de alimentação** | 3,7–5,2 V | 
| **Interfaces digitais** | UART, I²C e I/O | 
| **Nível lógico de comunicação** | LVTTL 3,3 V | 


### c) (0,30 ponto)
Desenhe um diagrama de blocos mostrando a interface elétrica e lógica entre o sensor e o computador de voo, incluindo alimentação, interface de comunicação, driver e camada de software que disponibiliza a medida à aplicação.

**Resposta:**

O TF-Luna é alimentado por uma tensão entre 3,7 V e 5,2 V e utiliza nível lógico LVTTL de 3,3 V para comunicação. No diagrama foi considerada a interface UART, na qual o pino TXD do TF-Luna envia os dados de medição para o RX da controladora, enquanto o TX da controladora pode enviar comandos e configurações ao RXD do sensor.

No computador de voo, os dados percorrem diferentes camadas de software:

- o driver UART recebe os bytes da interface serial;

- o driver/parser do TF-Luna interpreta o protocolo e extrai as informações do sensor;

- uma interface lógica/API disponibiliza dados como distância, intensidade do sinal e status da medida;

- a aplicação utiliza essas informações em funções de controle, navegação ou detecção de obstáculos. O manual informa que o TF-Luna disponibiliza, entre outros dados, distância e intensidade do sinal.

A partir dessas informações foi desenvolvido o seguinte diagrama inicial:
[Diagrama inicial](diagrama_6c1.md)

Mais detalhes foram adicionados e uma nova versão mais completa foi desenvolvida. Nessa nova versão, as setas contínuas representam o fluxo principal de alimentação e dados de medição, enquanto as setas pontilhadas representam o fluxo de comandos e configuração enviado da aplicação para o sensor.

[Diagrama completo](diagrama_6c2.md)



### d) (0,20 ponto)
Explique como calibração, ruído, taxa de amostragem, latência e possíveis falhas do sensor podem afetar o comportamento do sistema embarcado e indique pelo menos uma estratégia de mitigação.

**Resposta:**

O desempenho do sistema embarcado depende diretamente da qualidade e da confiabilidade das medições fornecidas pelo LiDAR.

- Calibração: erros de offset, alinhamento ou montagem podem introduzir um viés sistemático nas distâncias medidas. Em um multirotor, isso pode fazer o sistema interpretar incorretamente a altura ou a proximidade de obstáculos. Para prevenir, pode-se realizar calibração inicial e verificar o alinhamento mecânico do sensor.

- Ruído: interferências ópticas, baixa refletividade do alvo e variações no sinal recebido podem causar flutuações nas medidas. O manual do TF-Luna indica que a confiabilidade depende da intensidade do sinal e que valores muito baixos ou saturados podem tornar a distância pouco confiável. Para prevenir, podem ser utilizados filtros digitais, validação da intensidade do retorno e descarte de amostras inválidas.

- Taxa de amostragem: uma frequência muito baixa pode fazer o sistema detectar mudanças no ambiente com atraso, enquanto frequências mais altas aumentam a quantidade de dados processados. O TF-Luna permite ajustar a taxa de saída entre 1 Hz e 250 Hz. A frequência deve, portanto, ser escolhida de acordo com a dinâmica da aplicação.

- Latência: o tempo entre a medição, transmissão, processamento e uso da informação pelo controlador pode fazer com que a decisão seja baseada em uma situação já alterada. Em aplicações de controle e navegação, latências elevadas podem reduzir a estabilidade e a qualidade da resposta. É importante considerar a latência para garantir que o sistema é capaz de cumprir os requisitos do projeto.

- Falhas do sensor: perda de comunicação, leituras fora da faixa válida ou medições inconsistentes podem levar a decisões incorretas. Para prevenir, o software pode monitorar limites, intensidade do sinal, timeout de comunicação e consistência temporal das amostras, ativando uma estratégia de contingência quando a medida não for confiável.

Assim, estratégias como calibração periódica, filtragem, validação das amostras, monitoramento de timeout e tratamento de falhas ajudam a reduzir o impacto das não idealidades do sensor no comportamento do sistema embarcado.

