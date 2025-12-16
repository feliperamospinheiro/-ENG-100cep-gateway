# Catálogo de Dados — 100cep Gateway
---

# 🥇 Gold

| Coluna | Tipo | Descrição | Description |
| ------ | ---- | --------- | ----------- |
| chargeback_id | string | Identificador único para cada pedido de chargeback, composto por 13 caracteres alfanuméricos em minúsculas. | Unique identifier for each chargeback request, consisting of 13 alphanumeric characters in lowercase. |
| motivo_chargeback | string | Motivo por trás da solicitação de chargeback, que pode incluir fraudes, produtos não recebidos, entre outros. | Reason for the chargeback request, which may include fraud, non-receipt of products, among other causes. |
| status_chargeback | string | Status atual da solicitação de chargeback, indicando seu estágio de progresso.  | Current status of the chargeback request, indicating its progress stage.|
| resposta_emitente | string | Resposta fornecida pelo emissor do cartão em relação à solicitação de chargeback. | Response provided by the card issuer regarding the chargeback request. |
| resposta_adquirente | string | Resposta fornecida pelo adquirente em relação à solicitação de chargeback. | Response provided by the acquirer regarding the chargeback request. |

### dim_clientes
| Coluna | Tipo | Descrição | Description |
| ------ | ---- | --------- | ----------- |
| cliente_id | string | Identificador único para cada cliente, composto por 13 caracteres alfanuméricos em minúsculas. | Unique identifier for each client, consisting of 13 alphanumeric characters in lowercase. |
| cep_prefixo | string | Os primeiros 5 dígitos do CEP do cliente. | The first 5 digits of the client postal code. |


### dim_data
| Coluna | Tipo | Descrição | Description |
| ------ | ---- | --------- | ----------- |
| data_calendario | date | Data em que o pedido foi realizado. Formato: AAAA-MM-DD.| Date when the order was placed. Format: YYYY-MM-DD. |
| dia | int | Número do dia correspondente à data no formato inteiro. | Day number corresponding to the date in integer format. |
| mes | int | Número do mês correspondente à data. | Month number corresponding to the date. |
| ano | int | Número do ano correspondente à data. | Year number corresponding to the date. |
| nome_dia_semana | string | Nome do dia da semana correspondente à data. | Weekday name corresponding to the date. |
| nome_mes | string | Nome do mês correspondente à data. | Month name corresponding to the date. |

### dim_geolocalizacao
| Coluna | Tipo | Descrição | Description |
| ------ | ---- | --------- | ----------- |
| cep_prefixo | string | Os primeiros 5 dígitos do CEP. | The first 5 digits of the ZIP code. |
| cidade | string | Nome da cidade brasileira associada ao CEP. | City name associated with the ZIP code. |
| estado | string | Sigla do estado brasileiro (duas letras maiúsculas) associada ao CEP. | Brazilian state abbreviation (two uppercase letters) associated with the ZIP code. |
| latitude | string | Coordenada geográfica que especifica a posição norte-sul. | Geographic coordinate specifying the north-south position. |
| longitude | string | Coordenada geográfica que especifica a posição leste-oeste. | Geographic coordinate specifying the east-west position. |

### dim_vendedores
| Coluna | Tipo | Descrição | Description |
| ------ | ---- | --------- | ----------- |
| vendedor_id | string | Identificador único para cada vendedor, composto por 13 caracteres alfanuméricos em minúsculas. | Unique identifier for each seller, consisting of 13 alphanumeric characters in lowercase. |
| cep_prefixo | string | Os primeiros 5 dígitos do CEP do vendedor. | The first 5 digits of the seller's postal code. |

### fato_transacoes
| Coluna | Tipo | Descrição | Description |
| ------ | ---- | --------- | ----------- |
| pedido_id | string | Identificador único para cada transação, composto por 13 caracteres alfanuméricos em minúsculas. | Unique identifier for each transaction, consisting of 13 alphanumeric characters in lowercase. |
| cliente_id | string | Identificador único para cada cliente, composto por 13 caracteres alfanuméricos em minúsculas. | Unique identifier for each client, consisting of 13 alphanumeric characters in lowercase. |
| vendedor_id | string | Identificador único para cada vendedor, composto por 13 caracteres alfanuméricos em minúsculas. | Unique identifier for each seller, consisting of 13 alphanumeric characters in lowercase. |
| data_pedido | date | Data em que o pedido foi realizado. Formato: AAAA-MM-DD. | Date when the order was placed. Format: YYYY-MM-DD.|
| horario_pedido | date | Horário em que o pedido foi realizado no formato HH:MM:SS.| Time when the order was placed in the format HH:MM:SS. |
| valor_transacao | decimal(12,2) | Valor da transação em formato decimal com até 12 dígitos e 2 casas decimais, contendo apenas valores positivos. | Transaction value in a decimal format with up to 12 digits and 2 decimal places, only positive values. |
| preco_total | decimal(16,2) | Preço total do pedido, incluindo o preço do produto e o custo de envio. | Total order price, including product price and shipping cost. |
| frete_total | decimal(15,2) | Custo total do frete em formato decimal com até 15 dígitos e 2 casas decimais, contendo apenas valores positivos. | Total shipping cost in a decimal format with up to 15 digits and 2 decimal places, only containing positive values. |
| status_pedido | string | Status atual do pedido. | Current status of the order. |
---

# Linhagem de Dados
- Kaggle → Bronze → Silver → Gold  
- Transformações documentadas em `/docs/etl_documentation.md`.  