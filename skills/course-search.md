# Course Search Skill — Busca de Cursos na Alura

## Visão Geral

Skill para buscar cursos na Alura (https://www.alura.com.br) usando Firecrawl. Extrai informações detalhadas de cursos para recomendar ao usuário.

## Pré-requisitos

- Firecrawl instalado e configurado
- `FIRECRAWL_API_KEY` definida no ambiente

## Comandos Principais

### Busca de Cursos na Alura

```bash
firecrawl search "site:alura.com.br/curso [habilidade]" --json
```

**Retorno**: JSON com campos:
- `url`: URL do curso na Alura
- `titulo`: Título do curso
- `descricao`: Descrição resumida do curso

### Extração de Detalhes do Curso

```bash
firecrawl scrape <url> --format markdown
```

**Retorno**: Markdown com descrição completa, carga horária, instrutor, habilidades abordadas e conteúdo programático.

## Fluxo de Busca

1. **Identificar habilidade faltante** a partir da análise de lacunas
2. **Executar busca**: `firecrawl search "site:alura.com.br/curso [habilidade]" --json`
3. **Extrair URLs**: Pegar até 5 URLs de resultados da busca
4. **Obter detalhes**: Para cada URL, executar `firecrawl scrape <url> --format markdown`
5. **Parsear detalhes**: Extrair título, instrutor, carga horária, habilidades abordadas
6. **Retornar cursos**: Lista estruturada com até 5 cursos por habilidade

## Fallback para Termos Alternativos

Se a busca não retornar resultados para uma habilidade específica, tentar termos alternativos:

| Habilidade Original | Termos Alternativos |
|---------------------|---------------------|
| JavaScript | JS, Javascript, ECMAScript |
| Python | Python 3, Python para Data Science |
| React | ReactJS, React.js, Biblioteca React |
| Node.js | Node, NodeJS, Backend com Node |
| SQL | Banco de Dados, MySQL, PostgreSQL |
| Git | Controle de Versão, GitHub, GitLab |
| Docker | Containers, Containerização, Docker Compose |
| AWS | Amazon Web Services, Cloud AWS |
| Machine Learning | ML, Aprendizado de Máquina |
| UI/UX | Design de Interface, Experiência do Usuário |

## Regras de Uso

1. **Sempre use `--json`** na busca para facilitar o parsing
2. **Use `--format markdown`** no scrape para obter texto limpo
3. Se `firecrawl search` falhar: reporte erro e pare
4. Se `firecrawl scrape` falhar em uma URL específica: use título/descrição do resultado da busca
5. Nunca invente dados se a busca não retornar resultados
6. Máximo de 5 cursos por habilidade faltante
7. Máximo de 15 cursos no total (para não sobrecarregar o usuário)

## Extração de Dados do Curso

A partir do markdown do scrape, extrair:

- **Título**: Geralmente no início do markdown ou no título da página
- **Instrutor**: Procurar por "Instrutor:", "Professor:", "Ministrado por"
- **Carga horária**: Procurar por "horas", "h", "carga horária"
- **Habilidades abordadas**: Listar tecnologias, ferramentas e conceitos mencionados na descrição

## Exemplos

### Buscar cursos de Python

```bash
firecrawl search "site:alura.com.br/curso Python" --json
```

### Extrair detalhes de um curso específico

```bash
firecrawl scrape "https://www.alura.com.br/curso/1234/python-para-data-science" --format markdown
```

## Formato de Saída Esperado

Para cada curso encontrado:

```
titulo: [título do curso]
instrutor: [nome do instrutor/organização]
carga_horaria: [Xh]
link: [URL]
habilidades_abordadas: [habilidade1, habilidade2, habilidade3]
```
