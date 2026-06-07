# Interview Skill - Fluxo de Simulação de Entrevista

## Visão Geral
Este skill define o fluxo completo que o agente **COACH** deve executar para conduzir uma entrevista simulada baseada em uma vaga selecionada pelo usuário.

## Entrada
- **perfil_usuario**: conteúdo de `data/user-profile.md` (área, nível, habilidades, etc.)
- **contexto**: detalhes da vaga selecionada extraídos de `data/job-search-results.md`, incluindo:
  - título da vaga
  - empresa
  - requisitos (texto livre)
  - habilidades faltantes (lista)

## Saída Esperada (Envelope de Resposta)
```
## RESPOSTA: COACH
### estado
[sucesso | erro]

### resumo
[breve descrição da entrevista e do feedback]

### dados
1. pergunta: "..."
   resposta_usuario: "..."
   avaliação: "..."
   pontos_fortes: "..."
   melhorias: "..."
2. ...

### erros
[Apenas se estado for erro]
```

## Passos do Fluxo

1. **Carregar Vaga**
   - Ler `data/job-search-results.md`.
   - Se houver múltiplas vagas, escolher a primeira ou a indicada pelo usuário (para simplificação, usar a primeira).
   - Extrair título, empresa, requisitos e lista de `habilidades_faltantes`.

2. **Gerar Perguntas**
   - Criar entre 5 e 10 perguntas alinhadas ao cargo e às habilidades faltantes.
   - Estrutura sugerida:
     - Pergunta 1: Experiência geral relacionada ao cargo.
     - Pergunta 2: Pergunta técnica sobre uma das habilidades faltantes.
     - Pergunta 3: Resolução de problemas / case.
     - Pergunta 4: Trabalho em equipe / comunicação.
     - Pergunta 5: Objetivos de carreira e motivação.
   - Opcional: usar `firecrawl` (via `terminal`) para buscar exemplos de boas respostas e enriquecer a pergunta.

3. **Coletar Respostas do Usuário**
   - Para cada pergunta, enviar a pergunta ao usuário e aguardar a resposta.
   - Registrar a pergunta e a resposta em `data/interview-session.md` (ver esquema abaixo).
   - Limitar a interação a um único turno por pergunta (não repetir).

4. **Avaliar Respostas**
   - Para cada resposta, aplicar regras de avaliação simples:
     - **Palavras‑chave**: verificar presença de termos relevantes à habilidade (ex.: "Python", "REST", "liderança").
     - **Clareza**: medir comprimento mínimo (ex.: > 20 caracteres) como heurística.
     - **Exemplos Práticos**: detectar presença de palavras como "exemplo", "projeto", "situ", "resultado".
   - Gerar avaliação textual (ex.: "Boa", "Adequada", "Precisa melhorar").
   - Identificar **pontos fortes** (palavras‑chave encontradas) e **áreas de melhoria** (ausência de palavras‑chave ou respostas curtas).

5. **Persistir Sessão**
   - Criar/atualizar `data/interview-session.md` com o seguinte formato:
```
Data da Entrevista: [AAAA-MM-DD HH:MM]
Vaga: [título da vaga]
Empresa: [nome da empresa]

Perguntas e Respostas:
1. pergunta: [texto]
   resposta: [texto]
2. pergunta: [texto]
   resposta: [texto]
...
```

6. **Gerar Feedback**
   - Consolidar avaliações de todas as perguntas.
   - Produzir feedback estruturado em `data/interview-feedback.md` contendo:
     - Resumo geral da entrevista.
     - Lista de **Pontos Fortes**.
     - Lista de **Áreas de Melhoria**.
     - **Sugestões de Estudo** (ex.: cursos relevantes de `data/course-recommendations.md` se existir).
   - Formato:
```
Data da Entrevista: [AAAA-MM-DD HH:MM]
Vaga: [título]
Resumo: [texto]

Pontos Fortes:
- [item]
- [item]

Áreas de Melhoria:
- [item]
- [item]

Sugestões de Estudo:
- Curso: [nome] (ver `data/course-recommendations.md`)
- Leitura: [link]
```

7. **Retornar Envelope de Resposta**
   - Preencher o envelope conforme especificado na seção "Saída Esperada".
   - Em caso de falha (arquivo ausente, erro de parsing, etc.) definir `estado` como `erro` e listar a causa em `erros`.

## Regras de Tratamento de Erros
- Se `data/job-search-results.md` não existir ou estiver vazio → `estado: erro`, `erros: "Arquivo de resultados de vagas não encontrado. Execute a opção A antes de iniciar a entrevista."`
- Se a extração da vaga falhar → `estado: erro`, `erros: "Não foi possível extrair detalhes da vaga selecionada."`
- Se a coleta de respostas for interrompida pelo usuário → ainda considerar `sucesso` e registrar respostas obtidas até o momento.
- Qualquer exceção inesperada deve ser capturada e retornada como `erro` com mensagem descritiva.

## Ferramentas Opcionais
- **firecrawl**: `firecrawl search "[habilidade] interview tips" --json` para obter dicas de respostas que podem ser incluídas nas avaliações.
- **terminal**: executar o comando acima, parsear JSON e usar os snippets como referência.

## Observações
- Todo o processamento ocorre localmente; nenhum dado é enviado para serviços externos, exceto buscas opcionais via `firecrawl`.
- O skill deve ser carregado pelo agente COACH antes da execução.
- O agente deve manter o tom amigável e encorajador, conforme definido nas regras de comportamento do Maestro.
