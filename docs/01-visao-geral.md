# 01 — Visão Geral

## 1. Contexto

O mercado imobiliário é um dos setores mais relevantes da economia brasileira. Hoje, a maior parte da oferta de imóveis para venda e aluguel é divulgada em portais de anúncios online e sites de imobiliárias. Esses anúncios contêm informações valiosas — preço, área, número de quartos, localização, entre outras — que, reunidas, permitem compreender o comportamento do mercado.

## 2. Problema

Apesar de públicos, esses dados:

- estão **dispersos** entre diferentes sites;
- apresentam **formatos heterogêneos** (cada portal estrutura as informações de um jeito);
- **não oferecem histórico**: um anúncio removido desaparece, e com ele a informação de preço daquele momento;
- **não estão disponíveis de forma estruturada** para consultas e análises.

**Pergunta norteadora:** _como coletar, estruturar e analisar de forma automatizada dados de anúncios imobiliários, a fim de gerar informações úteis sobre preços e oferta do mercado?_

## 3. Justificativa

- **Acadêmica:** integra conhecimentos de programação, engenharia de dados, bancos de dados e análise estatística em um projeto aplicado.
- **Prática:** dados estruturados do mercado auxiliam compradores, locatários, investidores e pesquisadores na tomada de decisão.
- **Técnica:** o web scraping envolve desafios reais — páginas dinâmicas, mudanças de layout, padronização de dados e respeito às fontes.

## 4. Objetivos

### 4.1 Objetivo geral

Desenvolver uma ferramenta de coleta, armazenamento e análise de dados de anúncios imobiliários que permita investigar padrões de preço e oferta do mercado.

### 4.2 Objetivos específicos

1. Levantar e selecionar fontes de dados adequadas técnica e legalmente;
2. Implementar coletores (scrapers) modulares em Python;
3. Modelar e implementar um banco de dados relacional com histórico dos anúncios;
4. Desenvolver rotinas de limpeza, padronização e validação dos dados;
5. Realizar análises exploratórias com SQL e Python;
6. Documentar o processo de forma detalhada e reprodutível.

## 5. Escopo

### 5.1 Dentro do escopo

- Coleta de anúncios de imóveis **residenciais e comerciais** (venda e aluguel) em fontes públicas;
- Recorte geográfico inicial: _a definir_ (ex.: uma cidade ou região metropolitana) — expansível posteriormente;
- Armazenamento histórico (cada coleta registra o estado do anúncio naquele momento);
- Análises descritivas: preço médio e mediano, preço por m², distribuição por bairro/tipo, evolução temporal;
- Documentação técnica completa.

### 5.2 Fora do escopo (nesta fase)

- Coleta de dados pessoais de anunciantes (nomes, telefones, e-mails);
- Contorno de mecanismos de proteção (CAPTCHAs, bloqueios, áreas autenticadas);
- Interface web para usuários finais;
- Modelos preditivos de preço (podem ser tratados como trabalho futuro).

## 6. Perguntas de análise (hipóteses iniciais)

Exemplos de perguntas que o projeto pretende responder:

- Qual o preço médio do m² por bairro e por tipo de imóvel?
- Como o preço varia em função da área, número de quartos e vagas?
- Qual a relação entre valor de venda e valor de aluguel (rentabilidade bruta) por região?
- Como os preços anunciados evoluem ao longo do período de coleta?
- Quanto tempo, em média, um anúncio permanece ativo?

## 7. Público-alvo da documentação

- Banca avaliadora e orientador(a) do TCC;
- Desenvolvedores que queiram reproduzir ou estender o projeto;
- O(a) próprio(a) autor(a), como registro das decisões tomadas.
