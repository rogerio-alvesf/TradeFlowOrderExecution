# Order.API

## 🎯 Finalidade
A `Order.API` é o ponto de entrada do sistema TradeFlowOrderExecution.  
Ela recebe solicitações para criar ordens de compra e venda e publica essas ordens no RabbitMQ para processamento assíncrono.

Essa API **não processa ordens** — ela apenas valida e envia o comando de criação.

## 🧱 Por que foi estruturada dessa forma?
- **Separação de responsabilidades**  
  Toda a lógica pesada e demorada acontece em serviços internos (Order.Processor).  
  A API continua leve, rápida e escalável.

- **Evita travar o cliente**  
  Distribuir o processamento evita filas longas no atendimento HTTP.

- **Arquitetura orientada a eventos**  
  APIs são usadas para *comandos*, não para processamento.

- **Melhor para escalar**  
  Em momentos de alta demanda (como abertura do mercado), escalar apenas a API é suficiente.

## 📡 Comunicação
- Publica mensagens na fila: `orders.created`
- Não consome nenhuma fila

## 🔧 Justificativas técnicas
- .NET 9 (Minimal API) para leveza e simplicidade
- Validação via FluentValidation (opcional)
- DTOs limpos e contratos estáveis
- Publicação via RabbitMQ com conexão resiliente

## 🧠 Notas para o eu do futuro
- A API nunca deve aplicar regras complexas aqui.  
- Futuramente, adicionar autenticação JWT / API Key.  
- Logging estruturado ajuda muito na auditoria.  
- Documentar contratos no Swagger.
