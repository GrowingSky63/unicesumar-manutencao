# Gabarito - Exercícios de fixação do conteúdo da prova 1

1. Manutenção Evolutiva, pois o sistema está ganhando funcionalidades que o mantêm competitivo.
2. Manutenção Corretiva, pois foca em eliminar um defeito técnico que causou uma falha.
3. Greenfield (Projetos criados a partir de um "campo verde" intocado).
4. Brownfield (Projetos criados sobre bases legadas já existentes).
5. Refatoração (ou Manutenção Perfectiva), pois altera a estrutura sem mudar o comportamento ou valor retornado ao cliente.

6. Manutenção Evolutiva.
7. A complexidade do software aumenta sistematicamente, o que gera uma "corrosão do código" e o forte declínio em sua qualidade interna (dificultando a compreensão). 
8. O software é um reflexo direto do mundo real e dos modelos de negócio; para continuar satisfazendo as necessidades dos usuários e não se tornar obsoleto, ele precisará mudar. 
9. Refatoração periódica do código.
10. O tempo a mais (tempo excedente) que a equipe gasta para implementar novas manutenções devido à presença de código mal estruturado ou "gambiarras". 

11. Não. Em cenários de incerteza (como startups), ela pode ser boa e estratégica para testar uma ideia rapidamente no mercado, desde que os ganhos superem os custos e a dívida técnica seja posteriormente "paga". 
12. O tempo exato que será necessário para refatorar o código e quitar o problema adotando a solução técnica ideal (ex: seriam precisos 2 dias para reescrever o módulo). 
13. A equipe perde drasticamente a produtividade, gerando um aumento no custo, na lentidão de novas entregas e no aparecimento sucessivo de bugs devido à degradação estrutural.
14. Prefixos de correção: fix. 
15. Exemplo exato: fix(respostas): corrige travamento no contador de visualizações. 

16. Prefixo: feat (feature / nova funcionalidade). 
17. Porque viola o princípio fundamental do Conventional Commits de que cada commit deve representar apenas um único tipo de manutenção.
18. Em média, 58% do seu tempo (com base nos estudos de compreensão como de Xia/Hassan). 
19. Porque a compreensão é repetitiva; economizar minutos na leitura de um módulo hoje, multiplicado pelas centenas de vezes que ele será alterado por dezenas de devs nos próximos 10 anos, gera uma massiva economia de horas de trabalho da engenharia (fator financeiro). 
20. Não. É um grave anti-padrão. Nomes genéricos diluem a semântica do código, forçando o leitor a olhar a implementação interna da classe para entender de fato o que aquele "Gerenciador" faz. 

21. Exclusivamente para contadores em laços de repetição de curtíssima duração (loops com poucas linhas e escopo altamente restrito). 
22. "Não comente código ruim; reescreva-o". A recomendação é apagar os comentários e refatorar o algoritmo problemático (por exemplo, extraindo condicionais para novos métodos descritivos) até que a própria leitura do código se torne autoexplicativa.
23. Modularidade (A capacidade do sistema possuir módulos bem divididos e baseados em APIs). 
24. Impacta duramente a Modificabilidade também. Sem testes automatizados, as equipes têm medo de alterar o código por receio de causar regressões catastróficas. 
25. É denominado "Big Ball of Mud" (Uma grande bola de lama). 

26. É o processo de antecipar customizações desnecessárias, criando hierarquias complexas para requisitos ou extensões futuras que simplesmente ainda não existem. 
27. Porque aplicar essas técnicas onde não há demanda para customização apenas introduz complexidade (Overengineering) que atrapalha a leitura, ou seja, o custo cognitivo imposto pelo padrão de projeto ultrapassa o seu suposto ganho em flexibilidade.
28. A responsabilidade é inteiramente dos desenvolvedores (da equipe humana), uma vez que LLMs são algoritmos probabilísticos sujeitos a falhas e alucinações; a revisão do código gerado (antes de ir para a produção) é obrigatória. 
29. Porque frequentemente as IA's produzem bugs ocultos, alucinam ou não conseguem completar a tarefa adequadamente; nesse momento, o código supostamente automatizado precisará ser inspecionado manualmente, lido e corrigido por um engenheiro humano. 
30. LLM's performam significativamente melhor em bases de código de alta qualidade, modulares e limpas, já que funções menores e bem documentadas cabem melhor na "janela de contexto" e geram respostas mais precisas.

31. O desenvolvimento de um novo relatório de comissões para a equipe de vendas de um sistema já em produção caracteriza a Manutenção Evolutiva. Esse tipo de manutenção ocorre quando são implementadas novas funções e recursos a fim de manter o sistema competitivo e atender às crescentes necessidades de negócio da empresa ou dos usuários.
32. Essa migração é classificada como Manutenção Adaptativa. Esse tipo de manutenção é realizado para adequar o sistema a uma nova tecnologia, a novas versões de linguagens de programação, ou a mudanças em sistemas operacionais e regras do ambiente, sem que isso implique na adição de funcionalidades novas (features) para o usuário do sistema.
33. A utilidade desses prefixos é padronizar o formato das mensagens de commit, indicando claramente qual é a natureza da alteração realizada (por exemplo, feat para manutenção evolutiva, fix para manutenção corretiva, e refactor para refatorações no design). Essa padronização torna o histórico de controle de versões muito mais legível e claro para humanos, além de facilitar imensamente o processamento automatizado por ferramentas, permitindo a geração autônoma de changelogs, controle de versionamento semântico e criação de notas de lançamento (release notes).
34. A mensagem de commit correta seria: fix(respostas): corrige bug na contagem de visualizações de respostas. De acordo com a convenção, o prefixo fix é obrigatório para indicar a correção de um defeito (manutenção corretiva), podendo ser seguido do escopo entre parênteses, e a descrição da mensagem deve obrigatoriamente começar com um verbo flexionado no tempo presente.
35. A técnica de Retornos Antecipados (Early Returns ou Fail Fast) consiste em realizar testes e checagens (como validação de entradas ou cenários de erro) logo nas primeiras linhas de uma função e, caso a condição seja atendida, tomar a decisão de abortar a execução imediatamente através de um return. Ela melhora significativamente a legibilidade do código porque elimina a necessidade de criar grandes blocos de estruturas condicionais aninhadas (como múltiplos if/else), tornando a leitura do "caminho feliz" (o fluxo principal de execução do método) muito mais enxuta e linear.

36. A principal diferença reside no público-alvo e naquilo que está sendo documentado. Os comentários públicos (usando formatos como o Javadoc) formam a documentação de referência do sistema e são voltados para desenvolvedores externos que irão consumir ou chamar a sua API, explicando estritamente como utilizá-la através dos parâmetros, retornos e pré-condições. Em contraste, os comentários privados ficam ocultos no corpo da implementação e são direcionados aos desenvolvedores internos da própria equipe que farão a manutenção do código no futuro, com o propósito de esclarecer o "porquê" de determinadas decisões técnicas ou explicar partes complexas do algoritmo.
37. A prática de deixar código comentado ("código zumbi") é um anti-padrão grave porque esse código morto polui visualmente os arquivos e causa enorme confusão mental para a equipe de manutenção. Com o passar do tempo, os leitores hesitam em apagá-lo porque já não sabem mais se aquele trecho abandonado ainda é relevante ou se alguém na equipe possuía planos de utilizá-lo. A regra fundamental determina que o código inútil deve ser definitivamente deletado, visto que as próprias ferramentas de controle de versões (como o comando git restore) mantêm o histórico salvo com segurança e podem recuperar a versão antiga caso isso seja necessário um dia.
38. Na metáfora do Iceberg, a ponta visível (que representa uma parcela menor fora da água) corresponde à interface do módulo. Ela deve ser propositalmente simples, direta e exibir apenas os serviços que o cliente externo precisa consumir. Já a parte submersa (a base massiva escondida na água) corresponde à implementação do módulo. É ali que se concentra e reside toda a complexidade arquitetural e o volume do código-fonte, o qual deve permanecer oculto aos olhos dos clientes externos, satisfazendo assim o princípio do Ocultamento de Informação.
39. Essa alteração e quebra de contrato é classificada como uma Breaking Change Sintática. Ela é caracterizada por gerar uma incompatibilidade estrutural direta na interface do sistema (como adicionar parâmetros obrigatórios, renomear um método ou alterar tipos de retorno), provocando falhas de compilação ou execução automáticas caso os desenvolvedores clientes não atualizem e adaptem seus próprios códigos para a nova assinatura.
40. A técnica de Depreciação ajuda a mitigar o impacto porque ela não rompe a compatibilidade com o cliente imediatamente, propiciando um intervalo de tempo seguro para adaptação e migração. O método obsoleto não é apagado da interface; ao contrário, ele é mantido anotado com @Deprecated e sua implementação interna é alterada para delegar (chamar) o novo método passando valores preenchidos por padrão (default), enquanto um alerta visual ou de compilação (warning) informa ao cliente que ele deve parar de utilizar aquela interface antiga no futuro.