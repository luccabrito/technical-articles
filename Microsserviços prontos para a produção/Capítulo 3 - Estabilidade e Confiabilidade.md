<br>
<div align="center">
<b>Capítulo 3 - Estabilidade e Confiabilidade</b></div>
<br>

Agora, o livro passa a se dedicar com mais atenção a cada um dos princípios elencados como fundamentais para garantir a disponibilidade de produção. Neste capítulo, serão exploradas maneiras de construir e executar um microsserviço estável e confiável, pensando em todo o ciclo de desenvolvimento, na construção de pipelines de deployment, de proteção contra falhas, em formas de construir um roteamento e descoberta estáveis, e nos procedimentos adequados de descontinuação de endpoints.

Retomando a conceituação, a autora define que “um microsserviço estável é aquele para o qual o desenvolvimento, a implantação, a adoção de novas tecnologias e a descontinuação de outros serviços não resultam em instabilidade de todo o amplo ecossistema de microsserviços”. Já um microsserviço confiável é aquele “no qual outros microsserviços e o ecossistema como um todo podem confiar”.


<hr>
<div align="center">
<b>Ciclo de desenvolvimento</b></div>
<br>

A maioria das falhas que ocorrem em microsserviços é causada por erros introduzidos no código que não foram detectados na fase de desenvolvimento ou nos testes. Por isso, é necessário pensar num ciclo de desenvolvimento estável e confiável.

O primeiro passo é o desenvolvedor fazer uma alteração no código. Isso começa com a retirada de uma cópia do código de um repositório central e a criação de uma branch individual.

Segundo, depois das mudanças terem sido implementadas, elas devem ser “cuidadosa e minuciosamente revisadas por outros engenheiros da equipe”. Caso eles aprovem, só então a alteração deverá ser adicionada ao repositório, empacotada (package), construída (build) e introduzida no pipeline de deployment.


1.	O microsserviço tem um repositório central, no qual todo o código está armazenado?
2.	Os desenvolvedores trabalham em um ambiente de desenvolvimento que reflete com precisão o estado da produção (por exemplo, que reflete com precisão o mundo real)?
3.	Existem testes adequados do tipo lint, de unidade, de integração e fim a fim em operação para o microsserviço?
4.	Existem procedimentos e políticas de revisão de código em vigor?
5.	O processo de teste, empacotamento, build e liberação (release) é automatizado?


<hr>
<div align="center">
<b>Pipeline de deployment</b></div>
<br>

Para gerenciar o alto risco de erro humano e a falta de coordenação em ecossistemas de microsserviços, a implementação de um pipeline de deployment padronizado é essencial. Como a maioria das interrupções de produção deriva de implantações falhas, esse modelo substitui processos manuais por um fluxo automatizado e rigoroso de validação, garantindo a estabilidade do sistema como um todo.

A estrutura mencionada como ideal pela autora é uma na qual a pipeline organiza a entrega em três fases críticas para minimizar impactos:

1.	**Staging**: Testes em ambiente controlado;
2.	**Canary (Pré-release)**: Implantação gradual em uma pequena parcela do tráfego (5% a 10%);
3.	**Produção Total**: Expansão definitiva apenas após a validação do comportamento real.

<hr>
<div align="center">
<b>Staging</b></div>
<br>

Qualquer nova versão deve primeiro ser implantada em um ambiente do tipo *staging*. Ele deve ser uma cópia exata do ambiente de produção: um reflexo do mundo real, mas sem o tráfego real. Embora ambientes de staging sejam ambientes de teste, eles diferem da fase de desenvolvimento e do ambiente de desenvolvimento, pois a versão implantada no ambiente staging é uma versão candidata à produção.

Aqui, a autora vai fazer uma distinção entre duas formas de configurar a fase de *staging*: *staging* total e *staging* parcial.

O ***staging* total** consiste na criação de uma cópia integral do ecossistema de produção em um ambiente isolado. Para garantir a fidelidade dos testes sem comprometer o sistema real, essa configuração exige que os serviços de staging se comuniquem exclusivamente entre si, sendo estritamente proibida qualquer troca de tráfego com instâncias de produção.

Já o ***staging* parcial** é uma abordagem mais leve onde, em vez de replicar todo o ecossistema, cada microsserviço possui apenas um pool de servidores isolado para testes. A principal característica desse modelo é que a nova versão do serviço em staging comunica-se diretamente com as dependências reais (upstream e downstream) que já estão rodando em produção. Ele possui fidelidade ao "mundo real" e menor custo, contudo exige extrema cautela para não afetar os dados de produção.

É importante que a equipe de microsserviços responda à pergunta: por quanto tempo uma nova versão deve ser executada em staging antes de poder ser implantada no ambiente de pré-release? Ela deve passar por todos os testes antes ser “promovida”.

<hr>
<div align="center">
<b>Pré-release (fase canary)</b></div>
<br>

Ao chegar no ambiente de pré-release, é necessário estar atento ao fato que ele atende ao tráfego de produção. Rollbacks automatizados são uma necessidade absoluta neste ambiente: caso ocorra algum erro conhecido, o sistema de implantação deve voltar à última versão estável. Também os alertas e dashs devem ser diferenciados para este ambiente, para não gerar confusão na equipe.


<hr>
<div align="center">
<b>Produção</b></div>
<br>

O ambiente de produção é o “mundo real”. Qualquer erro no código deve ter sido descoberto e resolvido antes de se chegar a este ponto. Depois de passar pelo pré-release, a implantação em ambiente produtivo pode ser feita de uma vez só ou pode ser gradualmente implantada (por exemplo, 25% dos servidores, depois 50%... etc).


<hr>
<div align="center">
<b>Impondo uma implantação estável e confiável</b></div>
<br>

Alguns pontos são levantados nesta sessão, que irei resumir nos dois tópicos abaixo:

1.	A importância da pipeline abrangente de implantação para construir um microsserviço estável e confiável não pode ser subestimada. O “atraso” introduzido pelas fases do pipeline é muito pequeno frente aos ganhos que se tem.
2.	Só deve-se permitir que os desenvolvedores implantem diretamente no ambiente de produção no caso de interrupções mais graves.

<hr>

1.	O ecossistema de microsserviços tem um pipeline de deployment padronizado?
2.	Existe uma fase de staging total ou parcial no pipeline de deployment?
3.	Qual o acesso que o ambiente de staging tem aos serviços de produção?
4.	Existe uma fase de pré-release no pipeline de deployment?
5.	As implantações são executadas na fase de pré-release por um período de tempo longo o suficiente para detectar quaisquer falhas?
6.	A fase de pré-release trata com precisão uma amostra aleatória do tráfego de produção?
7.	As portas do microsserviço são as mesmas para o ambiente de pré-release e de produção?
8.	As implantações no ambiente de produção são feitas todas ao mesmo tempo ou gradualmente?
9.	Existe um procedimento em vigor para pular as fases de staging e pré-release em caso de emergência?



<hr>
<div align="center">
<b>Dependências</b></div>
<br>

Neste tópico, o livro reforça a ideia de que todo microsserviço existe dentro de um ecossistema: ele recebe solicitações de outros microsserviços e para fornecer suas respostas muitas vezes ele também depende de um outro microsserviço. Desta forma, para construir microsserviços estáveis e confiáveis, é necessário se preparar para falhas de dependências, com planos de mitigação e proteção. Isso pode ser feito por meio de “backups, fallbacks, cache e/ou alternativas para cada dependência caso elas apresentem falhas”.

O primeiro trabalho é um estudo/documentação, para entender todas as dependências. Em seguida, quando elas forem conhecidas, o próximo passo é configurar backups, alternativas, fallbacks ou cache para cada dependência. Não existe uma maneira correta de fazer isso: depende completamente das necessidades do serviço.

O livro recomenda a utilização do cache LRU *(Least Recently Used)* em muitos cenários, no qual “dados relevantes são mantidos em uma fila e quaisquer dados não usados são apagados quando a fila do cache esgota a sua capacidade”. O argumento é de que eles são fáceis de implementar, eficientes, de alto desempenho e mitigam bem as falhas de dependência.

1.	Quais são as dependências deste microsserviço?
2.	Quais são seus clientes?
3.	Como este microsserviço mitiga as falhas de dependência?
4.	Existem backups, alternativas, fallbacks ou cache defensivo para cada dependência?

        
<hr>
<div align="center">
<b>Roteamento e descoberta</b></div>
<br>

Aqui, a autora ressalta a importância da confiabilidade na comunicação entre microsserviços. A saúde de um MS deve ser sempre conhecida, portanto devemos rodar *healthchecks* constantemente em nossos pods, para que uma solicitação nunca seja enviada para um servidor ou serviço com problemas. 

Entretanto, verificações de saúde não devem ser o único fator para avaliar se um serviço é saudável ou não. Muitas exceções não tratadas também deve fazer com que um serviço seja considerado não saudável.

1.	As verificações de saúde do microsserviço são confiáveis?
2.	As verificações de saúde refletem com precisão a saúde do microsserviço?
3.	As verificações de saúde são realizadas em um canal separado dentro da camada de comunicação?
4.	Existem circuit breakers ativados para evitar que microsserviços não saudáveis façam solicitações?
5.	Existem circuit breakers ativados para evitar que o tráfego de produção seja enviado para servidores e microsserviços não saudáveis?

<hr>
<div align="center">
<b>Descontinuação e desativação</b></div>
<br>

Quando um microsserviço não está mais em uso, sua desativação deve ser executada com atenção para garantir que nenhum cliente seja comprometido. Neste tópico, ela alerta que “...são mais um problema sociológico dentro da organização (...) do que um problema técnico”, no sentido de que a comunicação entre as equipes de desenvolvimento é crucial para uma descontinuação saudável.  Um monitoramento bem feito desempenha um papel crítico: com os endpoints sendo monitorados de perto *antes* da descontinuação, é possível verificar se há solicitações que ainda estão sendo enviadas para o serviço ou endpoint desatualizado.

1.	Existem procedimentos em funcionamento para desativar um microsserviço?
2.	Existem procedimentos em funcionamento para descontinuar endpoints de API de um microsserviço?

