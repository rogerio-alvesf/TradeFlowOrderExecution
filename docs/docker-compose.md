# Docker Compose – TradeFlowOrderExecution

Este documento explica a finalidade e o funcionamento do arquivo `docker-compose.yml` utilizado no projeto **TradeFlowOrderExecution**.

O objetivo do docker-compose é **subir o RabbitMQ** de forma simples, estável e totalmente configurada para o ambiente de desenvolvimento, permitindo que os serviços .NET (Order.API, Order.Processor e Portfolio.Service) possam se comunicar através de mensageria.

---

# 🎯 Objetivos do docker-compose

- Subir o **RabbitMQ** com:
  - management UI habilitada (localhost:15672)
  - criação automática de usuário e senha
  - volume persistente para as mensagens
- Fornecer uma infraestrutura mínima para:
  - publicação e consumo de eventos
  - testes independentes de mensageria
  - isolamento do ambiente de desenvolvimento
- Garantir que todas as mensagens publicadas pelos módulos sejam roteadas corretamente.

---

# 🧱 Estrutura geral do arquivo

O `docker-compose.yml` contém somente **um serviço principal**:

- `rabbitmq` → responsável por toda a camada de mensageria

Por que apenas um serviço?  
👉 Porque os outros módulos (Order.API, Order.Processor e Portfolio.Service) são executados **localmente via `dotnet run`** durante o desenvolvimento do sistema.

Em fases mais avançadas, podemos incluir esses módulos como containers no compose, mas por enquanto garantir o RabbitMQ funcionando já permite desenvolver toda a lógica de publish/consume.

---

# 🐇 Serviço: RabbitMQ

```yaml
services:
  rabbitmq:
    image: rabbitmq:3-management
    container_name: tradeflow-rabbit
    ports:
      - "5672:5672"    # Porta padrão AMQP
      - "15672:15672"  # Painel de gerenciamento Web
    environment:
      RABBITMQ_DEFAULT_USER: admin
      RABBITMQ_DEFAULT_PASS: admin
    volumes:
      - rabbitmq_data:/var/lib/rabbitmq/
    networks:
      - tradeflow-network
