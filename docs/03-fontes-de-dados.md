# 03 — Fontes de Dados

## 1. Critérios de seleção

Uma fonte só será utilizada se atender aos critérios abaixo:

| Critério | Pergunta |
|----------|----------|
| Legalidade | Os termos de uso permitem (ou não proíbem) a coleta automatizada? |
| `robots.txt` | As páginas de interesse são permitidas? |
| Acesso público | Os dados estão disponíveis sem login ou pagamento? |
| Existência de API | Existe API oficial ou dados abertos? (preferível ao scraping) |
| Qualidade | Os anúncios trazem preço, área e localização de forma consistente? |
| Volume | Há quantidade suficiente de anúncios no recorte geográfico? |
| Viabilidade técnica | Página estática ou dinâmica? Há bloqueios anti-bot? |

## 2. Fontes candidatas

> Tabela a ser preenchida durante a fase de levantamento. Cada fonte deve ser avaliada contra os critérios acima antes de ser implementada.

| Fonte | Tipo | URL | Termos de uso verificados? | `robots.txt` verificado? | Técnica | Status |
|-------|------|-----|----------------------------|--------------------------|---------|--------|
| _Portal A_ | Portal de anúncios | _a definir_ | ☐ | ☐ | _a definir_ | Em análise |
| _Portal B_ | Portal de anúncios | _a definir_ | ☐ | ☐ | _a definir_ | Em análise |
| _Imobiliária local_ | Site de imobiliária | _a definir_ | ☐ | ☐ | _a definir_ | Em análise |

### Fontes complementares (dados abertos)

Úteis para enriquecer a análise (sem necessidade de scraping):

- **IBGE** — dados demográficos e de renda por município/setor censitário;
- **Índice FipeZAP** — referência de preços para comparação;
- **Prefeituras** — bases de ITBI / valores venais, quando publicadas como dados abertos.

## 3. Campos a coletar

| Campo | Descrição | Obrigatório |
|-------|-----------|:-----------:|
| `id_fonte` | Identificador do anúncio na fonte | ✅ |
| `url` | Endereço do anúncio | ✅ |
| `finalidade` | Venda ou aluguel | ✅ |
| `tipo_imovel` | Apartamento, casa, terreno, sala comercial… | ✅ |
| `preco` | Valor anunciado (R$) | ✅ |
| `area_util` | Área útil (m²) | ✅ |
| `area_total` | Área total (m²) | |
| `quartos` | Número de quartos | |
| `suites` | Número de suítes | |
| `banheiros` | Número de banheiros | |
| `vagas` | Vagas de garagem | |
| `condominio` | Valor do condomínio (R$) | |
| `iptu` | Valor do IPTU (R$) | |
| `bairro` | Bairro | ✅ |
| `cidade` / `uf` | Cidade e estado | ✅ |
| `latitude` / `longitude` | Coordenadas, quando disponíveis | |
| `caracteristicas` | Piscina, elevador, portaria etc. | |
| `data_publicacao` | Data de publicação, quando disponível | |
| `data_coleta` | Data/hora da coleta (gerada pelo sistema) | ✅ |

**Não serão coletados:** nome, telefone, e-mail ou qualquer dado pessoal de anunciantes/corretores (ver [05-etica-e-aspectos-legais.md](05-etica-e-aspectos-legais.md)).

## 4. Ficha de avaliação de fonte (modelo)

Copiar para cada fonte avaliada:

```markdown
### Nome da fonte
- URL:
- Data da avaliação:
- Termos de uso (trecho relevante + link):
- robots.txt (regras relevantes):
- Página estática ou dinâmica:
- Paginação:
- Campos disponíveis:
- Proteções anti-bot observadas:
- Decisão: ✅ usar / ❌ descartar — justificativa:
```
