# Checkout

**Esse manual se encontra DEPRECIADO.** A partir de 20 de junho de 2021 as opções descritas não estarão disponíveis.

## Paleta de cores

Por meio de [variáveis CSS](https://developer.mozilla.org/pt-BR/docs/Web/CSS/Using_CSS_custom_properties) você pode personalizar praticamente todas as cores do Checkout.

Essa solução está disponível nos navegadores modernos e é suportada por mais de [97% dos dispositivos no Brasil.](https://caniuse.com/#feat=css-variables) Para usuários usando navegadores antigos serão exibidas as cores padrões.

?> Recomendamos que você leia sobre variáveis CSS antes de continuar: [Utilizando variáveis CSS](https://developer.mozilla.org/pt-BR/docs/Web/CSS/Using_CSS_custom_properties)

Por padrão as variáveis são aplicadas em `:root`, então recomendamos que você aplique suas variáveis no elemento `.convertpack_checkout` para que sobrescrevam os valores padrões.

Abaixo a lista completa de variáveis disponíveis.

```html
<style>
  .convertpack_checkout {
    /* General */
    --cpk_ch_font_family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue",
      Arial, "Noto Sans", sans-serif;
    --cpk_ch_color: #313131;
    --cpk_ch_general_border_radius: 4px;

    /* Inputs */
    --cpk_ch_input_border_radius: 3px;
    --cpk_ch_input_label_color: #555;

    /* Brand primary color */
    --cpk_ch_primary: #256eff;
    --cpk_ch_primary_low_opacity: rgba(37, 110, 255, 0.3);
    --cpk_ch_primary_inverted: #fff;

    /* Semantic colors */
    --cpk_ch_positive: #1bbb5b;
    --cpk_ch_positive_inverted: #fff;
    --cpk_ch_positive_low_opacity: rgba(27, 187, 91, 0.5);
    --cpk_ch_negative: #ff333e;
    --cpk_ch_negative_inverted: #fff;
    --cpk_ch_negative_low_opacity: rgba(250, 0, 0, 0.1);
    --cpk_ch_alert: #f5a227;
    --cpk_ch_alert_inverted: #421709;

    /* Buttons */
    --cpk_ch_button_border_radius: 5px;

    /* Primary color button */
    --cpk_ch_primary_button_bg: var(--cpk_ch_primary);
    --cpk_ch_primary_button_bg_hover: #377aff;
    --cpk_ch_primary_button_color: var(--cpk_ch_primary_inverted);
    --cpk_ch_primary_button_color_hover: var(--cpk_ch_primary_inverted);
    --cpk_ch_primary_button_box_shadow: none;
    --cpk_ch_primary_button_box_shadow_hover: none;
    --cpk_ch_primary_button_box_shadow_focus: 0 0 0 3px rgba(37, 110, 255, 0.3);

    /* Submit button ("Purchase") */
    --cpk_ch_submit_button_bg: var(--cpk_ch_positive);
    --cpk_ch_submit_button_bg_hover: #1dcb63;
    --cpk_ch_submit_button_color: var(--cpk_ch_positive_inverted);
    --cpk_ch_submit_button_color_hover: var(--cpk_ch_positive_inverted);
    --cpk_ch_submit_button_box_shadow: inset 0 1px 0 0 #1bbb5b, inset 0 2px 0 0
        rgba(255, 255, 255, 0.4), inset 0 -2px 0 0 #18a550, 0 10px 7px -7px rgba(27, 187, 91, 0.2);
    --cpk_ch_submit_button_box_shadow_hover: inset 0 1px 0 0 #1bbb5b, inset 0
        2px 0 0 rgba(255, 255, 255, 0.4), inset 0 -2px 0 0 #18a550, 0 10px
        7px -7px rgba(27, 187, 91, 0.2), 0 0 0 3px rgba(27, 187, 91, 0.3);
    --cpk_ch_submit_button_box_shadow_focus: inset 0 1px 0 0 #1bbb5b, inset 0
        2px 0 0 rgba(255, 255, 255, 0.4), inset 0 -2px 0 0 #18a550, 0 10px
        7px -7px rgba(27, 187, 91, 0.2), 0 0 0 3px rgba(27, 187, 91, 0.3);

    /* Form and layout blocks */
    --cpk_ch_main_block_box_shadow_mobile: 0 2px 3px 0 rgba(0, 0, 0, 0.05);
    --cpk_ch_main_block_box_shadow_desktop: 0 5px 15px -5px rgba(0, 0, 0, 0.1);

    /* Form title */
    --cpk_ch_form_title_color: var(--cpk_ch_color);
    --cpk_ch_form_title_weight: bold;
    --cpk_ch_form_title_transform_mobile: uppercase;
    --cpk_ch_form_title_transform_desktop: none;

    /* Step number on form title */
    --cpk_ch_form_step_display: inline-flex;
    --cpk_ch_form_step_bg: #f4f4f4;
    --cpk_ch_form_step_color: #999;
    --cpk_ch_form_step_box_shadow: none;
    --cpk_ch_form_step_border_radius: var(--cpk_ch_general_border_radius);

    /* Tooltip */
    --cpk_ch_tooltip_bg: var(--cpk_ch_primary);
    --cpk_ch_tooltip_color: var(--cpk_ch_primary_inverted);

    /* Order bump */
    --cpk_ch_order_bump_title_bg: #f4f4f4;
    --cpk_ch_order_bump_title_color: #828282;
    --cpk_ch_order_bump_border_color: #ddd;
    --cpk_ch_order_bump_current_price_color: var(--cpk_ch_positive);
    --cpk_ch_order_bump_old_price_color: var(--cpk_ch_negative);
    --cpk_ch_order_bump_border_radius: var(--cpk_ch_general_border_radius);
    --cpk_ch_order_bump_tooltip_bg: var(--cpk_ch_primary);
    --cpk_ch_order_bump_tooltip_color: var(--cpk_ch_primary_inverted);

    /* Trust bar ("Safe purchase") */
    --cpk_ch_trust_bar_bg: var(--cpk_ch_positive);
    --cpk_ch_trust_bar_color: var(--cpk_ch_positive_inverted);

    /* Countdown */
    --cpk_ch_countdown_bg: rgba(0, 0, 0, 0.95);
    --cpk_ch_countdown_color: #fff;
    --cpk_ch_countdown_progress_bg: #fff;
    --cpk_ch_countdown_urgency_bg: rgba(224, 13, 13, 0.95);
    --cpk_ch_countdown_urgency_color: #fff;
    --cpk_ch_countdown_urgency_progress_bg: #fff;
  }
</style>
```

!> A ausência de declaração de alguma variável no seu código implica no uso do valor padrão enviado pelo Convertpack. Certifique-se de declarar todas as variáveis necessárias para não mostrar cores sem sentido para seus usuários. Por exemplo, ao alterar a `--cpk_ch_submit_button_bg` certifique-se de também declarar as outras variáveis relativas ao `submit_button`.

## Opções

Por meio do objeto `window.CONVERTPACK_CHECKOUT_OPTIONS` você pode alterar ou adicionar informações ao Checkout.

Veja abaixo as variáveis disponíveis:

```html
<script>
  window.CONVERTPACK_CHECKOUT_OPTIONS = {
    mode: "inline",

    element_id: "convertpack_checkout_inline",
    products: ["ABCDEFG:1"],

    header: {
      title: "Finalizar pedido",
      logo: "https://loja.com.br/logotipo.png",
      banner: "https://loja.com.br/destaque.png",
    },

    order_bump: {
      title: "Aproveite e compre esses produtos!",
    },

    customer: {
      name: "John Doe",
      name_input_disabled: true,
      email: "johndoe@gmail.com",
      email_input_disabled: true,
      phone: "11999999999",
      phone_input_disabled: false,
      doc: "73700165013",
      doc_input_disabled: false,

      address: {
        country: "Brazil",
        country_code: "BR",
        zip: "11222333",
        address_1: "Rua Abacaxi",
        address_2: "",
        address_number: "123",
        neighborhood: "Centro",
        city: "São Paulo",
        state: "SP",
      },
    },
  };
</script>
```

- `mode`: Altera o modo de abertura do Checkout. Pode ser `modal` (padrão, abre o Checkout em janela modal) ou `inline` (Checkout aberto automaticamente na página).

- `element_id`: Somente para `mode: inline`. ID do elemento onde o Checkout será aberto. Por padrão buscamos o elemento com ID `convertpack_checkout_inline`.
- `products`: Somente para `mode: inline`. _Array_ com IDs e quantidades dos produtos que deverão ser automaticamente colocados no Checkout. Formato de cada item da Array: `['{product_id}:{quantity}']`. Se o usuário clicar em algum link de compra na sua página os produtos serão substituídos.

- `header`:
  - `title`: Somente para `mode: modal`. Altera o título do cabeçalho. Padrão: "Finalizar pedido".
  - `logo`: Somente para `mode: modal`. Mostra o logotipo da sua loja no topo do Checkout. A imagem será exibida com altura de 25px e largura automática, dessa forma a imagem deve ter **pelo menos** 25px de altura e até 200px de largura. O tamanho **recomendado** é: até 400px de largura e exatamente 50px de altura, pensando em dispositivos com tela Retina.
  - `banner`: Somente para `mode: modal`.
- `order_bump`: Opções para a seção Order bump
  - `title`: Altera o título da seção de Order bump. Padrão: "COMPRE JUNTO!"
- `customer`: Define dados padrões para o cliente
  - `name`: Preenche o nome do comprador
  - `name_input_disabled`: _Boolean._ Se `true` bloqueia a edição do nome do comprador
  - `email`: Preenche o e-mail do comprador
  - `email_input_disabled`: _Boolean._ Se `true` bloqueia a edição do e-mail do comprador
  - `doc`: Preenche o documento do comprador
  - `doc_input_disabled`: _Boolean._ Se `true` bloqueia a edição do documento do comprador
  - `address`: Preenche o endereço do comprador
    - `zip`: _String._ Código postal, CEP. String, mas somente números.
    - `address_1`: Primeira linha do endereço.
    - `address_2`: Segunda linha do endereço.
    - `address_number`: Número do endereço.
    - `neighborhood`: Bairro.
    - `city`: Cidade.
    - `state`: Estado.

Os dados declarados são validados internamente antes de serem exibidos ao usuário, evitando que informações quebrados sejam exibidas e atrapalhem o fluxo de compra.
