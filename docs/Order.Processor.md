# Order.Processor

## 🎯 Finalidade
O `Order.Processor` é um Worker Service responsável por processar ordens recebidas da Order.API.

Etapas principais:
- Ler mensagens da fila `orders.created`
- Validar regra de negócio (limites, tipo de ordem, saldo simulado)
- Simular execução da ordem (compra/venda)
- Publicar resultado em `orders.executed` ou `orders.rejected`

## 🧱 Por que é um Worker Service?
- Precisa trabalhar 24/7 consumindo fila
- Não tem endpoints HTTP
- Escala facilmente com múltiplas instâncias
- Melhor performance que uma API tradicional
- Evita bloquear chamadas do cliente

## 📡 Comunicação
- Consome: `orders.created`
- Publica: 
  - `orders.executed`
  - `orders.rejected`

## 🔧 Justificativas técnicas
- BackgroundService é o padrão recomendado para consumidores RabbitMQ
- Workers são independentes da camada web
- Permite aplicar retry, DLQ (Dead Letter Queue), backoff
- Pode ser reiniciado pelo Docker automaticamente em falhas

## 🧠 Notas para o eu do futuro
- Criar uma strategy para simulação de execução de ordens.
- Considere idempotência caso mensagens duplicadas apareçam.
- Logs devem sempre incluir o OrderId.
- Evitar escrever direto em banco se possível — separar por responsabilidade.
