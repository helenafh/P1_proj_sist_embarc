``` mermaid
flowchart LR

    PWR["Fonte de alimentação<br/>3,7 V a 5,2 V"]
    LIDAR["Benewake TF-Luna<br/>LiDAR ToF"]
    FC["Computador de voo / MCU"]
    DRV["Driver UART"]
    SW["Camada de software<br/>leitura e interpretação dos dados"]
    APP["Aplicação<br/>controle / navegação / detecção de obstáculos"]

    PWR -->|VCC + GND| LIDAR

    LIDAR -->|TXD - dados de distância| FC
    FC -->|RXD - comandos/configuração| LIDAR

    FC --> DRV
    DRV --> SW
    SW --> APP
```