# 05 — Ética e Aspectos Legais

> Este documento não constitui parecer jurídico. Ele registra as diretrizes adotadas pelo projeto para que a coleta seja feita de forma responsável.

## 1. Legislação e referências

- **LGPD — Lei nº 13.709/2018:** regula o tratamento de dados pessoais no Brasil;
- **Marco Civil da Internet — Lei nº 12.965/2014;**
- **Lei de Direitos Autorais — Lei nº 9.610/1998** (proteção de bases de dados e conteúdo);
- **Termos de uso** de cada site utilizado como fonte.

## 2. Diretrizes adotadas

### 2.1 Dados coletados

- Apenas dados **públicos** e **não pessoais** sobre os imóveis (preço, área, localização aproximada, características);
- **Nenhum** dado pessoal de anunciantes, corretores ou proprietários (nome, telefone, e-mail, CRECI, fotos de pessoas);
- Endereços são armazenados no nível de **bairro/cidade**; coordenadas apenas quando divulgadas publicamente pela fonte;
- Fotos e textos descritivos dos anúncios **não** são armazenados nem redistribuídos.

### 2.2 Respeito às fontes

- Verificar e respeitar o `robots.txt` antes de cada coleta;
- Ler e registrar os termos de uso de cada fonte (ver ficha em [03-fontes-de-dados.md](03-fontes-de-dados.md));
- Limitar a taxa de requisições (ex.: no máximo 1 requisição a cada poucos segundos) e evitar horários de pico;
- Usar um `User-Agent` identificável, com contato do projeto;
- **Não** contornar CAPTCHAs, bloqueios, _paywalls_ ou áreas autenticadas;
- Preferir APIs oficiais ou dados abertos quando existirem;
- Interromper a coleta de uma fonte caso haja solicitação do responsável pelo site.

### 2.3 Uso e divulgação

- Os dados serão usados exclusivamente para fins **acadêmicos**;
- O conjunto de dados bruto **não será redistribuído publicamente**; o TCC apresentará apenas resultados agregados (médias, distribuições, gráficos);
- Credenciais e configurações sensíveis ficam em `.env`, fora do controle de versão.

## 3. Checklist antes de ativar uma nova fonte

- [ ] Termos de uso lidos e registrados
- [ ] `robots.txt` verificado para as rotas usadas
- [ ] Nenhum campo de dado pessoal no extrator
- [ ] _Rate limiting_ configurado
- [ ] Validação com orientador(a), se necessário
