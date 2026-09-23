# 06 — Ambiente de Desenvolvimento

> As instruções abaixo refletem o ambiente planejado e serão ajustadas assim que o código for adicionado ao repositório.

## 1. Pré-requisitos

- Python **3.11+**
- Git
- PostgreSQL **15+** (ou Docker) — opcional em desenvolvimento, pois o SQLite pode ser usado
- (Opcional) Navegadores do Playwright, para fontes com páginas dinâmicas

## 2. Instalação

```bash
# Clonar o repositório
git clone https://github.com/rafaelbertochi1/Web_Scraper_TCC.git
cd Web_Scraper_TCC

# Criar e ativar ambiente virtual
python -m venv .venv
source .venv/bin/activate        # Linux/macOS
# .venv\Scripts\activate         # Windows

# Instalar dependências (arquivo a ser criado)
pip install -r requirements.txt

# (Opcional) Instalar navegadores do Playwright
playwright install chromium
```

## 3. Configuração

Criar um arquivo `.env` na raiz (baseado no futuro `.env.example`):

```env
DATABASE_URL=postgresql://usuario:senha@localhost:5432/imoveis
# DATABASE_URL=sqlite:///data/imoveis.db
REQUEST_DELAY_SECONDS=3
USER_AGENT="WebScraperTCC/0.1 (+contato@exemplo.com)"
LOG_LEVEL=INFO
```

## 4. Banco de dados

```bash
# Subir PostgreSQL via Docker (opcional)
docker run --name imoveis-db -e POSTGRES_PASSWORD=senha -e POSTGRES_DB=imoveis -p 5432:5432 -d postgres:16

# Criar tabelas
psql "$DATABASE_URL" -f sql/001_schema.sql
```

## 5. Execução (prevista)

```bash
# Executar a coleta de uma fonte
python -m scraper_imoveis coletar --fonte <nome_da_fonte>

# Rodar os testes
pytest

# Verificar estilo de código
ruff check .
```

## 6. Convenções

- Código e nomes de variáveis em **português** para o domínio (ex.: `preco`, `bairro`) e em inglês para termos técnicos consagrados;
- Estilo verificado com `ruff`;
- Mensagens de commit no padrão [Conventional Commits](https://www.conventionalcommits.org/pt-br/) (ex.: `feat: adiciona scraper da fonte X`, `docs: atualiza modelo de dados`).
