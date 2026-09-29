``` mermaid
flowchart LR

    subgraph SENS["Macrobloco: Sensoriamento"]
        IMU["IMUs integradas"]
        BARO["Barômetros integrados"]
        GPS["GPS externo"]
        LIDAR["TF-Luna"]
    end

    subgraph CTRL["Macrobloco: Processamento e Controle"]
        PIX["Holybro Pixhawk 6X"]

        EST["Estimativa e validação de estado"]
        NAV["Navegação e gerenciamento da missão"]
        FC["Controle de voo<br/>atitude / altitude / posição"]
        MIX["Alocação de comandos<br/>para atuadores"]
        SAFE["Supervisão / Failsafe"]

        PIX --> EST
        EST --> NAV
        EST --> FC
        NAV --> FC
        FC --> MIX
        SAFE -.-> FC
    end

    subgraph ACT["Macrobloco: Atuação"]
        ESC1["ESC 1"]
        ESC2["ESC 2"]
        ESC3["ESC 3"]
        ESC4["ESC 4"]

        M1["Motor 1 + hélice"]
        M2["Motor 2 + hélice"]
        M3["Motor 3 + hélice"]
        M4["Motor 4 + hélice"]
    end

    IMU -->|"aceleração / velocidade angular"| PIX
    BARO -->|"pressão / altitude"| PIX
    GPS -->|"posição / velocidade"| PIX
    LIDAR -->|"distância"| PIX

    MIX -->|"comandos de atuação"| ESC1
    MIX -->|"comandos de atuação"| ESC2
    MIX -->|"comandos de atuação"| ESC3
    MIX -->|"comandos de atuação"| ESC4

    ESC1 --> M1
    ESC2 --> M2
    ESC3 --> M3
    ESC4 --> M4
```