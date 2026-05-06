<br>
<div align="center">
<b>Capítulo 4 - Escalabilidade e desempenho</b></div>
<br>

Um microsserviço escalável e de alto desempenho é orientado por eficiência, e não apenas é capaz de tratar um grande número de tarefas ou solicitações ao mesmo tempo, mas também pode tratá-las eficientemente e está preparado para o crescimento futuro do número de tarefas e solicitações.

Conforme já foi discutido, para construir uma aplicação escalável, o projeto precisa levar em consideração a simultaneidade e o particionamento: a simultaneidade permite que cada tarefa seja dividida em partes menores, enquanto o particionamento é essencial para permitir que essas partes menores sejam processadas em paralelo. Desta forma, a escalabilidade está relacionada à forma como dividimos e conquistamos o processamento de tarefas, o desempenho é a medida da eficiência da aplicação que processa essas tarefas.

Os pontos que serão abordados a fundo nesse capítulo são os seguintes: 1. escala de crescimento quantitativo e qualitativo para cada microsserviço; 2. utilização eficiente de recursos de hardware; 3. requisitos e gargalos de recursos de um microsserviço; 4. elaboração de um planejamento de capacidade; 5. formas de garantir que as dependências de um microsserviço possam crescer com ele; 6. gerenciamento de tráfego; 7. tratamento e processamento de tarefas; e 8. formas escaláveis de armazenar dados.

<hr>
<div align="center">
<b>Escala de crescimento</b></div>
<br>

O livro faz um esforço para diferenciar entre a escala de crescimento qualitativa da quantitativa. A métrica mais clássica (quantas solicitações por segundo/minuto o serviço suporta) é uma métrica quantitativa, e sem dúvidas que é muito importante, mas inútil sem um contexto adicional: onde aquele microsserviço se encaixa no produto como um todo. Como a autora sempre relembra, um microsserviço não existe sozinho: ele faz parte de um ecossistema maior.

É aí que entra a escala de crescimento qualitativo. Para um planejamento de capacidade eficaz, é preciso entender o que gera requisições no seu serviço. Por exemplo, se o gatilho é o login, logo o serviço escala com novos usuários. Contudo, se o gatilho é a compra, o serviço escala com o volume de vendas. É essa visão qualitativa, não puramente numérica, que permite que a engenharia antecipe a demanda técnica a partir de previsões estratégicas da empresa.

Já em relação à escala de quantitativo, ela consiste em traduzir métricas de negócio (qualitativas) em números técnicos mensuráveis, como requisições ou transações por segundo. Ao entender que cada ação do usuário gera um volume específico de chamadas ao sistema, os desenvolvedores podem prever com precisão custos operacionais e necessidades de hardware.

1.	Qual a escala de crescimento qualitativo do microsserviço?
2.	Qual a escala de crescimento quantitativo do microsserviço?

<hr>
<div align="center">
<b>Uso eficiente de recursos</b></div>
<br>

Esta seção trata sobre o desafio organizacional de alocação e da distribuição de recursos entre os diversos microsserviços: CPU, memória, armazenamento de dados etc…

Este desafio pode ser amenizado se fornecermos aos microsserviços que são críticos ao negócio uma parte maior dos recursos, e para isso podemos classificar os vários microsserviços de acordo com sua importância e seu valor para o negócio como um todo.
	
A autora argumenta que uma das maneiras mais eficientes de alocar e distribuir recursos de hardware por um ecossistema de microsserviços é “abstrair totalmente a noção de um servidor e substituí-la por recursos de hardware usando tecnologias de abstração de recursos como Apache Mesos”, que aloca recursos dinamicamente.
	
Para gerenciar um ecossistema de microsserviços de forma eficiente, é fundamental identificar os requisitos e os gargalos de recursos (como CPU e RAM) de cada componente. Ao quantificar quanto hardware uma instância precisa para operar e escalar, as equipes conseguem realizar uma alocação precisa e identificar limitações de desempenho, garantindo que o serviço processe tarefas com eficiência tanto em escala vertical quanto horizontal.

A forma mais eficaz e eficiente de escalar um serviço é horizontalmente: se nosso tráfego está prestes a crescer, precisamos adicionar mais alguns servidores e implantar nosso serviço neles.

1.	O microsserviço está sendo executado em hardware dedicado ou compartilhado? 
2.	Estão sendo usadas tecnologias de abstração e alocação de recursos?

<hr>
<div align="center">
<b>Gargalos de recursos</b></div>
<br>

Gargalos de recursos são limitações na arquitetura ou infraestrutura que impedem a escalabilidade de um microsserviço, como o limite de conexões com um banco de dados. O exemplo mais crítico ocorre quando um serviço só consegue crescer verticalmente (aumentando CPU e RAM de uma única instância), o que viola os princípios de simultaneidade e particionamento. Identificar esses gargalos é essencial para refatorar o sistema, permitindo que ele suporte o aumento de tráfego de forma horizontal e eficiente.

1.	Quais os requisitos de recursos do microsserviço (CPU, RAM etc.)?
2.	Quanto tráfego uma instância do microsserviço consegue tratar?
3.	Quanta CPU/memória uma instância do serviço exige?
4.	Quais os gargalos de recursos deste microsserviço?
5.	Este microsserviço precisa ser escalado verticalmente, horizontalmente ou ambos?

<hr>
<div align="center">
<b>Planejamento de capacidade</b></div>
<br>

Neste trecho, a autora ressalta a importância de prever recursos financeiros adequados para que um microsserviço possa escalar em momentos de pico. Caso não existam recursos de hardware disponíveis para escalar quando for necessário, todo o resto do planejamento se torna quase inútil. Um planejamento inadequado de capacidade resulta em uma menor disponibilidade dentro e todo o ecossistema, o que leva a interrupções, o que custa dinheiro à empresa.

1.	O planejamento de capacidade é executado de forma programada?
2.	Qual o tempo de espera pelo novo hardware?
3.	Qual a frequência das solicitações de hardware?
4.	Alguns microsserviços têm prioridade quando as solicitações de hardware são feitas?

<hr>
<div align="center">
<b>Escalamento de dependências</b></div>
<br>

Um microsserviço projetado e construído para ser perfeitamente escalável ainda terá desafios se suas dependências não puderem escalar com ele. Portanto, para construir um serviço pronto para produção, é necessário atentar-se a este ponto. Neste ponto, é ressaltada a importância do conceito *ecossistema de microsserviços*, pois eles não existem isoladamente, e é necessário que cada peça do sistema escale junto com o restante. "As equipes de desenvolvimento responsáveis  quaisquer dependências de um microsserviço precisam ser alertadas quando forem esperados crescimentos de tráfego".

1.	Quais as dependências desse microsserviço?
2.	As dependências são escaláveis e de alto desempenho?
3.	As dependências escalarão com o crescimento esperado deste microsserviço?
4.	Os donos das dependências estão preparados para o crescimento esperado deste microsserviço?

<hr>
<div align="center">
<b>Gerenciamento de tráfego</b></div>
<br>

A importância de monitorar os padrões de tráfego de um microsserviço não pode ser subestimada. Com esses dados, as mudanças no serviço, os downtimes operacionais e as implantações podem ser programadas para evitar horários de pico de tráfego. O monitoramento também é importante para ajudar a detectar problemas e incidentes rapidamente, antes que eles causem alguma interrupção. 

Outro ponto importante trazido nesta seção é a necessidade de implantar microsserviços em múltiplos datacenters para evitar que falhas regionais causem a queda de todo o ecossistema. Embora a infraestrutura gerencie o tráfego, os microsserviços devem estar preparados para redirecionar operações entre localidades sem perda de disponibilidade.

1.	Os padrões de tráfego do microsserviço são bem entendidos?
2.	As mudanças no serviço são programadas de acordo com os padrões de tráfego?
3.	As mudanças drásticas nos padrões de tráfego (especialmente picos de tráfego) são tratadas de forma cuidadosa e adequada?
4.	O tráfego pode ser automaticamente roteado para outros datacenters em caso de falha?

<hr>
<div align="center">
<b>Limitações da linguagem de programação</b></div>
<br>

Cada linguagem de programação tem seus pontos fortes e suas fraquezas. Se o seu serviço precisa processar muitas tarefas assíncronas, é recomendado escolher uma linguagem com suporte para tais processos. Entender as limitações de escalabilidade e desempenho de cada potencial linguagem deve ser um dos fatores decisivos nessa escolha. "Não existe a melhor linguagem para escrever um microsserviço, mas *existem* linguagens que são mais adequadas do que outras para certos tipos de microsserviços."

<hr>
<div align="center">
<b>Tratando das solicitações e processando tarefas de modo eficiente</b></div>
<br>

Para garantir a escalabilidade e o desempenho, os microsserviços precisam processar tarefas de modo eficiente. Para fazê-lo, eles precisam adotar a simultaneidade e o particionamento. A simultaneidade demanda que o serviço não tenha um único processo que faça todo o trabalho, mas sim que cada tarefa seja dividida em partes menores. Essas tarefas menores podem ser processadas de forma mais eficiente utilizando do particionamento, que é a capacidade de processar as tarefas em paralelo. Linguagens como C++, Java e Go foram construídas visando otimizar a simultaneidade e o particionamento.

1.	O microsserviço está escrito em uma linguagem de programação que permite que o serviço seja escalável e de alto desempenho?
2.	Existem limitações de escalabilidade ou desempenho na forma como o microsserviço trata e processa tarefas?
3.	Os desenvolvedores na equipe do microsseriço entendem como seu serviço processa tarefas, o quão eficientemente ele processa essas tarefas e como o serviço irá se comportar à medida que o número de tarefas e solicitações aumentar?

<hr>
<div align="center">
<b>Armazenamento escalável de dados</b></div>
<br>

A forma como um microsserviço armazena e trata os dados pode facilmente se tornar a limitação mais significativa que impede sua estabilidade e desempenho: a escolha de database incorreto ou de schemas incorretos acaba comprometendo a disponibilidade de um microsserviço. Discussões referentes a BD relacionais e não-relacionais, consistência forte ou eventual, proporção de operações de leitura e escrita são tópicos extremamente complexos, que não podem ser resumidos a apenas um parágrafo. Desta forma, o que vamos tentar ver aqui é os desafios de database *especificamente* em relação à arquitetura de microsserviços.

Um ponto de atenção é que, quando databases são compartilhados entre microsserviços, a competição por recursos entra em jogo e um MS pode utilizar mais do que a sua "parte justa" do BD. Outro ponto relevante é o número de conexões ao banco de dados que podem ser abertas simultaneamente. Portanto, toda conexão precisa ser fechada adequadamente, para evitar comprometer a disponibilidade do database para todos os microsserviços que o utilizam.

Outro ponto que vale a pena ser mencionado é o gerenciamento de consistência de dados distribuídos. Tendo em vista que em microsserviços cada serviço costuma ter seu próprio banco (princípio de database per service), a possibilidade de transações distribuídas simples é eliminada. Dessa forma, surgem novos problemas, aos quais é necessário estar atento: a inconsistência temporária de dados, as dificuldade em transações distribuídas e a necessidade de padrões tipo SAGA.

1.	Este microsserviço trata dados de forma escalável e com alto desempenho?
2.	Que tipos de dados este microsserviço precisa armazenar?
3.	Qual o schema necessário para seus dados?
4.	Este microsserviço precisa de um desempenho maior de escrita ou leitura?
5.	O database deste serviço é escalado horizontalmente ou verticalmente? Ele é replicado ou particionado?
