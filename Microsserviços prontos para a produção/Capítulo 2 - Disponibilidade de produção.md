<br>
<div align="center">
<b>Capítulo 2 - Disponibilidade de produção</b></div>
<br>

No segundo capítulo, a autora sintetiza os pilares essenciais para que um microsserviço seja considerado pronto para produção - temas que serão explorados em profundidade, de forma individual, nos capítulos seguintes.

A autora argumenta sobre a importância de determinar padrões e requisitos para os microsserviços, e pensando em empresas grandes (tal qual a Uber), eles “precisam ser genéricos o suficiente para ser aplicados a todos os microsserviços, porém específicos o suficiente também para ser quantificáveis e produzir resultados mensuráveis”. É com esse contexto em mente que é introduzido o conceito de disponibilidade de produção.

SLAs (service-level agreements ou acordos de nível de serviço) são o método mais usado de avaliar o sucesso de um serviço. Se um serviço tem um downtime muito pequeno, então provavelmente ele está cumprindo o seu objetivo.

A forma de calcular a disponibilidade é a seguinte: pegamos o <i>uptime</i> e o <i>downtime</i> de um serviço e fazemos a seguinte conta: uptime / (uptime + downtime). A disponibilidade é o <i>objetivo</i> da padronização, e não um princípio. Ela argumenta que devemos nos atentar aos princípios para garantir a disponibilidade: ela é a meta final.

Os oito princípios elencados como fundamentais para garantir a disponibilidade de produção são os seguintes: <i>estabilidade, confiabilidade, escalabilidade, tolerância a falhas, prontidão para catástrofes, desempenho, monitoramento e documentação.</i> 

<hr>
<div align="center">
<b>Estabilidade</b></div>
<br>

Com a arquitetura de microsserviços, a frequência com que novos códigos são escritos e implementados é bem maior, o que gera uma maior instabilidade. A estabilidade é sobre encontrar formas responsáveis de lidar com essas alterações. “Um microsserviço estável é aquele cujo desenvolvimento, implantação, acréscimo de novas tecnologias e a desativação e descontinuidade não resultam em instabilidade dentro de um ecossistema maior de microsserviços”.

Para seguir este princípio, devemos garantir que os microsserviços sejam implantados cuidadosamente com <i>rollouts</i> do tipo staging, pré-release e produção.
<hr>
<div align="center">
<b>Confiabilidade</b></div>
<br>

Enquanto a estabilidade foca em mitigar os impactos de mudanças, a confiabilidade estabelece a confiança necessária para o tráfego de produção, garantindo que o serviço atenda às expectativas de clientes e dependências.

Essa confiança é construída através de processos rigorosos, como testes de integração abrangentes, estratégias de cache (defensivo e para clientes) e mecanismos de roteamento precisos, que asseguram a resiliência do ecossistema mesmo diante de falhas externas. Devem ser implementadas verificações de saúde (<i>healthcheck</i>) do sistema, e os erros devem ser tratados de forma cuidadosa e adequada.
<hr>
<div align="center">
<b>Escalabilidade</b></div>
<br>

O tráfego de um microsserviço raramente é constante, e ele precisa estar preparado para um aumento no volume de requisições sem que isso comprometa a sua disponibilidade. Por isso, a escalabilidade é um dos requisitos.

Para garantir a escalabilidade de um microsserviço, é fundamental compreender sua escala de crescimento sob duas perspectivas: a <b>qualitativa</b> (natureza do tráfego, como pedidos ou acessos) e a <b>quantitativa</b> (volume de requisições por segundo). Com esses indicadores, torna-se possível prever necessidades futuras de capacidade, identificar gargalos precocemente e planejar a alocação de recursos de forma eficiente.

Neste tópico, a autora menciona dois tópicos de extrema importância: a preparação para rajadas de tráfego, normalmente através da implementação de padrões de resiliência no serviço; e sobre a importância da comunicação entre os diversos times de uma empresa, para que as pessoas sejam alertadas quando forem esperados crescimentos de tráfego.


<hr>
<div align="center">
<b>Tolerância a falhas e prontidão para catástrofes</b></div>
<br>

Inevitavelmente, em algum momento do tempo de vida de um microsserviço, ele irá falhar; ela pode acontecer devido a falhas internas (erros de código) ou falhas externas (interrupções de datacenters, por exemplo).

O primeiro passo para se preparar adequadamente é identificar potenciais falhas e cenários de catástrofe. Uma vez identificadas, deve-se criar uma estratégia para quando ela ocorrer. A padronização da mitigação é extremamente importante, na forma de procedimentos cuidadosamente executados e facilmente compreensíveis. “Se cada desenvolvedor souber exatamente o que deve fazer em caso de interrupção, souber como mitigar (...), então os tempos de mitigação e resolução cairão drasticamente."

Para tornar falhas previsíveis, é necessário ir além do planejamento passivo e forçar o sistema a falhar por meio de testes de resiliência. Esse processo ocorre em três etapas progressivas: primeiro, os <b>testes de código</b> (unitários, integração e regressão); em seguida, os <b>testes de carga</b>, que avaliam a resposta a picos de tráfego; e, por fim, o <b>teste de caos</b>. Este último é o estágio mais avançado, simulando falhas reais em ambiente de produção para garantir que tanto a infraestrutura quanto os microsserviços suportem cenários críticos de forma controlada ou aleatória.


<hr>
<div align="center">
<b>Desempenho</b></div>
<br>

O <i>desempenho</i>, um dos princípios de disponibilidade de produção, se refere a quão bem um microsserviço consegue tratar as requisições que chegam até ele. Caso ele trate as solicitações rapidamente, processe as tarefas de forma eficiente e use adequadamente os seus recursos, este é um microsserviço de alto desempenho.

Por exemplo, caso um microsserviço processe e trate tarefas simultaneamente em casos em que um processamento assíncrono de chamadas melhoraria o seu desempenho, este é um dos gargalos que prejudicam o desempenho deste serviço.


<hr>
<div align="center">
<b>Monitoramento</b></div>
<br>

O monitoramento é outro princípio de disponibilidade de produção. O bom monitoramento possui três componentes: um *logging* adequado de informações relevantes; interfaces gráficas úteis; e um sistema de alerta eficaz e acionável sobre as principais métricas.

Ao começar a projetar um microsserviço, é importante determinar com precisão quais informações devemos registrar nos *logs*. O objetivo é o seguinte: caso ocorra algum erro, é com os logs que vamos entender exatamente o que e onde nosso fluxo deu errado.

No dashboard visual, devemos ter as principais métricas facilmente acessíveis: utilização de hardware, conexões com o banco de dados, respostas e tempo médio de respostas e HTTP status dos endpoints.

A detecção de falhas deve ser feita por meio de alertas: todas as principais métricas devem gerar alertas caso exceda algum *threshold* definido pela equipe. Os limites devem ser configurados entre normal, alerta e crítico. Encontrar o valor correto é um desafio constante: eles devem ser altos o suficiente para evitar ruídos, mas baixos o suficiente para detectar qualquer problema real. A autora também argumenta que cada alerta deve ser acompanhado de um roteiro, contendo as estratégias de mitigação que qualquer desenvolvedor possa usar enquanto tenta resolver o problema.


<hr>
<div align="center">
<b>Documentação</b></div>
<br>

Conforme mencionado no capítulo 1, entende-se que a arquitetura de microsserviços tem o potencial de causar maior defasagem técnica. Para tentar mitigar esse problema, a autora sugere exigir que cada microsserviço siga um conjunto rigorosamente padronizado de requisitos de documentação, contendo todo o conhecimento essencial sobre o microsserviço, o que inclui um diagrama da arquitetura, um guia de integração e de desenvolvimento, detalhes sobre o fluxo de solicitações e endpoints de API, e um roteiro para cada alerta que o serviço pode disparar.
        
<hr>
<div align="center">
<b>Implementando a disponibilidade de produção</b></div>
<br>

Agora que já sabemos todos os princípios fundamentais para garantir a disponibilidade de produção, a próxima questão é como implementá-los no mundo real; sair da teoria e ir para a prática.

É importante destacar que a padronização requer uma aceitação por parte de todos os níveis da organização: os níveis executivos, as lideranças e os desenvolvedores. Ela precisa ser vista e comunicada não como um obstáculo ou gargalo para o desenvolvimento, mas como um guia. Pode-se gastar mais tempo no ciclo de desenvolvimento, mas evita-se que todo o ecossistema de microsserviços seja derrubado mais para frente. A autora cita uma frase (“A forma é libertadora”) que me lembra bastante uma música de Legião Urbana (“disciplina é liberdade”).

A determinação de que um dado microsserviço atenda a esses requisitos podem ser feitas pelos próprios desenvolvedores, pelos líderes, pelos gerentes, pelos DevOps ou pelas equipes de engenharia de confiabilidade (SRE). Tendo em vista que os SREs são responsáveis pela disponibilidade dos serviços, aplicar esses padrões mencionados se encaixa bem às responsabilidades existentes.
