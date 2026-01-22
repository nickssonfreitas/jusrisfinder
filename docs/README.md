# JurisFinder - Documentacao de Propostas

Este diretorio contem as propostas tecnicas para o projeto JurisFinder, organizadas por versao e escopo.

---

## Estrutura de Diretórios

```
docs/
├── README.md                          # Este arquivo
└── proposals/
    ├── original/                      # Proposta Original (v1.0)
    │   ├── SOLUTIONS_PROPOSAL.md      # Proposta de solucao tecnica
    │   ├── PROPOSTA_TECNICA_COMERCIAL.md  # Proposta comercial completa
    │   └── diagrams/                  # Diagramas C4 completos (PlantUML + SVG)
    │       ├── c4-context.*           # L1 - Contexto do sistema
    │       ├── c4-container.*         # L2 - Containers
    │       ├── c4-component-search.*  # L3 - Componentes do Search
    │       ├── c4-deployment.*        # Infraestrutura de deployment
    │       ├── c4-dynamic-indexing.*  # Fluxo de indexacao
    │       ├── c4-dynamic-search.*    # Fluxo de busca
    │       └── INDEX.md               # Indice visual dos diagramas
    │
    ├── lean/                          # Proposta LEAN (v2.1)
    │   ├── SOLUTIONS_PROPOSAL_COMPLETE_LEAN.md  # Proposta LEAN completa
    │   ├── INFRASTRUCTURE_COSTS.md    # Analise de custos de infraestrutura
    │   ├── RISK_MATRIX.md             # Matriz de riscos detalhada
    │   └── diagrams/                  # Diagramas organizados por categoria
    │       ├── architecture/          # C4 Context, Container, Integration Map
    │       ├── pipeline/              # RAG Indexing, Search, Processing Flows
    │       ├── process/               # Cronograma, Equipe, Validacao IA
    │       ├── quality/               # Testes, Quality Gates, Riscos
    │       └── README.md              # Documentacao dos diagramas LEAN
    │
    └── mvp/                           # Proposta MVP Minimalista
        └── SOLUTIONS_PROPOSAL_MVP.md  # Proposta MVP 8 semanas
```

---

## Comparativo das Propostas

| Aspecto | Original (v1.0) | LEAN (v2.1) | MVP |
|---------|-----------------|-------------|-----|
| **Duracao** | 20 semanas | 16 semanas | 8 semanas |
| **Equipe** | 6-8 FTE | 3.5 FTE | 2 FTE |
| **Fontes de Dados** | AD, SharePoint, ELO, Sense | AD, SharePoint, File Server | File Server apenas |
| **Integracao SSO** | AD + SAML | AD | Nenhuma |
| **OCR** | Sim | Sim (Azure DI) | Nao |
| **ML Rules** | Sim | Sim | Nao |
| **Diagramas** | C4 L1-L3 + Deployment | C4 L1-L2 + Pipeline + Quality | Nenhum |
| **Documentacao** | Completa | Completa + Custos + Riscos | Basica |
| **Custo Estimado** | R$ 1.2M - 1.5M | R$ 400K - 600K | R$ 150K - 200K |

---

## Navegacao Rapida

### Proposta Original (v1.0)
- [Proposta de Solucao](./proposals/original/SOLUTIONS_PROPOSAL.md) - Especificacao tecnica
- [Proposta Comercial](./proposals/original/PROPOSTA_TECNICA_COMERCIAL.md) - Documento comercial completo
- [Indice de Diagramas](./proposals/original/diagrams/INDEX.md) - Visualizacao dos diagramas C4

### Proposta LEAN (v2.1) - Recomendada
- [Proposta Completa](./proposals/lean/SOLUTIONS_PROPOSAL_COMPLETE_LEAN.md) - Especificacao tecnica LEAN
- [Custos de Infraestrutura](./proposals/lean/INFRASTRUCTURE_COSTS.md) - Analise detalhada de custos
- [Matriz de Riscos](./proposals/lean/RISK_MATRIX.md) - Analise de riscos e mitigacoes
- [Diagramas](./proposals/lean/diagrams/README.md) - Documentacao dos diagramas

### Proposta MVP
- [Proposta MVP](./proposals/mvp/SOLUTIONS_PROPOSAL_MVP.md) - Versao minimalista para validacao

---

## Recomendacao

**Para a maioria dos cenarios, recomendamos a Proposta LEAN (v2.1)** por oferecer:

1. **Equilibrio custo-beneficio**: Equipe reduzida sem comprometer qualidade
2. **Time-to-market otimizado**: 16 semanas com entregas incrementais
3. **Documentacao completa**: Inclui analise de custos e riscos
4. **Abordagem Shift-Left**: Qualidade integrada desde o inicio
5. **Uso de IA para produtividade**: Claude Code para desenvolvimento acelerado

A Proposta Original e indicada para cenarios que exigem:
- Integracao com sistema ELO e Sense desde o inicio
- Equipe maior com especializacoes dedicadas
- Diagramas de deployment mais detalhados

A Proposta MVP e indicada para:
- Validacao rapida do conceito
- Orcamento muito limitado
- Prova de conceito antes de investimento maior

---

**Ultima atualizacao:** 22 de Janeiro de 2026
**Projeto:** JurisFinder - ThyssenKrupp
