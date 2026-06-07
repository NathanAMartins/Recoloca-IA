# Firecrawl Skill — Comandos e Regras do CLI

## Visão Geral

Skill de referência para comandos do Firecrawl CLI. O Firecrawl agrega resultados de busca de vagas de múltiplas fontes (Indeed, Catho, LinkedIn, Glassdoor, Infojobs).

## Pré-requisitos

- Firecrawl instalado e configurado
- `FIRECRAWL_API_KEY` definida no ambiente

## Comandos Principais

### Busca de Vagas

```bash
firecrawl search "vagas [area_de_interesse] [localizacao]" --json
```

**Retorno**: JSON com campos:
- `url`: URL da vaga
- `titulo`: Título da vaga
- `descricao`: Descrição resumida
- `cargo`: Cargo/função

### Extração de Detalhes da Vaga

```bash
firecrawl scrape <url> --format markdown
```

**Retorno**: Markdown com descrição completa e requisitos da vaga.

## Regras de Uso

1. **Sempre use `--json`** na busca para facilitar o parsing
2. **Use `--format markdown`** no scrape para obter texto limpo
3. Se `firecrawl search` falhar: reporte erro e pare
4. Se `firecrawl scrape` falhar em uma URL específica: use título/descrição do resultado da busca
5. Nunca invente dados se a busca não retornar resultados

## Fallback

Se o firecrawl falhar consistentemente, você PODE usar:
- `curl` ou `wget` como recuperação (limitações: sem renderização JS, possíveis bloqueios anti-bot, HTML bruto)

## Exemplos

### Buscar vagas de Frontend em São Paulo
```bash
firecrawl search "vagas Frontend São Paulo" --json
```

### Extrair detalhes de uma vaga específica
```bash
firecrawl scrape "https://www.indeed.com.br/vaga/12345" --format markdown
```
