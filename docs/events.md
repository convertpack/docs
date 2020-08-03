# Events (API)

O Convertpack oferece uma API de eventos Javascript, permitindo que você capture dados
a medida que os eventos aconteçam ou execute suas próprias funções quando o usuário
realizar determinadas ações.

## Como usar

O primeiro passo é carregar a API de eventos em sua página. Cole o código abaixo no
fim da sua página, antes da tag `</body>`:

```html
<script src="https://client.convertpack.io/js/events.js"></script>
```

Agora que a API está carregada em sua página você deve sinalizar quais eventos deseja
acompanhar e o que deseja fazer quando o evento for acionado.

Para isso, usaremos a função `convertpack_events.on()`, que precisa de 3 argumentos:

- `tool` _String:_ nome da ferramenta que executará o evento
- `event` _String:_ nome do evento que você deseja acompanhar
- `callback` _Function:_ função que será executada quando o evento for acionado

Por enquanto somente a ferramenta **Checkout** dispara eventos. Veja abaixo exemplos
reais de uso.


### Checkout

Abaixo alguns exemplos práticos de como o código deve ficar em sua página.

**Usuário adicionou produto ao carrinho** 

```html
<script src="https://client.convertpack.io/js/events.js"></script>
<script>
    convertpack_events.on('checkout', 'add_to_cart', (data) => {
        console.log('Usuário adicionou produto ao carrinho.')
        console.table(data)
    })
</script>
```

**Usuário iniciou formulário de pagamento**

```html
<script src="https://client.convertpack.io/js/events.js"></script>
<script>
    convertpack_events.on('checkout', 'initiate_checkout', (data) => {
        console.log('Usuário iniciou a compra.')
        console.table(data)
    })
</script>
```

**Usuário realizou uma compra**

```html
<script src="https://client.convertpack.io/js/events.js"></script>
<script>
    convertpack_events.on('checkout', 'purchase', (data) => {
        console.log('Usuário finalizou a compra.')
        console.table(data)
    })
</script>
```

### Usos avançados frequentes

Além dos usos exemplificados acima, algumas funções são muito solicitadas.
Veja abaixo algumas delas.

### Disparar Facebook Pixel e Google Analytics

Você pode disparar o Facebook Pixel, Google Analytics ou qualquer outro _tracker_ 
pelo Convertpack Events.

!> Se você disparar Facebook Pixel e/ou Google Analytics pelo Convertpack Events, lembre-se de **remover** seu Pixel ID ou Google Analytics ID do painel do Convertpack, senão os dados de compra serão enviados **duas vezes** – uma pelo código do Convertpack Events e outra vez pelo Checkout.

Exemplo para compra realizada:

```html
<!--
   Coloque aqui seu código de integração
   do Facebook Pixel e Google Analytics
-->
<script src="https://client.convertpack.io/js/events.js"></script>
<script>
    convertpack_events.on('checkout', 'purchase', (data) => {
        
        // Valor total da compra
        let purchase_amount = data.transaction.total_amount;

        // Facebook Pixel
        fbq("track", "Purchase", {
            value: purchase_amount,
            currency: data.transaction.currency,
            content_type: "product"
        });

        // Google Analytics
        gtag("event", "purchase", {
            transaction_id: data.transaction.id,
            value: purchase_amount,
            currency: data.transaction.currency,
            items: data.transaction.products,
            shipping: data.shipping.amount
        });

    })
</script>
```


### Enviar preço diferente se método de pagamento for boleto

Novamente, lembre-se de **remover** os dados do Facebook Pixel e Google Analytics 
no painel do Convertpack, senão estaremos enviando os dados duas vezes.

```html
<!--
   Coloque aqui seu código de integração
   do Facebook Pixel e Google Analytics
-->
<script src="https://client.convertpack.io/js/events.js"></script>
<script>
    convertpack_events.on('checkout', 'purchase', (data) => {
        
        // Valor total da compra
        let purchase_amount = data.transaction.total_amount;

        // Se o método de pagamento for boleto,
        // envia para Facebook e Google Analytics somente 40% do valor total
        if (data.transaction.method === "boleto") {
            purchase_amount = data.transaction.total_amount * 0.4;
        }

        // Facebook Pixel
        fbq("track", "Purchase", {
            value: purchase_amount,
            currency: data.transaction.currency,
            content_type: "product"
        });

        // Google Analytics
        gtag("event", "purchase", {
            transaction_id: data.transaction.id,
            value: purchase_amount,
            currency: data.transaction.currency,
            items: data.transaction.products,
            shipping: data.shipping.amount
        });

    })
</script>
```

## Ferramentas e eventos disponíveis

### Checkout

- `add_to_cart`
- `initiate_checkout`
- `purchase`

Todos os eventos retornam dados (`data`), que incluem:

- `tool` _String:_ ferramenta que disparou o evento ("checkout")
- `event` _String:_ evento disparado ("add_to_cart")
- `triggered_at` _Date:_ data que o evento foi disparado

Além disso, cada evento também fornece dados adicionais sobre si. Vej abaixo.

#### add_to_cart

Disparado quando o usuário adiciona um ou mais produtos ao carrinho.

Retorna dados adicionais:

- `cart` _Array:_ produtos no carrinho. Cada item da _Array_ é um objeto

```javascript
cart: [
    {
        short_id: "1YAJp53",
        quantity: "1"
    }
]
```

#### initiate_checkout

Disparado quando o usuário inicia o formulário de pagamento.

Retorna dados adicionais:

- `cart` _Array:_ produtos no carrinho. Cada item da _Array_ é um objeto

```javascript
cart: [
    {
        short_id: "1YAJp53",
        quantity: "1"
    }
]
```

#### `purchase`

Disparado quando o usuário finaliza a compra e o status é `paid` (pago) ou
`waiting` (aguardando).

Retorna dados adicionais:

- `transaction` _Object:_ dados da transação

```javascript
transaction: {
    id: "CPK-12345678",
    status: "paid",
    method: "credit_card",
    total_amount: 10.00,
    currency: "BRL",
    products: [
        {
            name: "Mochila do Convertpack",
            quantity: 1,
            id: "1YAJp53",
            price: 10.00
        }
    ],
    gateway: {
        name: "pagarme"
    },
    shipping: {
        amount: 0
    }
}
```

Se o método de pagamento for **boleto**, também retornará:

- `boleto` _Object:_ dados do boleto bancários gerado

```javascript
boleto: {
    barcode: "000000000000000000000000000000000",
    date_of_expiration: "2020-08-01 10:00:00",
    url: "https://boleto.com.br/123..."
}
```
