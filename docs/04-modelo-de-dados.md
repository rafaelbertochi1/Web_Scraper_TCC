# 04 — Modelo de Dados

> Versão inicial (proposta). O esquema definitivo será versionado em `sql/`.

## 1. Princípios

- **Histórico preservado:** o preço de um anúncio pode mudar; cada coleta gera uma nova _observação_ em vez de sobrescrever o valor anterior.
- **Normalização moderada:** entidades estáveis (fonte, localização) separadas dos anúncios; atributos voláteis (preço, condomínio) nas observações.
- **Rastreabilidade:** toda observação aponta para a execução de coleta que a originou.

## 2. Modelo conceitual

```
┌───────────┐ 1     N ┌───────────────┐ N     1 ┌─────────────┐
│   fonte   │────────▶│    anuncio    │◀────────│ localizacao │
└───────────┘         └───────┬───────┘         └─────────────┘
      │ 1                     │ 1
      │                       │
      │ N                     │ N
┌───────────┐ 1     N ┌───────────────┐
│  execucao │────────▶│  observacao   │
│  _coleta  │         │ (preço, data) │
└───────────┘         └───────────────┘
```

| Entidade | Descrição |
|----------|-----------|
| `fonte` | Site de onde os dados são coletados |
| `execucao_coleta` | Cada rodada de coleta (início, fim, status, métricas) |
| `localizacao` | Bairro, cidade, UF e coordenadas |
| `anuncio` | Imóvel anunciado — atributos estáveis (tipo, área, quartos) |
| `observacao` | "Foto" do anúncio em uma coleta — preço, condomínio, IPTU, ativo |

## 3. Esquema SQL inicial (PostgreSQL)

```sql
CREATE TABLE fonte (
    id          SERIAL PRIMARY KEY,
    nome        VARCHAR(100) NOT NULL UNIQUE,
    url_base    VARCHAR(255) NOT NULL,
    ativa       BOOLEAN NOT NULL DEFAULT TRUE
);

CREATE TABLE execucao_coleta (
    id                  SERIAL PRIMARY KEY,
    fonte_id            INT NOT NULL REFERENCES fonte(id),
    iniciada_em         TIMESTAMP NOT NULL DEFAULT NOW(),
    finalizada_em       TIMESTAMP,
    status              VARCHAR(20) NOT NULL DEFAULT 'em_andamento', -- em_andamento | sucesso | falha
    anuncios_coletados  INT DEFAULT 0,
    erros               INT DEFAULT 0
);

CREATE TABLE localizacao (
    id          SERIAL PRIMARY KEY,
    bairro      VARCHAR(120),
    cidade      VARCHAR(120) NOT NULL,
    uf          CHAR(2)      NOT NULL,
    UNIQUE (bairro, cidade, uf)
);

CREATE TABLE anuncio (
    id              SERIAL PRIMARY KEY,
    fonte_id        INT NOT NULL REFERENCES fonte(id),
    id_fonte        VARCHAR(100) NOT NULL,          -- ID do anúncio no site de origem
    url             TEXT NOT NULL,
    finalidade      VARCHAR(10) NOT NULL,           -- venda | aluguel
    tipo_imovel     VARCHAR(50) NOT NULL,
    localizacao_id  INT REFERENCES localizacao(id),
    area_util       NUMERIC(10,2),
    area_total      NUMERIC(10,2),
    quartos         SMALLINT,
    suites          SMALLINT,
    banheiros       SMALLINT,
    vagas           SMALLINT,
    latitude        NUMERIC(9,6),
    longitude       NUMERIC(9,6),
    caracteristicas JSONB,
    primeira_coleta TIMESTAMP NOT NULL DEFAULT NOW(),
    ultima_coleta   TIMESTAMP NOT NULL DEFAULT NOW(),
    UNIQUE (fonte_id, id_fonte)
);

CREATE TABLE observacao (
    id              BIGSERIAL PRIMARY KEY,
    anuncio_id      INT NOT NULL REFERENCES anuncio(id),
    execucao_id     INT NOT NULL REFERENCES execucao_coleta(id),
    coletado_em     TIMESTAMP NOT NULL DEFAULT NOW(),
    preco           NUMERIC(14,2) NOT NULL,
    condominio      NUMERIC(10,2),
    iptu            NUMERIC(10,2),
    ativo           BOOLEAN NOT NULL DEFAULT TRUE
);

CREATE INDEX idx_anuncio_localizacao ON anuncio (localizacao_id);
CREATE INDEX idx_observacao_anuncio_data ON observacao (anuncio_id, coletado_em);
```

## 4. Exemplo de consulta analítica

Preço médio do m² por bairro (última observação de cada anúncio de venda):

```sql
WITH ultima AS (
    SELECT DISTINCT ON (o.anuncio_id)
           o.anuncio_id, o.preco
    FROM observacao o
    ORDER BY o.anuncio_id, o.coletado_em DESC
)
SELECT l.cidade,
       l.bairro,
       COUNT(*)                                    AS qtd_anuncios,
       ROUND(AVG(u.preco / NULLIF(a.area_util, 0)), 2) AS preco_medio_m2
FROM anuncio a
JOIN ultima u      ON u.anuncio_id = a.id
JOIN localizacao l ON l.id = a.localizacao_id
WHERE a.finalidade = 'venda'
GROUP BY l.cidade, l.bairro
ORDER BY preco_medio_m2 DESC;
```

## 5. Dicionário de dados

O dicionário completo (tipo, domínio, obrigatoriedade e origem de cada coluna) será mantido neste documento à medida que o esquema for consolidado.
