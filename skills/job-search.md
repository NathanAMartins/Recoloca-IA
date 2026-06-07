# Job Search Skill — Capacidades de Busca de Vagas

## Visão Geral

Skill obrigatória para o Scout. Define o fluxo completo de busca de vagas via Firecrawl, extração de dados, correspondência de habilidades e formatação de resposta.

## Ferramenta Zed

- `terminal` — executar comandos CLI do `firecrawl`

## Fluxo de Busca

### 1. Descoberta de Vagas

Comando:
```
firecrawl search "vagas [area_de_interesse] [localizacao]" --json
```

Retorna JSON com campos:
- url
- titulo
- descricao
- cargo

### 2. Extração de Detalhes (Opcional)

Para cada vaga de interesse:
```
firecrawl scrape <url> --format markdown
```

Use para obter descrição completa e requisitos. Se falhar ou expirar, use título e descrição do resultado da busca.

### 3. Extração de Dados da Vaga

Para cada vaga, extrair:
- titulo: título da vaga
- empresa: nome da empresa (extraído da URL ou título)
- localizacao: cidade ou "Remoto"
- link: URL da vaga
- habilidades_requeridas: lista de habilidades mencionadas na descrição

### 4. Correspondência de Habilidades

Comparar `habilidades_requeridas` com `Habilidades atuais` do `data/user-profile.md`:
- Usar correspondência de strings sem distinção de maiúsculas/minúsculas
- Listar habilidades que correspondem
- Listar habilidades que faltam
- Contar: X de Y habilidades correspondem

### 5. Filtragem por Nível

- Se a vaga menciona um nível de experiência, preferir vagas que correspondam ao nível do usuário (Júnior, Pleno, Sênior)
- Se não houver vagas do nível correspondente nos primeiros resultados, expandir a busca e incluir vagas de nível adjacente, anotando a discrepância

### 6. Limite de Resultados

Retornar até 5 vagas no formato de resposta.

## Formato de Resposta (Dados)

```
1. titulo: [título da vaga]
   empresa: [nome da empresa]
   localizacao: [cidade ou Remoto]
   link: [URL]
   habilidades_correspondentes: [habilidade1, habilidade2]
   habilidades_faltantes: [habilidade3, habilidade4]
   contagem_correspondencia: [X de Y habilidades correspondem]

2. [próxima vaga no mesmo formato]
```

## Tratamento de Erros

- Se `firecrawl search` falhar: reportar erro no campo `erros` do envelope de resposta, estado = `erro`
- Se `firecrawl scrape` falhar em uma URL específica: usar título/descrição do resultado da busca, anotar falha
- Se busca não retornar resultados: sugerir ampliar termos de busca, reportar ao usuário
- Nunca inventar dados. Se a busca falhar, reporte o erro exato e pare.
