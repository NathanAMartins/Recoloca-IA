# recoloca.ia

Sistema multiagente para desenvolvimento de carreira criado durante a Imersão IA da Alura.

## Sobre o Projeto

O recoloca.ia auxilia profissionais em sua jornada de carreira através de agentes especializados que:

- Mapeiam o perfil do usuário
- Buscam vagas compatíveis
- Identificam lacunas de habilidades
- Recomendam cursos
- Simulam entrevistas

## Arquitetura

O projeto é composto por quatro agentes:

### Maestro
Responsável por:
- Conduzir o quiz inicial
- Gerar o perfil do usuário
- Coordenar os demais agentes

### Scout
Responsável por:
- Buscar vagas de emprego
- Identificar requisitos das vagas
- Comparar requisitos com o perfil do usuário

### Curator
Responsável por:
- Identificar habilidades faltantes
- Recomendar cursos relevantes
- Organizar trilhas de aprendizado

### Coach
Responsável por:
- Simular entrevistas
- Avaliar respostas
- Gerar feedback e pontuação

## Estrutura do Projeto

```text
recoloca.ia/
├── AGENTS.md
├── Plano.md
├── plano-aula-2.md
├── plano-aula-3.md
├── plano-aula-4.md
├── personas/
├── skills/
└── data/
