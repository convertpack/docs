# Webhooks

O Convertpack emite webhooks para todos os eventos do Checkout. São eles:

- Pagamento aprovado - `transaction_paid`
- Aguardando pagamento - `transaction_waiting`
- Pagamento abandonado - `transaction_abandoned`
- Pagamento reembolsado - `transaction_refunded`
- Pagamento recusado - `transaction_rejected`
- Pagamento cancelado - `transaction_cancelled`
- Pagamento em mediação - `transaction_mediation`
- Chargeback recebido - `transaction_chargeback`
- Código de rastreamento adicionado - `tracking_code_added`

Você pode conferir um exemplo de captura do Webhook do Convertpack [no repositório convertpack/webhooks no GitHub.](https://github.com/convertpack/webhooks/blob/master/php/checkout.php)

## Dados enviados

- `event` _(String)_ Evento que desencadeou o webhook
- `triggered_at` _(String)_ Data em que o webhook foi enviado, formato ISO 8601
- `checkout` _(Object)_ Dados do Checkout
  - `origin` _(Object)_ Dados da origem da venda
    - `url` _(String)_ URL onde a compra foi realizada
    - `params` _(Object)_ Parâmetros presentes na URL de compra, se disponíveis, em formato `key`: `value` (exemplo: `"utm_source": "google"`)
- `customer` _(Object)_ Dados do comprador
  - `first_name` _(String)_ Primeiro nome
  - `last_name` _(String)_ Último nome
  - `email` _(String)_ E-mail
  - `document` _(Object)_ Documento de identificação
    - `type` _(String)_ Tipo do documento (exemplo: `CPF` ou `CNPJ`)
    - `number` _(String)_ Número do documento, somente números
  - `phone` _(Object)_ Telefone
    - `ddi` _(String)_ DDI do telefone
    - `number` _(String)_ Número do telefone, somente números
  - `address` _(Object)_ Endereço do comprador, se disponível
    - `destination_format` _(Object)_ Endereço no formato mais aceito no país de destino
      - `city` _(String)_ Cidade
      - `complement` _(String)_ Complemento do endereço
      - `country` _(String)_ País
      - `country_code` _(String)_ Código do país (alpha-2)
      - `district` _(String)_ Bairro
      - `federal_unit` _(String)_ UF/estado
      - `street_name` _(String)_ Rua
      - `street_number` _(String)_ Número
      - `zip_code` _(String)_ CEP, somente números
    - `international_format` _(Object)_ Endereço no formato internacional
      - `city` _(String)_ Cidade
      - `address_1` _(String)_ Primeira linha do endereço
      - `address_2` _(String)_ Segunda linha do endereço
      - `country_code` _(String)_ Código do país (alpha-2)
      - `state` _(String)_ Bairro
      - `zip_code` _(String)_ CEP, somente números
- `transaction` _(Object)_ Dados da transação
  - `created_at` _(String)_ Data de criação da transação, formato ISO 8601
  - `updated_at` _(String)_ Data da última atualização, formato ISO 8601
  - `id` _(String)_ ID da transação (exemplo: `CPK-2112345678`)
  - `status` _(String)_ Status do pagamento, abaixo todos status possíveis
  - `method` _(String)_ Método de pagamento, abaixo todos status possíveis
  - `currency` _(String)_ Moeda de pagamento (alpha-3, exemplo: `BRL`)
  - `products` _(Array)_ Produtos comprados
    - []
      - `id` _(String)_ ID do produto
      - `name` _(String)_ Nome do produto
      - `quantity` _(Int)_ Unidades compradas
      - `type` _(string)_ Tipo do produto (`physical` ou `digital`)
      - `unit_price` _(string)_ Preço unitário do produto (exemplo: `"25.00"`)
      - `sku` _(string)_ SKU, se disponível
      - `is_upsell` _(Bool)_ Se o produto foi comprado como Upsell
      - `is_order_bump` _(Bool)_ Se o produto foi comprado como Order bump
  - `boleto` _(Object)_ Dados do boleto, caso o `method` seja `boleto`
    - `url` _(String)_ URL do boleto
    - `barcode` _(String)_ Código de barras do boleto, somente números

### Status de pagamento

Os status possíveis são:

- `paid`: Pagamento aprovado
- `waiting`: Aguardando pagamento
- `abandoned`: Pagamento abandonado
- `refunded`: Pagamento reembolsado
- `rejected`: Pagamento recusado
- `cancelled`: Pagamento cancelado
- `mediation`: Pagamento em mediação
- `chargeback`: Chargeback recebido

### Métodos de pagamento

Os métodos possíveis são:

- `credit_card`: Cartão de crédito
- `boleto`: Boleto
- `pix`: Pix

### Campos experimentais

Os campos abaixo estão em _alpha_ e podem mudar no futuro:

- `transaction.coupon`
- `transaction.gateway`
- `transaction.gateway_response`
- `transaction.shipping`
- `transaction.gateway_discount`

### JSON completo

Você pode enviar webhooks de teste diretamente para seu sistema pela interface do Convertpack.

[Vídeo com instruções](https://www.youtube.com/embed/IVAGXf26JBI ":include :type=iframe width=640px height=308px")

1. Vá para a seção [Integrações](https://app.convertpack.io/integrations)
2. Selecione a aba [Webhooks](https://app.convertpack.io/integrations/webhooks)
3. Clique no botão **Testar**
4. Na janela, selecione um evento
5. No campo URL, informe a URL que receberá o POST - você pode usar um serviço de testes como [webhook.site/](https://webhook.site/)
6. Clique no botão **Enviar**

#### Exemplo

Abaixo um exemplo do JSON enviado para o evento `transaction_paid`.

```json
{
  "event": "transaction_paid",
  "triggered_at": "2021-06-11T08:23:58-03:00",
  "checkout": {
    "origin": {
      "params": {},
      "url": null
    }
  },
  "customer": {
    "first_name": "Joaquim",
    "last_name": "Nabuco",
    "email": "joaquim@nabuco.com",
    "document": {
      "number": "24044827001",
      "type": "CPF"
    },
    "phone": {
      "ddi": "55",
      "number": "81994945566"
    },
    "address": {
      "destination_format": {
        "city": "Recife",
        "complement": "Apto 105",
        "country": "Brazil",
        "country_code": "BR",
        "district": "Boa Vista",
        "federal_unit": "PE",
        "street_name": "Rua da Imperatriz",
        "street_number": "147",
        "zip_code": "50060060"
      },
      "international_format": {
        "city": "Recife",
        "address_1": "Rua da Imperatriz, 147 - Boa Vista",
        "address_2": "Apto 105",
        "country": "Brazil",
        "country_code": "BR",
        "state": "PE",
        "zip_code": "50060060"
      }
    }
  },
  "transaction": {
    "created_at": "2021-06-11T08:23:58-03:00",
    "updated_at": "2021-06-11T08:23:58-03:00",
    "id": "CPK-191012345",
    "status": "paid",
    "method": "boleto",
    "currency": "BRL",
    "coupon": {},
    "gateway": {},
    "gateway_response": {},
    "products": [
      {
        "is_upsell": false,
        "is_order_bump": false,
        "quantity": 1,
        "name": "Ebook: Camões e os Lusíadas",
        "type": "physical",
        "id": "PR3C1849",
        "unit_price": "25.00",
        "sku": ""
      }
    ],
    "products_subtotal": "25.00",
    "volume_discount": "0.00",
    "shipping": {
      "amount": 0,
      "id": "br-correios",
      "name": "Frete grátis"
    },
    "gateway_discount": 0,
    "total_amount": "25.00"
  },
  "boleto": {
    "url": "https://www.mercadopago.com/mlb/payments/sandbox/ticket/helper?payment_id=123123123&payment_method_reference_id=123123123&caller_id=123123123&hash=123123-123123-123123-123-123123",
    "barcode": "23111101100000012345678110002222222000123456"
  }
}
```
