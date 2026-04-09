# Exercícios de fixação do conteúdo da prova 1

1. O desenvolvimento de um novo recurso de "Lista de Desejos" (Wishlist) em um sistema já existente caracteriza qual tipo de manutenção?
2. A solução de um erro de cálculo exibido no extrato bancário dos usuários é qual tipo de manutenção?
3. Como se chama o tipo de projeto de software em que o desenvolvimento começa do zero, sem a presença de um sistema legado?
4. Como se chama o desenvolvimento que exige modificação ou integração com sistemas antigos já operantes no mercado?
5. Dividir uma função de cálculo de 500 linhas em 5 funções menores para melhorar a legibilidade, sem alterar a regra de negócio final, é uma ação de qual tipo de manutenção?

6. A criação de um novo relatório de comissões de vendas dentro de um ERP já existente se enquadra em qual categoria de evolução?
7. Segundo as Leis de Lehman, o que acontece progressivamente com a qualidade e a complexidade interna de um software se ele receber contínuas manutenções evolutivas sem nenhum investimento em refatoração estrutural? 
8. De acordo com Lehman, por que um sistema de software precisa passar por mudanças contínuas (Manutenções Adaptativas e Evolutivas) e não pode simplesmente permanecer estático após o seu lançamento? 
9. Qual ação técnica é apontada pelas Leis de Lehman como o "remédio" obrigatório para conter o declínio de qualidade causado pelas frequentes alterações no código?
10. Na metáfora da Dívida Técnica proposta por Ward Cunningham, o que representam os "juros" dessa dívida durante o ciclo de vida do software? 

11. Assumir intencionalmente uma dívida técnica em um projeto para acelerar o lançamento de um produto no mercado (time-to-market) é considerado um atestado de incompetência da equipe? Justifique. 
12. Dentro da mesma metáfora, o que constitui o "principal" da dívida técnica? 
13. O que acontece com a velocidade da equipe de desenvolvimento a médio e longo prazo se o projeto focar exclusivamente em entregar funcionalidades rápidas e negligenciar o pagamento da dívida técnica?
14. Segundo o padrão Conventional Commits, qual prefixo obrigatório o desenvolvedor deve usar ao subir uma correção de bug de sistema? 
15. Escreva um exemplo prático de mensagem de commit, com a sintaxe perfeita, para registrar a correção de um travamento no contador de visualizações de um fórum de perguntas e respostas.

16. Qual prefixo do Conventional Commits mapeia de forma direta a Manutenção Evolutiva (adição de nova funcionalidade)?
17. Por que um único commit que implementa uma nova tela (feature) e ao mesmo tempo conserta um bug no banco de dados viola gravemente as diretrizes do Conventional Commits?
18. Em média, qual porcentagem do tempo efetivo de trabalho um desenvolvedor passa realizando atividades de leitura e compreensão de código? 
19. Considerando o ciclo de vida de um software (que pode durar décadas), por que a escrita de código limpo deixou de ser apenas uma questão estética para ser considerada uma decisão financeira e estratégica vital? 
20. A utilização de nomes como Gerenciador, Sistema ou Dados para classes é considerada um forte indicativo de código limpo e boa abstração? Justifique. 

21. De acordo com o Clean Code, em qual cenário único e altamente específico a utilização de nomes de variáveis com apenas uma letra (ex: i, j) é aceitável e isenta de punição de legibilidade? 
22. Qual é a recomendação máxima de autores de Clean Code (como Brian Kernighan) quando um desenvolvedor se depara com a necessidade de escrever dezenas de linhas de comentário para explicar um if/else muito complexo?
23. Segundo a ISO/IEC 25010, qual subcaracterística da Manutenibilidade indica se o sistema é formado por componentes bem delimitados que se comunicam por APIs, facilitando a análise pontual? 
24. A ausência de testes automatizados afeta exclusivamente a "Testabilidade" do sistema, deixando a subcaracterística "Facilidade de Mudanças (Modificabilidade)" intacta? Explique. 
25. Como se define a degradação estrutural e arquitetural máxima de um sistema na área de Engenharia de Software, situação em que realizar qualquer modificação se torna caótico? 

26. O que caracteriza o anti-padrão de projeto conhecido como "Overengineering" (Otimização Prematura) em relação à flexibilidade de um sistema? 
27. Por que a aplicação desenfreada de Injeção de Dependências e Padrões de Projeto (como o Strategy) em módulos que nunca precisarão de customização é considerada um erro de design?
28. Se um Agente Autônomo ou Modelo de Linguagem (LLM) inserir um bug no sistema através da geração de código que resulte em um prejuízo milionário, de quem é a responsabilidade técnica perante a Engenharia de Software e por quê? 
29. Com o aumento do uso de LLMs para gerar trechos inteiros de código ("Autocomplete on steroids"), justifique por que o conhecimento prático em Código Limpo (Clean Code) e modularização continua sendo uma habilidade fundamental para o desenvolvedor humano. 
30. Segundo as evidências atuais da literatura sobre IA e Engenharia de Software, Modelos de Linguagem apresentam o seu melhor desempenho de correção de bugs em qual tipo de ambiente de código?


31. O desenvolvimento de um novo relatório de comissões para a equipe de vendas de um sistema já em produção caracteriza qual tipo de manutenção?
32. A migração de um sistema para uma versão mais recente de uma linguagem de programação ou sistema operacional, sem adicionar funcionalidades, é classificada como qual tipo de manutenção?
33. Qual é a utilidade do uso de prefixos como feat, fix e refactor de acordo com a especificação do Conventional Commits?
34. Escreva a mensagem de commit correta, no padrão Conventional Commits, para registrar a correção de um bug na contagem de visualizações de respostas em um fórum.
35. Descreva a técnica de "Retornos Antecipados" (Early Returns ou Fail Fast) e explique por que ela melhora a legibilidade do código. 

36. Qual é a principal diferença de finalidade entre os comentários públicos (como o Javadoc) e os comentários privados (de implementação)? 
37. Por que a prática de deixar código comentado (também conhecido como "código zumbi") no arquivo com medo de apagá-lo é considerada um anti-padrão grave na manutenção de software?
38. Na metáfora proposta por Bertrand Meyer que compara módulos a "Icebergs", o que representam a ponta visível do iceberg e a sua parte submersa? 
39. A alteração na assinatura de uma API pública, como a inserção de um parâmetro obrigatório que não existia antes, quebra o código do cliente. Como essa quebra de contrato é classificada?
40. Como a técnica de Depreciação (usando a anotação @Deprecated, por exemplo) ajuda a mitigar o impacto de mudanças nas interfaces de um sistema em produção?