``` mermaid
stateDiagram-v2
    [*] --> Carregamento

    Carregamento --> Decolagem: bateria == 100%

    Decolagem --> Navegacao_A: decolagem concluída

    Navegacao_A --> Permanencia_A: posição == A
    Permanencia_A --> Navegacao_B: tempo_em_A >= 3 min

    Navegacao_B --> Permanencia_B: posição == B
    Permanencia_B --> Navegacao_C: tempo_em_B >= 3 min

    Navegacao_C --> Permanencia_C: posição == C
    Permanencia_C --> Retorno_Base: tempo_em_C >= 3 min

    Retorno_Base --> Pouso_Encerramento: base alcançada
    Pouso_Encerramento --> [*]: pouso concluído

    Decolagem --> Retorno_Base: bateria <= 20%
    Navegacao_A --> Retorno_Base: bateria <= 20%
    Permanencia_A --> Retorno_Base: bateria <= 20%
    Navegacao_B --> Retorno_Base: bateria <= 20%
    Permanencia_B --> Retorno_Base: bateria <= 20%
    Navegacao_C --> Retorno_Base: bateria <= 20%
    Permanencia_C --> Retorno_Base: bateria <= 20%
```