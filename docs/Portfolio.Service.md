# Portfolio.Service

## 🎯 Finalidade
O `Portfolio.Service` é responsável por atualizar o portfólio do investidor sempre que uma ordem é executada.

Ele:
- Lê eventos `orders.executed`
- Recalcula quantidade de ativos
- Atualiza preço médio
- Registra histórico transacional
- Mantém o estado do portfólio atualizado

## 🧱 Por que também é um Worker Service?
- Ele reage a *eventos*, não a requisições HTTP
- Processamento é interno e assíncrono
- Services de portfólio **não ficam expostos ao público**
- Permite escalar isoladamente quando houver muito movimento

## 📡 Comunicação
- Consome: `orders.executed`
- (Opcional futuro) Publica: `portfolio.updated`

## 🔧 Justificativas técnicas
- Portfolio é dado crítico → melhor que esteja isolado
- Usar Worker evita sobrecarregar API
- Fácil de adicionar lógica complexa como:
  - cálculo de preço médio ponderado
  - cálculo de prejuízo/futuros impostos
  - margens e garantias

## 🧠 Notas para o eu do futuro
- Separar armazenamento de portfólio por investidor (sharding leve)
- Pensar em idempotência no cálculo (evitar duplicação)
- Logar alterações para auditoria
- Não misturar com leitura → isso seria uma outra API (Portfolio.API)
