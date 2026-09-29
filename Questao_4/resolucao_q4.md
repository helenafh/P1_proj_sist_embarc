## Questão 4 — Modelagem da Dinâmica Física

Considere o problema de estabilização de atitude de um multirotor.

### a) (0,30 ponto)
Apresente as principais grandezas físicas envolvidas na dinâmica de atitude (por exemplo, orientação, velocidades angulares, torques e empuxos) e indique as entradas e saídas de um modelo simplificado.

**Resposta:**

Na dinâmica de atitude de um multirotor, as principais grandezas físicas são a orientação do veículo, normalmente representada pelos ângulos de roll ($\phi$), pitch ($\theta$) e yaw ($\psi$), as velocidades angulares em torno dos três eixos do corpo, os torques resultantes aplicados nesses eixos e os empuxos gerados pelos motores.

Os quatro motores produzem forças de empuxo que, de acordo com sua magnitude e distribuição, geram os torques responsáveis pelas rotações do veículo. Diferenças de empuxo entre motores permitem controlar roll e pitch, enquanto a diferença entre os torques de reação dos pares de motores que giram em sentidos opostos permite controlar yaw.

Em um modelo simplificado da dinâmica de atitude, podem ser consideradas como entradas os torques de controle aplicados em torno dos eixos de roll, pitch e yaw:

$\tau_\phi, \tau_\theta, \tau_\psi$

Esses torques são resultantes dos empuxos produzidos pelos quatro motores.
Como saídas, podem ser consideradas a orientação do veículo:

$\phi, \theta, \psi$

e, quando necessário, as respectivas velocidades angulares:

$p, q, r $

As velocidades angulares do corpo são representadas por p, q e r, associadas aos eixos de roll, pitch e yaw, respectivamente. Para pequenos ângulos de atitude, pode-se utilizar a aproximação

$p \approx \dot \phi , q \approx \dot \theta  , r \approx \dot \psi $




### b) (0,35 ponto)
Escreva e explique um modelo matemático simplificado para a dinâmica rotacional do veículo. Podem ser utilizadas equações diferenciais, diagramas ou representação em espaço de estados, desde que as hipóteses adotadas sejam explicitadas.

**Resposta:**

Para modelar a dinâmica rotacional do multirotor, pode-se tratá-lo como um corpo rígido sujeito a torques em torno dos três eixos principais. Considerando os momentos de inércia Ix, Iy e Iz, as velocidades angulares p, q e r, e os torques de controle ,  e , a dinâmica pode ser representada por:

$I_x \dot p = \tau_\phi + (I_y-I_z)qr$

$I_y \dot q = \tau_\theta + (I_z-I_x)qr$

$I_z \dot r = \tau_\psi + (I_x-I_y)qr$

onde $p$, $q$ e $r$ representam as velocidades angulares em torno dos eixos de roll, pitch e yaw, respectivamente. Os termos envolvendo produtos entre velocidades angulares representam os acoplamentos entre os movimentos de rotação do veículo.

Para uma aproximação simplificada, considerando pequenos ângulos e pequenas velocidades angulares, pode-se adotar:

$p \approx \dot \phi , q \approx \dot \theta  , r \approx \dot \psi $

Assim, os torques produzidos pela diferença de empuxo entre os motores determinam as acelerações angulares, que alteram as velocidades angulares e, consequentemente, a orientação do multirotor.

As principais hipóteses do modelo são:

- o multi-rotor é considerado um corpo rígido;

- os eixos considerados coincidem com os eixos principais de inércia;

- a matriz de inércia é aproximadamente diagonal;

- efeitos aerodinâmicos externos e perturbações são inicialmente desprezados;

- para relacionar velocidades angulares e ângulos de atitude, considera-se a aproximação de pequenos ângulos.




### c) (0,20 ponto)
Discuta pelo menos duas hipóteses simplificadoras do modelo e explique em quais situações elas podem deixar de ser válidas.

**Resposta:**

O modelo simplificado adotado anteriormente depende de algumas hipóteses que facilitam a análise, mas que podem limitar sua validade em determinadas condições.

1. Hipótese de pequenos ângulos e pequenas velocidades angulares: assume-se que o multirotor opera próximo da condição de equilíbrio, de modo que $p \approx \dot \phi , q \approx \dot \theta  , r \approx \dot \psi $, e que os acoplamentos não lineares sejam reduzidos. Essa aproximação pode deixar de ser válida durante manobras agressivas, grandes inclinações ou rotações rápidas, quando os efeitos não lineares da dinâmica se tornam relevantes.

2. Corpo rígido com matriz de inércia aproximadamente diagonal: considera-se que a estrutura do drone não sofre deformações significativas e que os eixos adotados coincidem aproximadamente com os eixos principais de inércia. Essa hipótese pode perder validade quando há distribuição assimétrica de massa, carga útil deslocada, flexibilidade estrutural ou alterações importantes na configuração do veículo.

Também podem ser desprezados efeitos aerodinâmicos externos, como vento, arrasto e turbulência. Essa simplificação é adequada para uma primeira análise, mas pode produzir erros quando o multirotor opera em condições ambientais severas ou em velocidades mais elevadas.




### d) (0,15 ponto)
Explique como o modelo físico se relaciona com o controlador implementado no computador de voo, sem repetir a discretização solicitada na Questão 3.

**Resposta:**

O modelo físico fornece ao controlador uma representação da relação entre os torques aplicados ao multirotor e a consequente variação de suas velocidades angulares e atitude. Com base nesse modelo, o controlador compara a orientação desejada com a orientação estimada pelos sensores e calcula os torques de correção necessários para reduzir esse erro, ou seja, para se aproximar dos valores desejados.

Esses torques de controle são então convertidos em comandos para os motores, por meio da distribuição adequada de empuxo entre os quatro rotores. Dessa forma, o modelo físico serve de base para o projeto e ajuste das leis de controle, permitindo prever como o veículo deve responder aos comandos e às perturbações.

Na implementação real, o computador de voo utiliza continuamente as medições dos sensores para estimar o estado do multirotor e atualizar os comandos dos atuadores, fechando a malha de realimentação entre o sistema físico e o controlador.

