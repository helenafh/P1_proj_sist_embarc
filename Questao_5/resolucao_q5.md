## Questão 5 — Máquinas de Estado

Modele a missão abaixo por meio de uma máquina de estados finita de tempo discreto.

Um multirotor permanece em sua base de carregamento até atingir 100% de bateria. Em seguida, decola e visita sequencialmente três coordenadas GPS:

- $A=(X_a,Y_a,Z_a)$
- $B=(X_b,Y_b,Z_b)$
- $C=(X_c,Y_c,Z_c)$

permanecendo 3 minutos em cada uma. Após C, retorna à base e encerra a missão. Se, em qualquer momento de voo, a carga atingir um limiar crítico definido em 20%, deve abortar a missão e retornar imediatamente à base.

### a) (0,25 ponto)
Identifique os estados necessários e descreva a função de cada um.

**Resposta:**

Com base na missão definida pelo enunciado, os estados necessários para sua realização são os seguintes:

1. Carregamento: estado inicial, no qual o multirotor permanece na base aguardando a bateria atingir 100%.

2. Decolagem: responsável por iniciar o voo e levar o multirotor da base até a condição adequada para navegação.

3. Navegação para A: o veículo desloca-se da base até a coordenada $A=(X_A, Y_A, Z_A)$.

4. Permanência em A: o multirotor mantém-se na coordenada A durante 3 minutos.

5. Navegação para B: deslocamento de A até $B=(X_B, Y_B, Z_B)$.

6. Permanência em B: permanência na coordenada B durante 3 minutos.

7. Navegação para C: deslocamento de B até $C=(X_C, Y_C, Z_C)$.

8. Permanência em C: permanência na coordenada C durante 3 minutos.

9. Retorno à base: responsável por conduzir o multirotor de volta à base, tanto após a conclusão normal da missão quanto em caso de interrupção por nível crítico de bateria.

10. Pouso / Encerramento: realiza o pouso na base e encerra a missão.




### b) (0,35 ponto)
Desenhe a máquina de estados indicando eventos, condições de guarda e transições.

**Resposta:**

[Diagrama da máquina de estados](diagrama_5b.md)

> Durante qualquer estado de voo, a condição bateria <= 20% possui prioridade sobre a progressão normal da missão e provoca uma transição imediata para o estado Retorno à base, abortando as etapas restantes.



### c) (0,20 ponto)
Indique quais variáveis devem ser mantidas pelo sistema para representar tempo, posição, estado da missão e condição da bateria.

**Resposta:**

Para executar corretamente a missão, o sistema deve manter pelo menos as seguintes variáveis:

- Tempo: um temporizador ou contador de tempo associado à permanência em cada coordenada, permitindo verificar quando os 3 minutos exigidos em A, B e C foram completados.

- Posição: a posição atual do multirotor, por exemplo em coordenadas (x, y, z) ou a posição fornecida pelo GPS, para verificar a chegada aos pontos A, B, C e à base.

- Estado da missão: uma variável que represente o estado atual da máquina de estados, por exemplo: uso de constantes CARREGAMENTO, NAVEGACAO_A, PERMANENCIA_A, RETORNO_BASE, etc.

- Bateria: uma variável representando o nível atual de carga da bateria, em porcentagem, utilizada tanto para detectar a condição de 100% antes da decolagem quanto o limiar crítico de 20% durante o voo.

Também pode ser útil manter variáveis auxiliares, como uma indicação de missão abortada e flags de chegada aos pontos de interesse, dependendo da implementação da máquina de estados.




### d) (0,20 ponto)
Explique como trataria pelo menos uma situação excepcional, como perda de GPS, falha de comunicação ou impossibilidade de retorno à base.

**Resposta:**

Em caso de falha de comunicação, o comportamento depende do grau de autonomia disponível no multirotor. Se o sistema possuir capacidade de navegação e controle local suficiente para continuar a missão com segurança, ele pode manter a sequência prevista mesmo sem comunicação externa.

Caso a comunicação seja necessária para a continuidade segura da missão, o sistema deve entrar em um estado de contingência, realizando tentativas de reconexão durante um intervalo de tempo pré-definido. Se a comunicação não for restabelecida dentro desse período, a missão deve ser abortada e o multirotor deve retornar à base utilizando suas funções locais de navegação.

Esse comportamento pode ser implementado adicionando um estado de Falha de Comunicação / Reconexão, com transição para o estado Retorno à Base caso o tempo máximo de tentativa seja excedido.

