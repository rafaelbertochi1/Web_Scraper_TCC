# 02 — Arquitetura

> Documento vivo: será atualizado conforme a implementação avançar. Decisões relevantes são registradas em [ADRs](adr/).

## 1. Visão geral

O sistema é organizado como um **pipeline de dados** em quatro etapas desacopladas:

```
 Fontes ──▶ Coleta ──▶ Tratamento (ETL) ──▶ Armazenamento (SQL) ──▶ Análise
```

Cada etapa pode ser executada e testada de forma independente, o que facilita a manutenção quando um site muda de layout, por exemplo.

## 2. Componentes

### 2.1 Coleta (`scrapers/`)

- Um módulo por fonte, todos implementando uma **interface comum** (ex.: classe base `BaseScraper` com métodos `listar_anuncios()` e `extrair_detalhes()`).
- Responsável apenas por obter e extrair dados brutos — sem regras de negócio.
- Recursos transversais:
  - controle de taxa de requisições (_rate limiting_) e intervalos aleatórios;
  - novas tentativas com _backoff_ exponencial em caso de falha;
  - verificação do `robots.txt`;
  - `User-Agent` identificável;
  - _logging_ estruturado de cada execução.
- Páginas estáticas: `requests`/`httpx` + `BeautifulSoup`. Páginas dinâmicas (JavaScript): `Playwright`.

### 2.2 Tratamento / ETL (`etl/`)

- **Limpeza:** remoção de caracteres, conversão de `"R$ 450.000"` → `450000.00`, `"85 m²"` → `85`.
- **Padronização:** tipos de imóvel, nomes de bairros/cidades, unidades.
- **Validação:** esquemas com `pydantic` (tipos, faixas plausíveis de valores).
- **Deduplicação:** identificação do mesmo anúncio entre coletas (por ID da fonte/URL).
- Registros inválidos são descartados ou marcados, com motivo registrado em log.

### 2.3 Armazenamento (`db/` e `sql/`)

- Banco relacional: **PostgreSQL** (principal) e **SQLite** (desenvolvimento/testes).
- Acesso via **SQLAlchemy**; scripts DDL versionados na pasta `sql/`.
- Separação entre dados **brutos** (opcionalmente preservados em `data/raw/`) e dados **tratados** (no banco).
- Modelo detalhado em [04-modelo-de-dados.md](04-modelo-de-dados.md).

### 2.4 Análise (`notebooks/` e `sql/`)

- Consultas analíticas em SQL (views e queries versionadas);
- Notebooks Jupyter com `pandas`, `matplotlib`/`seaborn` para exploração e gráficos;
- Resultados que alimentarão o texto do TCC.

## 3. Fluxo de uma execução

1. O agendador (manual ou `cron`) inicia uma **execução de coleta** para uma fonte;
2. O scraper percorre as páginas de listagem e coleta os links dos anúncios;
3. Para cada anúncio, extrai os campos de interesse;
4. O ETL limpa, valida e padroniza os registros;
5. Os dados são gravados no banco (novo anúncio ou nova observação de preço);
6. Métricas da execução (quantidade coletada, erros, duração) são registradas.

## 4. Requisitos não funcionais

| Requisito | Descrição |
|-----------|-----------|
| Respeito às fontes | Limite de requisições, respeito ao `robots.txt` e aos termos de uso |
| Resiliência | Falha em um anúncio não interrompe a execução inteira |
| Rastreabilidade | Toda observação é associada à execução de coleta que a gerou |
| Reprodutibilidade | Ambiente e dependências documentados; scripts SQL versionados |
| Manutenibilidade | Scrapers isolados por fonte; testes com páginas HTML de exemplo |
| Configurabilidade | Parâmetros (cidades, limites, credenciais do banco) via arquivo `.env` |

## 5. Decisões em aberto

- [ ] Fontes de dados definitivas (ver [03-fontes-de-dados.md](03-fontes-de-dados.md))
- [ ] Recorte geográfico
- [ ] Frequência de coleta (diária, semanal?)
- [ ] Uso de framework (`Scrapy`) vs. implementação própria
- [ ] Orquestração (script + `cron` vs. ferramenta como Prefect/Airflow)
