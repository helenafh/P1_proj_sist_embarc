``` mermaid
flowchart LR

    ENV["Ambiente / Sistema físico"]

    subgraph SENS["Macrobloco: Sensoriamento"]
        IMU["Sensoriamento inercial<br/>aceleração e velocidade angular"]
        BARO["Sensoriamento barométrico<br/>pressão / altitude"]
        GPS["Posicionamento global<br/>posição / velocidade"]
        LIDAR["Sensoriamento de distância<br/>LiDAR"]
        COND["Aquisição e<br/>condicionamento de dados"]
        FUSION["Pré-processamento /<br/>fusão de sensores"]
    end

    CTRL["Processamento e Controle"]

    ENV -->|"movimento / orientação"| IMU
    ENV -->|"pressão atmosférica"| BARO
    ENV -->|"sinais de posicionamento"| GPS
    ENV -->|"distância a obstáculos / solo"| LIDAR

    IMU -->|"aceleração, p, q, r"| COND
    BARO -->|"pressão / altitude"| COND
    GPS -->|"posição / velocidade"| COND
    LIDAR -->|"distância / intensidade"| COND

    COND -->|"dados adquiridos"| FUSION
    FUSION -->|"estado estimado do veículo"| CTRL
```