## Questão 10 — Projeto de sistemas embarcados orientado por Inteligência Artificial

Utilize uma ferramenta de IA generativa para auxiliar no projeto conceitual de um drone destinado à inspeção externa da integridade física de aeronaves em aeroportos. A IA deve ser tratada como ferramenta de engenharia, e não como autoridade final de projeto.

### a) (0,20 ponto)
Formule o problema de engenharia, requisitos e restrições fornecidos à ferramenta de IA. Anexe os principais prompts e respostas utilizados.

**Resposta:**

Após definição inicial de requisitos e ideias iniciais, utilizei uma IA para me auxiliar com o desenvolvimento de um prompt para começar uma nova conversa, a fim de explorar a atuação de uma nova IA para a atividade desejada.

[Prompt inicial com definição do projeto](prompt.md) (desenvolvido em arquivo .md), enviados junto com os manuais, datasheets e documentos de especificação técnica, listadas nas referências bibliográficas

Recortes da [resposta gerada pelo ChatGPT](resposta_ia.md) (Conversa iniciada apenas com o prompt e com os arquivos mencionados anteriormente) sobre definição do projeto (demais informações relevantes serão anexadas nos próximo itens):

- [3 - Estratégia de ativação da missão](resposta_ia.md#3-estratégia-de-ativação-da-missão)
- [10 - Distância operacional proposta](resposta_ia.md#10-distância-operacional-proposta)
- [35 - Decisões, hipóteses e validações](resposta_ia.md#35-decisões-hipóteses-e-validações)



### b) (0,25 ponto)
Apresente a arquitetura proposta pela IA, incluindo tipo de aeronave, sensores, computador/controladora de voo, software, comunicação, energia/autonomia, atuação e estratégias de redundância.

**Resposta:**

Recortes da resposta do ChatGPT - após envio do prompt com detalhes iniciais do projeto - com detalhes, sugestões e ponderações técnicas: 

- [1 - Visão geral da arquitetura proposta](resposta_ia.md#1-visão-geral-da-arquitetura-proposta)
- [4 - Controladora de voo](resposta_ia.md#4-controladora-de-voo)
- [11 - Sistema de captura de imagens](resposta_ia.md#11-sistema-de-captura-de-imagens)
- [14 - Computador embarcado](resposta_ia.md#14-computador-embarcado)
- [15 - Arquitetura de software](resposta_ia.md#15-arquitetura-de-software)
- [20 - Comunicação](resposta_ia.md#20-comunicação)
- [23 - Energia](resposta_ia.md#23-energia)
- [31 - Redundância](resposta_ia.md#31-redundância)
- [33 - Interfaces principais](resposta_ia.md#33-interfaces-principais)
- [34 - Arquitetura funcional consolidada](resposta_ia.md#34-arquitetura-funcional-consolidada)




### c) (0,30 ponto)
Realize uma verificação crítica da proposta: identifique pelo menos dois aspectos tecnicamente adequados e dois aspectos que necessitam correção ou validação adicional. Para cada análise, relacione conceitos estudados na disciplina e fontes técnicas independentes.

**Resposta:**

Aspectos tecnicamente adequados:

- Máquina de Estados: A máquina de estados sugerida pela IA está de acordo com o comportamento descrito na especificação inicial e serviria como uma ótima base para o desenvolvimento do projeto.
- Arquitetura geral de Hardware: A arquitetura geral de hardware proposta está condizente com o que foi discutido até agora e com informações técnicas precisas (obtidas a partir das referências anexadas e referenciando-as de forma a simplificar a conferência).

Aspectos que necessitam de correção ou validação:

- Distância Operacional Proposta: Apesar de a resposta da IA apresentar fundamento, antes da implementação é essencial a verificação de dados e informações, utilizados para gerar essa sugestão, e realizar testes para avaliar a eficiência do sistema com essa especificação.

- Objetivo inicial do projeto: A IA definiu como objetivo inicial do projeto a captura de imagens, sem a etapa de diagnóstico inicialmente. A captura de imagens poderia ser feita com certa facilidade com um drone de propósito geral. Seria importante definir com as pessoas envolvidas no projeto o que exatamente “inspeção” se refere, para ter um objetivo claro e alinhado. Se o diagnóstico por meio de visão computacional fosse considerado essencial, por exemplo, a hipótese adotada pela IA não estaria de acordo com o objetivo principal do projeto. Como consequência, as sugestões sobre metadados e maneiras de armazenar as imagens capturadas precisariam ser revisadas com o objetivo correto em mente.




### d) (0,25 ponto)
Explique quais evidências, testes, simulações, revisões humanas e etapas de V&V seriam necessárias antes que um engenheiro pudesse assumir responsabilidade técnica pelo projeto. Discuta também como deve ser documentado o uso da IA para garantir rastreabilidade das decisões de engenharia.

**Resposta:**

Antes de qualquer teste ou simulação, é importante que o engenheiro leia o projeto com atenção e verifique o alinhamento do que foi gerado com os requisitos e especificações do projeto. Por mais que o projeto esteja detalhado no prompt, é comum que Inteligências Artificiais sofram com alucinações que desviam o foco da tarefa ou geram informações sem fundamento. Além disso, é importante que o engenheiro verifique a lógica e o que foi definido pela IA. Assim como as alucinações podem desviar a IA do objetivo, elas também podem alimentar o contexto com informações falsas e/ou infundadas que causam brechas no plano desenvolvido.

Após verificar que o projeto está de acordo com o esperado, baseado em informações verdadeiras e não possui brechas lógicas, o engenheiro deve seguir os princípios de MBD e partir para simulações (MIL), especialmente funcionais, a fim de verificar que o projeto funciona como esperado. Após um sucesso nas simulações, o engenheiro deve partir para testes (SIL e HIL) a fim de garantir que o funcionamento também ocorra no mundo real.

Durante esse processo é provável que apareçam necessidades de ajustes e reformulações, as quais são responsabilidades do engenheiro garantir que sejam sanadas corretamente.

Após aprovação por parte do engenheiro em todas essas etapas, é importante que seja documentado o uso da IA como método de desenvolvimento. Ao documentar e registrar o projeto, o engenheiro deve deixar claro em quais partes a IA foi utilizada e como.


