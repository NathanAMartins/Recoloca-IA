# Course Analysis Skill — Análise de Lacunas e Recomendações

## Visão Geral

Skill para identificar lacunas de habilidades do usuário comparando seu perfil com as vagas encontradas, e recomendar cursos para preencher essas lacunas.

## Entradas

- `data/user-profile.md`: Habilidades atuais, área de interesse, nível
- `data/job-search-results.md` (opcional): Habilidades faltantes identificadas pelo Scout

## Identificação de Lacunas

### Cenário 1: job-search-results.md existe

1. Ler `data/job-search-results.md`
2. Extrair todas as `habilidades_faltantes` de todas as vagas listadas
3. Remover duplicatas
4. Priorizar habilidades que aparecem em múltiplas vagas (mais demandadas)
5. Selecionar até 3 habilidades faltantes para buscar cursos

### Cenário 2: job-search-results.md não existe

1. Ler `data/user-profile.md` para obter:
   - Área de interesse
   - Nível de experiência
   - Habilidades atuais
2. Comparar habilidades atuais com habilidades típicas da área:

#### Habilidades Típicas por Área

**Frontend**: HTML, CSS, JavaScript, React, Vue.js, Angular, TypeScript, Git, Figma, Responsividade, Acessibilidade

**Backend**: Python, Java, Node.js, SQL, MongoDB, Docker, APIs REST, Git, Linux, Microsserviços

**Ciência de Dados**: Python, SQL, Pandas, NumPy, Scikit-learn, Machine Learning, Estatística, Power BI, Tableau, R

**Mobile**: Swift, Kotlin, React Native, Flutter, Android, iOS, Git, Firebase

**DevOps**: Docker, Kubernetes, AWS, Azure, CI/CD, Linux, Terraform, Monitoring, Shell Script

**Full Stack**: HTML, CSS, JavaScript, Python, SQL, React, Node.js, Git, APIs REST

**Design UX**: Figma, Research, Prototyping, User Testing, Wireframing, Design Thinking

**Design UI**: Figma, Design System, Typography, Color Theory, Adobe XD, Illustrator

**Cibersegurança**: Linux, Redes, Python, Ethical Hacking, Penetration Testing, Firewalls, Cryptography

3. Identificar quais habilidades típicas NÃO estão nas habilidades atuais
4. Selecionar até 3 habilidades faltantes para buscar cursos

## Busca de Cursos por Lacuna

Para cada habilidade faltante selecionada:

1. Usar `skills/course-search.md` para buscar cursos na Alura
2. Executar: `firecrawl search "site:alura.com.br/curso [habilidade]" --json`
3. Se a busca não retornar resultados, tentar termos alternativos (ver tabela em `course-search.md`)
4. Extrair detalhes dos cursos com `firecrawl scrape`
5. Retornar até 5 cursos por habilidade

## Priorização de Cursos

1. **Cursos multi-habilidade**: Priorizar cursos que abordam múltiplas habilidades faltantes
2. **Nível adequado**: 
   - Júnior: Preferir cursos introdutórios, fundamentos
   - Pleno: Preferir cursos intermediários, práticos
   - Sênior: Preferir cursos avançados, arquitetura, liderança
3. **Carga horária**: Cursos mais curtos para usuários com pouco tempo, mais longos para aprofundamento
4. **Relevância**: Cursos mais recentes e bem avaliados (se informação disponível)

## Formato de Saída

```
1. habilidade_faltante: [habilidade]
   cursos:
   1.1. titulo: [título do curso]
        instrutor: [nome do instrutor/organização]
        carga_horaria: [Xh]
        link: [URL]
        habilidades_abordadas: [habilidade1, habilidade2]

   1.2. [próximo curso no mesmo formato]

2. [próxima habilidade faltante e seus cursos]
```

## Tratamento de Erros

1. Se `job-search-results.md` não existir: Prosseguir com análise baseada apenas no perfil
2. Se busca na Alura falhar para uma habilidade: Tentar fallback com termo alternativo
3. Se fallback também falhar: Pular essa habilidade e continuar com as restantes
4. Se nenhuma busca retornar resultados: Reportar ao usuário e sugerir termos de busca alternativos
5. Se `firecrawl` falhar completamente: Reportar erro e parar

## Exemplo de Fluxo

1. Ler user-profile.md → Habilidades atuais: Python, SQL
2. Ler job-search-results.md → Habilidades faltantes: Docker, AWS, Machine Learning
3. Selecionar 3 habilidades: Docker, AWS, Machine Learning
4. Buscar cursos na Alura para cada uma:
   - Docker → 5 cursos encontrados
   - AWS → 5 cursos encontrados
   - Machine Learning → 5 cursos encontrados
5. Retornar 15 cursos no total, agrupados por habilidade
