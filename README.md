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
