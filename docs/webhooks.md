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

- `event` (String) Evento que desencadeou o webhook
- `triggered_at` (String) Data em que o webhook foi enviado, formato ISO 8601
- `checkout` (Object) Dados do Checkout
  - `origin` (Object) Dados da origem da venda
    - `url` (String) URL onde a compra foi realizada
    - `params` (Object) Parâmetros presentes na URL de compra, se disponíveis, em formato `key`: `value` (exemplo: `"utm_source": "google"`)
- `customer` (Object) Dados do comprador
  - `first_name` (String) Primeiro nome
  - `last_name` (String) Último nome
  - `email` (String) E-mail
  - `document` (Object) Documento de identificação
    - `type` (String) Tipo do documento (exemplo: `CPF` ou `CNPJ`)
    - `number` (String) Número do documento, somente números
  - `phone` (Object) Telefone
    - `ddi` (String) DDI do telefone
    - `number` (String) Número do telefone, somente números
  - `address` (Object) Endereço do comprador, se disponível
    - `destination_format` (Object) Endereço no formato mais aceito no país de destino
      - `city` (String) Cidade
      - `complement` (String) Complemento do endereço
      - `country` (String) País
      - `country_code` (String) Código do país (alpha-2)
      - `district` (String) Bairro
      - `federal_unit` (String) UF/estado
      - `street_name` (String) Rua
      - `street_number` (String) Número
      - `zip_code` (String) CEP (somente números)
    - `international_format` (Object) Endereço no formato internacional
      - `city` (String) Cidade
      - `address_1` (String) Primeira linha do endereço
      - `address_2` (String) Segunda linha do endereço
      - `country_code` (String) Código do país (alpha-2)
      - `state` (String) Bairro
      - `zip_code` (String) CEP, somente números
- `transaction` (Object) Dados da transação
  - `created_at` (String) Data de criação da transação, formato ISO 8601
  - `updated_at` (String) Data da última atualização, formato ISO 8601
  - `id` (String) ID da transação (exemplo: `CPK-2112345678`)
  - `status` (String) Status do pagamento, abaixo todos status possíveis
  - `method` (String) Método de pagamento, abaixo todos status possíveis
  - `currency` (String) Moeda de pagamento (alpha-3, exemplo: `BRL`)
  - `products` (Array) Produtos comprados
    - []
      - `id` (String) ID do produto
      - `name` (String) Nome do produto
      - `quantity` (Int) Unidades compradas
      - `type` (string) Tipo do produto (`physical` ou `digital`)
      - `unit_price` (string) Preço unitário do produto (exemplo: `"25.00"`)
      - `sku` (string) SKU, se disponível
      - `is_upsell` (Bool) Se o produto foi comprado como Upsell
      - `is_order_bump` (Bool) Se o produto foi comprado como Order bump
  - `boleto` (Object) Dados do boleto, caso o `method` seja `boleto`
    - `url` (String) URL do boleto
    - `barcode` (String) Código de barras do boleto, somente números

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

[Vídeo com instruções](https://github.com/convertpack/docs/blob/master/assets/videos/integrations-webhooks-test.mp4?raw=true ":include :type=video width=640px height=308px")

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
