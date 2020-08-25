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
