# Pipeline de Combustíveis Brasil 🇧🇷

Análise de preços de combustíveis no Brasil utilizando 
arquitetura medalhão (Bronze, Silver, Gold) no Databricks.

## Motivação

Com a recente alta dos preços de combustíveis no Brasil, 
surgiu a oportunidade de explorar os dados históricos da ANP 
(Agência Nacional do Petróleo) para entender a evolução de 
preços, margens dos revendedores e variações regionais desde 
2004.

## Arquitetura
```
CSV (ANP)
    ↓
Bronze — dado cru preservado
    ↓
Silver — limpeza e padronização
    ↓
Gold   — agregações para análise
```

## Camadas

**Bronze** — ingestão direta do CSV sem transformações.
Preserva o dado original para rastreabilidade e 
reprocessamento.

**Silver** — dado tratado e padronizado:
- Tipos convertidos (datas, decimais)
- Nulos removidos em colunas críticas
- Duplicatas eliminadas
- CNPJ normalizado
- Margem do revendedor calculada

**Gold** — três tabelas analíticas:
- `gold_preco_estado` — preço médio por estado, 
   produto e ano
- `gold_preco_bandeira` — margem e preço médio 
   por bandeira e ano
- `gold_evolucao_mensal` — evolução mensal de 
   preços desde 2004

## Tecnologias

- Databricks Community Edition
- Apache Spark (PySpark)
- Delta Lake
- Python

## Fonte dos dados

ANP — Agência Nacional do Petróleo, Gás Natural 
e Biocombustíveis  
Disponível em: [[dados.gov.br](https://dados.gov.br)](https://www.gov.br/anp/pt-br/centrais-de-conteudo/dados-abertos)

## Insights encontrados

- Gasolina subiu 198% desde 2004, saindo de R$2,14 para R$6,37 em 2026
- Acre é o estado com maior preço médio em 2026 — R$7,46/L
- A ANP interrompeu a coleta de preço de compra após 2020, 
  impossibilitando análise de margem para anos recentes
- O pipeline identificou que essa ausência eliminava silenciosamente 
  4,9 milhões de registros na camada Silver — corrigido preservando 
  os dados de preço de venda completos até 2026

## Observação sobre qualidade dos dados

A coluna valor_compra contém dados apenas até 2020. 
A partir de 2021 a ANP deixou de coletar essa informação, 
o que foi identificado durante a investigação de qualidade 
de dados entre as camadas Bronze e Silver.
