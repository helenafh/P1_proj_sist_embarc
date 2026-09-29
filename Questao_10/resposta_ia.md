A arquitetura mais coerente para uma primeira versão é um **quadricóptero DJI F450 com Pixhawk 6X executando o controle de voo, um computador embarcado leve para navegação relativa/gerenciamento da inspeção e uma câmera RGB dedicada**, mantendo o TF-Luna como sensor de distância frontal, mas **não como único mecanismo anticolisão**.

Há um ponto importante antes da arquitetura: o link do *Cube Orange+* que você forneceu descreve uma controladora diferente da Holybro Pixhawk 6X. Portanto, abaixo uso as especificações do **Pixhawk 6X** para o projeto e não misturo características do Cube Orange+ com ele. A documentação atual do Pixhawk 6X indica processador STM32H753, três IMUs, dois barômetros, Ethernet, CAN, UARTs, I²C e múltiplas entradas de alimentação. :chatgpt-content-reference{index="1"}

---

# 1. Visão geral da arquitetura proposta

A arquitetura pode ser dividida em cinco blocos principais:

```text
                    ┌─────────────────────────────┐
                    │       ESTAÇÃO DE SOLO      │
                    │                             │
                    │ • supervisão da missão     │
                    │ • telemetria               │
                    │ • vídeo / imagens          │
                    │ • ABORT / controle manual  │
                    └──────────────┬──────────────┘
                                   │
                             rádio / MAVLink
                                   │
                                   ▼
┌──────────────────────────────────────────────────────────┐
│                         DRONE                            │
│                                                          │
│  ┌────────────── SENSORIAMENTO ──────────────────────┐   │
│  │                                                    │   │
│  │  IMUs ───────────┐                                 │   │
│  │  barômetros ─────┤                                 │   │
│  │  GPS ────────────┤                                 │   │
│  │  TF-Luna ────────┤                                 │   │
│  │  câmera navegação├──► estimação / percepção        │   │
│  │  câmera inspeção ┘                                 │   │
│  └──────────────────────────┬─────────────────────────┘   │
│                             │                             │
│                             ▼                             │
│  ┌────────── PROCESSAMENTO / CONTROLE ────────────────┐   │
│  │                                                    │   │
│  │ Computador embarcado                              │   │
│  │ • gerenciamento da missão                         │   │
│  │ • VIO / navegação relativa                        │   │
│  │ • aquisição de imagens                            │   │
│  │ • planejamento de trajetória                      │   │
│  │ • monitoramento de sensores                       │   │
│  │                  │                                 │   │
│  │                  ▼ MAVLink/Ethernet               │   │
│  │ Pixhawk 6X                                         │   │
│  │ • EKF                                              │   │
│  │ • estabilização                                    │   │
│  │ • attitude/position control                        │   │
│  │ • failsafes                                        │   │
│  └──────────────────────────┬─────────────────────────┘   │
│                             │                             │
│                             ▼                             │
│  ┌──────────────── ATUAÇÃO ──────────────────────────┐   │
│  │ Pixhawk → 4 ESCs → 4 motores → hélices           │   │
│  └────────────────────────────────────────────────────┘   │
│                                                          │
│          ┌──────── ENERGIA ─────────────────┐             │
│          │ LiPo → distribuição de potência │             │
│          │      → ESCs                     │             │
│          │      → power module → Pixhawk   │             │
│          │      → DC/DC → companion/câmera │             │
│          └──────────────────────────────────┘             │
└──────────────────────────────────────────────────────────┘
```

A ideia fundamental é separar o **controle crítico de voo** do processamento de alto nível. O Pixhawk deve conseguir manter o drone estabilizado e executar *failsafes* mesmo que o computador embarcado reinicie ou trave.

---

# 2. Aeronave

### Decisão de projeto
Usar inicialmente o **DJI FlameWheel F450 em configuração quadricóptero X**.

O manual especifica:

- distância diagonal entre motores: **450 mm**;
- massa do frame: **282 g**;
- massa de decolagem indicada: **800–1600 g**;
- bateria: **LiPo 3S–4S**;
- hélices de aproximadamente 9,4 × 5";
- motores de 960 KV. :chatgpt-content-reference{index="2"}

### Justificativa

É uma plataforma simples, conhecida pelo grupo e compatível com o objetivo de construir um demonstrador conceitual.

Entretanto, o limite de **1600 g é uma restrição importante**. A soma de:

- bateria;
- Pixhawk;
- GPS;
- câmera;
- computador embarcado;
- comunicação;
- sensores adicionais;
- conversores DC/DC;
- estrutura de montagem;

pode aproximar rapidamente o drone desse limite.

### Validação necessária

Antes de fechar a arquitetura física deve ser feita uma **planilha de orçamento de massa**, seguida de ensaio de empuxo.

Para uma aeronave de inspeção real eu não trataria o F450 como plataforma definitivamente selecionada. Ele é apropriado como **plataforma experimental**, condicionado à validação de massa, autonomia, vibração e margem de empuxo.

---

# 3. Estratégia de ativação da missão

## Opção A — ativação manual assistida

Fluxo:

```text
aeronave estaciona
       ↓
operador confirma aeronave / posição
       ↓
sistema verifica pré-condições
       ↓
operador autoriza missão
       ↓
drone inicia inspeção
```

As verificações poderiam incluir:

- bateria suficiente;
- sensores operacionais;
- enlace de comunicação ativo;
- área liberada;
- posição inicial conhecida;
- aeronave confirmada como estacionada;
- ausência de pessoal ou veículos dentro da zona de operação.

---

## Opção B — ativação automática

Seria necessário receber informações como:

```text
aeronave pousou
      ↓
taxiou
      ↓
chegou ao stand correto
      ↓
motores desligados
      ↓
área liberada
      ↓
aeronave identificada
      ↓
autorização operacional
      ↓
início da inspeção
```

Isso cria dependência de sistemas aeroportuários que estão fora do drone.

Seriam necessários, por exemplo:

- identificação da aeronave;
- posição de estacionamento;
- estado do *stand*;
- liberação operacional;
- identificação do modelo da aeronave;
- eventualmente interface com sistemas de gestão aeroportuária.

---

## Recomendação da IA para V1

Para a primeira versão, recomendo **ativação manual assistida**.

Não porque a automatização seja tecnicamente impossível, mas porque ela introduz complexidade em uma parte que não é central para validar o sistema embarcado.

Uma evolução particularmente interessante seria uma arquitetura híbrida:

```text
sistema externo detecta chegada
            ↓
      missão é preparada
            ↓
 operador recebe solicitação
            ↓
      CONFIRMAR / CANCELAR
            ↓
          decolagem
```

Assim, os mecanismos automáticos podem ser desenvolvidos sem remover inicialmente a autorização humana.

---

# 4. Controladora de voo

### Decisão de projeto
**Holybro Pixhawk 6X**, executando PX4.

A controladora dispõe de três IMUs e dois barômetros redundantes. O PX4 pode selecionar sensores alternativos quando identifica falhas. Além disso, a controladora oferece Ethernet, UART, I²C, CAN e múltiplas saídas PWM. :chatgpt-content-reference{index="3"}

Isso é bastante interessante porque permite separar:

```text
Pixhawk
│
├── flight control
├── IMU/barômetro
├── controle de motores
├── failsafes
├── GPS
└── interfaces críticas

Companion computer
│
├── câmera
├── navegação visual
├── planejamento
├── gerenciamento da inspeção
└── armazenamento
```

O Pixhawk 6X possui inclusive interface Ethernet destinada à integração com computadores de missão. :chatgpt-content-reference{index="4"}

---

# 5. Sensores

## 5.1 IMUs

Já existentes no Pixhawk.

### Função

Medir:

- aceleração;
- velocidade angular;
- atitude;
- dinâmica de curto prazo.

As três IMUs oferecem **redundância de medição**, embora isso não torne o sistema automaticamente tolerante a qualquer falha. :chatgpt-content-reference{index="5"}

---

# 6. Barômetros

Utilizados principalmente para auxílio na estimação da altitude.

O Pixhawk 6X possui dois barômetros redundantes. :chatgpt-content-reference{index="6"}

### Limitação

Próximo a uma aeronave, o barômetro não deve ser considerado suficiente para manter distância de superfícies porque mede pressão atmosférica, não distância relativa à aeronave.

---

# 7. GPS

### Decisão de projeto

Manter GPS, porém **não utilizá-lo como referência primária durante a inspeção próxima à aeronave**.

Ele continua útil para:

- posição global;
- navegação até a área de inspeção;
- *geofence*;
- referência aproximada;
- retorno quando o ambiente permitir.

### Problema

A grande estrutura metálica da aeronave pode produzir:

- bloqueio de satélites;
- multipercurso;
- degradação de geometria;
- erros abruptos de posição.

Portanto, uma lógica como:

```text
GPS → posição absoluta
```

não é suficiente para:

```text
manter exatamente 2 m da fuselagem.
```

---

# 8. TF-Luna

O TF-Luna é um LiDAR de ponto único baseado em *Time of Flight*. O datasheet indica alcance de **0,2–8 m para alvo de 90% de refletividade**, mas somente **0,2–2,5 m para um alvo de 10% de refletividade**. A precisão especificada é ±6 cm entre 0,2 e 3 m e ±2% entre 3 e 8 m. :chatgpt-content-reference{index="7"}

O manual também destaca que o resultado depende da refletividade e que medições abaixo de 20 cm não são confiáveis. :chatgpt-content-reference{index="8"}

### Decisão de projeto

Usar o TF-Luna como **sensor frontal de distância à superfície**.

```text
             TF-Luna
                 ↓
drone ───────────────► fuselagem
              d
```

Ele ajuda a manter o *stand-off* durante passagens laterais.

### Interface

Pode ser conectado por UART ou I²C. O datasheet especifica UART, I²C e I/O e alimentação de 3,7–5,2 V, com potência de até aproximadamente 0,35 W. :chatgpt-content-reference{index="9"}

Para o protótipo eu utilizaria **UART**, por simplicidade e isolamento da comunicação.

---

# 9. Limitação fundamental do TF-Luna

O TF-Luna é **single-point**.

Isso significa que ele responde essencialmente:

```text
"qual é a distância do objeto nesta direção?"
```

e não:

```text
"existem obstáculos ao redor do drone?"
```

Há ainda uma situação descrita pelo próprio manual: se o feixe atingir superfícies em distâncias diferentes, a medição pode assumir valores intermediários. :chatgpt-content-reference{index="10"}

Isso é relevante ao trabalhar perto de:

- bordo de asa;
- trem de pouso;
- motores;
- empenagem;
- interfaces entre fuselagem e asa.

### Recomendação da IA

Não utilizar o TF-Luna como único sistema anticolisão.

Para uma versão mais robusta acrescentaria **percepção tridimensional ou multidirecional**, como:

- câmera estéreo/de profundidade;
- LiDAR 2D/3D leve;
- vários sensores ToF em direções diferentes.

Para uma primeira bancada experimental, também seria possível utilizar vários pequenos sensores de distância:

```text
          frontal
             ↑
             │
lateral ← [DRONE] → lateral

             ↓
            solo
```

Mas isso continua sendo menos completo que percepção 3D.

---

# 10. Distância operacional proposta

### Recomendação inicial

Eu começaria com:

**faixa operacional: aproximadamente 1,5–2,5 m da superfície**, com **2,0 m como distância nominal**.

```text
mínimo             nominal                máximo
1,5 m                2,0 m                 2,5 m
 |--------------------|----------------------|
                    drone
                       → superfície
```

Essa faixa não deve ser interpretada como um valor certificado.

Há três razões principais.

### 1. Margem física

O F450 possui wheelbase de 450 mm e hélices próximas de 24 cm de diâmetro. :chatgpt-content-reference{index="11"}

Manter o centro/sensor a aproximadamente 2 m da superfície deixa uma margem razoável entre o envelope das hélices e a aeronave.

### 2. TF-Luna

Superfícies aeronáuticas podem apresentar diferentes:

- cores;
- pinturas;
- brilho;
- ângulos;
- refletividades.

Como o alcance declarado cai para cerca de 2,5 m em um alvo de baixa refletividade, operar nominalmente em torno de 2 m mantém o sensor dentro de uma faixa mais conservadora. :chatgpt-content-reference{index="12"}

### 3. Qualidade de imagem

Uma câmera de inspeção normalmente se beneficia de não estar excessivamente distante do objeto.

Por exemplo, com uma lente de aproximadamente 50° de campo horizontal:

\[
W=2D\tan\left(\frac{FOV}{2}\right)
\]

Para \(D=2\,m\):

\[
W \approx 1,87\,m
\]

Uma imagem com aproximadamente 5000 pixels de largura corresponderia idealmente a:

\[
\frac{1,87}{5000}\approx0,37\text{ mm/pixel}
\]

Isso **não significa automaticamente que danos de 0,37 mm possam ser detectados**, porque foco, vibração, contraste, iluminação e movimento afetam bastante o resultado. Serve apenas como estimativa geométrica inicial.

### Validação obrigatória

A faixa 1,5–2,5 m precisa ser testada considerando:

- rotor wash;
- *Foreign Object Debris*;
- precisão do controle;
- erro máximo do sensor;
- erro de navegação;
- vento;
- textura/refletividade da pintura;
- curvatura da fuselagem;
- desempenho da câmera;
- tamanho mínimo de defeito desejado.

---

# 11. Sistema de captura de imagens

Este é um componente adicional obrigatório.

### Recomendação

Uma **câmera RGB de alta resolução, aproximadamente 12–20 MP**, com:

- foco ajustável ou calibrado para 1,5–3 m;
- exposição curta;
- boa sensibilidade à luz;
- controle de disparo;
- *timestamp*;
- lente com baixa distorção;
- FOV aproximadamente 40–60°.

Idealmente utilizaria **global shutter**, principalmente para reduzir deformações associadas ao movimento.

Entretanto, uma câmera de alta resolução com *rolling shutter* também pode ser testada se:

- a velocidade for baixa;
- o drone estabilizar antes da captura;
- forem usados tempos curtos de exposição.

### Por que não usar somente uma câmera FPV?

Porque a câmera de inspeção precisa priorizar:

- resolução;
- qualidade óptica;
- rastreabilidade da imagem;
- sincronização.

A câmera usada para navegação pode ter requisitos diferentes.

---

# 12. Duas câmeras são preferíveis

Arquiteturalmente eu separaria:

```text
CAMERA DE NAVEGAÇÃO
       │
       └── VIO / percepção / obstáculos

CAMERA DE INSPEÇÃO
       │
       └── imagens de alta resolução da aeronave
```

Assim uma mudança de lente ou exposição da câmera de inspeção não interfere na navegação.

### Hipótese para V1

Se orçamento e massa forem muito restritos, uma única câmera pode ser compartilhada, mas isso cria acoplamento indesejável entre funções.

---

# 13. Montagem da câmera

Há duas possibilidades.

### V1 — montagem rígida

```text
drone ───► câmera ───► fuselagem
```

O próprio drone muda *yaw* para manter a câmera aproximadamente normal à superfície.

Vantagens:

- pouca massa;
- simplicidade;
- menor consumo.

É a minha recomendação para o primeiro protótipo F450.

### Versão futura — gimbal

Um *gimbal* de dois ou três eixos permite melhor enquadramento independentemente da atitude.

Porém:

- aumenta massa;
- aumenta consumo;
- ocupa volume;
- aproxima o sistema do limite do F450.

---

# 14. Computador embarcado

### Decisão recomendada

Adicionar um **companion computer Linux leve**.

Para a V1 eu favoreceria algo da classe:

- Raspberry Pi CM4/CM5;
- Raspberry Pi 5;
- computador ARM equivalente.

Um Jetson pode ser utilizado futuramente se houver necessidade de IA embarcada para identificar danos.

### Por que não executar tudo no Pixhawk?

Porque o Pixhawk deve ser dedicado a tarefas de tempo real:

```text
IMU → EKF → controle → motor
```

Enquanto o computador de missão executa:

```text
câmeras
VIO
processamento de imagem
missão
trajetória
armazenamento
```

A própria arquitetura do Pixhawk 6X disponibiliza Ethernet para integração com computador de missão. :chatgpt-content-reference{index="13"}

---

# 15. Arquitetura de software

Eu estruturaria o sistema aproximadamente assim:

```text
                     COMPANION COMPUTER

 ┌─────────────────────────────────────────────────────┐
 │ Mission Manager                                     │
 │                                                     │
 │ estados:                                            │
 │ IDLE                                                │
 │ PREFLIGHT                                           │
 │ TAKEOFF                                             │
 │ APPROACH                                            │
 │ INSPECTION                                          │
 │ RETREAT                                             │
 │ LAND                                                │
 │ ABORT                                               │
 └───────────┬─────────────────────────────────────────┘
             │
      ┌──────┼────────┐
      │      │        │
      ▼      ▼        ▼
   VIO   Camera    Obstacle
         Manager   Detection
      │      │        │
      └──────┼────────┘
             │
        trajectory
             │
             ▼
        MAVLink
             │
             ▼

                  PIXHAWK / PX4

       ┌────────────────────────┐
       │ State Estimator (EKF)  │
       │ Position Controller    │
       │ Attitude Controller    │
       │ Failsafe Manager       │
       │ Motor Mixer            │
       └────────────┬───────────┘
                    │
               PWM / ESC
```

---

# 16. Navegação próxima à aeronave

A estratégia deve ser híbrida.

## Região distante

```text
GPS + IMU + barômetro
```

podem fornecer navegação convencional.

## Região de inspeção

Passaria para:

```text
IMU
 +
VIO
 +
rangefinder
 +
barômetro
 +
eventualmente GPS degradado
```

O computador enviaria ao Pixhawk uma estimativa de posição externa ou *setpoints* de velocidade/posição.

O PX4 mantém os loops de:

- taxa angular;
- atitude;
- posição.

### Recomendação importante

O computador embarcado **não deve controlar diretamente os ESCs**.

Sempre:

```text
companion
    ↓
position / velocity setpoint
    ↓
Pixhawk
    ↓
control loops
    ↓
ESCs
```

Dessa forma, a perda do computador de missão não elimina a estabilização básica.

---

# 17. VIO

**Visual-Inertial Odometry** seria uma alternativa ao GPS próximo à aeronave.

Ela combina:

```text
imagem + IMU → movimento relativo
```

Entretanto há uma limitação importante.

Uma fuselagem pode apresentar:

- grandes áreas uniformes;
- pintura branca;
- superfícies curvas;
- reflexos;
- padrões repetitivos.

Tudo isso pode degradar algoritmos visuais.

Portanto, VIO também não deve ser considerado infalível.

### Recomendação

Acrescentar uma câmera de navegação observando, por exemplo:

- parcialmente a aeronave;
- parcialmente o solo;
- ou utilizar sensores visuais adicionais.

---

# 18. Estratégia de trajetória de inspeção

Para uma primeira versão, eu não começaria tentando gerar trajetória arbitrária automaticamente.

Utilizaria **rotas previamente definidas para um modelo de aeronave conhecido**.

Exemplo:

```text
                    cauda
                      ▲

       percurso ─────────────────►

      ┌─────────────────────────────┐
      │                             │
      │          fuselagem          │
      │                             │
      └─────────────────────────────┘

       ◄────────────────────────────
              segundo passe
```

Cada trajetória teria *inspection segments*:

```text
SEG01 — nariz esquerdo
SEG02 — fuselagem dianteira
SEG03 — asa esquerda
SEG04 — fuselagem central
...
```

Isso facilita a associação das imagens à aeronave.

---

# 19. Associação das imagens ao trecho da aeronave

Cada captura deve gerar metadados.

Por exemplo:

```text
Mission ID:      M2026-0042
Aircraft ID:     Aircraft_01
Segment:         Left_Wing_03
Timestamp:       14:32:15.438
Vehicle pose:    x,y,z
Vehicle attitude roll,pitch,yaw
Camera attitude: ...
TF-Luna:         1.94 m
Image ID:        IMG_00428
```

Assim:

```text
imagem
 +
posição
 +
orientação
 +
distância
 +
segmento
```

formam um pacote de inspeção rastreável.

O Pixhawk 6X também possui entrada de *camera capture*, útil para registrar eventos de disparo e sincronizar a câmera com os logs de voo. :chatgpt-content-reference{index="14"}

---

# 20. Comunicação

Eu separaria dois enlaces.

### Link de comando/telemetria

```text
Ground Station
      ⇅
 telemetry radio
      ⇅
 Pixhawk
```

MAVLink pode transportar:

- posição;
- atitude;
- bateria;
- estado da missão;
- alarmes;
- qualidade GPS;
- integridade dos sensores.

### Link de dados

Para vídeo/imagens:

```text
Companion
    ⇅
Wi-Fi / rádio de dados
    ⇅
Ground Station
```

As imagens de alta resolução **não precisam necessariamente ser transmitidas em tempo real**.

Uma abordagem melhor para V1 é:

```text
preview comprimido → estação
imagem original → armazenamento local
```

Isso reduz a dependência da largura de banda.

---

# 21. Armazenamento

### Pixhawk

microSD:

- logs de voo;
- estados dos sensores;
- eventos;
- parâmetros.

### Companion computer

SSD/eMMC/microSD:

```text
/mission_0042/
 ├── metadata.json
 ├── flight/
 │    └── telemetry.csv
 ├── SEG01/
 │    ├── IMG_001.jpg
 │    ├── IMG_002.jpg
 │    └── ...
 ├── SEG02/
 └── ...
```

Isso simplifica processamento posterior.

---

# 22. Atuação

Os quatro motores brushless são comandados por quatro ESCs.

O manual do F450 especifica para os ESCs originais:

- até 20 A contínuos;
- 30 A de pico por 3 s;
- entrada PWM;
- bateria 3S–4S. :chatgpt-content-reference{index="15"}

Fluxo:

```text
PX4
 ↓
PWM
 ↓
ESC
 ↓
motor
 ↓
hélice
 ↓
empuxo
```

### Validação necessária

O conjunto motor/hélice/ESC deve ser ensaiado na massa final.

Não basta verificar que o drone "consegue decolar". Deve existir margem para:

- controle de atitude;
- rajada de vento;
- manobras de afastamento;
- degradação da bateria.

---

# 23. Energia

Arquitetura:

```text
                     LiPo 3S/4S
                         │
             ┌───────────┴──────────┐
             │                      │
         Power board               Power Module
             │                      │
        ┌────┼────┐                 ▼
       ESC  ESC  ESC...          Pixhawk
                                   │
                              5V DC/DC
                                   │
                   ┌───────────────┼─────────────┐
                   ▼               ▼             ▼
               companion         câmera       sensores
```

Eu evitaria alimentar o computador de missão diretamente por uma saída periférica do Pixhawk.

Utilizaria um **DC/DC dedicado** dimensionado para seus picos de corrente.

---

# 24. Estimativa conceitual de autonomia

Suponha, apenas para dimensionamento preliminar, uma LiPo 4S 5000 mAh:

\[
E = 14,8 \times 5
\]

\[
E\approx74Wh
\]

Se apenas cerca de 80% forem utilizados:

\[
E_{util}\approx59Wh
\]

Se o sistema completo consumir em média algo como 300–400 W em inspeção:

\[
t=\frac{59}{300}\times60 \approx11,8min
\]

até

\[
t=\frac{59}{400}\times60\approx8,9min
\]

Portanto eu usaria inicialmente **aproximadamente 8–12 minutos de voo útil como ordem de grandeza**, antes de acrescentar uma reserva operacional adicional.

Isso **não é uma especificação do F450**. É uma estimativa de engenharia.

O valor real precisa ser obtido medindo:

- massa final;
- corrente em *hover*;
- corrente durante manobras;
- capacidade efetiva da bateria;
- temperatura;
- vento.

A missão deve ser projetada com reserva, portanto o tempo disponível para inspeção será menor que o tempo máximo de voo.

---

# 25. *Failsafes*

Eu não utilizaria um único comportamento de emergência para todas as falhas.

## Bateria baixa

```text
LOW BATTERY
     ↓
interromper captura
     ↓
afastar da aeronave
     ↓
retornar à área segura
     ↓
pousar
```

Em bateria crítica:

```text
CRITICAL BATTERY
      ↓
pouso seguro prioritário
```

Os limiares devem incluir energia suficiente para realizar a retirada da área.

---

# 26. Perda de comunicação

Um simples:

> comunicação perdida → Return-to-Home

pode ser perigoso.

Se o GPS estiver degradado e o drone estiver ao lado de uma asa, o RTH pode gerar uma trajetória incompatível com a geometria da aeronave.

A estratégia deveria ser dependente do estado:

```text
COMUNICAÇÃO PERDIDA
        │
        ├── localização relativa válida
        │          ↓
        │   afastamento controlado
        │          ↓
        │      área segura
        │
        └── localização degradada
                   ↓
          procedimento de emergência
```

Um **corredor de retirada** deve fazer parte do planejamento da missão.

---

# 27. Falha do TF-Luna

Condições como:

- ausência de leitura;
- distância impossível;
- salto abrupto;
- baixa qualidade do sinal;
- inconsistência com outro sensor;

devem resultar em:

```text
TF-Luna inválido
       ↓
não aproximar mais
       ↓
reduzir velocidade
       ↓
afastar / interromper missão
```

O manual informa que a intensidade do sinal deve ser utilizada para identificar medições não confiáveis. :chatgpt-content-reference{index="16"}

---

# 28. Falha de VIO

```text
VIO quality ↓
      ↓
posição incerta
      ↓
não continuar trajetória
      ↓
HOLD / RETREAT
```

Novamente, o comportamento exato depende de quais sensores ainda estão válidos.

---

# 29. Falha do companion computer

Esse caso é particularmente importante.

```text
Companion crash
      ↓
heartbeat desaparece
      ↓
Pixhawk detecta timeout
      ↓
ABORT MODE
      ↓
retirada / pouso seguro
```

O Pixhawk permanece responsável por manter o voo estável.

---

# 30. Aborto pelo operador

O operador deve possuir um comando de **ABORT independente do software da missão**.

Idealmente:

```text
botão físico / RC
        │
        ▼
    receptor RC
        │
        ▼
     Pixhawk
```

e não:

```text
botão
 ↓
companion
 ↓
Pixhawk
```

Assim, mesmo que o computador de missão trave, o operador ainda pode assumir o controle.

Eu preveria pelo menos:

```text
ABORT 1 → parar missão e afastar-se
ABORT 2 → controle manual
EMERGENCY → pouso de emergência
```

O comportamento concreto teria que ser definido após análise de risco.

---

# 31. Redundância

Podemos separar redundância em níveis.

### Interna à controladora

Pixhawk 6X:

- 3 IMUs;
- 2 barômetros;
- domínios independentes de sensores. :chatgpt-content-reference{index="17"}

### Navegação

```text
GPS
 + VIO
 + IMU
 + barômetro
 + ranging
```

Nenhum sensor isolado deve determinar toda a navegação próxima à aeronave.

### Comunicação

Idealmente:

```text
telemetria/autonomia
       +
RC independente
```

### Energia

O Pixhawk 6X permite múltiplas entradas de alimentação, recurso que pode ser aproveitado em versões futuras para aumentar tolerância a falhas. :chatgpt-content-reference{index="18"}

---

# 32. Fluxo completo da missão

A máquina de estados poderia ser:

```text
                       ┌────────┐
                       │  IDLE  │
                       └───┬────┘
                           │ operador START
                           ▼
                    ┌──────────────┐
                    │  PREFLIGHT   │
                    └──────┬───────┘
                           │ checks OK
                           ▼
                     ┌───────────┐
                     │ TAKE-OFF  │
                     └─────┬─────┘
                           ▼
                     ┌───────────┐
                     │ APPROACH  │
                     └─────┬─────┘
                           ▼
                     ┌────────────┐
                     │ INSPECTION │
                     └─────┬──────┘
                           ▼
                      ┌─────────┐
                      │ RETREAT │
                      └────┬────┘
                           ▼
                       ┌──────┐
                       │ LAND │
                       └──────┘
```

De qualquer estado:

```text
          ABORT
            ↓
     ┌─────────────┐
     │ SAFE RETREAT│
     └──────┬──────┘
            ↓
          LAND
```

---

# 33. Interfaces principais

Uma possibilidade concreta seria:

| Origem | Destino | Interface | Informação |
|---|---|---|---|
| GPS | Pixhawk | UART/I²C | posição global |
| IMUs | Pixhawk | interna | aceleração/giroscópio |
| barômetros | Pixhawk | interna | altitude relativa |
| TF-Luna | Pixhawk ou companion | UART | distância |
| câmera navegação | companion | CSI/USB | imagens |
| câmera inspeção | companion | CSI/USB/GigE | imagens |
| Pixhawk | companion | Ethernet/MAVLink | estado + setpoints |
| companion | Pixhawk | Ethernet/MAVLink | posição externa/comandos |
| Pixhawk | ESCs | PWM | atuação |
| Pixhawk | GCS | rádio/MAVLink | telemetria |
| companion | GCS | rádio/Wi-Fi | imagens/diagnóstico |

---

# 34. Arquitetura funcional consolidada

O fluxo principal fica:

```text
                         SENSORIAMENTO
                              │
          ┌───────────────────┼───────────────────┐
          │                   │                   │
     IMU/barômetro           GPS              câmeras
          │                   │                   │
          └───────────┐       │       ┌───────────┘
                      ▼       ▼       ▼
                    ESTIMAÇÃO DE ESTADO
                           │
             ┌─────────────┴─────────────┐
             │                           │
          Pixhawk                    Companion
        EKF / attitude           VIO / percepção
             │                           │
             └───────────┬───────────────┘
                         ▼
                 PLANEJAMENTO / CONTROLE
                         │
           TF-Luna ──────┤
                         │
                         ▼
                       PX4
                         │
                         ▼
                       ESCs
                         │
                         ▼
                      motores
                         │
                         ▼
                     movimento
```

Paralelamente:

```text
               CAMERA DE INSPEÇÃO
                        │
                        ▼
                  Companion
                        │
             ┌──────────┴──────────┐
             ▼                     ▼
      armazenamento          transmissão
             │                     │
             └──────► estação ◄────┘
```

---

# 35. Decisões, hipóteses e validações

Para manter bem clara a diferença que você pediu:

### **Decisões de projeto**

- quadricóptero F450 para o protótipo;
- Pixhawk 6X como controladora;
- PX4 como autopiloto;
- companion computer;
- câmera RGB dedicada;
- TF-Luna frontal;
- GPS para navegação global, não para posicionamento preciso junto à aeronave;
- navegação relativa baseada em visão + inercial;
- ativação manual assistida na primeira versão;
- operador capaz de abortar diretamente via Pixhawk/RC.

### **Hipóteses adotadas**

- aeronave completamente estacionada;
- área isolada;
- missão ocorre sem veículos/pessoas cruzando o espaço;
- condições meteorológicas compatíveis;
- modelo da aeronave é conhecido;
- trajetória de inspeção pode ser previamente planejada;
- objetivo inicial é coleta de imagens, não diagnóstico automático dos danos.

### **Recomendações da IA**

- distância nominal ≈ **2 m**, inicialmente limitada aproximadamente a **1,5–2,5 m**;
- usar câmera de inspeção de 12–20 MP;
- utilizar computador embarcado leve;
- não utilizar TF-Luna como único sensor anticolisão;
- evitar *gimbal* pesado na primeira implementação;
- utilizar trajetória previamente definida por modelo de aeronave;
- manter *flight-control loop* exclusivamente no Pixhawk;
- criar um canal de intervenção humana independente do companion.

### **Validação adicional obrigatória**

- distância mínima segura;
- efeitos de rotor wash;
- risco de FOD;
- erro de TF-Luna em diferentes pinturas;
- efeito de superfícies curvas/metálicas;
- desempenho do VIO;
- degradação de GPS;
- qualidade mínima da imagem;
- velocidade máxima durante captura;
- vento máximo permitido;
- autonomia real;
- massa máxima;
- empuxo/margem de controle;
- EMI;
- confiabilidade da comunicação;
- procedimento de falha;
- legislação e certificação aeronáutica/aeroportuária.

---

# 36. Principal conclusão de engenharia

A arquitetura que eu consideraria **baseline para o projeto conceitual** é:

```text
DJI F450
│
├── 4 motores + 4 ESCs
│
├── Pixhawk 6X / PX4
│   ├── 3× IMU
│   ├── 2× barômetro
│   ├── GPS
│   ├── failsafes
│   └── flight control
│
├── TF-Luna frontal
│   └── distância à superfície
│
├── câmera de navegação
│   └── VIO / percepção
│
├── câmera RGB 12–20 MP
│   └── inspeção
│
├── companion computer
│   ├── mission manager
│   ├── VIO
│   ├── planejamento
│   ├── aquisição das imagens
│   └── armazenamento
│
├── rádio de telemetria
│
├── receptor RC independente
│   └── takeover / ABORT
│
└── LiPo + distribuição + DC/DC
```

O aspecto mais importante da proposta é que **GPS, TF-Luna e câmera individualmente não são suficientes** para garantir o voo próximo à aeronave. O sistema precisa combinar sensores, limitar a aproximação, detectar degradações e possuir uma forma independente de intervenção humana.

E há uma limitação que merece destaque desde já: **o F450 pode acabar sendo o gargalo do projeto**. Seu limite de massa indicado é 1600 g, enquanto câmera, computador, sensores, Pixhawk e bateria rapidamente consomem a margem disponível. :chatgpt-content-reference{index="19"} Portanto, eu manteria o F450 como **decisão provisória para prototipagem**, e colocaria a confirmação da plataforma como um dos primeiros itens de validação física.