# Proposta de Solucao MVP: JurisFinder

**Cliente:** ThyssenKrupp
**Projeto:** JurisFinder MVP - Prova de Conceito para Busca Inteligente em Documentos
**Versao:** 1.0
**Data:** 22 de Janeiro de 2026
**Modo:** QUICK (PoC/MVP)

---

## Sumario Executivo

### Problema (Resumo)

A ThyssenKrupp perde processos trabalhistas por nao localizar documentos a tempo. A busca manual em multiplas fontes e ineficiente e arriscada.

### Proposta MVP

Desenvolver um **PoC funcional** do JurisFinder em **10-12 semanas** para:

1. **Validar a viabilidade tecnica** da busca semantica com RAG
2. **Demonstrar valor rapido** com 1-2 fontes de dados
3. **Obter feedback real** dos usuarios antes de investir na solucao completa
4. **Reduzir risco** de investimento em solucao que pode nao atender

### Numeros-Chave (MVP vs Completo)

| Metrica | MVP | Solucao Completa | Reducao |
|---------|-----|------------------|---------|
| **Prazo** | 10-12 semanas | 20 semanas | **50%** |
| **Equipe** | 2-3 profissionais | 5-6 profissionais | **55%** |
| **Custo Desenvolvimento** | R$ 180.000 - R$ 240.000 | R$ 702.500 | **66-74%** |
| **Custo Infra/mes** | R$ 3.000 - R$ 5.000 | R$ 11.000 | **55-73%** |

### Recomendacao

| | |
|---|---|
| [X] **GO** | |
| [ ] NO-GO | |
| [ ] CONDITIONAL | |

**Justificativa:** MVP de baixo risco permite validar hipoteses criticas (qualidade RAG, aceitacao usuarios) antes do investimento completo. Se MVP falhar, perda limitada. Se MVP suceder, base solida para evolucao.

---

## 1. Escopo do MVP

### 1.1 O que ENTRA no MVP

| ID | Funcionalidade | Justificativa |
|----|----------------|---------------|
| RF01 | **Busca Inteligente** (semantica) | Core value - demonstra o diferencial da solucao |
| RF02 | **Indexacao de 1-2 fontes** | Minimo para validar pipeline RAG |
| - | Interface web basica | Necessario para usuarios testarem |
| - | Autenticacao simples | Seguranca minima (pode ser user/password local) |

### 1.2 O que SAI do MVP (Fase 2+)

| ID | Funcionalidade | Motivo da Exclusao |
|----|----------------|-------------------|
| RF03 | Controle de acesso granular (AD/LDAP) | Complexidade; usar auth simples no MVP |
| RF04 | Identificacao automatica de docs obrigatorios | Requer regras de negocio ainda nao definidas |
| RF05 | Preparacao e exportacao avancada | Basico suficiente; download direto |
| RF06 | Integracao ELO | API nao confirmada; fora do escopo inicial |
| - | Conector Sense | Sistema legado complexo; priorizar fontes mais simples |
| - | OCR avancado | Focar em documentos digitais primeiro |
| - | Reranker sofisticado | Otimizacao posterior |

### 1.3 Fontes de Dados Priorizadas

**Opcao A (Recomendada):** SharePoint Online
- API bem documentada (Graph API)
- Facil acesso
- Representa boa parte dos documentos

**Opcao B (Alternativa):** Pastas de Rede (File Server)
- Depende de conectividade VPN/rede
- Mais complexo mas factivel

**Decisao:** Iniciar com **uma fonte** e adicionar segunda apenas se houver tempo.

---

## 2. Arquitetura Simplificada

### 2.1 Diagrama MVP

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         JURISFINDER MVP                                      │
└─────────────────────────────────────────────────────────────────────────────┘

                              ┌─────────────────┐
                              │    Usuario      │
                              │   (Browser)     │
                              └────────┬────────┘
                                       │
                                       │ HTTPS
                                       ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                    FRONTEND (Streamlit)                              │   │
│  │  - Interface de busca simples                                        │   │
│  │  - Exibicao de resultados                                            │   │
│  │  - Download de documentos                                            │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                    │                                        │
│                                    │ API calls                              │
│                                    ▼                                        │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                    BACKEND (FastAPI - Monolito)                      │   │
│  │                                                                      │   │
│  │  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐           │   │
│  │  │ Search API    │  │ Index API     │  │ Auth API      │           │   │
│  │  │               │  │               │  │ (basico)      │           │   │
│  │  └───────┬───────┘  └───────┬───────┘  └───────────────┘           │   │
│  │          │                  │                                       │   │
│  │          ▼                  ▼                                       │   │
│  │  ┌─────────────────────────────────────────────────────────────┐   │   │
│  │  │                    RAG Pipeline                              │   │   │
│  │  │  - Embeddings (OpenAI)                                       │   │   │
│  │  │  - Busca vetorial (Qdrant)                                   │   │   │
│  │  │  - Chunking simples                                          │   │   │
│  │  └─────────────────────────────────────────────────────────────┘   │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                    │                                        │
│              ┌─────────────────────┼─────────────────────┐                 │
│              │                     │                     │                  │
│              ▼                     ▼                     ▼                  │
│  ┌───────────────────┐ ┌───────────────────┐ ┌───────────────────┐         │
│  │   Qdrant (Docker) │ │   SQLite/Postgres │ │   File Storage    │         │
│  │   (Vectors)       │ │   (Metadata)      │ │   (Local/S3)      │         │
│  └───────────────────┘ └───────────────────┘ └───────────────────┘         │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ Indexacao
                                    ▼
                          ┌───────────────────┐
                          │   SharePoint      │
                          │   (Graph API)     │
                          └───────────────────┘
```

### 2.2 Stack Tecnologica MVP

| Camada | MVP | Completo | Justificativa Simplificacao |
|--------|-----|----------|----------------------------|
| **Frontend** | Streamlit | React + TypeScript | Desenvolvimento 5x mais rapido |
| **Backend** | FastAPI (monolito) | Microservicos | Menos complexidade operacional |
| **Vector DB** | Qdrant (Docker local) | Qdrant Cloud | Custo zero; suficiente para PoC |
| **Database** | SQLite | PostgreSQL | Zero config; dados pequenos |
| **Queue** | Sem fila (sync) | Celery + Redis | Indexacao pode ser manual/agendada |
| **Auth** | Basic Auth | SSO/SAML | Suficiente para validacao |
| **Deploy** | Docker Compose | Kubernetes | Operacao simplificada |
| **Embeddings** | OpenAI text-embedding-3-small | text-embedding-3-large | 5x mais barato, qualidade OK |
| **LLM** | GPT-4o-mini | GPT-4o | 10x mais barato para query parsing |

### 2.3 Decisoes Arquiteturais MVP

#### ADR-001: Monolito vs Microservicos

**Decisao:** Monolito FastAPI

**Justificativa:**
- Equipe pequena (2-3 pessoas)
- Timeline curto (10-12 semanas)
- Complexidade reduzida de deploy
- Facil refatorar para microservicos depois

#### ADR-002: Streamlit vs React

**Decisao:** Streamlit para frontend

**Justificativa:**
- Python end-to-end (mesma linguagem do backend)
- Prototipagem rapida (dias vs semanas)
- Suficiente para validar UX basico
- Migracao para React na fase 2

#### ADR-003: Modelo de Embeddings

**Decisao:** text-embedding-3-small (1536 dimensoes)

**Justificativa:**
- Custo 5x menor que text-embedding-3-large
- Qualidade suficiente para PoC (87% vs 92% no MTEB)
- Upgrade facil para modelo maior depois

---

## 3. Pipeline RAG Simplificado

### 3.1 Indexacao

```
┌───────────────────────────────────────────────────────────────────────────┐
│                        PIPELINE INDEXACAO MVP                             │
└───────────────────────────────────────────────────────────────────────────┘

   SharePoint         Extrair           Chunking           Embedding
   ┌─────────┐       ┌─────────┐       ┌─────────┐       ┌─────────┐
   │ Lista   │──────►│ Texto   │──────►│ 500 tok │──────►│ 1536d   │───► Qdrant
   │ Docs    │       │ (PDF,   │       │ overlap │       │ OpenAI  │
   │         │       │  DOCX)  │       │ 50 tok  │       │         │
   └─────────┘       └─────────┘       └─────────┘       └─────────┘

Simplificacoes:
- Sem OCR (apenas docs digitais)
- Chunking fixo (sem analise semantica)
- Processamento sincrono (sem fila)
```

### 3.2 Busca

```
┌───────────────────────────────────────────────────────────────────────────┐
│                         PIPELINE BUSCA MVP                                │
└───────────────────────────────────────────────────────────────────────────┘

   Query              Embedding          Busca             Resultados
   Usuario            Query              Vetorial          Top-K
   ┌─────────┐       ┌─────────┐       ┌─────────┐       ┌─────────┐
   │ "docs   │──────►│ 1536d   │──────►│ Qdrant  │──────►│ Top 10  │
   │  EPI    │       │ OpenAI  │       │ search  │       │ docs    │
   │  Joao"  │       │         │       │         │       │         │
   └─────────┘       └─────────┘       └─────────┘       └─────────┘

Simplificacoes:
- Sem busca hibrida (apenas vetorial)
- Sem reranking (confiamos no embedding)
- Sem query parsing com LLM
```

### 3.3 Custos de AI/ML (MVP)

| Componente | Modelo | Custo Estimado/mes |
|------------|--------|-------------------|
| **Embeddings** | text-embedding-3-small | ~$50-100 |
| **LLM (opcional)** | GPT-4o-mini | ~$20-50 |
| **Total AI/mes** | | **$70 - $150** (~R$ 400 - R$ 900) |

---

## 4. Cronograma MVP

### 4.1 Timeline (10-12 semanas)

```
              SEMANA  1   2   3   4   5   6   7   8   9   10  11  12
            ┌─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┐
 SETUP      │█████│█████│     │     │     │     │     │     │     │     │     │     │
 & DESIGN   │     │     │     │     │     │     │     │     │     │     │     │     │
            ├─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┤
 RAG        │     │█████│█████│█████│█████│     │     │     │     │     │     │     │
 PIPELINE   │     │     │     │     │     │     │     │     │     │     │     │     │
            ├─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┤
 CONECTOR   │     │     │     │█████│█████│█████│     │     │     │     │     │     │
 SHAREPOINT │     │     │     │     │     │     │     │     │     │     │     │     │
            ├─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┤
 FRONTEND   │     │     │     │     │█████│█████│█████│█████│     │     │     │     │
 STREAMLIT  │     │     │     │     │     │     │     │     │     │     │     │     │
            ├─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┤
 INTEGRACAO │     │     │     │     │     │     │     │█████│█████│█████│     │     │
 & TESTES   │     │     │     │     │     │     │     │     │     │     │     │     │
            ├─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┤
 PILOTO     │     │     │     │     │     │     │     │     │     │█████│█████│█████│
 & FEEDBACK │     │     │     │     │     │     │     │     │     │     │     │     │
            └─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┴─────┘

            ◄──────── Desenvolvimento ────────►◄─── Validacao ───►
```

### 4.2 Entregaveis por Fase

| Fase | Semanas | Entregaveis |
|------|---------|-------------|
| **1. Setup & Design** | 1-2 | Ambiente, arquitetura, backlog |
| **2. RAG Pipeline** | 2-5 | Indexacao, embeddings, busca funcionando |
| **3. Conector** | 4-6 | Integracao SharePoint completa |
| **4. Frontend** | 5-8 | Interface Streamlit funcional |
| **5. Integracao** | 8-10 | Sistema end-to-end, testes |
| **6. Piloto** | 10-12 | Usuarios reais, coleta feedback, ajustes |

### 4.3 Marcos (Milestones)

| Marco | Semana | Criterio de Aceite |
|-------|--------|-------------------|
| **M1: Pipeline RAG funcional** | 5 | Busca retorna resultados relevantes em dados de teste |
| **M2: Integracao SharePoint** | 6 | Documentos reais indexados e buscaveis |
| **M3: MVP Funcional** | 9 | Sistema end-to-end funcionando |
| **M4: Piloto Concluido** | 12 | Feedback de 5+ usuarios, metricas coletadas |

---

## 5. Equipe e Custos

### 5.1 Composicao da Equipe MVP

| Papel | Quantidade | Alocacao | Responsabilidades |
|-------|------------|----------|-------------------|
| **Tech Lead / Full Stack** | 1 | 100% | Arquitetura, RAG pipeline, backend, frontend |
| **Backend Developer** | 1 | 100% | Conectores, APIs, integracao |
| **DevOps (parcial)** | 1 | 25% | Docker, deploy, ambiente |

**Total FTE:** ~2.25 pessoas

### 5.2 Estimativa de Custos MVP

#### Desenvolvimento

| Item | Quantidade | Valor Unitario | Total |
|------|------------|----------------|-------|
| Tech Lead | 3 meses | R$ 35.000/mes | R$ 105.000 |
| Backend Developer | 3 meses | R$ 25.000/mes | R$ 75.000 |
| DevOps (25%) | 3 meses | R$ 6.250/mes | R$ 18.750 |
| **Subtotal Desenvolvimento** | | | **R$ 198.750** |
| Contingencia (10%) | | | R$ 19.875 |
| **Total Desenvolvimento** | | | **R$ 218.625** |

#### Infraestrutura (Mensal - MVP)

| Item | Especificacao | Custo/mes |
|------|---------------|-----------|
| VM/Container (Azure/AWS) | 2 vCPU, 8GB RAM | R$ 800 |
| Storage | 100GB SSD | R$ 100 |
| OpenAI API | Embeddings + queries | R$ 500 |
| Dominio + SSL | - | R$ 50 |
| **Total Infraestrutura/mes** | | **R$ 1.450** |

*Nota: Qdrant roda em container local, sem custo adicional*

#### Comparativo de Custos

| Categoria | MVP | Completo | Economia |
|-----------|-----|----------|----------|
| Desenvolvimento | R$ 218.625 | R$ 702.500 | **R$ 483.875 (69%)** |
| Infra (3 meses) | R$ 4.350 | R$ 33.000 | **R$ 28.650 (87%)** |
| **Total MVP** | **R$ 222.975** | R$ 735.500 | **R$ 512.525 (70%)** |

---

## 6. Analise de Trade-offs

### 6.1 O que se PERDE no MVP

| Aspecto | Impacto | Mitigacao |
|---------|---------|-----------|
| **Interface rica (React)** | UX basica, menos intuitiva | Streamlit suficiente para validacao; React na fase 2 |
| **Busca hibrida** | Menor precisao em termos tecnicos | Embeddings ainda capturam semantica; adicionar depois |
| **Reranking** | Ranking pode ser subotimo | Qualidade do embedding compensa; medir e ajustar |
| **Multiplas fontes** | Apenas 1-2 fontes | Priorizar fonte mais importante; expandir depois |
| **Controle de acesso AD** | Sem permissoes granulares | Auth basico OK para piloto controlado |
| **Alta disponibilidade** | Single point of failure | Aceitavel para PoC; nao e producao critica |
| **Escalabilidade** | Limite de ~100k docs | Suficiente para PoC; arquitetura permite crescer |

### 6.2 O que se GANHA no MVP

| Aspecto | Beneficio |
|---------|-----------|
| **Time-to-value** | Validacao em 3 meses vs 5 meses |
| **Risco reduzido** | Investimento 70% menor para testar hipoteses |
| **Feedback rapido** | Usuarios reais usando em 10 semanas |
| **Flexibilidade** | Pivotar se necessario com perda minima |
| **Aprendizado** | Entender problemas reais antes de escalar |
| **Base para evolucao** | Codigo reutilizavel na solucao completa |

### 6.3 Matriz de Decisao

| Criterio | Peso | MVP | Completo |
|----------|------|-----|----------|
| Custo | 25% | 9 | 3 |
| Prazo | 25% | 9 | 5 |
| Risco | 20% | 8 | 5 |
| Funcionalidades | 15% | 5 | 9 |
| Qualidade Final | 15% | 6 | 9 |
| **Score Ponderado** | 100% | **7.55** | **5.70** |

**Conclusao:** MVP e a melhor estrategia para o momento atual.

---

## 7. Criterios de Sucesso do MVP

### 7.1 Metricas Tecnicas

| Metrica | Meta MVP | Como Medir |
|---------|----------|------------|
| **Recall@10** | > 75% | Queries de teste com ground truth |
| **Latencia P95** | < 8 segundos | Logs de API |
| **Uptime** | > 95% (horario comercial) | Monitoramento basico |
| **Docs indexados** | > 10.000 | Contagem no Qdrant |

### 7.2 Metricas de Negocio

| Metrica | Meta MVP | Como Medir |
|---------|----------|------------|
| **Usuarios testando** | >= 5 advogados | Registro de uso |
| **Buscas realizadas** | >= 100 no piloto | Logs |
| **Satisfacao** | >= 3.5/5.0 | Questionario |
| **Docs encontrados que nao achavam antes** | >= 3 casos | Feedback qualitativo |

### 7.3 Criterios GO/NO-GO para Fase 2

**GO para Fase 2 (solucao completa) se:**
- [ ] Recall@10 >= 75%
- [ ] Satisfacao usuarios >= 3.5/5.0
- [ ] Ao menos 1 caso de documento encontrado que salvaria processo
- [ ] Nenhum bloqueio tecnico critico identificado

**NO-GO (pivotar ou cancelar) se:**
- Recall@10 < 50% mesmo apos ajustes
- Usuarios nao veem valor (satisfacao < 2.5)
- Custos de API maiores que estimado (inviabiliza escala)

---

## 8. Riscos do MVP

| ID | Risco | Prob. | Impacto | Mitigacao |
|----|-------|-------|---------|-----------|
| R1 | Qualidade embeddings insuficiente | Media | Alto | Testar com subset; trocar modelo se necessario |
| R2 | SharePoint API bloqueada por TI | Media | Alto | Validar acesso na semana 1; ter file server como backup |
| R3 | Usuarios nao engajam no piloto | Baixa | Alto | Identificar champions; demonstrar valor cedo |
| R4 | Streamlit muito limitado | Baixa | Medio | Aceitar limitacoes; foco em funcionalidade |
| R5 | Custo API maior que esperado | Baixa | Medio | Monitorar uso; cache de queries |

---

## 9. Roadmap: MVP para Solucao Completa

### 9.1 Fases de Evolucao

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         ROADMAP DE EVOLUCAO                                  │
└─────────────────────────────────────────────────────────────────────────────┘

  MVP                 V1.0                V1.5                V2.0
  (3 meses)          (+2 meses)          (+2 meses)          (+3 meses)
  ┌─────────┐        ┌─────────┐        ┌─────────┐        ┌─────────┐
  │ Busca   │───────►│ React   │───────►│ Sense   │───────►│ ELO     │
  │ basica  │        │ UI      │        │ conector│        │ integ   │
  │ 1 fonte │        │ AD auth │        │ Regras  │        │ Analytics│
  │ Streamlit│       │ + fontes│        │ auto    │        │ ML pred │
  └─────────┘        └─────────┘        └─────────┘        └─────────┘

  R$ 220k            +R$ 200k           +R$ 150k           +R$ 200k
  ──────────────────────────────────────────────────────────────────────────────
                                                            Total: ~R$ 770k
```

### 9.2 Features por Versao

| Versao | Features | Investimento Adicional |
|--------|----------|------------------------|
| **MVP** | Busca semantica, 1 conector, UI basica | R$ 220k (base) |
| **V1.0** | React UI, AD/SSO, File Server, busca hibrida | +R$ 200k |
| **V1.5** | Conector Sense, regras automaticas, reranking | +R$ 150k |
| **V2.0** | Integracao ELO, analytics, predicao ML | +R$ 200k |

**Total para solucao completa:** ~R$ 770k (vs R$ 960k original = economia de R$ 190k)

### 9.3 Componentes Reutilizaveis do MVP

| Componente MVP | Reutilizacao em V1+ |
|----------------|---------------------|
| Pipeline RAG (FastAPI) | 90% - adicionar reranking |
| Conector SharePoint | 100% - ja pronto |
| Modelo de dados | 80% - expandir para mais metadados |
| Qdrant setup | 100% - migrar para cloud |
| Testes de qualidade RAG | 100% - base para CI/CD |

---

## 10. Proximos Passos

### 10.1 Acoes Imediatas (GO)

| # | Acao | Responsavel | Prazo |
|---|------|-------------|-------|
| 1 | Confirmar acesso SharePoint (Graph API) | ThyssenKrupp TI | 3 dias |
| 2 | Identificar 5 usuarios para piloto | ThyssenKrupp Juridico | 1 semana |
| 3 | Provisionar conta Azure/OpenAI | Equipe | 3 dias |
| 4 | Definir 50-100 queries de teste | Juridico | 1 semana |
| 5 | Kickoff com equipe | Todos | 1 semana |

### 10.2 Entregaveis da Semana 1

- [ ] Ambiente de desenvolvimento configurado
- [ ] Acesso SharePoint validado
- [ ] Repositorio Git criado
- [ ] Backlog inicial priorizado
- [ ] Arquitetura detalhada documentada

---

## 11. Conclusao

O MVP do JurisFinder representa a **estrategia de menor risco** para validar a solucao antes do investimento completo:

| Aspecto | Valor |
|---------|-------|
| **Investimento** | R$ 220k (70% menos que completo) |
| **Prazo** | 3 meses (50% mais rapido) |
| **Risco** | Limitado - perda maxima controlada |
| **Valor** | Validacao real com usuarios reais |
| **Evolucao** | Base solida para solucao completa |

**Recomendacao Final:** **GO** - Iniciar MVP imediatamente para validar hipoteses e acelerar time-to-value.

---

**Documento gerado em:** 22 de Janeiro de 2026
**Versao:** 1.0
**Autor:** Solution Architect Agent
**Modo:** QUICK (PoC/MVP)
