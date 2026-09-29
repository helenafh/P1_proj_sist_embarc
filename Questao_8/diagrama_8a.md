``` mermaid
flowchart LR

    USER["Usuário / Piloto"]
    RC["Rádio controle"]
    GCS["Estação de solo"]
    SENS["Sensores externos<br/>GPS / LiDAR / outros"]
    ENERGY["Fonte de energia<br/>Bateria LiPo"]
    ENV["Ambiente / Sistema físico"]

    DRONE["DRONE DJI F450<br/><br/>Sistema Embarcado<br/>(Caixa-preta)"]

    IND["Indicadores ao usuário<br/>estado / alarmes / telemetria"]

    USER -->|"Comandos do piloto"| RC
    RC -->|"Comandos de voo"| DRONE

    GCS -->|"Missão / configuração / comandos"| DRONE
    DRONE -->|"Telemetria / estado"| GCS

    SENS -->|"Medições do ambiente"| DRONE

    ENERGY -->|"Energia elétrica"| DRONE

    ENV -->|"Posição, movimento,<br/>distância, perturbações"| DRONE
    DRONE -->|"Empuxo / movimento"| ENV

    DRONE -->|"Estado do sistema"| IND
    IND -->|"Informação visual / sonora"| USER
```