# REST API

O Convertpack oferece uma REST API (experimental) que permite:

- Checkout
  - Consulta de produtos
  - Consulta de transações
  
O endpoint base é: `https://api.convertpack.io/v1/`

# Autenticação

Todas as consultas devem enviar a chave do usuário como forma de autenticação.

Modelo de chave de usuário: `userkey_secret_1A2B3C4D5E6F7G8H9IV3MVX0XY1GY5`

Nessa fase experimental é necessário solicitar ao [suporte do Convertpack](mailto:support@convertpack.io) sua chave de usuário para ter acesso a API.

A autenticação deve ser feita pelo método Bearer no cabeçalho da requisição.

```
curl "https://api.convertpack.io/v1/checkout/products/fetch_all" \
     -H 'Authorization: Bearer userkey_secret_1A2B3C4D5E6F7G8H9IV3MVX0XY1GY5' \
```

Requisições feitas sem a chave de usuário no cabeçalho irão falhar com erro 401.

# Checkout
## Produtos

O objeto `product`:

```json
{
   "id": "RMG3KWL",
   "name": "Mochila do Convertpack",
   "image": {
     "1x": {
       "width": 200,
       "height": 200,
       "url": "https://convertpack-user-data.s3.amazonaws.com/uploads/5ee12b8782f7ba09f728febf/20-07/RMG3KWL_1x.png"
     },
     "2x": {
       "width": 400,
       "height": 400,
       "url": "https://convertpack-user-data.s3.amazonaws.com/uploads/5ee12b8782f7ba09f728febf/20-07/RMG3KWL_2x.png"
     }
   },
   "price": {
     "currency": "BRL",
     "value": 29.9
   },
   "type": "physical",
   "created_at": "2020-10-01T12:09:45-03:00",
   "updated_at": "2020-10-02T00:33:37-03:00"
}
```

### Listar produtos

```shell script
GET https://api.convertpack.io/v1/checkout/products/fetch_all
```

Argumentos permitidos como parâmetros de URL:

**Paginação**
- `page`: número da página
  - Padrão: `1`
- `per_page`: número de resultados por página
  - Padrão: `15`
  - Máximo: `200`

Cada produto será um objeto na Array `products`.

### Visualizar produto

```
GET https://api.convertpack.io/v1/checkout/products/fetch/{id}
```

## Transações

O objeto `transaction`:

```json
{
  "id": "CPK-201234567890",
  "created_at": "2020-11-01T18:49:26-02:00",
  "updated_at": "2020-11-02T10:01:35-02:00",
  "customer": {
    "first_name": "João",
    "last_name": "Silva",
    "document": {
      "type": "CPF",
      "number": "12345678900"
    },
    "email": "joao@gmail.com",
    "phone": {
      "ddi": "55",
      "number": "991919191"
    }
  },
  "payment": {
    "status": "paid",
    "method": "credit_card",
    "currency": "BRL",
    "total_amount": 29.9,
    "net_amount": 27.45,
    "installments": 1,
    "paid_at": "2020-11-01T18:51:12-02:00"
  },
  "products": [
    {
      "id": "RMG3KWL",
      "name": "Mochila do Convertpack",
      "image": {
        "1x": {
          "width": 200,
          "height": 200,
          "url": "https://convertpack-user-data.s3.amazonaws.com/uploads/5ee12b8782f7ba09f728febf/20-07/RMG3KWL_1x.png"
        },
        "2x": {
          "width": 400,
          "height": 400,
          "url": "https://convertpack-user-data.s3.amazonaws.com/uploads/5ee12b8782f7ba09f728febf/20-07/RMG3KWL_2x.png"
        }
      },
      "price": {
        "currency": "BRL",
        "value": 29.9
      },
      "type": "physical",
      "created_at": "2020-10-01T12:09:45-03:00",
      "updated_at": "2020-10-02T00:33:37-03:00",
      "unities": "1",
      "is_order_bump": false,
      "discount": 0,
      "discounted": 0,
      "reference": null,
      "charged_amount": {
        "currency": "BRL",
        "amount": 29.9
      }
    }
  ],
  "shipping": {
    "amount": 0,
  },
  "gateway": {
    "account": "convertpack@convertpack.com",
    "name": "mercado_pago",
    "id": "12345678910111213",
    "fee": 2.44
  },
  "gateway_response": {
    "payment_id": "123456789",
    "status_detail": "accredited"
  }
}
```

### Listar transações

```
GET https://api.convertpack.io/v1/checkout/transactions/fetch_all
```

Argumentos permitidos como parâmetros de URL:

**Paginação**
- `page`: número da página
  - Padrão: `1`
- `per_page`: número de resultados por página
  - Padrão: `15`
  - Máximo: `200`

**Status**
- `status`: status das transações que devem ser retornadas
  - Padrão: todos
  - Permitidos: `paid` `waiting` `rejected` `abandoned` `refunded` `cancelled` `mediation` `chargeback`
  - Para trazer mais de um status na mesma requisição, use `+` entre cada status, ex: `status=rejected+abandoned`

**Período**
- `date_start`: data inicial da consulta
  - Obrigatório
  - Formato: `YYYY-MM-DD` (ex: `2020-10-15`)
- `date_end`: data final da consulta
  - Padrão: dia atual
  - Formato: `YYYY-MM-DD` (ex: `2020-10-20`)

**Data de pagamento**
- `paid_date`: considerar data de pagamento ao invés de criação da transação
  - Padrão: `false`
  - Se esse parâmetro for determinado como `true`, o parâmetro `status` deve obrigatóriamente ser `paid`

Por padrão as transações mais recentes são exibidas primeiro.

Cada transação será um objeto na Array `transactions`.

### Visualizar transação

```
GET https://api.convertpack.io/v1/checkout/transactions/fetch/{id}
```
