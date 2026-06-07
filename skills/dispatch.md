# Dispatch Protocol (dispatch.md)

## 1. Tabela de Roteamento
1. **A – Scout** – Busca de vagas
2. **B – Curator** – Busca de cursos
3. **C – Coach** – Simulação de entrevistas (6 despachos sequenciais)
4. **D – Maestro** – Orquestrador principal (quiz e menu)

## 2. Envelope de Despacho (construído pelo Maestro)
```
## DESPACHO: [NOME_DO_AGENTE]
### referencia_persona
[conteúdo completo de personas/<nome_do_agente_minusculo>.md]

### tarefa
[frase curta descrevendo a tarefa]

### perfil_usuario
[conteúdo de data/user-profile.md]

### contexto
[contexto específico, ex.: vaga alvo, habilidades a buscar, etc.]

### saida_esperada
[formato exato que o agente deve retornar]
```

## 3. Envelope de Resposta (retornado pelo agente despachado)
```
## RESPOSTA: [NOME_DO_AGENTE]
### estado
[sucesso | erro]

### resumo
[2‑3 frases legíveis para o usuário]

### dados
[lista numerada de pares chave‑valor, sem tabelas]

### erros
[apenas se estado for erro]
```

## 4. Especificações de Handoff por Agente
- **Scout**: recebe `contexto` com lista de sites de vagas e retorna lista de vagas (lista numerada, chave‑valor).
- **Curator**: recebe `contexto` com lista de habilidades e retorna cursos recomendados.
- **Coach**: recebe `contexto` com pergunta de entrevista; será despachado 6 vezes sequenciais, cada resposta alimenta o próximo despacho.
- **Maestro**: nunca é despachado; ele orquestra todo o fluxo.

## 5. Despacho Sequencial do Coach (6 etapas)
1. Pergunta de abertura – "Apresente-se e descreva a vaga que deseja simular."
2. Pergunta de experiência – "Fale sobre sua experiência mais relevante."
3. Pergunta de competências técnicas – "Quais tecnologias domina?"
4. Pergunta de soft skills – "Como lida com conflitos?"
5. Pergunta de cenário – "Descreva como resolveria [situação típica]."
6. Pergunta de encerramento – "Tem alguma pergunta para o entrevistador?"

## 6. Regras de Tratamento de Erros
- Cada agente deve preencher o campo `estado` com **erro** se qualquer falha ocorrer.
- O campo `erros` deve conter a mensagem de erro detalhada.
- O Maestro, ao receber `estado: erro`, deve apresentar o resumo ao usuário e oferecer opção de retry ou abort.
- Se o fallback de busca falhar (ex.: skill/course‑analysis), pular a habilidade e continuar.
