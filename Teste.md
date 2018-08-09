RevanSAP
===

* [Banco](#banco)
	* [Item Fiscal](#item-fiscal)
	* [Cliente](#cliente)
	* [Fatura](#fatura)
	* [Items da Fatura](#items-da-fatura)
	* [Baixas da Fatura](#baixas-da-fatura)

## Banco

O DataBase tem na sua estrutura um esquema especifico para a integração do SAP. Esse esquema será preenchido automaticamente pelo REVAN garantindo a integridade dos dados. Os items a serem inseridos e atualizados pelo revan são:

  * Item Fiscal
  * Clientes
  * Faturas
  * Item da Fatura
  * Baixa da Fatura

Dados de conexão:

  * **HOST**: 192.168.111.8

  * **DB**: netmanager_homologacao

  * **Schema**: revansap

  * **usuario**: revan_sap

  * **senha**: dev123456

### Item Fiscal

> Tabela: **revansap.t_item_fiscal**

**Parâmetros:**

|NOME|TIPO|MÍNIMO|MÁXIMO|FORMATAÇÃO|OBRIGATÓRIO|DESCRIÇÃO|
|----|:--:|:----:|:----:|:--------:|:---------:|---------|
|descricao|String|-|-|-|Sim|Nome do Item Fiscal Vendido no Revan|
|base_plano_id|Bigint|-|-|-|Não|Código do plano Vendido no Revan|
|base_servico_avulso_id|Bigint|-|-|-|Não|Código de Serviço Avulso Vendido no Revan|
|sap_item_id|String|-|-|-|Não|Código do tem fiscal no SAP|
|sincronizado|Boolean|-|-|-|Sim|Identificação de sincronização entre o Revan e o SAP, atributo terá o valor padrão FALSE na hora de inserir um novo registro.|
|data_sincronizado|timestamp without time zone|-|-|YYYY-MM-DD HH:MI:SS|Não|Data que foi feita a sincronização entre o Revan e o SAP.|
|data_ultima_atualizacao|timestamp without time zone|-|-|YYYY-MM-DD HH:MI:SS|Sim|Data da ultma modificação do registro|
|descricao_erro|Text|-|-|-|Não|Descrição do erro na sincronização entre o Revan e o SAP|
|status_posicao|Boolean|-|-|-|Não|Atributo utilizado pelo SAP. Terá o valor padrão FALSE na hora de inserir um novo registro.|

**Observações Importantes:**

  * **base_plano_id e base_servico_avulso_id** São atributos obrigatório preenchidoa pelo REVAN. Os dois atributos não podem serem preenchidos ao mesmo tempo.
  * **sap_item_id** Atributo atualizado unicamente pelo SAP na hora da sincronização.
  * **sincronizado e data_sincronizado** Sao Atributos atualizado unicamente pelo SAP.
  * **descricao_erro** Atributo não é obrigatório na hora de inserir um novo registro mas será obrigatório preencher no momento que o SAP identificar erro na sincronização.

### Cliente

> Tabela: **revansap.t_cliente**

**Parâmetros:**

|NOME|TIPO|MÍNIMO|MÁXIMO|FORMATAÇÃO|OBRIGATÓRIO|DESCRIÇÃO|
|----|:--:|:----:|:----:|:--------:|:---------:|---------|
|revan_cliente_id|Bigint|-|-|-|NOT NULL|Código do cliente no Revan|
|nome|String|3|255|-|Sim|Nome do cliente.|
|nome_fantasia|String|3|255||Não|Nome de Fantasia se tiver preenchido CNPJ|
|cpf|String|-|-|XXX.XXX.XXX-XX|Não|CPF do cliente|
|cnpj|String|-|-|XXX.XXX.XXX-XX|Não|CNPJ do cliente|
|tipo_pessoa|Int|-|-|-|Sim|1: cpf 2: cnpj|
|tipo_assinante|Int|-|-|-|Não|Identifica o tipo de assinante (1:comercial,##2:orgão publico,3:residencial)|
|telefone_principal|BigInt|-|-|-|-|Telefone1 do cliente|
|telefone_secundario|BigInt|-|-|-|-|Telefone2 do cliente|
|email_principal|String|3|255|XX@XX.XX|-|E-mail do cliente|
|inscricao_estadual|String|-|-|-|-|Inscrição estadual no caso de CNPJ|
|inscricao_municipal|String|-|-|-|-|Inscrição municipal no caso de CNPJ|
|logradouro_tipo|String|3|255|-|Sim|Identificação do logradouro (Rua, Traverssa etc.) |
|logradouro|String|3|255|-|Sim|Endereço de Instalação do Cliente.|
|numero|String|1|30|-|Sim|Numero do endereço do cliente.|
|complemento|String|3|255|-|Não|Complemento do endereço do cliente.|
|bairro|String|3|100|-|Sim|Bairro do endereço do cliente.|
|cep|String|9|9|XX.XXX-XX|Não|CEP do endereço do cliente.|
|revan_base_cidade_id|Int|-|-|-|Sim|Código da Cidade no Revan.|
|revan_base_cidade_nome|String|3|100|-|Sim|Código da Cidade no Revan.|
|uf|String|2|2|-|Sim|UF da cidade do endereço do cliente.|
|pais|String|3|100|-|Sim|Pais do endereço do cliente por padrão ese atributo sera preenchido como *brasil*|
|sincronizado|Boolean|-|-|-|Sim|Identificação de sincronização entre o Revan e o SAP, atributo terá o valor padrão FALSE na hora de inserir um novo registro.|
|data_sincronizado|timestamp without time zone|-|-|YYYY-MM-DD HH:MI:SS|Não|Data que foi feita a sincronização entre o Revan e o SAP|
|descricao_erro|Text|-|-|-|Não|Descrição do erro na sincronização entre o Revan e o SAP|
|data_ultima_atualizacao|timestamp without time zone|-|-|YYYY-MM-DD HH:MI:SS|Sim|Data da última modificação do registro|
|status_posicao|Boolean|-|-|-|Não|Atributo utilizado pelo SAP, atributo terá o valor padrão FALSE na hora de inserir um novo registro.|

**Observações Importantes:**

  * **sincronizado e data_sincronizado** São Atributos atualizado unicamente pelo SAP.
  * **descricao_erro** Atributo não é obrigatório na hora de inserir um novo registro mas sera obrigatório preencher no momento que o SAP identificar erro na sincronização.


### Fatura

> Tabela: **revansap.t_fatura**

**Parâmetros:**

|NOME|TIPO|MÍNIMO|MÁXIMO|FORMATAÇÃO|OBRIGATÓRIO|DESCRIÇÃO|
|----|:--:|:----:|:----:|:--------:|:---------:|---------|
|revan_financeiro_fatura_id|BigInt|-|-|-|Sim|Código da Fatura no Revan|
|cliente_id|BigInt|-|-|-|Sim|Código do cliente no esquema **revansap**|
|email|String|3|255|XX@XX.XX|Não|E-mail do cliente (Utilizado para enviar a Fatura via E-mail)|
|numero_fatura|BigInt|-|-|-|Sim|Identifica o número da Fatura|
|valor|Numeric(10,2)|-|-|-|Sim|Valor integral da Fatura cobrada no Revan|
|valor_renegociado|Numeric(10,2)|-|-|-|Não|Valor renegociado pelo cliente por débito atrasado|
|data_emissao|Timestamp without time zone|-|-|-|Sim|Data que foi emitida a Fatura|
|data_vencimento|Date|-|-|-|Sim|Data de vencimento da Fatura|
|nota_fiscal_antecipada|boolean|-|-|-|Sim|Identifica se a fatura é nota fiscal antecipada|
|nota_fiscal_manual|Boolean|-|-|-|Sim|Identifica se a fatura é nota fiscal manual|
|estado|estado_fatura|-|-|-|Sim|Estado da Fatura no Revan. O valor padrão na hora de inserir o registro é 'Aguardando pagamento' os possiveis estados são: ('Aguardando pagamento', 'Em análise', 'Paga', 'Disponível', 'Em disputa', 'Devolvida', 'Cancelada')|
|cancelada|Boolean|-|-|-|Sim|Identifica se a Fatura foi cancelada|
|data_cancelamento|Timestamp without time zone|-|-|-|Não|Data de cancelamento da Fatura|
|descricao_cancelamento|Text|-|-|-|Não|Descrição do motivo de cancelamento|
|logradouro_tipo|String|3|255|-|Sim|Identificação do logradouro (Rua, Traverssa etc.) |
|logradouro|String|3|255|-|Sim|Endereço de Cobrança do Cliente.|
|numero|String|1|30|-|Sim|Numero do endereço de Cobrança do cliente.|
|complemento|String|3|255|-|Não|Complemento do endereço de Cobrança do cliente.|
|bairro|String|3|100|-|Sim|Bairro do endereço de Cobrança do cliente.|
|cep|String|9|9|XX.XXX-XX|Não|CEP do endereço de Cobrança do cliente.|
|revan_base_cidade_id|Int|-|-|-|Sim|Código da Cidade no Revan.|
|revan_base_cidade_nome|String|3|100|-|Sim|Código da Cidade no Revan.|
|uf|String|2|2|-|Sim|UF da cidade do endereço de Cobrança do cliente.|
|pais|String|3|100|-|Sim|Pais do endereço de Cobrança do cliente por padrão ese atributo sera preenchido como *brasil*|
|sincronizado|Boolean|-|-|-|Sim|Identificação de sincronização entre o Revan e o SAP. O atributo terá o valor padrão FALSE na hora de inserir um novo registro.|
|data_sincronizado|timestamp without time zone|-|-|YYYY-MM-DD HH:MI:SS|Não|Data que foi feita a sincronização entre o Revan e o SAP |
|descricao_erro|Text|-|-|-|Não|Descrição do erro na sincronização entre o Revan e o SAP|
|data_ultima_atualizacao|timestamp without time zone|-|-|YYYY-MM-DD HH:MI:SS|Sim|Data da ultma modificação do registro|
|status_posicao|Boolean|-|-|-|Não|Atributo utilizado pelo SAP, atributo terá o valor padrão FALSE na hora de inserir um novo registro.|
|sincronizado_painel|Boolean|-|-|-|Não|Atributo utilizado pelo SAP.|
|msg_erro_painel|Text|-|-|-|Não|Atributo utilizado pelo SAP.|
|"StatusB1if"|Boolean|-|-|-|Não|Atributo utilizado pelo SAP.|
|"Documents"|Text|-|-|-|Não|Atributo utilizado pelo SAP.|

**Observações Importantes:**
  * Atributos **sincronizado e data_sincronizado** São Atributos atualizado unicamente pelo SAP.
  * Atributo **descricao_erro** Atributo não é obrigatório na hora de inserir um novo registro mas sera obrigatório preencher no momento que o SAP identificar erro na sincronização.
  * Atributo **valor_renegociado** é para futuras implementações de renegociação de débito.
  * Atributo **estado** Identifica o estado da fatura no REVAN. Quando o estado for Paga significa que a Fatura está quitada.

### Items da Fatura

> Tabela: **revansap.t_fatura_item_fiscal**

**Parâmetros:**

|NOME|TIPO|MÍNIMO|MÁXIMO|FORMATAÇÃO|OBRIGATÓRIO|DESCRIÇÃO|
|----|:--:|:----:|:----:|:--------:|:---------:|---------|
|fatura_id|BigInt|-|-|-|Sim|Código da Fatura no esquema **revansap**|
|item_fiscal_id|BigInt|-|-|-|Sim|Código do Item fiscal no esquema **revansap**|
|valor_item|Numeric(10,2)|-|-|-|Sim|Valor a ser cobrado|
|operacao|Integer|-|-|-|Sim|Tipo de opereção (1: Débito - 2: Credito))|
|numero_brisanet|String|8|20|-|Não|Número telefone Brisanet contratado pelo cliente|
|informacoes_adicionais|String|3|255|-|Não|Informações adicionais do Item|
|sincronizado|Boolean|-|-|-|Sim|Identificação de sincronização entre o Revan e o SAP. O atributo terá o valor padrão FALSE na hora de inserir um novo registro.|
|data_sincronizado|timestamp without time zone|-|-|YYYY-MM-DD HH:MI:SS|Não|Data que foi feita a sincronização entre o Revan e o SAP |
|descricao_erro|Text|-|-|-|Não|Descrição do erro na sincronização entre o Revan e o SAP|
|processado|Boolean|-|-|-|Não|Atributo utilizado pelo SAP. O atributo terá o valor padrão FALSE na hora de inserir um novo registro.|


### Baixas da Fatura

> Tabela: **revansap.t_fatura_baixa**

**Parâmetros:**

|NOME|TIPO|MÍNIMO|MÁXIMO|FORMATAÇÃO|OBRIGATÓRIO|DESCRIÇÃO|
|----|:--:|:----:|:----:|:--------:|:---------:|---------|
|fatura_id|BigInt|-|-|-|Sim|Código da Fatura no esquema **revansap**|
|data_pagamento| date,|-|-|-|Sim|Data de pagamento|
|valor_pago| numeric,|-|-|-|Sim|Valor pago|
|situacao|Int|-|-|-|Sim|Situação da Baixa (1:paga 2: estornada) |
|data_hora_ultima_atualizacao| timestamp without time zone NOT NULL,|-|-|-|Sim|Data da última atualização do registro|
|forma_pagamento| integer NOT NULL,|-|-|-|Sim|Código da forma de pagamento utilizada pelo Revan|
|cancelada|Boolean|-|-|-|Sim|Identifica se a baixa foi cancelada|
|data_cancelamento|Timestamp without time zone|-|-|-|Não|Data de cancelamento da baixa|
|descricao_cancelamento|Text|-|-|-|Não|Descrição do motivo de cancelamento|
|sincronizado|Boolean|-|-|-|Sim|Identificação de sincronização entre o Revan e o SAP. O atributo terá o valor padrão FALSE na hora de inserir um novo registro.|
|data_sincronizado|timestamp without time zone|-|-|YYYY-MM-DD HH:MI:SS|Não|Data que foi feita a sincronização entre o Revan e o SAP|
|descricao_erro|Text|-|-|-|Não|Descrição do erro na sincronização de Revan e SAP|
|processado|Boolean|-|-|-|Não|Atributo utilizado pelo SAP. O atributo terá o valor padrão FALSE na hora de inserir um novo registro.|
|data_processado|timestamp without time zone|-|-|YYYY-MM-DD HH:MI:SS|Não|Data que foi Processado o registro.|
