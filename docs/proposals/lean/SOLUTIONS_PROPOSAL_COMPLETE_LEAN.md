# Proposta de Solucao: JurisFinder (Versao LEAN - AI-Assisted)

**Cliente:** ThyssenKrupp
**Projeto:** JurisFinder - Sistema de Busca Inteligente em Documentos para Processos Trabalhistas
**Versao:** 2.1 LEAN (AI-Assisted)
**Data:** 22 de Janeiro de 2026
**Modo:** STANDARD - Desenvolvimento 100% Assistido por IA

---

## Sumario Executivo

### Contexto

Esta proposta apresenta uma **versao otimizada** do projeto JurisFinder, mantendo o **escopo funcional essencial** da proposta original (versao 1.0), porem com uma equipe reduzida e desenvolvimento 100% assistido por ferramentas de IA generativa (Claude Code, Cursor, GitHub Copilot).

### Problema

A ThyssenKrupp enfrenta desafios criticos na gestao de documentos para processos trabalhistas:

- **Busca manual ineficiente:** Documentos dispersos em multiplas fontes (pastas de rede, SharePoint)
- **Risco juridico elevado:** Ja ocorreram perdas de processos por nao localizar documentos dentro do prazo legal
- **Ausencia de padronizacao:** Processo sem controle de qualidade e rastreabilidade
- **Alto custo operacional:** Tempo significativo gasto em tarefas manuais repetitivas

### Solucao Proposta

Desenvolver o **JurisFinder**, um sistema inteligente de busca e recuperacao de documentos baseado em:

- **RAG (Retrieval-Augmented Generation):** Busca semantica avancada que entende o contexto das solicitacoes
- **Indexacao unificada:** Consolidacao de todas as fontes de documentos em um indice centralizado
- **OCR Avancado:** Processamento de documentos escaneados com Azure Document Intelligence
- **Regras ML:** Identificacao automatica de documentos obrigatorios por tipo de processo
- **Controle de acesso:** Sistema de permissoes integrado com AD/LDAP corporativo

### Numeros-Chave

| Metrica | Proposta Original | Proposta LEAN | Reducao |
|---------|-------------------|---------------|---------|
| **Esforco Total** | 5 meses (20 semanas) | **4 meses (16 semanas)** | 20% |
| **Equipe** | 5-6 profissionais | 3.5 FTE | 36% |
| **Custo Desenvolvimento** | R$ 702.500 | R$ 341.200 | **51%** |
| **Custo Infraestrutura/mes** | R$ 11.000 | R$ 12.300* | -12% |
| **ROI Esperado** | 6-12 meses | 3-6 meses | 50% |
| **Reducao de Tempo (busca)** | 70-85% | 70-85% | - |

*Inclui Azure Document Intelligence (OCR) e ferramentas IA

### Recomendacao

| | |
|---|---|
| [ ] GO | |
| [ ] NO-GO | |
| [X] **CONDITIONAL** | |

**Justificativa:** A solucao e tecnicamente viavel com o modelo LEAN e apresenta **excelente custo-beneficio**. Os gaps remanescentes precisam ser resolvidos antes do inicio:

1. **Regras de negocio:** Definir criterios de selecao de documentos por tipo de processo
2. **Acesso aos sistemas:** Validar conectividade com File Server e SharePoint
3. **Metricas baseline:** Obter tempos e volumes atuais para medir ROI
4. **Dataset ML:** Exemplos de documentos para treinar classificador
5. **Profissionais qualificados:** Equipe com experiencia comprovada em ferramentas AI-assisted
6. **Politica de IA:** Aprovacao do uso de ferramentas de IA generativa no projeto

---

## 1. Analise de Requisitos

### 1.1 Personas

| Persona | Descricao | Necessidades Principais |
|---------|-----------|------------------------|
| **Advogado Interno** | Usuario principal, recebe demandas de processos trabalhistas | Busca rapida, documentos corretos, exportacao organizada |
| **Analista Juridico** | Apoia na analise e organizacao de documentos | Interface intuitiva, filtros avancados, exportacao |
| **Gestor de Seguranca** | Fornece documentos de seguranca do trabalho | Notificacoes de solicitacao, acesso restrito |
| **Administrador RH** | Fornece registros de funcionarios | Controle de acesso, auditoria |
| **Administrador TI** | Gerencia sistema e integracoes | Monitoramento, logs, manutencao |

### 1.2 Requisitos Funcionais

#### RF01 - Busca Inteligente de Documentos
- **Descricao:** O sistema deve permitir busca em linguagem natural (ex: "documentos de EPI do funcionario Joao Silva de 2020 a 2023")
- **Prioridade:** Alta
- **Status LEAN:** Incluido
- **Criterios de Aceite:**
  - Busca semantica retorna documentos relevantes mesmo sem match exato de palavras
  - Tempo de resposta < 5 segundos para consultas tipicas
  - Resultados ranqueados por relevancia

#### RF02 - Indexacao Multi-Fonte
- **Descricao:** Indexar documentos de pastas de rede (File Server) e SharePoint
- **Prioridade:** Alta
- **Status LEAN:** Incluido (File Server + SharePoint)
- **Criterios de Aceite:**
  - Conector para sistemas de arquivos Windows (SMB/CIFS)
  - Conector para SharePoint Online via Graph API
  - Indexacao incremental (apenas documentos novos/alterados)

#### RF03 - Controle de Acesso
- **Descricao:** Validar permissoes do usuario antes de exibir documentos
- **Prioridade:** Alta
- **Status LEAN:** Incluido
- **Criterios de Aceite:**
  - Integracao com Active Directory/LDAP
  - Respeitar permissoes das fontes originais
  - Registro de auditoria de todos os acessos

#### RF04 - Identificacao Automatica de Documentos Obrigatorios (Regras ML)
- **Descricao:** Com base no tipo de processo, sugerir documentos obrigatorios usando ML
- **Prioridade:** Alta
- **Status LEAN:** Incluido com Regras ML
- **Criterios de Aceite:**
  - Cadastro de regras por tipo de processo trabalhista
  - Classificacao automatica de tipos de documento
  - Alerta quando documentos obrigatorios nao forem encontrados
  - Sugestoes de documentos complementares

#### RF05 - Preparacao e Exportacao
- **Descricao:** Organizar documentos selecionados para exportacao
- **Prioridade:** Media
- **Status LEAN:** Incluido
- **Criterios de Aceite:**
  - Agrupamento de documentos em pacotes
  - Geracao de sumario/indice
  - Exportacao em formatos padrao (ZIP, PDF consolidado)

#### RF06 - OCR Avancado
- **Descricao:** Processamento de documentos escaneados com OCR
- **Prioridade:** Media
- **Status LEAN:** Incluido (Azure Document Intelligence)
- **Criterios de Aceite:**
  - Extracao de texto de PDFs escaneados
  - Suporte a imagens (JPG, PNG, TIFF)
  - Integracao com pipeline de indexacao

### 1.3 Requisitos Nao-Funcionais

| ID | Categoria | Requisito | Meta |
|----|-----------|-----------|------|
| RNF01 | Performance | Tempo de busca | < 5 segundos (P95) |
| RNF02 | Performance | Indexacao incremental | < 1 hora/dia |
| RNF03 | Disponibilidade | Uptime | 99.5% (horario comercial) |
| RNF04 | Seguranca | Autenticacao | SSO/SAML com AD corporativo |
| RNF05 | Seguranca | Criptografia | TLS 1.3 em transito, AES-256 em repouso |
| RNF06 | Seguranca | Auditoria | Log de todas as operacoes |
| RNF07 | Escalabilidade | Volume | 1M+ documentos indexados |
| RNF08 | Compliance | LGPD | Dados pessoais protegidos |

### 1.4 Restricoes

1. **Ambiente Cloud:** Aprovado, desde que atenda criterios de seguranca ThyssenKrupp
2. **Dados Sensiveis:** Documentos trabalhistas contem dados pessoais (LGPD)
3. **Rede Corporativa:** Acesso a pastas de rede requer conectividade VPN/ExpressRoute

### 1.5 Premissas

1. ThyssenKrupp fornecera acesso aos repositorios de documentos para desenvolvimento
2. Regras de negocio para documentos obrigatorios serao definidas com equipe juridica
3. Ambiente de homologacao sera disponibilizado com dados anonimizados
4. Equipe de TI ThyssenKrupp apoiara nas integracoes de rede e AD
5. Uso de ferramentas de IA generativa sera aprovado

### 1.6 Gaps Identificados (Requerem Esclarecimento)

| Gap | Impacto | Acao Necessaria |
|-----|---------|-----------------|
| Tempo medio atual do processo | Impossibilita medir ROI | Levantar metricas com equipe juridica |
| Volume mensal de processos | Afeta dimensionamento | Obter historico de 12 meses |
| Dataset para Regras ML | Define precisao do modelo | Coletar exemplos com juridico |
| Criterios de documentos obrigatorios | Core da solucao | Workshop com juridico |
| Amostras de docs escaneados | Validar qualidade OCR | Coletar exemplos representativos |

### 1.7 Escopo Removido (Proposta LEAN)

| Item Removido | Motivo | Alternativa |
|---------------|--------|-------------|
| Integracao ELO | Reducao de escopo | Exportacao manual (ZIP/PDF) |
| Conector Sense | Reducao de escopo | Foco em File Server + SharePoint |

---

## 2. Arquitetura Proposta

### 2.1 Visao Geral

A arquitetura segue o padrao **RAG (Retrieval-Augmented Generation)** combinado com busca hibrida (semantica + keyword), projetada para alta seguranca e integracao com sistemas corporativos.

### 2.2 Diagrama de Contexto (C4 - Level 1)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           CONTEXTO DO SISTEMA                                │
└─────────────────────────────────────────────────────────────────────────────┘

                              ┌─────────────────┐
                              │   Advogado /    │
                              │   Analista      │
                              │   Juridico      │
                              └────────┬────────┘
                                       │
                                       │ Busca documentos
                                       │ Visualiza resultados
                                       │ Exporta pacotes
                                       ▼
┌──────────────┐            ┌─────────────────────┐
│   Active     │◄──────────►│                     │
│   Directory  │   Auth     │    JURISFINDER      │
│              │            │                     │
└──────────────┘            └──────────┬──────────┘
                                       │
                            ┌──────────┴──────────┐
                            │                     │
                            ▼                     ▼
                  ┌─────────────────┐   ┌─────────────────┐
                  │   Pastas de     │   │   SharePoint    │
                  │     Rede        │   │    Online       │
                  │  (File Server)  │   │                 │
                  └─────────────────┘   └─────────────────┘
```

**Diferenca da Proposta Original:** Removidos ELO e Sense do contexto.

### 2.3 Diagrama de Container (C4 - Level 2)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              JURISFINDER LEAN                                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                         FRONTEND (SPA)                               │   │
│  │  React + TypeScript + TailwindCSS                                    │   │
│  │  - Interface de busca                                                │   │
│  │  - Visualizacao de resultados                                        │   │
│  │  - Gestao de pacotes                                                 │   │
│  │  - Alertas de documentos obrigatorios                                │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                    │                                        │
│                                    │ HTTPS/REST                             │
│                                    ▼                                        │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                         API GATEWAY                                  │   │
│  │  Kong / AWS API Gateway                                              │   │
│  │  - Rate limiting                                                     │   │
│  │  - Autenticacao JWT                                                  │   │
│  │  - Logging                                                           │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                    │                                        │
│              ┌─────────────────────┼─────────────────────┐                 │
│              │                     │                     │                  │
│              ▼                     ▼                     ▼                  │
│  ┌───────────────────┐ ┌───────────────────┐ ┌───────────────────┐         │
│  │  SEARCH SERVICE   │ │ INDEXER SERVICE   │ │  EXPORT SERVICE   │         │
│  │  Python/FastAPI   │ │  Python/Celery    │ │  Python/FastAPI   │         │
│  │                   │ │                   │ │                   │         │
│  │ - Query parsing   │ │ - Crawlers        │ │ - Package builder │         │
│  │ - Hybrid search   │ │ - OCR (Azure)     │ │ - PDF generation  │         │
│  │ - Reranking       │ │ - Embeddings      │ │ - ZIP export      │         │
│  │ - Access control  │ │ - Chunking        │ │                   │         │
│  │ - ML Rules        │ │ - Metadata        │ │                   │         │
│  └─────────┬─────────┘ └─────────┬─────────┘ └───────────────────┘         │
│            │                     │                                          │
│            │                     │                                          │
│            ▼                     ▼                                          │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                         DATA LAYER                                   │   │
│  │                                                                      │   │
│  │  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐     │   │
│  │  │   PostgreSQL    │  │   Qdrant        │  │     Redis       │     │   │
│  │  │   (Metadata)    │  │ (Vector Store)  │  │   (Cache/Queue) │     │   │
│  │  │   + ML Rules    │  │                 │  │                 │     │   │
│  │  └─────────────────┘  └─────────────────┘  └─────────────────┘     │   │
│  │                                                                      │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘

                    │                     │
    ┌───────────────┼─────────────────────┼───────────────┐
    │               │                     │               │
    │               ▼                     ▼               │
    │   ┌───────────────────┐ ┌───────────────────┐       │
    │   │   File Server     │ │   SharePoint      │       │
    │   │   Connector       │ │   Connector       │       │
    │   │   (SMB/CIFS)      │ │   (Graph API)     │       │
    │   └───────────────────┘ └───────────────────┘       │
    │                                                      │
    │            CONECTORES DE FONTES DE DADOS             │
    └──────────────────────────────────────────────────────┘

                              │
                              ▼
              ┌───────────────────────────────┐
              │   Azure Document Intelligence │
              │         (OCR Service)         │
              └───────────────────────────────┘
```

### 2.4 Pipeline RAG Detalhado (com OCR)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           PIPELINE DE INDEXACAO                             │
└─────────────────────────────────────────────────────────────────────────────┘

┌───────────┐    ┌───────────┐    ┌───────────┐    ┌───────────┐    ┌─────────┐
│  Fonte    │───►│  Crawler  │───►│   OCR?    │───►│  Parser   │───►│Chunking │
│  (docs)   │    │           │    │  (Azure)  │    │  (text)   │    │(512 tok)│
└───────────┘    └───────────┘    └─────┬─────┘    └───────────┘    └────┬────┘
                                        │                                 │
                                        │ PDF escaneado?                  │
                                        │ Sim -> Azure Doc Intelligence   │
                                        │ Nao -> Parser direto            │
                                        │                                 │
                                        ▼                                 ▼
                              ┌───────────────┐                   ┌───────────┐
                              │ Azure Doc     │                   │ Embedding │
                              │ Intelligence  │                   │  (1536d)  │
                              │ OCR + Layout  │                   └─────┬─────┘
                              └───────────────┘                         │
                                                                        ▼
                                                                 ┌───────────┐
                                                                 │  Qdrant   │
                                                                 │ (vectors) │
                                                                 └───────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                           PIPELINE DE BUSCA                                 │
└─────────────────────────────────────────────────────────────────────────────┘

┌───────────┐    ┌───────────┐    ┌───────────┐    ┌───────────┐    ┌─────────┐
│  Query    │───►│  Query    │───►│  Hybrid   │───►│ Reranker  │───►│ Access  │
│  Usuario  │    │ Embedding │    │  Search   │    │  (Cross)  │    │ Filter  │
└───────────┘    └───────────┘    └───────────┘    └───────────┘    └────┬────┘
                                                                          │
                                        ┌─────────────────────────────────┘
                                        │
                                        ▼
                             ┌─────────────────────┐
                             │   ML Rules Engine   │
                             │ - Tipo de processo  │
                             │ - Docs obrigatorios │
                             │ - Alertas faltantes │
                             └──────────┬──────────┘
                                        │
                                        ▼
                                  ┌───────────┐
                                  │ Resultados│
                                  │ Ranqueados│
                                  │ + Alertas │
                                  └───────────┘
```

### 2.5 Stack Tecnologica

| Camada | Tecnologia | Justificativa |
|--------|------------|---------------|
| **Frontend** | React 18 + TypeScript + TailwindCSS | Produtividade, tipagem, excelente suporte IA |
| **API Gateway** | Kong OSS ou AWS API Gateway | Rate limiting, auth, observabilidade |
| **Backend** | Python 3.12 + FastAPI | Ecossistema ML/AI maduro, excelente suporte IA |
| **Task Queue** | Celery + Redis | Processamento assincrono de indexacao |
| **Vector DB** | Qdrant | Open-source, performance, filtering nativo |
| **Relational DB** | PostgreSQL 16 | Metadata, usuarios, auditoria, ML Rules |
| **Cache** | Redis | Sessoes, cache de queries, filas |
| **Embeddings** | OpenAI text-embedding-3-large | 3072 dimensoes, melhor qualidade semantica |
| **LLM (Query)** | GPT-4o ou Azure OpenAI | Parsing de queries, sugestoes |
| **Reranker** | Cohere Rerank ou Cross-encoder local | Melhora precisao do ranking |
| **OCR** | Azure Document Intelligence | Documentos escaneados, alta precisao |
| **Containerizacao** | Docker + Kubernetes | Escalabilidade, portabilidade |
| **CI/CD** | GitHub Actions | Automacao de deploy |
| **Observabilidade** | Prometheus + Grafana + Loki | Metricas, logs, alertas |

### 2.6 Decisoes Arquiteturais (ADRs)

#### ADR-001: Busca Hibrida (Semantica + Keyword)

**Contexto:** Documentos juridicos possuem termos tecnicos especificos (ex: "ASO", "PPRA", "PCMSO") que precisam de match exato, alem de contexto semantico.

**Decisao:** Implementar busca hibrida combinando:
- **Busca vetorial** (Qdrant) para semantica
- **Busca BM25** (Elasticsearch ou PostgreSQL FTS) para keywords
- **Fusion** com RRF (Reciprocal Rank Fusion)

**Consequencias:**
- (+) Melhor precisao em termos tecnicos
- (+) Captura contexto semantico
- (-) Maior complexidade de infraestrutura

#### ADR-002: Qdrant como Vector Database

**Contexto:** Necessidade de armazenar embeddings de 1M+ documentos com filtragem por metadados.

**Decisao:** Usar Qdrant em vez de Pinecone, Weaviate ou pgvector.

**Justificativa:**
- Open-source (pode rodar on-premises se necessario)
- Performance superior em grandes volumes
- Filtragem nativa por payload (permissoes, datas)
- Menor custo que alternativas managed

**Consequencias:**
- (+) Flexibilidade de deploy
- (+) Custo controlado
- (-) Requer gerenciamento de infraestrutura

#### ADR-003: Chunking por Documento com Overlap

**Contexto:** Documentos variam de 1 pagina a 100+ paginas.

**Decisao:** Implementar chunking inteligente:
- Chunks de 512 tokens com 50 tokens de overlap
- Respeitar limites de secao/paragrafo quando possivel
- Preservar metadados do documento pai em cada chunk

**Consequencias:**
- (+) Contexto preservado entre chunks
- (+) Busca mais precisa em documentos longos
- (-) Aumento no volume de vetores (~3x)

#### ADR-004: Arquitetura de Conectores Plugavel

**Contexto:** Multiplas fontes de dados com diferentes protocolos.

**Decisao:** Criar interface abstrata de Conector com implementacoes especificas:
- `FileServerConnector` (SMB/CIFS)
- `SharePointConnector` (Graph API)

**Consequencias:**
- (+) Facilidade para adicionar novas fontes no futuro
- (+) Testabilidade isolada
- (-) Overhead inicial de design

#### ADR-005: Azure Document Intelligence para OCR

**Contexto:** Muitos documentos trabalhistas sao escaneados (PDFs de imagem).

**Decisao:** Usar Azure Document Intelligence para OCR.

**Justificativa:**
- Alta precisao em documentos brasileiros
- Suporte a layout e tabelas
- Integracao nativa com Azure
- Custo competitivo por pagina

**Consequencias:**
- (+) Alta qualidade de extracao
- (+) Suporte a multiplos formatos
- (-) Custo adicional por pagina processada (~R$ 800/mes)

#### ADR-006: Regras ML para Documentos Obrigatorios

**Contexto:** Cada tipo de processo trabalhista requer documentos especificos.

**Decisao:** Implementar sistema de regras ML com:
- Classificador de tipo de documento (Random Forest / BERT)
- Matriz tipo_processo x documentos_obrigatorios
- Engine de alertas para documentos faltantes

**Consequencias:**
- (+) Automacao de verificacao de completude
- (+) Reducao de erros por documentos faltantes
- (-) Requer dataset de treinamento inicial

---

## 3. Componentes de IA/ML

### 3.1 Visao Geral do Pipeline RAG

O JurisFinder utiliza **RAG (Retrieval-Augmented Generation)** para combinar busca semantica com geracao de respostas contextualizadas.

### 3.2 Modelos e Custos

| Componente | Modelo | Custo Estimado/mes |
|------------|--------|-------------------|
| **Embeddings** | text-embedding-3-large (OpenAI) | ~$200-400 (1M docs inicial + queries) |
| **LLM Query Parsing** | GPT-4o-mini | ~$50-100 |
| **Reranking** | Cohere Rerank v3 | ~$100-200 |
| **OCR** | Azure Document Intelligence | ~$200-500 (volume dependente) |
| **ML Classification** | BERT/Random Forest (local) | Incluso na infra |
| **Total AI/mes** | | **$650 - $1.400 (~R$ 3.500 - R$ 7.500)** |

### 3.3 Fluxo de Processamento

```
                         INDEXACAO
                         =========

┌──────────────────────────────────────────────────────────────────────────┐
│                                                                          │
│  1. CRAWLING           2. OCR (se necessario)   3. PARSING               │
│  ┌─────────────┐       ┌─────────────┐          ┌─────────────┐          │
│  │ Detectar    │──────►│ PDF/Imagem  │─────────►│ Extrair     │          │
│  │ novos/      │       │ escaneado?  │          │ texto       │          │
│  │ alterados   │       │ Azure OCR   │          │             │          │
│  └─────────────┘       └─────────────┘          └──────┬──────┘          │
│                                                        │                 │
│  4. CHUNKING           5. EMBEDDING           6. CLASSIFICATION          │
│  ┌─────────────┐       ┌─────────────┐        ┌─────────────┐            │
│  │ Dividir em  │──────►│ Gerar       │───────►│ Classificar │            │
│  │ chunks de   │       │ vetores     │        │ tipo doc    │            │
│  │ 512 tokens  │       │ 3072d       │        │ (ML Rules)  │            │
│  └─────────────┘       └─────────────┘        └──────┬──────┘            │
│                                                      │                   │
│  7. INDEXING           8. METADATA                   │                   │
│  ┌─────────────┐       ┌─────────────┐               │                   │
│  │ Armazenar   │──────►│ Salvar      │◄──────────────┘                   │
│  │ em Qdrant   │       │ metadados   │                                   │
│  │             │       │ PostgreSQL  │                                   │
│  └─────────────┘       └─────────────┘                                   │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘


                           BUSCA
                           =====

┌──────────────────────────────────────────────────────────────────────────┐
│                                                                          │
│  1. QUERY PARSING      2. EMBEDDING         3. HYBRID SEARCH            │
│  ┌─────────────┐       ┌─────────────┐      ┌─────────────┐             │
│  │ Extrair     │──────►│ Gerar       │─────►│ Busca       │             │
│  │ entidades:  │       │ embedding   │      │ vetorial +  │             │
│  │ nome, data, │       │ da query    │      │ keyword     │             │
│  │ tipo doc    │       │             │      │             │             │
│  └─────────────┘       └─────────────┘      └──────┬──────┘             │
│                                                     │                    │
│  4. FILTERING          5. RERANKING         6. ML RULES                 │
│  ┌─────────────┐       ┌─────────────┐      ┌─────────────┐             │
│  │ Aplicar     │──────►│ Cross-      │─────►│ Verificar   │             │
│  │ permissoes  │       │ encoder     │      │ docs        │             │
│  │ do usuario  │       │ rerank      │      │ obrigatorios│             │
│  └─────────────┘       └─────────────┘      └──────┬──────┘             │
│                                                     │                    │
│  7. ALERTAS            8. RESPONSE                  │                    │
│  ┌─────────────┐       ┌─────────────┐              │                    │
│  │ Gerar       │◄──────┤ Retornar    │◄─────────────┘                    │
│  │ alertas de  │       │ top-k       │                                   │
│  │ docs        │       │ documentos  │                                   │
│  │ faltantes   │       │             │                                   │
│  └─────────────┘       └─────────────┘                                   │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

### 3.4 Estrategia de Embeddings

**Modelo Selecionado:** `text-embedding-3-large` (OpenAI)

| Caracteristica | Valor |
|----------------|-------|
| Dimensoes | 3072 (ou 1536 com reducao) |
| Max Tokens | 8191 |
| Custo | $0.00013/1K tokens |
| Qualidade | SOTA para retrieval |

**Alternativa On-Premises:** Se necessario, usar `BAAI/bge-large-en-v1.5` ou `intfloat/multilingual-e5-large` (menor custo, requer GPU).

### 3.5 Estrategia de Chunking

```python
# Configuracao recomendada
CHUNK_CONFIG = {
    "chunk_size": 512,      # tokens
    "chunk_overlap": 50,    # tokens
    "separator": ["\n\n", "\n", ". ", " "],
    "length_function": "tiktoken",
    "keep_separator": True
}
```

**Documentos especiais:**
- **PDFs escaneados:** OCR (Azure Doc Intelligence) + chunking
- **Planilhas:** Chunk por aba/secao
- **Apresentacoes:** Chunk por slide

### 3.6 Regras ML para Documentos Obrigatorios

```python
# Exemplo de estrutura de regras
ML_RULES_CONFIG = {
    "classifier_model": "bert-base-portuguese-cased",
    "document_types": [
        "ASO",           # Atestado de Saude Ocupacional
        "PPRA",          # Programa de Prevencao de Riscos
        "PCMSO",         # Programa de Controle Medico
        "Ficha_EPI",     # Ficha de EPI
        "Contrato",      # Contrato de Trabalho
        "Rescisao",      # Termo de Rescisao
        # ...
    ],
    "process_type_rules": {
        "Acidente_Trabalho": ["ASO", "CAT", "Ficha_EPI", "PPRA"],
        "Rescisao_Indireta": ["Contrato", "Rescisao", "Folha_Ponto"],
        "Horas_Extras": ["Folha_Ponto", "Contrato", "Acordo_Compensacao"],
        # ...
    }
}
```

### 3.7 Metricas de Qualidade

| Metrica | Meta | Medicao |
|---------|------|---------|
| **Recall@10** | > 90% | % de documentos relevantes no top 10 |
| **MRR** | > 0.7 | Mean Reciprocal Rank |
| **Latencia P95** | < 3s | Tempo de resposta |
| **OCR Accuracy** | > 95% | Precisao da extracao de texto |
| **ML Classification F1** | > 85% | Precisao da classificacao de docs |
| **User Satisfaction** | > 4.0/5.0 | Feedback explicito |

---

## 4. Integracao de Sistemas

### 4.1 Mapa de Integracoes (LEAN)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          MAPA DE INTEGRACOES                                │
└─────────────────────────────────────────────────────────────────────────────┘

┌───────────────────┐         ┌───────────────────┐         ┌───────────────────┐
│  ACTIVE DIRECTORY │         │    SHAREPOINT     │         │   FILE SERVER     │
│                   │         │     ONLINE        │         │                   │
│  Protocolo: LDAP  │         │ Protocolo: REST   │         │ Protocolo: SMB    │
│  Porta: 389/636   │         │ API: Graph API    │         │ Porta: 445        │
│  Uso: Auth + ACL  │         │ Auth: OAuth2      │         │ Auth: Kerberos    │
└─────────┬─────────┘         └─────────┬─────────┘         └─────────┬─────────┘
          │                             │                             │
          │                             │                             │
          └─────────────────────────────┼─────────────────────────────┘
                                        │
                                        ▼
                              ┌───────────────────┐
                              │                   │
                              │    JURISFINDER    │
                              │                   │
                              └─────────┬─────────┘
                                        │
                                        ▼
                              ┌───────────────────┐
                              │   Azure Document  │
                              │   Intelligence    │
                              │   (OCR Service)   │
                              └───────────────────┘
```

**Diferenca da Proposta Original:** Removidos ELO e Sense do mapa de integracoes.

### 4.2 Conector SharePoint Online

```python
# Exemplo de implementacao
from msgraph import GraphServiceClient
from azure.identity import ClientSecretCredential

class SharePointConnector:
    def __init__(self, tenant_id: str, client_id: str, client_secret: str):
        self.credential = ClientSecretCredential(
            tenant_id=tenant_id,
            client_id=client_id,
            client_secret=client_secret
        )
        self.client = GraphServiceClient(self.credential)

    async def list_documents(self, site_id: str, drive_id: str) -> list[Document]:
        """Lista documentos de um drive SharePoint."""
        items = await self.client.sites[site_id].drives[drive_id].items.get()
        return [self._map_to_document(item) for item in items.value]

    async def download_content(self, drive_id: str, item_id: str) -> bytes:
        """Baixa conteudo de um documento."""
        return await self.client.drives[drive_id].items[item_id].content.get()

    def _map_to_document(self, item) -> Document:
        """Mapeia item do SharePoint para modelo interno."""
        return Document(
            id=item.id,
            name=item.name,
            path=item.parent_reference.path,
            modified_at=item.last_modified_date_time,
            size=item.size,
            source="sharepoint"
        )
```

### 4.3 Conector File Server

```python
# Exemplo usando smbprotocol
from smbprotocol.connection import Connection
from smbprotocol.session import Session
from smbprotocol.tree import TreeConnect
import uuid

class FileServerConnector:
    def __init__(self, server: str, share: str, username: str, password: str):
        self.server = server
        self.share = share
        self.connection = Connection(uuid.uuid4(), server, 445)
        self.connection.connect()
        self.session = Session(self.connection, username, password)
        self.session.connect()
        self.tree = TreeConnect(self.session, f"\\\\{server}\\{share}")
        self.tree.connect()

    def list_documents(self, path: str, recursive: bool = True) -> list[Document]:
        """Lista documentos recursivamente."""
        documents = []
        # Implementacao com walk recursivo
        for file_info in self._walk(path, recursive):
            documents.append(self._map_to_document(file_info))
        return documents

    def read_content(self, path: str) -> bytes:
        """Le conteudo de arquivo."""
        with self.tree.open(path, "rb") as f:
            return f.read()

    def _map_to_document(self, file_info) -> Document:
        """Mapeia arquivo SMB para modelo interno."""
        return Document(
            id=file_info.file_id,
            name=file_info.name,
            path=file_info.path,
            modified_at=file_info.last_write_time,
            size=file_info.file_size,
            source="file_server"
        )
```

### 4.4 Servico de OCR (Azure Document Intelligence)

```python
from azure.ai.documentintelligence import DocumentIntelligenceClient
from azure.core.credentials import AzureKeyCredential

class OCRService:
    def __init__(self, endpoint: str, key: str):
        self.client = DocumentIntelligenceClient(
            endpoint=endpoint,
            credential=AzureKeyCredential(key)
        )

    async def extract_text(self, document_bytes: bytes) -> str:
        """Extrai texto de documento escaneado."""
        poller = await self.client.begin_analyze_document(
            "prebuilt-read",
            document_bytes
        )
        result = await poller.result()

        # Concatenar texto de todas as paginas
        full_text = ""
        for page in result.pages:
            for line in page.lines:
                full_text += line.content + "\n"

        return full_text

    def is_scanned_pdf(self, document_bytes: bytes) -> bool:
        """Verifica se PDF e escaneado (imagem) ou texto."""
        # Implementacao com PyMuPDF ou pdfminer
        # Retorna True se PDF nao tem texto extraivel
        pass
```

---

## 5. Estimativa de Esforco

### 5.1 Cronograma de Alto Nivel (16 Semanas)

```
                    MES 1       MES 2       MES 3       MES 4
                 ┌─────────┬─────────┬─────────┬─────────┐
 DISCOVERY       │████     │         │         │         │
 & SETUP         │ S1-S2   │         │         │         │
                 ├─────────┼─────────┼─────────┼─────────┤
 RAG CORE        │    █████│█████████│         │         │
 PIPELINE + OCR  │   S3-S8 │         │         │         │
                 ├─────────┼─────────┼─────────┼─────────┤
 CONECTORES      │         │    █████│██       │         │
                 │         │  S9-S10 │         │         │
                 ├─────────┼─────────┼─────────┼─────────┤
 FRONTEND        │         │         │█████████│██       │
                 │         │         │ S11-S13 │         │
                 ├─────────┼─────────┼─────────┼─────────┤
 INTEGRACOES     │         │         │         │████     │
 (AD/SSO)        │         │         │         │  S14    │
                 ├─────────┼─────────┼─────────┼─────────┤
 QA CONTINUO     │█████████│█████████│█████████│█████████│
                 │   S1-S16 (Shift-Left Testing)        │
                 ├─────────┼─────────┼─────────┼─────────┤
 TESTES &        │         │         │         │    ████ │
 GO-LIVE         │         │         │         │ S15-S16 │
                 └─────────┴─────────┴─────────┴─────────┘

Legenda: TL=Tech Lead, FS=Full Stack, ML=ML Engineer, QA=QA Tester
```

### 5.2 Detalhamento por Fase

| Fase | Duracao | Descricao |
|------|---------|-----------|
| **1. Discovery & Setup** | 2 semanas | Levantamento, ambiente, arquitetura, workshop ML |
| **2. Core RAG Pipeline** | 6 semanas | Indexador, OCR, embeddings, busca, reranking, ML Rules |
| **3. Conectores** | 2 semanas | FileServer, SharePoint |
| **4. Frontend** | 3 semanas | Interface de busca, resultados, alertas, exportacao |
| **5. Integracoes** | 1 semana | AD/SSO, ACLs |
| **6. Testes & Deploy** | 2 semanas | QA, performance, go-live |
| **Total** | **16 semanas (4 meses)** | |

### 5.3 Composicao da Equipe (LEAN vs Original)

| Papel | Original | LEAN | Observacao |
|-------|----------|------|------------|
| **Tech Lead / Arquiteto** | 100% (5m) | 50% (4m) | Code review, decisoes |
| **Developer Senior Backend** | 200% (5m) | 0% | Absorvido pelo Full Stack |
| **Frontend Developer** | 100% (5m) | 0% | Absorvido pelo Full Stack |
| **Full Stack Senior** | 0% | 100% (4m) | Cobre front + back + devops |
| **ML Engineer** | 50% (5m) | 100% (4m) | RAG + OCR + ML Rules |
| **DevOps/SRE** | 50% (5m) | 0% | Absorvido pelo Full Stack + TL |
| **QA Engineer** | 50% (3m) | 50% (4m) | Shift-left, todo projeto |
| **Total FTE** | **5.5** | **3.5** | **-36%** |

### 5.4 Responsabilidades por Papel (LEAN)

#### Tech Lead (50% - 0.5 FTE)

| Responsabilidade | Tempo | Observacao |
|------------------|-------|------------|
| Decisoes arquiteturais | 20% | ADRs, stack, padroes |
| Code review rigoroso | 40% | **Critico** - valida codigo gerado por IA |
| Mentoria e alinhamento | 20% | Sessoes com equipe |
| Stakeholder management | 20% | Reunioes com cliente |

#### Full Stack Senior (100% - 1 FTE)

| Responsabilidade | Tempo | AI-Assisted? |
|------------------|-------|--------------|
| Backend Python/FastAPI | 35% | Sim - alta produtividade |
| Frontend React/TypeScript | 30% | Sim - componentes padrao |
| Conectores (SharePoint, SMB) | 15% | Sim - APIs documentadas |
| DevOps/Infra | 15% | Sim - IaC templates |
| Documentacao | 5% | Sim - alta produtividade |

#### ML Engineer (100% - 1 FTE)

| Responsabilidade | Tempo | AI-Assisted? |
|------------------|-------|--------------|
| Pipeline RAG (core) | 30% | Parcial - logica critica |
| OCR Integration | 15% | Sim - Azure SDK |
| Embeddings e chunking | 20% | Parcial - experimentacao |
| Regras ML | 20% | Parcial - requer dataset |
| Metricas de qualidade | 15% | Sim - dashboards |

#### QA (50% - 0.5 FTE)

| Responsabilidade | Tempo | AI-Assisted? |
|------------------|-------|--------------|
| Testes manuais exploratorios | 30% | Nao |
| Testes automatizados (E2E) | 35% | Sim - Playwright/Cypress |
| Testes de integracao | 20% | Parcial |
| Validacao de qualidade RAG | 15% | Parcial |

### 5.5 Estimativa de Custos

#### Desenvolvimento LEAN (4 meses)

| Item | Quantidade | Valor Unitario | Total |
|------|------------|----------------|-------|
| Tech Lead (50%) | 4 meses | R$ 17.500/mes | R$ 70.000 |
| Full Stack Senior | 4 meses | R$ 28.000/mes | R$ 112.000 |
| ML Engineer | 4 meses | R$ 30.000/mes | R$ 120.000 |
| QA (50%) | 4 meses | R$ 7.000/mes | R$ 28.000 |
| **Subtotal Equipe** | | | **R$ 330.000** |

#### Ferramentas de IA

| Item | Quantidade | Valor Unitario | Total (4 meses) |
|------|------------|----------------|-----------------|
| Claude Code (Pro/Team) | 3.5 devs | ~R$ 500/mes | R$ 7.000 |
| Cursor Pro | 3.5 devs | ~R$ 100/mes | R$ 1.400 |
| GitHub Copilot Enterprise | 3.5 devs | ~R$ 200/mes | R$ 2.800 |
| **Subtotal Ferramentas IA** | | | **R$ 11.200** |

#### Comparativo de Desenvolvimento

| Item | Original (5 meses) | LEAN (4 meses) | Economia |
|------|----------|------|----------|
| Tech Lead | R$ 175.000 | R$ 70.000 | R$ 105.000 |
| Backend Senior (2x) | R$ 250.000 | R$ 0 | R$ 250.000 |
| Frontend Developer | R$ 110.000 | R$ 0 | R$ 110.000 |
| Full Stack Senior | R$ 0 | R$ 112.000 | -R$ 112.000 |
| ML Engineer | R$ 75.000 | R$ 120.000 | -R$ 45.000 |
| DevOps | R$ 62.500 | R$ 0 | R$ 62.500 |
| QA | R$ 30.000 | R$ 28.000 | R$ 2.000 |
| Ferramentas IA | R$ 0 | R$ 11.200 | -R$ 11.200 |
| **Total Desenvolvimento** | **R$ 702.500** | **R$ 341.200** | **R$ 361.300 (51%)** |

#### Infraestrutura (Mensal - Producao)

| Item | Especificacao | Original | LEAN |
|------|---------------|----------|------|
| Kubernetes (AKS/EKS) | 3 nodes, 8 vCPU, 32GB | R$ 3.500 | R$ 3.500 |
| PostgreSQL Managed | 4 vCPU, 16GB, 500GB | R$ 1.500 | R$ 1.500 |
| Qdrant Cloud | 2M vectors, 3 replicas | R$ 2.000 | R$ 2.000 |
| Redis Managed | 4GB | R$ 500 | R$ 500 |
| Storage (Blob/S3) | 1TB | R$ 200 | R$ 200 |
| Network/CDN | - | R$ 300 | R$ 300 |
| OpenAI/Azure OpenAI | Embeddings + LLM | R$ 2.500 | R$ 2.500 |
| **Azure Doc Intelligence** | OCR avancado | R$ 0 | R$ 800 |
| Monitoring (Datadog/Grafana) | - | R$ 500 | R$ 500 |
| **Ferramentas IA (operacao)** | Claude, Copilot (1 dev) | R$ 0 | R$ 500 |
| **Total Infraestrutura/mes** | | **R$ 11.000** | **R$ 12.300** |

#### Resumo de Custos (Primeiro Ano)

| Categoria | Original (5 meses) | LEAN (4 meses) | Delta |
|-----------|----------|------|-------|
| Desenvolvimento | R$ 702.500 | R$ 341.200 | **-R$ 361.300** |
| Infraestrutura (12 meses) | R$ 132.000 | R$ 147.600 | +R$ 15.600 |
| Contingencia (15%) | R$ 125.175 | R$ 73.320 | -R$ 51.855 |
| **Total Primeiro Ano** | **R$ 959.675** | **R$ 562.120** | **-R$ 397.555 (41%)** |

---

## 6. Estrategia de Qualidade

### 6.1 Piramide de Testes

```
                        ┌───────────┐
                        │    E2E    │  10%
                        │(Playwright)│
                    ┌───┴───────────┴───┐
                    │   Integration     │  20%
                    │    (pytest)       │
                ┌───┴───────────────────┴───┐
                │         Unit Tests        │  70%
                │      (pytest + vitest)    │
                └───────────────────────────┘
```

### 6.2 Cobertura de Testes

| Camada | Meta | Ferramenta |
|--------|------|------------|
| Backend Unit | > 85% | pytest + coverage |
| Frontend Unit | > 80% | vitest + testing-library |
| Integracao | Fluxos criticos | pytest + testcontainers |
| E2E | Happy paths | Playwright |
| Performance | P95 < 5s | k6 / Locust |

### 6.3 Quality Gates

| Gate | Criterio | Bloqueante |
|------|----------|------------|
| Lint | 0 errors (ruff, eslint) | Sim |
| Type Check | 0 errors (mypy, tsc) | Sim |
| Unit Tests | 100% pass | Sim |
| Coverage | > 80% | Sim |
| Security Scan | 0 critical/high | Sim |
| E2E Tests | 100% pass | Sim |

### 6.4 RAG Quality Metrics

| Metrica | Descricao | Meta |
|---------|-----------|------|
| **Recall@10** | Documentos relevantes no top 10 | > 90% |
| **Precision@10** | Proporcao de relevantes no top 10 | > 70% |
| **MRR** | Mean Reciprocal Rank | > 0.7 |
| **NDCG@10** | Normalized DCG | > 0.8 |
| **Latencia P50** | Tempo de busca mediano | < 1.5s |
| **Latencia P95** | Tempo de busca percentil 95 | < 5s |

### 6.5 Abordagem Shift-Left (LEAN)

No modelo LEAN, QA atua desde o inicio do projeto:

```
         Proposta Original          │        Proposta LEAN
                                    │
  Mes 1  Mes 2  Mes 3  Mes 4  Mes 5 │  Mes 1  Mes 2  Mes 3  Mes 4
    │      │      │      │      │   │    │      │      │      │
    │      │      │      │  QA 50%  │    │  QA  │  QA  │  QA  │
    │      │      │      ├────────► │    ├──────┴──────┴──────┤
    │      │      │      │      │   │    │  QA 50% CONTINUO   │
                                    │
  Detecta problemas tarde           │  Detecta problemas CEDO
  Custo de correcao ALTO            │  Custo de correcao BAIXO
```

### 6.6 Validacao de Codigo Gerado por IA

```
┌─────────────────────────────────────────────────────────────────────┐
│                   FLUXO DE VALIDACAO DE CODIGO IA                   │
└─────────────────────────────────────────────────────────────────────┘

  Developer        IA (Claude/Copilot)       TL Review          QA
      │                    │                     │               │
      │  1. Prompt/Contexto│                     │               │
      ├───────────────────►│                     │               │
      │                    │                     │               │
      │  2. Codigo Gerado  │                     │               │
      │◄───────────────────┤                     │               │
      │                    │                     │               │
      │  3. Revisao Local  │                     │               │
      │  (entender codigo) │                     │               │
      │                    │                     │               │
      │  4. Testes Locais  │                     │               │
      │  (unit tests)      │                     │               │
      │                    │                     │               │
      │  5. PR + CI        │                     │               │
      ├────────────────────┼────────────────────►│               │
      │                    │                     │               │
      │                    │   6. Code Review    │               │
      │                    │   - Logica correta? │               │
      │                    │   - Seguranca?      │               │
      │                    │   - Padroes?        │               │
      │                    │                     │               │
      │  7. Ajustes        │                     │               │
      │◄───────────────────┼─────────────────────┤               │
      │                    │                     │               │
      │  8. Merge          │                     │               │
      ├────────────────────┼─────────────────────┼──────────────►│
      │                    │                     │               │
      │                    │                     │  9. Validacao │
      │                    │                     │  Funcional    │
```

---

## 7. Analise de Riscos

### 7.1 Matriz de Riscos

| ID | Risco | Probabilidade | Impacto | Score | Mitigacao |
|----|-------|---------------|---------|-------|-----------|
| R1 | Performance inadequada em grande volume | Media | Alto | **6** | PoC de carga; arquitetura escalavel desde inicio |
| R2 | Qualidade dos embeddings insuficiente | Media | Alto | **6** | Testes com subset; fine-tuning se necessario |
| R3 | Latencia de rede com file servers | Media | Medio | **4** | Cache local; sync incremental |
| R4 | Resistencia dos usuarios | Baixa | Alto | **4** | Change management; treinamento; quick wins |
| R5 | Dataset ML insuficiente | Media | Alto | **6** | Workshop com juridico; dados sinteticos |
| R6 | OCR com baixa qualidade | Baixa | Medio | **3** | Azure Doc Intelligence; ajuste de threshold |

### 7.2 Riscos Especificos do Modelo LEAN

| ID | Risco | Prob. | Impacto | Score | Mitigacao |
|----|-------|-------|---------|-------|-----------|
| **RL1** | Codigo gerado por IA com bugs sutis | Alta | Alto | **9** | Code review rigoroso; QA continuo; testes automatizados |
| **RL2** | Full Stack sobrecarregado | Media | Alto | **6** | Monitorar velocity; TL assume tarefas se necessario |
| **RL3** | Dependencia de ferramentas IA | Media | Medio | **4** | Conhecimento documentado; ferramentas alternativas |
| **RL4** | Qualidade RAG comprometida (ML solo) | Media | Alto | **6** | ML Engineer 100%; TL apoia; metricas rigorosas |
| **RL5** | Atrasos por aprendizado de ferramentas IA | Baixa | Medio | **3** | Onboarding; equipe com experiencia previa |
| **RL6** | Problemas de seguranca em codigo gerado | Media | Alto | **6** | SAST/DAST no CI; code review; pentest |
| **RL7** | Tech Lead indisponivel (gargalo) | Baixa | Alto | **4** | Backup definido; documentacao; async reviews |

### 7.3 Matriz Visual de Riscos LEAN

```
                    IMPACTO
           Baixo    Medio    Alto
         ┌────────┬────────┬────────┐
  Alta   │        │        │  RL1   │  <- CRITICO
         ├────────┼────────┼────────┤
  Media  │        │RL3,RL5 │RL2,RL4 │  <- ATENCAO
         │        │        │  RL6   │
         ├────────┼────────┼────────┤
  Baixa  │        │   R6   │  RL7   │
         │        │        │   R4   │
         └────────┴────────┴────────┘
  PROB.
```

### 7.4 Plano de Contingencia

| Risco | Trigger | Acao |
|-------|---------|------|
| RL1 - Codigo IA com bugs | Bug critico em prod | Rollback; review completo da feature |
| RL2 - Full Stack overload | Velocity < 70% | TL assume DevOps; cortar escopo |
| RL4 - Qualidade RAG | Recall < 70% | Consultor ML externo; ajuste chunking |
| R3 - Performance | P95 > 10s | Escalar infra; otimizar queries; cache |

---

## 8. Criterios de Sucesso

### 8.1 KPIs do Projeto

| KPI | Baseline (Atual) | Meta | Prazo |
|-----|------------------|------|-------|
| Tempo medio de busca | TBD (estimar 4h) | < 30 min | 3 meses apos go-live |
| Documentos nao encontrados | TBD | < 5% | 3 meses apos go-live |
| Processos perdidos por documentacao | TBD | 0 | 6 meses apos go-live |
| Satisfacao do usuario | N/A | > 4.0/5.0 | 3 meses apos go-live |
| Adocao do sistema | 0% | > 80% usuarios | 2 meses apos go-live |

### 8.2 Criterios de Aceite do MVP

- [ ] Busca semantica funcional com Recall@10 > 85%
- [ ] Integracao com 2 fontes de dados (FileServer + SharePoint)
- [ ] OCR funcional para documentos escaneados
- [ ] Regras ML identificando tipos de documento
- [ ] Controle de acesso via AD funcionando
- [ ] Interface de busca e visualizacao de resultados
- [ ] Exportacao de pacote de documentos (ZIP)
- [ ] Alertas de documentos obrigatorios faltantes
- [ ] Tempo de resposta < 5s (P95)
- [ ] Documentacao tecnica e de usuario

---

## 9. Proximos Passos

### 9.1 Acoes Imediatas (Antes de GO)

| # | Acao | Responsavel | Prazo |
|---|------|-------------|-------|
| 1 | Levantar metricas atuais (tempo, volume) | ThyssenKrupp Juridico | 1 semana |
| 2 | Validar acesso File Server (SMB) | ThyssenKrupp TI | 1 semana |
| 3 | Validar acesso SharePoint (Graph API) | ThyssenKrupp TI | 1 semana |
| 4 | Workshop de regras de negocio + dataset ML | Juridico + Equipe | 1 semana |
| 5 | Validar ambiente cloud (Azure/AWS) | ThyssenKrupp TI | 1 semana |
| 6 | **Aprovar uso de ferramentas IA** | ThyssenKrupp + Seguranca | 1 semana |
| 7 | **Validar perfil da equipe LEAN** | RH/Gestao | 1 semana |
| 8 | Coletar amostras de docs escaneados (OCR) | Juridico | 1 semana |

### 9.2 Condicoes para GO

A recomendacao passa de **CONDITIONAL** para **GO** quando:

1. **Confirmado:** Metricas baseline (tempo atual, volume de processos)
2. **Validado:** Acesso ao File Server (SMB/CIFS)
3. **Validado:** Acesso ao SharePoint (Graph API + credenciais)
4. **Documentado:** Regras de documentos obrigatorios por tipo de processo
5. **Coletado:** Dataset inicial para treinamento do modelo ML
6. **Coletado:** Amostras de documentos escaneados para validar OCR
7. **Aprovado:** Ambiente cloud e conectividade de rede
8. **Aprovado:** Politica de uso de IA generativa no projeto
9. **Confirmado:** Equipe com experiencia em AI-assisted development disponivel

### 9.3 Roadmap Pos-Go-Live

| Fase | Features | Estimativa |
|------|----------|------------|
| **Go-Live** | Busca, 2 conectores, OCR, ML Rules, exportacao | 16 semanas |
| **V1.1** | Refinamento ML, novos tipos de processo | +4 semanas |
| **V1.2** | Novos conectores (sob demanda) | +4 semanas |
| **V2.0** | Analytics, predicao, dashboard | +8 semanas |

---

## 10. Anexos

### Anexo A: Glossario

| Termo | Definicao |
|-------|-----------|
| **RAG** | Retrieval-Augmented Generation - tecnica que combina busca com geracao de texto |
| **Embedding** | Representacao vetorial de texto para busca semantica |
| **Vector Database** | Banco de dados otimizado para busca por similaridade vetorial |
| **Chunking** | Divisao de documentos em pedacos menores para processamento |
| **Reranking** | Reordenacao de resultados de busca para melhorar relevancia |
| **OCR** | Optical Character Recognition - extracao de texto de imagens |
| **ML Rules** | Regras baseadas em Machine Learning para classificacao de documentos |

### Anexo B: Referencias Tecnicas

- OpenAI Embeddings: https://platform.openai.com/docs/guides/embeddings
- Qdrant Documentation: https://qdrant.tech/documentation/
- Microsoft Graph API: https://learn.microsoft.com/en-us/graph/
- Azure Document Intelligence: https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/
- RAG Best Practices: https://docs.anthropic.com/en/docs/build-with-claude/retrieval-augmented-generation

### Anexo C: Perguntas Pendentes

1. Qual o tempo medio atual para localizar todos os documentos de um processo?
2. Quantos processos trabalhistas sao tratados por mes?
3. Quais tipos de processo trabalhista sao mais frequentes?
4. Qual o percentual estimado de documentos escaneados (vs texto nativo)?
5. Existem documentos em formatos nao-texto (imagens, videos)?
6. Qual o tamanho medio dos documentos em cada repositorio?
7. Ha requisitos de retencao/expurgo de documentos?

### Anexo D: Custos Detalhados por Mes

#### Desenvolvimento LEAN (16 Semanas / 4 Meses)

| Mes | Tech Lead (50%) | Full Stack | ML Engineer | QA (50%) | Ferramentas IA | Total/Mes |
|-----|-----------------|------------|-------------|----------|----------------|-----------|
| 1 | R$ 17.500 | R$ 28.000 | R$ 30.000 | R$ 7.000 | R$ 2.800 | R$ 85.300 |
| 2 | R$ 17.500 | R$ 28.000 | R$ 30.000 | R$ 7.000 | R$ 2.800 | R$ 85.300 |
| 3 | R$ 17.500 | R$ 28.000 | R$ 30.000 | R$ 7.000 | R$ 2.800 | R$ 85.300 |
| 4 | R$ 17.500 | R$ 28.000 | R$ 30.000 | R$ 7.000 | R$ 2.800 | R$ 85.300 |
| **Total** | **R$ 70.000** | **R$ 112.000** | **R$ 120.000** | **R$ 28.000** | **R$ 11.200** | **R$ 341.200** |

### Anexo E: Checklist de Code Review para Codigo AI-Generated

#### Seguranca

- [ ] Nao ha credenciais hardcoded
- [ ] Inputs sao validados/sanitizados
- [ ] SQL queries usam parametros (nao concatenacao)
- [ ] Nao ha vulnerabilidades obvias (XSS, CSRF, injection)
- [ ] Permissoes sao verificadas antes de acoes

#### Qualidade

- [ ] Codigo e legivel e bem estruturado
- [ ] Nomes de variaveis/funcoes sao descritivos
- [ ] Nao ha codigo duplicado desnecessario
- [ ] Tratamento de erros adequado
- [ ] Logs apropriados (sem dados sensiveis)

#### Corretude

- [ ] Logica atende ao requisito
- [ ] Edge cases sao tratados
- [ ] Testes unitarios cobrem o codigo
- [ ] Nao ha race conditions obvias
- [ ] Recursos sao liberados corretamente

#### Performance

- [ ] Queries sao eficientes (N+1 problem?)
- [ ] Nao ha loops desnecessarios
- [ ] Cache e usado onde apropriado
- [ ] Nao ha memory leaks obvios

### Anexo F: Criterios de Selecao da Equipe LEAN

#### Tech Lead (50%)

- [ ] 8+ anos de experiencia
- [ ] Projetos anteriores com RAG/LLM
- [ ] Experiencia com code review de codigo AI-generated
- [ ] Disponibilidade para reviews assincronos

#### Full Stack Senior

- [ ] 5+ anos Python + React
- [ ] Portfolio demonstrando uso de Claude/Copilot
- [ ] Conhecimento basico de DevOps/Kubernetes
- [ ] Autonomia e proatividade

#### ML Engineer

- [ ] 3+ anos em NLP/Information Retrieval
- [ ] Experiencia com RAG pipelines em producao
- [ ] Conhecimento de metricas de avaliacao (NDCG, MRR)
- [ ] Experiencia com Qdrant ou similar

#### QA

- [ ] Experiencia com testes automatizados (Playwright/Cypress)
- [ ] Familiaridade com testes de sistemas ML
- [ ] Mindset de qualidade continua

---

**Documento gerado em:** 22 de Janeiro de 2026
**Versao:** 2.1 LEAN (AI-Assisted)
**Autor:** Solution Architect Agent
**Base:** Proposta Original v1.0 (21/01/2026)
**Cronograma:** 16 semanas (4 meses)
**Revisao pendente:** ThyssenKrupp + Equipe Tecnica
