# Análise da Cobertura Vacinal no Brasil: Indicadores, Distribuição Regional e Evolução Temporal

**Desenvolvido por Júlia de Souza Silva**

## Descrição

Projeto de análise de dados sobre a cobertura vacinal no Brasil, utilizando
dados públicos e reais do Programa Nacional de Imunizações (PNI/SI-PNI),
disponibilizados pelo DATASUS. O projeto percorre todo o fluxo de um projeto
de dados: coleta, tratamento em Python, modelagem e consultas em SQL,
dashboard em Power BI e documentação.

## Status atual do projeto

Este repositório está **estruturalmente completo e pronto para rodar**, mas
a coleta dos dados reais tem uma etapa manual obrigatória (ver abaixo) que
só quem tem acesso ao navegador pode fazer — o TABNET não oferece link de
download direto, é um formulário interativo. Nenhum número, tabela ou
resultado deste projeto foi inventado; onde ainda não há dado real
carregado, os scripts avisam isso claramente em vez de simular um resultado.

| Etapa | Status |
|---|---|
| 1. Pesquisa e validação das bases | Concluída — `docs/ETAPA1_pesquisa_bases.md` |
| 2. Definição do problema | Concluída — `docs/ETAPA2_definicao_problema.md` |
| 3. Coleta dos dados | Concluída — 16 CSVs reais em `data/raw/` (Febre Amarela e Tríplice Viral D1, 2015–2022); registro em `docs/ETAPA3_coleta_dados.md` |
| 4. Tratamento com Python | Script pronto — `python/02_tratamento.py` |
| 5. Banco de dados SQL | Scripts prontos — `sql/` |
| 6. Análise exploratória | Script pronto — `python/03_analise_exploratoria.py` |
| 7. Dashboard Power BI | Guia + medidas DAX prontas — `powerbi/GUIA_DASHBOARD_POWERBI.md` |
| 8. Documentação final | Este README |

## Fonte dos dados

- **Base:** Imunizações – Cobertura – Brasil (SI-PNI/CGPNI)
- **Órgão responsável:** Ministério da Saúde / DATASUS
- **Link:** https://tabnet.datasus.gov.br/cgi/dhdat.exe?bd_pni/cpnibr.def 
- **Período utilizado:** 1994–2022 (defina o recorte exato ao exportar, conforme `docs/ETAPA3_coleta_dados.md`)
- **Data de validação das fontes:** 18/09/2026

## Tecnologias

- **Python** (pandas, matplotlib) — importação, limpeza e análise exploratória
- **SQL (PostgreSQL)** — modelagem, carga e consultas analíticas
- **Power BI** — dashboard interativo com medidas DAX
- **GitHub** — versionamento e documentação

## Estrutura do projeto

```
projeto-cobertura-vacinal/
├── README.md
├── LICENSE
├── requirements.txt
├── .env.example         <- Modelo de credenciais do banco (copie para .env)
├── .gitignore
├── data/
│   ├── raw/            <- CSVs reais exportados do TABNET (você adiciona)
│   └── tratado/         <- Saída dos scripts de tratamento (gerado automaticamente)
├── python/
│   ├── 01_inspecao_amostra.py
│   ├── 02_tratamento.py
│   └── 03_analise_exploratoria.py
├── sql/
│   ├── 01_criacao_tabelas.sql
│   ├── 02_carga_dados.py
│   └── 03_consultas_analiticas.sql
├── powerbi/
│   ├── TelaPoweBI.png
│   └── GUIA_DASHBOARD_POWERBI.md
└── docs/
    ├── ETAPA1_pesquisa_bases.md
    ├── ETAPA2_definicao_problema.md
    ├── ETAPA3_coleta_dados.md
    └── figuras/          <- Gráficos gerados pela análise exploratória
```

## Como executar

```bash
# 1. Instale as dependências
pip install -r requirements.txt

# 2. Colete os dados reais (manual, ver docs/ETAPA3_coleta_dados.md)
#    Exporte pelo menos 3 vacinas do TABNET e salve em data/raw/

# 3. Confira a amostra real antes de tratar
python python/01_inspecao_amostra.py

# 4. Trate os dados
python python/02_tratamento.py

# 5. (opcional) Suba um banco PostgreSQL local e carregue os dados
#    Antes de rodar, copie .env.example para .env e preencha suas credenciais
psql -d cobertura_vacinal -f sql/01_criacao_tabelas.sql
python sql/02_carga_dados.py

# 6. Rode a análise exploratória
python python/03_analise_exploratoria.py

# 7. Monte o dashboard no Power BI Desktop seguindo powerbi/GUIA_DASHBOARD_POWERBI.md
```

## Indicadores

- Cobertura vacinal (%) por UF, região e ano, por imunobiológico
- Variação da cobertura ano a ano
- Percentual de combinações UF/ano que atingem a meta de 95% do PNI
- Percentual de registros ausentes por imunobiológico (transparência sobre a qualidade dos dados)

A fórmula de cobertura usada pela fonte é: **doses aplicadas da dose de
referência ÷ população-alvo × 100** — não é o mesmo que "total de doses
aplicadas", e coberturas acima de 100% podem ocorrer por imprecisão da
população-alvo estimada (ver `docs/ETAPA2_definicao_problema.md`).

## Limitações

- Análise descritiva, não causal: o projeto não afirma *por que* a cobertura
  varia, apenas *como* ela varia nos dados oficiais
- Dados de 1994–1996 têm UFs ausentes na fonte original
- Cobertura mais fraca/fragmentada nesta base para o período pós-2022
- Nível geográfico limitado a Brasil/região/UF/capitais nesta base

## Autoria

Júlia de Souza Silva
