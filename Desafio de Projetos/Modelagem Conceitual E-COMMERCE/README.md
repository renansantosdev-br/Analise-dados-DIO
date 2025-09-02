\# Modelagem de Dados para E-commerce



Este repositório contém a modelagem de dados conceitual para um sistema de e-commerce. O objetivo deste projeto é estruturar um banco de dados relacional que suporte as principais operações de uma loja virtual, incluindo gerenciamento de produtos, clientes, pedidos, pagamentos, estoque e entregas.



\## Visão Geral do Modelo



O diagrama de entidade-relacionamento (DER) foi projetado para ser flexível, permitindo que a plataforma opere com produtos de fornecedores próprios e também como um marketplace, com vendedores terceirizados.



!\[Diagrama de Entidade-Relacionamento](Projeto\_modelagem\_ecommerce.png)



\## Estrutura e Entidades



O modelo é composto pelas seguintes entidades principais, representando os diferentes contextos do negócio:



\### 1. Clientes e Pedidos



\* \*\*`Cliente`\*\*: Armazena as informações cadastrais dos clientes.

&nbsp;   \* `idCliente`: Chave primária.

&nbsp;   \* `Nome`: Nome completo do cliente.

&nbsp;   \* `Identificação`: Documento de identificação (CPF/CNPJ).

&nbsp;   \* `Endereço`: Endereço de cadastro do cliente.



\* \*\*`Pedido`\*\*: Registra cada compra realizada por um cliente.

&nbsp;   \* `idPedido`: Chave primária.

&nbsp;   \* `Status do Pedido`: Status atual do pedido (ex: "Aguardando Pagamento", "Enviado", "Entregue").

&nbsp;   \* `Descrição`: Descrição ou observação sobre o pedido.

&nbsp;   \* `Despesas`: Valor do frete.

&nbsp;   \* `Cliente\_idCliente`: Chave estrangeira para a tabela `Cliente`.

&nbsp;   \* `Pagamento\_idPagamento`: Chave estrangeira para a tabela `Pagamento`.



\### 2. Produtos e Estoque



\* \*\*`Produto`\*\*: Tabela central de produtos, contendo informações genéricas.

&nbsp;   \* `idProduto`: Chave primária.

&nbsp;   \* `Descrição`: Nome ou descrição do produto.

&nbsp;   \* `Valor`: Preço do produto.

&nbsp;   \* `Categoria`: Categoria à qual o produto pertence.



\* \*\*`Estoque`\*\*: Gerencia os locais de armazenamento dos produtos.

&nbsp;   \* `idEstoque`: Chave primária.

&nbsp;   \* `Local`: Nome ou identificação do local de estoque.

&nbsp;   \* `Saldo`: Saldo total do estoque (pode ser removido se o controle for por produto).



\* \*\*`Produto\_has\_Estoque`\*\*: Tabela associativa que informa a quantidade de um produto em um determinado estoque.

&nbsp;   \* `Produto\_idProduto`: Chave estrangeira para `Produto`.

&nbsp;   \* `Estoque\_idEstoque`: Chave estrangeira para `Estoque`.

&nbsp;   \* `Quantidade`: Quantidade do produto disponível naquele estoque.



\### 3. Fornecedores e Vendedores Terceirizados (Marketplace)



\* \*\*`Fornecedor`\*\*: Empresas que fornecem produtos para o e-commerce.

&nbsp;   \* `idFornecedor`: Chave primária.

&nbsp;   \* `Razão Social`: Nome da empresa fornecedora.

&nbsp;   \* `CNPJ`: CNPJ do fornecedor.



\* \*\*`Disponibilizando um Produto`\*\*: Tabela associativa que liga os produtos aos seus fornecedores.



\* \*\*`Terceiro - Vendedor`\*\*: Vendedores independentes que utilizam a plataforma para vender seus produtos.

&nbsp;   \* `idTerceiro - Vendedor`: Chave primária.

&nbsp;   \* `Razão Social`: Nome do vendedor.

&nbsp;   \* `Identificação`: Documento de identificação (CPF/CNPJ).



\* \*\*`Produtos\_Vendedores\_terceiros`\*\*: Tabela associativa que define quais produtos são vendidos por quais vendedores terceirizados.



\### 4. Transações e Logística



\* \*\*`Pagamento`\*\*: Detalhes sobre a transação financeira de um pedido.

&nbsp;   \* `idPagamento`: Chave primária.

&nbsp;   \* `Forma de Pagamento`: Método utilizado (ex: "Cartão de Crédito", "Boleto").

&nbsp;   \* `Numero do Cartão`: Número do cartão (deve ser tratado com segurança).

&nbsp;   \* `Numero de Transação`: Código da transação gerado pelo gateway.



\* \*\*`Entrega`\*\*: Informações sobre o envio dos pedidos.

&nbsp;   \* `idEntrega`: Chave primária.

&nbsp;   \* `Status Entrega`: Status da entrega (ex: "Em trânsito", "Entregue").

&nbsp;   \* `Codigo de rastreio`: Código para rastreamento do envio.



\* \*\*`Produtos\_para\_entrega`\*\*: Tabela associativa que vincula os itens do pedido à sua respectiva entrega.



\### 5. Tabelas Associativas (Junction Tables)



\* \*\*`Produtos\_dos\_Pedidos`\*\*: Liga os produtos aos pedidos, formando o carrinho de compras. É o "item do pedido".

\* \*\*`Produto\_has\_Estoque`\*\*: Controla o inventário de cada produto em cada local de estoque.

\* \*\*`Disponibilizando um Produto`\*\*: Associa produtos a fornecedores.

\* \*\*`Produtos\_Vendedores\_terceiros`\*\*: Associa produtos a vendedores do marketplace.

\* \*\*`Produtos\_para\_entrega`\*\*: Associa os itens de um pedido a uma entrega específica.



\## Como Utilizar



Este modelo pode ser usado como base para a criação de um banco de dados físico em qualquer SGBD relacional, como MySQL, PostgreSQL, SQL Server, etc. Os scripts para a criação das tabelas (`CREATE TABLE`) podem ser desenvolvidos a partir deste diagrama.



\## Possíveis Melhorias e Próximos Passos



\* \*\*Normalização de Endereços:\*\* A entidade `Cliente` pode ter seu atributo `Endereço` decomposto em campos como `logradouro`, `numero`, `cidade`, `estado` e `CEP` para facilitar a logística e a validação.

\* \*\*Atributos em Tabelas Associativas:\*\* Adicionar campos como `quantidade` e `preco\_unitario` na tabela `Produtos\_dos\_Pedidos`.

\* \*\*Tipos de Dados:\*\* Revisar os tipos de dados para otimização, utilizando `DECIMAL` para valores monetários e `ENUM` ou tabelas de domínio para campos de status e categoria.

\* \*\*Segurança:\*\* Implementar uma camada de segurança para dados sensíveis, como o `Numero do Cartão` na tabela `Pagamento`.

\* \*\*Índices:\*\* Adicionar índices nas chaves estrangeiras (`Foreign Keys`) e em colunas frequentemente consultadas para otimizar a performance das queries.

