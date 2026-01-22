# JurisFinder - Diagramas da Proposta LEAN

Este diretorio contem todos os diagramas da proposta tecnica JurisFinder LEAN em formato PlantUML (.puml).

## Estrutura de Pastas

```
docs/diagrams/
├── architecture/          # Diagramas de arquitetura C4 e integracoes
│   ├── c4-context.puml    # C4 Level 1 - Contexto do Sistema
│   ├── c4-container.puml  # C4 Level 2 - Containers
│   └── integration-map.puml # Mapa de Integracoes LEAN
│
├── pipeline/              # Diagramas do pipeline RAG
│   ├── rag-indexing.puml          # Pipeline de Indexacao (com OCR)
│   ├── rag-search.puml            # Pipeline de Busca (com ML Rules)
│   ├── processing-flow-indexing.puml  # Fluxo detalhado de indexacao
│   └── processing-flow-search.puml    # Fluxo detalhado de busca
│
├── process/               # Diagramas de processo e equipe
│   ├── schedule-16-weeks.puml     # Cronograma de 16 semanas
│   ├── code-validation-flow.puml  # Fluxo de validacao de codigo IA
│   └── team-structure.puml        # Estrutura da equipe LEAN
│
└── quality/               # Diagramas de qualidade e riscos
    ├── test-pyramid.puml          # Piramide de testes
    ├── shift-left-comparison.puml # Comparacao QA tradicional vs LEAN
    ├── risk-matrix.puml           # Matriz visual de riscos
    └── quality-gates.puml         # Quality gates do CI/CD
```

## Descricao dos Diagramas

### Arquitetura (`architecture/`)

| Arquivo | Descricao |
|---------|-----------|
| `c4-context.puml` | Diagrama C4 Level 1 mostrando o JurisFinder no contexto do ecossistema ThyssenKrupp |
| `c4-container.puml` | Diagrama C4 Level 2 detalhando os containers internos do sistema |
| `integration-map.puml` | Mapa de integracoes simplificado (LEAN) - AD, SharePoint, File Server |

### Pipeline RAG (`pipeline/`)

| Arquivo | Descricao |
|---------|-----------|
| `rag-indexing.puml` | Fluxo do pipeline de indexacao com OCR (Azure Document Intelligence) |
| `rag-search.puml` | Fluxo do pipeline de busca com ML Rules Engine |
| `processing-flow-indexing.puml` | 8 etapas: Crawling → OCR → Parsing → Chunking → Embedding → Classification → Indexing → Metadata |
| `processing-flow-search.puml` | 8 etapas: Query Parsing → Embedding → Hybrid Search → Filtering → Reranking → ML Rules → Alertas → Response |

### Processo (`process/`)

| Arquivo | Descricao |
|---------|-----------|
| `schedule-16-weeks.puml` | Cronograma de alto nivel do projeto em 16 semanas |
| `code-validation-flow.puml` | Fluxo de validacao de codigo gerado por IA |
| `team-structure.puml` | Estrutura da equipe LEAN (3.5 FTE) |

### Qualidade (`quality/`)

| Arquivo | Descricao |
|---------|-----------|
| `test-pyramid.puml` | Piramide de testes: 70% Unit, 20% Integration, 10% E2E |
| `shift-left-comparison.puml` | Comparacao QA tradicional vs Shift-Left LEAN |
| `risk-matrix.puml` | Matriz de riscos (Probabilidade x Impacto) |
| `quality-gates.puml` | Quality gates do CI/CD |

## Como Visualizar os Diagramas

### Opcao 1: PlantUML Online
1. Acesse https://www.plantuml.com/plantuml/uml/
2. Cole o conteudo do arquivo .puml
3. Visualize o diagrama gerado

### Opcao 2: VS Code
```bash
# Instale a extensao PlantUML (jebbs.plantuml)
# Abra o arquivo .puml e pressione Alt+D
```

### Opcao 3: Linha de Comando
```bash
# Instalar PlantUML (requer Java)
brew install plantuml  # macOS
apt install plantuml   # Ubuntu

# Gerar PNG
plantuml docs/diagrams/architecture/c4-context.puml

# Gerar SVG
plantuml -tsvg docs/diagrams/architecture/c4-context.puml

# Gerar todos os diagramas
plantuml -tsvg docs/diagrams/**/*.puml
```

## Convencoes de Cores

| Cor | Uso |
|-----|-----|
| Azul claro (`#E3F2FD`) | Componentes de entrada/lideranca |
| Verde claro (`#E8F5E9`) | Processamento/desenvolvimento |
| Amarelo (`#FFF3E0`) | ML/OCR |
| Rosa (`#FCE4EC`) | Qualidade/QA |
| Vermelho (`#FFCDD2`) | Riscos criticos |

## Referencia da Proposta

| Diagrama | Secao na Proposta LEAN |
|----------|------------------------|
| c4-context.puml | 2.2 Diagrama de Contexto |
| c4-container.puml | 2.3 Diagrama de Container |
| rag-indexing.puml | 2.4 Pipeline RAG Detalhado |
| rag-search.puml | 2.4 Pipeline RAG Detalhado |
| processing-flow-*.puml | 3.3 Fluxo de Processamento |
| integration-map.puml | 4.1 Mapa de Integracoes |
| schedule-16-weeks.puml | 5.1 Cronograma de Alto Nivel |
| team-structure.puml | 5.3 Composicao da Equipe |
| test-pyramid.puml | 6.1 Piramide de Testes |
| shift-left-comparison.puml | 6.5 Abordagem Shift-Left |
| code-validation-flow.puml | 6.6 Validacao de Codigo IA |
| risk-matrix.puml | 7.3 Matriz Visual de Riscos |
| quality-gates.puml | 6.3 Quality Gates |

---

**Versao:** 2.1 LEAN | **Data:** Janeiro 2026 | **Projeto:** JurisFinder - ThyssenKrupp
