``` mermaid
flowchart LR

    ENERGY["Energia<br/>bateria + distribuição"]
    SENS["Sensoriamento<br/>IMU, barômetro, GPS, LiDAR"]
    CTRL["Processamento e Controle<br/>computador de voo"]
    COMM["Comunicação<br/>rádio controle / telemetria"]
    ACT["Atuação<br/>ESCs + motores + hélices"]
    STRUCT["Estrutura<br/>frame F450"]
    ENV["Sistema físico / ambiente"]

    ENERGY -->|"alimentação"| SENS
    ENERGY -->|"alimentação"| CTRL
    ENERGY -->|"alimentação"| COMM
    ENERGY -->|"potência"| ACT

    SENS -->|"medições"| CTRL
    COMM -->|"comandos / missão"| CTRL
    CTRL -->|"telemetria / estado"| COMM
    CTRL -->|"comandos de atuação"| ACT

    ACT -->|"forças e torques"| ENV
    ENV -->|"movimento / perturbações"| SENS

    STRUCT --- SENS
    STRUCT --- CTRL
    STRUCT --- COMM
    STRUCT --- ACT
```