# Events (API)

O Convertpack oferece uma API de eventos Javascript, permitindo que você capture dados
a medida que os eventos acontecem ou execute suas próprias funções quando o usuário
realiza determinadas ações.

## Como usar

O primeiro passo é carregar a API de eventos em sua página. Cole o código abaixo no
fim da sua página, antes da tag `</body>`:

```html
<script src="https://client.convertpack.io/js/events.js"></script>
```

Agora que a API está carregada em sua página você deve sinalizar quais eventos deseja
acompanhar e o que deseja fazer quando o evento for acionado.

Para isso, usaremos a função `convertpack_events.on()`, que precisa de 3 argumentos:

- `tool`: _String:_ nome da ferramenta que executará o evento
- `event`: _String:_ nome do evento que você deseja acompanhar
- `callback`: _Function:_ função que será executada quando o evento for acionado

Essa função deve ser chamada **após** o código anteriormente carregado.

Abaixo alguns exemplos práticos de como o código deve ficar em sua página:

### Checkout

#### Usuário adicionou produto ao carrinho
```html
<script src="https://client.convertpack.io/js/events.js"></script>
<script>
    convertpack_events.on('checkout', 'add_to_cart', (data) => {
        console.log('Usuário adicionou produto ao carrinho.')
        console.table(data)
    })
</script>
```

#### Usuário iniciou formulário de pagamento
```html
<script src="https://client.convertpack.io/js/events.js"></script>
<script>
    convertpack_events.on('checkout', 'initiate_checkout', (data) => {
        console.log('Usuário iniciou a compra.')
        console.table(data)
    })
</script>
```

#### Usuário realizou uma compra
```html
<script src="https://client.convertpack.io/js/events.js"></script>
<script>
    convertpack_events.on('checkout', 'purchase', (data) => {
        console.log('Usuário finalizou a compra.')
        console.table(data)
    })
</script>
```

## Usos avançados frequentes

Além dos usos exemplificados acima, algumas funções são muito solicitadas.
Veja abaixo algumas delas.

### Disparar Facebook Pixel e Google Analytics

Você pode disparar o Facebook Pixel, Google Analytics ou qualquer outro _tracker_ 
pelo Convertpack Events.

!> Se você disparar Facebook Pixel e/ou Google Analytics pelo Convertpack Events, lembre-se de **remover** seu Pixel ID ou Google Analytics ID do painel do Convertpack, senão os dados de compra serão enviados **duas vezes** – uma pelo código do Convertpack Events e outra vez pelo Checkout.

Exemplo para compra realizada:

```html
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
<script src="https://client.convertpack.io/js/events.js"></script>
<script>
    convertpack_events.on('checkout', 'purchase', (data) => {
        
        // Valor total da compra
        let purchase_amount = data.transaction.total_amount;

        // Se o método de pagamento for boleto,
        // envie para Facebook e Google Analytics somente 40% do valor total
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

Por enquanto somente a ferramenta **Checkout** dispara eventos. São eles:

- `add_to_cart`: usuário adicionou um ou mais produtos ao carrinho
- `initiate_checkout`: usuário iniciou o checkout
- `purchase`: usuário finalizou a compra (status "pago" ou "aguardando")
