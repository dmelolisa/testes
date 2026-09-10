# API Consulta Pedidos WMS

## Sumário

1. Objetivo
2. Endpoint
3. Parâmetros
4. Exemplo de requisição
5. Retorno
6. Estrutura da resposta
7. Status dos pedidos
8. Exemplo de consumo
9. Detalhamento das variáveis

## 1. Objetivo

A API Consulta Pedidos Wms permite consultar detalhes dos pedidos registrados no sistema do cliente.

O retorno apresenta uma lista com as informações de cada pedido:

- Número do pedido cliente;
- Pedido;
- Nota;
- Status;
- Campanha;
- Transportadora;
- CNPJ da transportadora;
- Data/Hora de integração;
- Data/Hora do pedido;
- Data do Romaneio;
- Data/Hora de finalização do pedido.

## 2. Endpoint

### Consulta de pedidos por status

**Método:** `GET`

**URL:**

```text
{{host}}/rest/wms/v2/pedidoWms/query
```

**Autenticação:**

Tipo Basic

```text
Authorization: Basic {{ senhaAutenticacao }}
```

**Header:**

```text
tenantId: {{empresa}}, {{filial}}
```

## 3. Parâmetros

| Parâmetro | Obrigatório | Exemplo | Descrição |
|---|---|---|---|
| `senha` | Sim | `XXXXXXXX` | Senha utilizada na requisição |
| `chave` | Sim | `XXXXXXXX` | Chave utilizada na requisição |
| `dataDe` | Não | `20260812` | Data utilizada para consulta dos pedidos, no formato AAAAMMDD, será a data inicial para uma busca por período. |
| `dataAte` | Não | `20260812` | Data utilizada para consulta dos pedidos, no formato AAAAMMDD, será a data final para uma busca por período. |
| `status` | Não | `1` | Código do status para filtro dos pedidos |
| `pedido` | Não | `3254698` | Número do pedido. Utilizar quando necessitar retornar a informação de apenas um pedido em específico. |
| `page` | Sim | `1` | Número da página |
| `pageSize` | Sim | `1000` | Quantidade de registros por página. A quantidade máxima possível por página é de 3000 registros. |

### Formato de dataDe e dataAte

O parâmetro deve ser informado no formato:

```text
AAAAMMDD
```

Exemplo:

```text
20260812
```

Representa:

```text
12/08/2026
```

### Status dos pedidos

| Código | Descrição |
|---|---|
| `1` | A SEPARAR |
| `3` | A CONFERIR |
| `4` | CONFERIDO |
| `5` | A EXPEDIR |
| `6` | FINALIZADO |
| `7` | CANCELADO |

## 4. Exemplo de requisição

```http
GET {{host}}/rest/wms/v2/pedidosPorStatus/query?senha={{senha}}&chave={{chave}}&dataDe=20260801&dataAte=20260821&status=&pedido=&page=3&pageSize=3000
```

## 5. Retorno

A API retorna uma lista de objetos JSON contendo os dados de cada pedido e o campo `hasNext`, que identifica se há mais registros (páginas) para listagem.

### Exemplo

```json
{
  "items": [
    {
      "pedidoCliente": "4074405V3",
      "pedido": "U06538",
      "nota": "",
      "status": "1",
      "campanha": "YBERA.COM",
      "transportadora": "L4B LOGISTICA LTDA.",
      "cnpjTransportadora": "",
      "dataHoraIntegracao": "",
      "dataHoraPedido": "20260804 11:17",
      "dataRomaneio": "",
      "dataHoraFinalizado": ""
    },
    {
      "pedidoCliente": "4074406V3",
      "pedido": "U06539",
      "nota": "",
      "status": "1",
      "campanha": "YBERA.COM",
      "transportadora": "TEX COURIER S.A",
      "cnpjTransportadora": "",
      "dataHoraIntegracao": "",
      "dataHoraPedido": "20260804 11:17",
      "dataRomaneio": "",
      "dataHoraFinalizado": ""
    }
  ],
  "hasNext": true
}
```

## 6. Estrutura da resposta

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| `items` | Array | Sim | Lista de pedidos retornados pela consulta. Cada elemento representa um pedido. |
| `items.pedidoCliente` | String | Sim | Número ou identificador do pedido informado pelo cliente. |
| `items.pedido` | String | Sim | Número do pedido no sistema. |
| `items.nota` | String | Não | Número da nota fiscal vinculada ao pedido. Retornado vazio quando ainda não houver nota fiscal. |
| `items.status` | String | Sim | Status atual do pedido no sistema. |
| `items.campanha` | String | Não | Identificação da campanha associada ao pedido. |
| `items.transportadora` | String | Não | Nome da transportadora responsável pelo pedido. |
| `items.cnpjTransportadora` | String | Não | CNPJ da transportadora responsável pelo pedido. |
| `items.dataHoraIntegracao` | String | Não | Data e hora em que o pedido foi integrado ao sistema. |
| `items.dataHoraPedido` | String | Sim | Data e hora de criação do pedido. Formato: `YYYYMMDD HH:mm`. |
| `items.dataRomaneio` | String | Não | Data de geração do romaneio do pedido. Retornado vazio quando ainda não houver romaneio. |
| `items.dataHoraFinalizado` | String | Não | Data e hora em que o pedido foi finalizado. Retornado vazio quando o pedido ainda não estiver finalizado. |
| `hasNext` | Boolean | Sim | Indica se existem mais registros disponíveis para consulta/paginação. `true` = existem mais registros; `false` = não existem mais registros. |

## 7. Status dos pedidos

| Código | Descrição |
|---|---|
| `1` | A SEPARAR |
| `3` | A CONFERIR |
| `4` | CONFERIDO |
| `5` | A EXPEDIR |
| `6` | FINALIZADO |
| `7` | CANCELADO |

## 8. Exemplo de consumo

### Requisição

```http
GET {{host}}/rest/wms/v2/pedidoWms/query?senha={{senha}}&chave={{chave}}&dataDe=20260801&dataAte=20260821&status=&pedido=&page=1&pageSize=2
```

### Resposta

```json
{
  "items": [
    {
      "pedidoCliente": "4074405V3",
      "pedido": "U06538",
      "nota": "",
      "status": "1",
      "campanha": "XPTO",
      "transportadora": " LOGISTICA LTDA.",
      "cnpjTransportadora": "",
      "dataHoraIntegracao": "",
      "dataHoraPedido": "20260804 11:17",
      "dataRomaneio": "",
      "dataHoraFinalizado": ""
    },
    {
      "pedidoCliente": "4074406V3",
      "pedido": "U06539",
      "nota": "",
      "status": "1",
      "campanha": "XPTO2",
      "transportadora": "TEX COURIER S.A",
      "cnpjTransportadora": "",
      "dataHoraIntegracao": "",
      "dataHoraPedido": "20260804 11:17",
      "dataRomaneio": "",
      "dataHoraFinalizado": ""
    }
  ],
  "hasNext": true
}
```

## 9. Detalhamento das variáveis

| Variável | Descrição |
|---|---|
| `{{empresa}}` | Código da empresa (utilizar 03) |
| `{{filial}}` | Código da filial Lisalog |
| `{{host}}` | Endereço host do servidor |
| `{{senha}}` | Senha de acesso ao endpoint |
| `{{chave}}` | Chave de acesso ao endpoint |
| `{{senhaAutenticacao}}` | Senha de autenticação da API |
