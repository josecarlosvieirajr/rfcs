# RFC Topic Naming

## Recomendação para os nomes dos tópicos

Buscando a padronização dos nomes de tópicos dentro do SNS, com o objetivo de facilitar a leitura dos nomes, e também compreender somente na leitura os dados que provavelmente estarão disponíveis dentro do tópico.

### Formatação dos Nomes

Os nomes dos tópicos devem estar escritos em minúsculas. Todos os tópicos devem seguir snake_case, como exemplo `meu_topico_favorito`

A legibilidade e a facilidade de compreensão desempenham um papel importante na nomenclatura adequada dos tópicos. Os nomes dos tópicos em minúsculas são fáceis de ler e o snake_case melhora a compreensão; Além disso, o subtraço duplo (`__`) é um ótimo separador entre as seções em um nome de tópico.


### O que **NÃO** usar
Os campos que podem ser alterados não devem ser usados em nomes de tópicos. Campos como nome da equipe, nome do produto e proprietário do serviço **nunca devem ser usados** em nomes de tópicos.

Com o tempo, esses dados irão mudar conforme a evolução da empresa. Não é uma tarefa fácil alterar o nome de um tópico quando ele está em uso em toda a empresa, portanto, é melhor deixar esses campos de fora desde o início.

Os nomes de tópicos não devem ser vinculados a nomes de serviços, a menos que sejam completamente internos a um único serviço e não sejam produzidos ou consumidos de qualquer outro serviço.

A maioria dos tópicos acaba com mais de um consumidor e seu produtor pode mudar no futuro. É melhor nomear os tópicos de acordo com os dados que eles contêm, em vez do que está criando ou lendo dos dados.


### Estrutura

Feito todas as ressalvas, vamos a um modelo de implementação para os nomes dos tópicos.

#### Conceito Geral

`<ambiente>__<dominio>_<sub-dominio>__<classificação>__<descrição>_<versão>`

 - **ambiente** *(Obrigatório)*
    - Como ainda temos apenas uma AWS para ambas as fases de desenvolvimento, colocar o ambiente como primeira etapa, facilita na filtragem dos tópicos
    - Ex: `stg`, `prd`

- **domínio** *(Obrigatório)*
    - Classifica a origem do dado, deve ser usado com o nome mais popular do serviço, plataforma ou domínio.
    - Ex:`pipedrive`, `user`, `leads`

- **sub-domínio** *(Não Obrigatório)*
    - Complemente a descrição da origem junto ao domínio, demonstrando uma variação existente no serviço, plataforma ou domínio. Lembrando de colocar sub-domínio somente quando achar **extremamente** necessário, pois esse sub-domínio pode ficar obsoleto.
    - Ex: `marketing`, `coliving`

- **classificação** *(Obrigatório)*
    - **fct** *(Fact)*
        - Dados de fatos, são informações sobre uma coisa que aconteceu. É um evento imutável em um ponto específico no tempo. 
        - Exemplos disso incluem dados de eventos de atividade do usuário ou notificações.
        - Por convenção esse tópico sempre deveria ser FIFO
    - **cdc** *(Change data capture)*
        - Este tópico contém todas as instâncias de uma coisa específica e recebe todas as alterações dessas coisas. Esses tópicos não capturam deltas e podem ser usados para repovoar armazenamentos de dados ou caches
    - **cmd** *(Command)*
        - os tópicos de comando representam operações que devem ocorrer no sistema. Isso geralmente é encontrado como o padrão de solicitação-resposta, onde você tem um verbo e uma instrução. Por exemplo uma atualização ou deleção do usuário
        - Por convenção esse tópico sempre deveria ser FIFO
    - **sys** *(System)*
        - os tópicos do sistema são usados como mensagerias internas em um único serviço. São tópicos operacionais que não contêm nenhuma informação relevante fora do sistema proprietário. Esses tópicos não são destinados ao consumo público.
    - **log**
        - Tópico destinado a armazenamento de logs.
    - **trk** *(Tracking)*
        - Para acompanhar eventos como cliques de usuários, visualizações de página, visualizações de anúncios

- **descrição** *(Obrigatório)*
    - A descrição é sem dúvida a parte mais importante do nome e é o nome do evento que descreve o tipo de dado que o tópico contém. 
    - Ex: `integration`, `invoices`, `payments`

- **versão** *(Obrigatório)*
    - A versão de um tópico geralmente é a seção mais esquecida de um nome de tópico adequado. À medida que os dados evoluem dentro de um tópico, pode haver alterações no esquema ou uma mudança completa no formato dos dados. Ao criar tópicos de versão, você pode permitir um período de transição em que os consumidores podem alternar para os novos dados sem afetar os consumidores antigos.
    - Por convenção, é preferível versionar todos os tópicos e iniciá-los em `0`.

### Alguns Exemplos de topicos.

`prd__core__fct__errors_2`
`pipedrive_marketing__cmd__webhook_0`
`stg__identity__cdc__users_1`
`notifications_leads__cmd_emails_3`
