## Questão 3 — Modelagem Discreta

Um controlador embarcado observa o mundo físico em instantes discretos e atualiza suas saídas periodicamente.

### a) (0,20 ponto)
Explique a diferença entre um modelo de tempo contínuo e um modelo de tempo discreto. Dê dois exemplos de grandezas contínuas e dois exemplos de variáveis discretas em um multirotor.

**Resposta:**

Um modelo de tempo contínuo representa grandezas que podem variar a qualquer instante de tempo, sendo normalmente descritas por funções contínuas x(t) e, em muitos casos, por equações diferenciais. Já um modelo de tempo discreto representa o sistema apenas em instantes específicos de amostragem, utilizando sequências como x[k], em que k indica o instante discreto considerado.

Em um multirotor, exemplos de grandezas contínuas são: a posição do veículo no espaço e a velocidade angular do corpo.

Exemplos de variáveis discretas são: a leitura de um sensor armazenada a cada período de amostragem e o comando digital enviado periodicamente pelo controlador aos atuadores.

Assim, embora o comportamento físico do multirotor evolua continuamente no tempo, o sistema embarcado observa e atualiza esse comportamento em instantes discretos.




### b) (0,25 ponto)
Explique o significado do período de amostragem $T_s$ e da frequência de amostragem $f_s = 1/T_s$. Discuta como sua escolha afeta desempenho, estabilidade, carga computacional e consumo de energia.

**Resposta:**

O período de amostragem $T_s$ representa o intervalo de tempo entre duas amostras consecutivas de um sinal ou duas atualizações sucessivas do algoritmo embarcado. A frequência de amostragem é dada por $f_s = \frac 1 {T_s}$ e indica quantas vezes por segundo o sistema realiza essa amostragem.

A escolha do período de amostragem influencia diretamente o comportamento do sistema. Um período menor — ou, equivalentemente, uma frequência de amostragem maior — permite acompanhar mais rapidamente as variações do processo físico e tende a melhorar a resposta do controlador. Em contrapartida, exige mais execuções do algoritmo por unidade de tempo, aumentando a carga computacional e, em geral, o consumo de energia.

Por outro lado, um período de amostragem muito grande reduz a carga de processamento e pode diminuir o consumo energético, mas faz com que o controlador observe o sistema com menor frequência. Isso pode degradar o desempenho, aumentar atrasos na resposta e, em sistemas de controle, até comprometer a estabilidade.

Portanto, $T_s$ deve ser escolhido de forma a equilibrar desempenho e estabilidade com as limitações de processamento e energia do sistema embarcado.




### c) (0,30 ponto)
Considere $dx(t)/dt = u(t)$. Utilizando a aproximação de Euler para frente, obtenha a equação de diferenças que permite calcular $x[k+1]$ a partir de $x[k]$, $u[k]$ e $T_s$. Explique o significado físico de cada termo.

**Resposta:**

Aplicando a aproximação de Euler para frente à equação

$\frac {dx(t)}{dt} = u(t)$

temos:

$\frac {x[k+1] - x[k]}{T_s} \approx u[k]$

Portanto,

$x[k+1] = x[k] + T_s u[k]$

Nessa equação:

- $x[k]$ representa o valor atual da variável de estado no instante discreto $k$;

- $u[k]$ representa a taxa de variação aplicada durante o intervalo de amostragem;

- $T_s$ é o período de amostragem;

- $T_s u[k]$ representa a variação aproximada de x durante esse intervalo;

- $x[k+1]$ é o valor estimado da variável no próximo instante de amostragem.

Fisicamente, a equação indica que o próximo valor de $x$ é obtido somando ao valor atual a variação acumulada ao longo de um intervalo $T_s$, assumindo $u(t)$ aproximadamente constante nesse intervalo.




### d) (0,25 ponto)
Represente em um diagrama a cadeia:

**grandeza física → sensor → condicionamento/conversão → amostragem → algoritmo embarcado → saída → atuador → sistema físico**

identificando os domínios contínuo e discreto.

**Resposta:**

Em seguida, está um diagrama com a cadeia solicitada. Os domínios foram demarcados por blocos, os blocos azuis indicam as partes de domínio contínuo enquanto o bloco cinza representa o domínio discreto.

[Diagrama dos domínios contínuo e discreto](diagrama_3d.md)
