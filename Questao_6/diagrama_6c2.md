``` mermaid
flowchart TB

    subgraph SENSOR["Sensor LiDAR"]
        LIDAR["Benewake TF-Luna"]
    end

    subgraph ELETRICA["Interface elétrica"]
        PWR["Alimentação<br/>3,7–5,2 V"]
        UART_PHY["Sinais UART<br/>LVTTL 3,3 V"]
    end

    subgraph FC_HW["Computador de voo — Hardware"]
        UART_PERIPH["Periférico UART<br/>RX / TX"]
    end

    subgraph FC_SW["Computador de voo — Software"]
        UART_DRV["Driver UART"]
        TFL_DRV["Driver / Parser TF-Luna"]
        API["Interface lógica / API<br/>distância, intensidade, status"]
        APP["Aplicação<br/>controle / navegação"]
    end

    PWR -->|"VCC + GND"| LIDAR

    LIDAR -->|"TXD do TF-Luna → RX da controladora"| UART_PHY
    UART_PHY --> UART_PERIPH

    UART_PERIPH -->|"bytes recebidos"| UART_DRV
    UART_DRV -->|"stream de dados"| TFL_DRV
    TFL_DRV -->|"medida interpretada"| API
    API -->|"distância disponível"| APP

    APP -.->|"configuração / requisição"| API
    API -.->|"comandos do sensor"| TFL_DRV
    TFL_DRV -.->|"bytes de comando"| UART_DRV
    UART_DRV -.->|"TX da controladora"| UART_PERIPH
    UART_PERIPH -.->|"TX controladora → RXD do TF-Luna"| UART_PHY
    UART_PHY -.-> LIDAR
```