# Web Scraper TCC — Coleta e Análise de Dados Imobiliários

> Projeto de Trabalho de Conclusão de Curso (TCC) para coleta automatizada (web scraping) de anúncios de imóveis, armazenamento em banco de dados relacional e análise exploratória com **Python** e **SQL**.

![status](https://img.shields.io/badge/status-em%20planejamento-yellow)
![python](https://img.shields.io/badge/python-3.11%2B-blue)
![licença](https://img.shields.io/badge/licen%C3%A7a-a%20definir-lightgrey)

---

## Sumário

- [Sobre o projeto](#sobre-o-projeto)
- [Objetivos](#objetivos)
- [Visão geral da arquitetura](#visão-geral-da-arquitetura)
- [Stack tecnológica](#stack-tecnológica)
- [Estrutura do repositório](#estrutura-do-repositório)
- [Documentação](#documentação)
- [Status e roadmap](#status-e-roadmap)
- [Aspectos éticos e legais](#aspectos-éticos-e-legais)
- [Autoria](#autoria)
- [Licença](#licença)

---

## Sobre o projeto

O mercado imobiliário brasileiro divulga grande parte de sua oferta em portais de anúncios na internet. Esses dados, porém, estão dispersos, em formatos heterogêneos e não ficam disponíveis de forma estruturada para análise.

Este projeto propõe um pipeline completo que:

1. **Coleta** anúncios de imóveis (venda e/ou aluguel) em fontes públicas na web;
2. **Trata e padroniza** os dados coletados (preço, área, localização, características);
3. **Armazena** as informações em um banco de dados relacional (SQL);
4. **Analisa** os dados por meio de consultas SQL e bibliotecas Python, gerando indicadores e visualizações sobre o mercado.

## Objetivos

### Objetivo geral

Desenvolver uma ferramenta de coleta, armazenamento e análise de dados de anúncios imobiliários que permita investigar padrões de preço e oferta do mercado.

### Objetivos específicos

- Mapear e selecionar fontes de dados de imóveis adequadas (técnica e legalmente);
- Implementar scrapers em Python robustos, modulares e respeitosos com as fontes;
- Modelar um banco de dados relacional para armazenar o histórico dos anúncios;
- Construir uma etapa de limpeza e padronização (ETL) dos dados;
- Produzir análises exploratórias (ex.: preço médio por m², distribuição por bairro, evolução temporal);
- Documentar todo o processo de forma detalhada e reprodutível.

Detalhes em [docs/01-visao-geral.md](docs/01-visao-geral.md).

## Visão geral da arquitetura

```
┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│   Fontes     │    │   Coleta     │    │  Tratamento  │    │ Armazenamento│
│  (portais    │───▶│  (scrapers   │───▶│   (ETL /     │───▶│    (SQL)     │
│  imobiliários│    │   Python)    │    │  validação)  │    │              │
└──────────────┘    └──────────────┘    └──────────────┘    └──────┬───────┘
                                                                   │
                                                                   ▼
                                                           ┌──────────────┐
                                                           │   Análise    │
                                                           │ (SQL, pandas,│
                                                           │  notebooks)  │
                                                           └──────────────┘
```

Detalhes em [docs/02-arquitetura.md](docs/02-arquitetura.md).

## Stack tecnológica

| Camada          | Tecnologias previstas                                   |
|-----------------|---------------------------------------------------------|
| Linguagem       | Python 3.11+                                            |
| Coleta          | `requests` / `httpx`, `BeautifulSoup4`, `Playwright` (páginas dinâmicas) |
| Tratamento      | `pandas`, `pydantic` (validação)                        |
| Banco de dados  | PostgreSQL (produção) / SQLite (desenvolvimento)        |
| Acesso ao banco | `SQLAlchemy`                                            |
| Análise         | SQL, `pandas`, `matplotlib` / `seaborn`, Jupyter        |
| Qualidade       | `pytest`, `ruff`                                        |

> As escolhas acima são a proposta inicial e serão confirmadas por meio de [registros de decisão (ADRs)](docs/adr/).

## Estrutura do repositório

Estrutura planejada (será criada ao longo do desenvolvimento):

```
Web_Scraper_TCC/
├── docs/                 # Documentação do projeto
│   └── adr/              # Registros de decisões de arquitetura
├── src/
│   └── scraper_imoveis/
│       ├── scrapers/     # Um módulo por fonte de dados
│       ├── etl/          # Limpeza, padronização e validação
│       ├── db/           # Modelos e conexão com o banco
│       └── config/       # Configurações
├── sql/                  # Scripts DDL e consultas analíticas
├── notebooks/            # Análises exploratórias (Jupyter)
├── data/                 # Dados brutos/intermediários (fora do Git)
├── tests/                # Testes automatizados
└── README.md
```

## Documentação

| Documento | Conteúdo |
|-----------|----------|
| [01 — Visão geral](docs/01-visao-geral.md) | Contexto, problema, justificativa, objetivos e escopo |
| [02 — Arquitetura](docs/02-arquitetura.md) | Componentes, fluxo de dados e decisões técnicas |
| [03 — Fontes de dados](docs/03-fontes-de-dados.md) | Critérios de seleção, fontes candidatas e campos coletados |
| [04 — Modelo de dados](docs/04-modelo-de-dados.md) | Modelo conceitual e esquema SQL inicial |
| [05 — Ética e aspectos legais](docs/05-etica-e-aspectos-legais.md) | LGPD, termos de uso, robots.txt e boas práticas |
| [06 — Ambiente de desenvolvimento](docs/06-ambiente-de-desenvolvimento.md) | Pré-requisitos, instalação e execução |
| [07 — Roadmap](docs/07-roadmap.md) | Fases, entregas e cronograma |
| [08 — Glossário](docs/08-glossario.md) | Termos técnicos e do domínio imobiliário |
| [ADRs](docs/adr/) | Registros de decisões de arquitetura |

## Status e roadmap

🟡 **Fase atual:** planejamento e documentação inicial.

Veja o planejamento completo em [docs/07-roadmap.md](docs/07-roadmap.md).

## Aspectos éticos e legais

A coleta será feita apenas sobre dados publicamente acessíveis, respeitando `robots.txt`, termos de uso das fontes, limites de requisição e a **Lei Geral de Proteção de Dados (LGPD — Lei nº 13.709/2018)**. Dados pessoais de anunciantes não serão armazenados. Detalhes em [docs/05-etica-e-aspectos-legais.md](docs/05-etica-e-aspectos-legais.md).

## Autoria

| Papel | Nome |
|-------|------|
| Autor(a) | _a definir_ |
| Orientador(a) | _a definir_ |
| Instituição / Curso | _a definir_ |

## Licença

_A definir._ (Sugestão: MIT para o código; os dados coletados não serão redistribuídos.)
