# 0001 — Python e banco relacional (SQL) como base do projeto

- **Status:** Aceita
- **Data:** 2026-09-23

## Contexto

O projeto precisa coletar dados da web, tratá-los, armazená-los com histórico e analisá-los. A proposta do TCC define Python e SQL como tecnologias centrais.

## Opções consideradas

1. **Python + banco relacional (PostgreSQL/SQLite)** — ecossistema maduro para scraping (`requests`, `BeautifulSoup`, `Playwright`, `Scrapy`) e análise (`pandas`); SQL permite consultas analíticas expressivas e integridade referencial.
2. **Python + banco NoSQL (ex.: MongoDB)** — flexível para dados heterogêneos, porém menos adequado a análises com junções e agregações e fora da proposta do TCC.
3. **Armazenamento apenas em arquivos (CSV/Parquet)** — simples, mas dificulta o controle de histórico, integridade e consultas.

## Decisão

Adotar **Python 3.11+** para coleta, tratamento e análise, e um **banco relacional** para armazenamento: **PostgreSQL** como banco principal e **SQLite** para desenvolvimento e testes, com acesso via **SQLAlchemy** para manter a portabilidade entre os dois.

## Consequências

- ✅ Grande disponibilidade de bibliotecas e material de referência;
- ✅ Consultas SQL podem ser apresentadas diretamente no TCC;
- ⚠️ Diferenças de dialeto entre PostgreSQL e SQLite (ex.: `JSONB`, `DISTINCT ON`) exigem cuidado nas consultas específicas;
- ➡️ Próximos passos: definir o esquema inicial em `sql/` e a estrutura de pacotes em `src/`.
