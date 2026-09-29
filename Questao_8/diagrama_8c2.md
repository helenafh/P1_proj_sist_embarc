``` mermaid
flowchart LR

    SENS["Sensoriamento<br/>estado estimado"]
    COMM["Comunicação<br/>piloto / estação de solo"]

    subgraph CTRL["Macrobloco: Processamento e Controle"]
        INPUT["Aquisição das entradas"]
        EST["Estimativa / validação<br/>do estado"]
        NAV["Navegação e<br/>gerenciamento da missão"]
        CONTROL["Controle de voo<br/>atitude / altitude / posição"]
        MIX["Alocação dos comandos<br/>para os atuadores"]
        SAFETY["Supervisão /<br/>tratamento de falhas"]
    end

    ACT["Atuação<br/>ESCs + motores"]
    TELE["Comunicação / telemetria"]

    SENS -->|"posição, atitude,<br/>velocidades, distância"| INPUT
    COMM -->|"referências / missão"| INPUT

    INPUT --> EST
    EST -->|"estado validado"| NAV
    EST -->|"realimentação"| CONTROL

    NAV -->|"referências de voo"| CONTROL
    CONTROL -->|"torques / empuxo desejados"| MIX
    MIX -->|"comandos aos motores"| ACT

    EST --> SAFETY
    NAV --> SAFETY
    SAFETY -.->|"failsafe / override"| CONTROL

    EST -->|"estado do veículo"| TELE
    NAV -->|"estado da missão"| TELE
```