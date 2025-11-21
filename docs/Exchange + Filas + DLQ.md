| Conceito                    | O que é                                                      | Por que usamos                                         |
| --------------------------- | ------------------------------------------------------------ | ------------------------------------------------------ |
| **Exchange**                | Ponto de entrada das mensagens no RabbitMQ                   | Roteia mensagens para filas específicas                |
| **Fila (Queue)**            | Local onde mensagens ficam até serem consumidas              | Consumidor lê apenas da fila que precisa               |
| **DLQ (Dead Letter Queue)** | Fila especial que recebe mensagens que falharam ou expiraram | Permite análise de mensagens que não foram processadas |