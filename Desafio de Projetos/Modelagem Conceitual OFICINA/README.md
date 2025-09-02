# Modelagem de Dados - Oficina Mecânica

Este repositório apresenta o projeto de modelagem de dados conceitual para um sistema de gerenciamento de ordens de serviço (OS) de uma oficina mecânica. O esquema foi desenvolvido a partir de uma narrativa de negócio, visando estruturar as informações de forma lógica e coerente para suportar as operações da oficina.

## Descrição do Projeto

O objetivo do sistema é controlar o fluxo de trabalho da oficina, desde a recepção do veículo do cliente até a conclusão e entrega do serviço. O processo inclui:

1.  Cadastro de clientes e seus veículos.
2.  Designação de uma equipe de mecânicos para avaliar o veículo.
3.  Criação de uma Ordem de Serviço (OS) detalhando os serviços a serem executados e as peças necessárias.
4.  Cálculo do valor total da OS com base em uma tabela de referência para mão de obra e no custo das peças.
5.  Obtenção da autorização do cliente para iniciar o trabalho.
6.  Execução dos serviços pela equipe designada.
7.  Acompanhamento do status da OS até sua finalização.

## Esquema Conceitual

O diagrama abaixo representa as entidades, seus atributos e os relacionamentos que compõem o sistema.

*(Insira a imagem gerada do esquema aqui)*

### Entidades e Atributos

* **Clientes**
    * `idCliente` (PK): Identificador único do cliente.
    * `Nome`: Nome completo do cliente.
    * `CPF`: CPF do cliente.
    * `Telefone`: Número de contato.

* **Veiculos**
    * `idVeiculo` (PK): Identificador único do veículo.
    * `idCliente` (FK): Referência ao cliente proprietário.
    * `Placa`: Placa do veículo.
    * `Modelo`: Modelo do veículo.
    * `Ano`: Ano de fabricação/modelo.

* **Equipes**
    * `idEquipe` (PK): Identificador único da equipe de mecânicos.
    * `DescricaoEquipe`: Nome ou descrição da equipe (ex: "Equipe A - Motor").

* **Mecanicos**
    * `idMecanico` (PK): Identificador único do mecânico (baseado no código).
    * `Nome`: Nome completo do mecânico.
    * `Endereco`: Endereço do mecânico.
    * `Especialidade`: Especialidade principal do mecânico.

* **Ordens_Servico**
    * `idOS` (PK): Número da ordem de serviço.
    * `idVeiculo` (FK): Referência ao veículo em serviço.
    * `idEquipe` (FK): Referência à equipe responsável.
    * `DataEmissao`: Data em que a OS foi criada.
    * `DataConclusao`: Data prevista para a entrega do serviço.
    * `ValorTotal`: Valor total calculado (soma de serviços e peças).
    * `Status`: Situação atual da OS (ex: "Aguardando Avaliação", "Aguardando Autorização", "Em Execução", "Concluído").
    * `Autorizado`: Flag (Sim/Não) que indica a autorização do cliente.

* **Servicos**
    * `idServico` (PK): Identificador único do serviço.
    * `Descricao`: Nome do serviço (ex: "Troca de óleo", "Alinhamento").
    * `ValorMaoDeObra`: Valor da mão de obra para execução do serviço.

* **Pecas**
    * `idPeca` (PK): Identificador único da peça.
    * `NomePeca`: Nome ou descrição da peça.
    * `ValorUnitario`: Preço de cada unidade da peça.

### Tabelas Associativas (Relacionamentos N:M)

* **Equipe_Mecanicos**: Associa mecânicos a equipes, permitindo que um mecânico possa fazer parte de mais de uma equipe e que uma equipe tenha vários mecânicos.
    * `idEquipe` (FK)
    * `idMecanico` (FK)

* **OS_Servicos**: Associa os serviços que serão realizados em uma Ordem de Serviço.
    * `idOS` (FK)
    * `idServico` (FK)

* **OS_Pecas**: Associa as peças que serão utilizadas em uma Ordem de Serviço.
    * `idOS` (FK)
    * `idPeca` (FK)
    * `Quantidade`: Quantas unidades da peça serão usadas.

## Considerações Adicionais

Durante a modelagem, algumas decisões foram tomadas para complementar a narrativa original, visando um modelo mais completo e robusto:

1.  **Dados de Cliente:** Foram adicionados os campos `CPF` e `Telefone` na entidade `Clientes`, pois são informações essenciais para o cadastro e contato.
2.  **Chaves Primárias (PK):** Foi adotado um padrão de `id` como chave primária para todas as entidades principais, facilitando a padronização e a geração automática de códigos.
3.  **Relacionamento Equipe-Mecânico:** Foi criado um relacionamento N:M (muitos-para-muitos) entre `Equipes` e `Mecanicos` para dar flexibilidade ao sistema, permitindo que um mecânico possa atuar em diferentes equipes ao longo do tempo.
4.  **Valor Total na OS:** O campo `ValorTotal` foi adicionado à `Ordens_Servico`. Embora possa ser calculado dinamicamente, armazená-lo garante o registro histórico do valor no momento do fechamento da OS.
5.  **Campo `Autorizado`:** Foi adicionado um campo booleano (Sim/Não) para registrar de forma explícita a autorização do cliente, complementando o campo `Status`.

Este esquema conceitual serve como uma base sólida para a próxima fase de criação do modelo lógico e, posteriormente, a implementação do banco de dados físico.
