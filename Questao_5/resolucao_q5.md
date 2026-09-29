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

<!-- Escreva sua resposta aqui. -->



### b) (0,35 ponto)
Desenhe a máquina de estados indicando eventos, condições de guarda e transições.

**Resposta:**

<!-- Escreva sua resposta aqui. -->



### c) (0,20 ponto)
Indique quais variáveis devem ser mantidas pelo sistema para representar tempo, posição, estado da missão e condição da bateria.

**Resposta:**

<!-- Escreva sua resposta aqui. -->



### d) (0,20 ponto)
Explique como trataria pelo menos uma situação excepcional, como perda de GPS, falha de comunicação ou impossibilidade de retorno à base.

**Resposta:**

<!-- Escreva sua resposta aqui. -->
