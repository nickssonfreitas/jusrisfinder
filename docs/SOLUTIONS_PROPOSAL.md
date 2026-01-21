# Proposta de Solucao: JurisFinder

**Cliente:** ThyssenKrupp
**Projeto:** JurisFinder - Sistema de Busca Inteligente em Documentos para Processos Trabalhistas
**Versao:** 1.0
**Data:** 21 de Janeiro de 2026
**Modo:** STANDARD

---

## Sumario Executivo

### Problema

A ThyssenKrupp enfrenta desafios criticos na gestao de documentos para processos trabalhistas:

- **Busca manual ineficiente:** Documentos dispersos em multiplas fontes (pastas de rede, SharePoint, sistema Sense)
- **Risco juridico elevado:** Ja ocorreram perdas de processos por nao localizar documentos dentro do prazo legal
- **Ausencia de padronizacao:** Processo sem controle de qualidade e rastreabilidade
- **Alto custo operacional:** Tempo significativo gasto em tarefas manuais repetitivas

### Solucao Proposta

Desenvolver o **JurisFinder**, um sistema inteligente de busca e recuperacao de documentos baseado em:

- **RAG (Retrieval-Augmented Generation):** Busca semantica avancada que entende o contexto das solicitacoes
- **Indexacao unificada:** Consolidacao de todas as fontes de documentos em um indice centralizado
- **Automacao inteligente:** Identificacao automatica de documentos obrigatorios por tipo de processo
- **Controle de acesso:** Sistema de permissoes integrado com AD/LDAP corporativo

### Numeros-Chave

| Metrica | Estimativa |
|---------|------------|
| **Esforco Total** | 4-5 meses |
| **Equipe** | 5-6 profissionais |
| **Custo Desenvolvimento** | R$ 450.000 - R$ 600.000 |
| **Custo Infraestrutura/mes** | R$ 8.000 - R$ 15.000 |
| **ROI Esperado** | 6-12 meses |
| **Reducao de Tempo** | 70-85% no processo de busca |

### Recomendacao

| | |
|---|---|
| [ ] GO | |
| [ ] NO-GO | |
| [X] **CONDITIONAL** | |

**Justificativa:** A solucao e tecnicamente viavel e apresenta alto potencial de ROI. Porem, existem **gaps criticos** que precisam ser resolvidos antes do inicio do desenvolvimento:

1. **Integracao com ELO:** Necessario confirmar se existe API disponivel
2. **Regras de negocio:** Definir criterios de selecao de documentos por tipo de processo
3. **Acesso aos sistemas:** Validar conectividade com Sense e SharePoints
4. **Metricas baseline:** Obter tempos e volumes atuais para medir ROI

---

## 1. Analise de Requisitos

### 1.1 Personas

| Persona | Descricao | Necessidades Principais |
|---------|-----------|------------------------|
| **Advogado Interno** | Usuario principal, recebe demandas de processos trabalhistas | Busca rapida, documentos corretos, integracao com ELO |
| **Analista Juridico** | Apoia na analise e organizacao de documentos | Interface intuitiva, filtros avancados, exportacao |
| **Gestor de Seguranca** | Fornece documentos de seguranca do trabalho | Notificacoes de solicitacao, acesso restrito |
| **Administrador RH** | Fornece registros de funcionarios | Controle de acesso, auditoria |
| **Administrador TI** | Gerencia sistema e integracoes | Monitoramento, logs, manutencao |

### 1.2 Requisitos Funcionais

#### RF01 - Busca Inteligente de Documentos
- **Descricao:** O sistema deve permitir busca em linguagem natural (ex: "documentos de EPI do funcionario Joao Silva de 2020 a 2023")
- **Prioridade:** Alta
- **Criterios de Aceite:**
  - Busca semantica retorna documentos relevantes mesmo sem match exato de palavras
  - Tempo de resposta < 5 segundos para consultas tipicas
  - Resultados ranqueados por relevancia

#### RF02 - Indexacao Multi-Fonte
- **Descricao:** Indexar documentos de pastas de rede, SharePoint e sistema Sense
- **Prioridade:** Alta
- **Criterios de Aceite:**
  - Conector para sistemas de arquivos Windows (SMB/CIFS)
  - Conector para SharePoint Online via Graph API
  - Conector para sistema Sense (API ou scraping)
  - Indexacao incremental (apenas documentos novos/alterados)

#### RF03 - Controle de Acesso
- **Descricao:** Validar permissoes do usuario antes de exibir documentos
- **Prioridade:** Alta
- **Criterios de Aceite:**
  - Integracao com Active Directory/LDAP
  - Respeitar permissoes das fontes originais
  - Registro de auditoria de todos os acessos

#### RF04 - Identificacao Automatica de Documentos Obrigatorios
- **Descricao:** Com base no tipo de processo, sugerir documentos obrigatorios
- **Prioridade:** Media
- **Criterios de Aceite:**
  - Cadastro de regras por tipo de processo trabalhista
  - Alerta quando documentos obrigatorios nao forem encontrados
  - Sugestoes de documentos complementares

#### RF05 - Preparacao e Exportacao
- **Descricao:** Organizar documentos selecionados para envio ao ELO
- **Prioridade:** Media
- **Criterios de Aceite:**
  - Agrupamento de documentos em pacotes
  - Geracao de sumario/indice
  - Exportacao em formatos padrao (ZIP, PDF consolidado)

#### RF06 - Integracao com ELO (Condicional)
- **Descricao:** Envio automatico de documentos para o sistema ELO
- **Prioridade:** Media (depende de API disponivel)
- **Criterios de Aceite:**
  - Upload automatico via API
  - Confirmacao de recebimento
  - Fallback manual se integracao indisponivel

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
3. **Integracao Legada:** Sistema Sense pode ter limitacoes de API
4. **Rede Corporativa:** Acesso a pastas de rede requer conectividade VPN/ExpressRoute

### 1.5 Premissas

1. ThyssenKrupp fornecera acesso aos repositorios de documentos para desenvolvimento
2. Regras de negocio para documentos obrigatorios serao definidas com equipe juridica
3. Ambiente de homologacao sera disponibilizado com dados anonimizados
4. Equipe de TI ThyssenKrupp apoiara nas integracoes de rede e AD

### 1.6 Gaps Identificados (Requerem Esclarecimento)

| Gap | Impacto | Acao Necessaria |
|-----|---------|-----------------|
| Tempo medio atual do processo | Impossibilita medir ROI | Levantar metricas com equipe juridica |
| Volume mensal de processos | Afeta dimensionamento | Obter historico de 12 meses |
| API do sistema ELO | Define escopo de integracao | Validar com fornecedor |
| Estrutura do sistema Sense | Define conector necessario | Analise tecnica do sistema |
| Criterios de documentos obrigatorios | Core da solucao | Workshop com juridico |

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
┌──────────────┐            ┌─────────────────────┐            ┌──────────────┐
│   Active     │◄──────────►│                     │◄──────────►│   Sistema    │
│   Directory  │   Auth     │    JURISFINDER      │   Upload   │     ELO      │
│              │            │                     │            │              │
└──────────────┘            └──────────┬──────────┘            └──────────────┘
                                       │
                    ┌──────────────────┼──────────────────┐
                    │                  │                  │
                    ▼                  ▼                  ▼
          ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
          │   Pastas de     │ │   SharePoint    │ │    Sistema      │
          │     Rede        │ │    Online       │ │     Sense       │
          │  (File Server)  │ │                 │ │                 │
          └─────────────────┘ └─────────────────┘ └─────────────────┘
```

### 2.3 Diagrama de Container (C4 - Level 2)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              JURISFINDER                                     │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                         FRONTEND (SPA)                               │   │
│  │  React + TypeScript + TailwindCSS                                    │   │
│  │  - Interface de busca                                                │   │
│  │  - Visualizacao de resultados                                        │   │
│  │  - Gestao de pacotes                                                 │   │
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
│  │ - Hybrid search   │ │ - Embeddings      │ │ - PDF generation  │         │
│  │ - Reranking       │ │ - Chunking        │ │ - ELO integration │         │
│  │ - Access control  │ │ - Metadata        │ │                   │         │
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
│  │  └─────────────────┘  └─────────────────┘  └─────────────────┘     │   │
│  │                                                                      │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘

                    │                     │                     │
    ┌───────────────┼─────────────────────┼─────────────────────┼───────────┐
    │               │                     │                     │           │
    │               ▼                     ▼                     ▼           │
    │   ┌───────────────────┐ ┌───────────────────┐ ┌───────────────────┐   │
    │   │   File Server     │ │   SharePoint      │ │    Sense API      │   │
    │   │   Connector       │ │   Connector       │ │    Connector      │   │
    │   │   (SMB/CIFS)      │ │   (Graph API)     │ │   (Custom)        │   │
    │   └───────────────────┘ └───────────────────┘ └───────────────────┘   │
    │                                                                        │
    │                    CONECTORES DE FONTES DE DADOS                       │
    └────────────────────────────────────────────────────────────────────────┘
```

### 2.4 Pipeline RAG Detalhado

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           PIPELINE DE INDEXACAO                             │
└─────────────────────────────────────────────────────────────────────────────┘

┌───────────┐    ┌───────────┐    ┌───────────┐    ┌───────────┐    ┌─────────┐
│  Fonte    │───►│  Crawler  │───►│  Parser   │───►│ Chunking  │───►│Embedding│
│  (docs)   │    │           │    │  (text)   │    │ (512 tok) │    │ (1536d) │
└───────────┘    └───────────┘    └───────────┘    └───────────┘    └────┬────┘
                                                                          │
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
                                  ┌───────────┐
                                  │ Resultados│
                                  │ Ranqueados│
                                  └───────────┘
```

### 2.5 Stack Tecnologica

| Camada | Tecnologia | Justificativa |
|--------|------------|---------------|
| **Frontend** | React 18 + TypeScript + TailwindCSS | Produtividade, tipagem, componentes modernos |
| **API Gateway** | Kong OSS ou AWS API Gateway | Rate limiting, auth, observabilidade |
| **Backend** | Python 3.12 + FastAPI | Ecossistema ML/AI maduro, performance async |
| **Task Queue** | Celery + Redis | Processamento assincrono de indexacao |
| **Vector DB** | Qdrant | Open-source, performance, filtering nativo |
| **Relational DB** | PostgreSQL 16 | Metadata, usuarios, auditoria |
| **Cache** | Redis | Sessoes, cache de queries, filas |
| **Embeddings** | OpenAI text-embedding-3-large ou Azure OpenAI | 3072 dimensoes, melhor qualidade semantica |
| **LLM (Query)** | GPT-4o ou Azure OpenAI | Parsing de queries, sugestoes |
| **Reranker** | Cohere Rerank ou Cross-encoder local | Melhora precisao do ranking |
| **OCR** | Azure Document Intelligence | Documentos escaneados |
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
- `SenseConnector` (a definir)

**Consequencias:**
- (+) Facilidade para adicionar novas fontes
- (+) Testabilidade isolada
- (-) Overhead inicial de design

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
| **Total AI/mes** | | **$550 - $1.200** |

### 3.3 Fluxo de Processamento

```
                         INDEXACAO
                         =========

┌──────────────────────────────────────────────────────────────────────────┐
│                                                                          │
│  1. CRAWLING           2. PARSING           3. CHUNKING                  │
│  ┌─────────────┐       ┌─────────────┐      ┌─────────────┐             │
│  │ Detectar    │──────►│ Extrair     │─────►│ Dividir em  │             │
│  │ novos/      │       │ texto       │      │ chunks de   │             │
│  │ alterados   │       │ (PDF, DOCX, │      │ 512 tokens  │             │
│  │             │       │  imagens)   │      │             │             │
│  └─────────────┘       └─────────────┘      └──────┬──────┘             │
│                                                     │                    │
│  4. EMBEDDING          5. INDEXING          6. METADATA                 │
│  ┌─────────────┐       ┌─────────────┐      ┌─────────────┐             │
│  │ Gerar       │──────►│ Armazenar   │─────►│ Salvar      │             │
│  │ vetores     │       │ em Qdrant   │      │ metadados   │             │
│  │ 3072d       │       │             │      │ PostgreSQL  │             │
│  └─────────────┘       └─────────────┘      └─────────────┘             │
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
│  4. FILTERING          5. RERANKING         6. RESPONSE                 │
│  ┌─────────────┐       ┌─────────────┐      ┌─────────────┐             │
│  │ Aplicar     │──────►│ Cross-      │─────►│ Retornar    │             │
│  │ permissoes  │       │ encoder     │      │ top-k       │             │
│  │ do usuario  │       │ rerank      │      │ documentos  │             │
│  └─────────────┘       └─────────────┘      └─────────────┘             │
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
- **PDFs escaneados:** OCR + chunking
- **Planilhas:** Chunk por aba/secao
- **Apresentacoes:** Chunk por slide

### 3.6 Metricas de Qualidade

| Metrica | Meta | Medicao |
|---------|------|---------|
| **Recall@10** | > 90% | % de documentos relevantes no top 10 |
| **MRR** | > 0.7 | Mean Reciprocal Rank |
| **Latencia P95** | < 3s | Tempo de resposta |
| **User Satisfaction** | > 4.0/5.0 | Feedback explicito |

---

## 4. Integracao de Sistemas

### 4.1 Mapa de Integracoes

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
                    ┌───────────────────┼───────────────────┐
                    │                   │                   │
                    ▼                   ▼                   ▼
          ┌───────────────────┐ ┌───────────────────┐ ┌───────────────────┐
          │   SISTEMA SENSE   │ │    SISTEMA ELO    │ │   NOTIFICACOES    │
          │                   │ │                   │ │                   │
          │ Protocolo: TBD    │ │ Protocolo: TBD    │ │ Email / Teams     │
          │ API: A validar    │ │ API: A validar    │ │                   │
          └───────────────────┘ └───────────────────┘ └───────────────────┘
```

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

    async def download_content(self, item_id: str) -> bytes:
        """Baixa conteudo de um documento."""
        return await self.client.drives[drive_id].items[item_id].content.get()
```

### 4.3 Conector File Server

```python
# Exemplo usando smbprotocol
from smbprotocol.connection import Connection
from smbprotocol.session import Session
from smbprotocol.tree import TreeConnect

class FileServerConnector:
    def __init__(self, server: str, share: str, username: str, password: str):
        self.connection = Connection(uuid.uuid4(), server, 445)
        self.session = Session(self.connection, username, password)
        self.tree = TreeConnect(self.session, f"\\\\{server}\\{share}")

    def list_documents(self, path: str) -> list[Document]:
        """Lista documentos recursivamente."""
        # Implementacao com walk recursivo
        pass

    def read_content(self, path: str) -> bytes:
        """Le conteudo de arquivo."""
        pass
```

### 4.4 Conector Sistema Sense (A Definir)

**Status:** Requer analise tecnica do sistema Sense para definir estrategia.

**Opcoes:**
1. **API REST** (se disponivel) - Preferencial
2. **Database Direct** (se permitido) - Com read-only user
3. **RPA/Scraping** (ultimo recurso) - Maior complexidade

### 4.5 Integracao ELO (Condicional)

**Status:** Pendente validacao de API com fornecedor.

**Estrategia de Fallback:**
1. **Com API:** Upload automatico via REST
2. **Sem API:** Geracao de pacote ZIP para upload manual

---

## 5. Estimativa de Esforco

### 5.1 Cronograma de Alto Nivel

```
                    MES 1       MES 2       MES 3       MES 4       MES 5
                 ┌─────────┬─────────┬─────────┬─────────┬─────────┐
 DISCOVERY       │████████ │         │         │         │         │
 & SETUP         │         │         │         │         │         │
                 ├─────────┼─────────┼─────────┼─────────┼─────────┤
 CORE RAG        │    ████ │█████████│████     │         │         │
 PIPELINE        │         │         │         │         │         │
                 ├─────────┼─────────┼─────────┼─────────┼─────────┤
 CONECTORES      │         │    █████│█████████│████     │         │
                 │         │         │         │         │         │
                 ├─────────┼─────────┼─────────┼─────────┼─────────┤
 FRONTEND        │         │         │    █████│█████████│██       │
                 │         │         │         │         │         │
                 ├─────────┼─────────┼─────────┼─────────┼─────────┤
 INTEGRACOES     │         │         │         │    █████│█████    │
 & TESTES        │         │         │         │         │         │
                 ├─────────┼─────────┼─────────┼─────────┼─────────┤
 DEPLOY &        │         │         │         │         │████████ │
 GO-LIVE         │         │         │         │         │         │
                 └─────────┴─────────┴─────────┴─────────┴─────────┘
```

### 5.2 Detalhamento por Fase

| Fase | Duracao | Descricao |
|------|---------|-----------|
| **1. Discovery & Setup** | 3 semanas | Levantamento detalhado, ambiente, arquitetura |
| **2. Core RAG Pipeline** | 6 semanas | Indexador, embeddings, busca, reranking |
| **3. Conectores** | 5 semanas | FileServer, SharePoint, Sense |
| **4. Frontend** | 5 semanas | Interface de busca, resultados, exportacao |
| **5. Integracoes** | 3 semanas | ELO, AD, notificacoes |
| **6. Testes & Deploy** | 3 semanas | QA, performance, go-live |
| **Total** | **~20 semanas (5 meses)** | |

### 5.3 Composicao da Equipe

| Papel | Quantidade | Alocacao | Responsabilidades |
|-------|------------|----------|-------------------|
| **Tech Lead / Arquiteto** | 1 | 100% | Arquitetura, decisoes tecnicas, code review |
| **Backend Developer Senior** | 2 | 100% | RAG pipeline, conectores, APIs |
| **Frontend Developer** | 1 | 100% | Interface React, UX |
| **ML Engineer** | 1 | 50% | Embeddings, fine-tuning, metricas |
| **DevOps/SRE** | 1 | 50% | Infraestrutura, CI/CD, monitoramento |
| **QA Engineer** | 1 | 50% (fases finais) | Testes, automacao |
| **Product Owner (Cliente)** | 1 | 25% | Validacoes, priorizacao |

**Total FTE:** ~5.5 pessoas

### 5.4 Estimativa de Custos

#### Desenvolvimento

| Item | Quantidade | Valor Unitario | Total |
|------|------------|----------------|-------|
| Tech Lead | 5 meses | R$ 35.000/mes | R$ 175.000 |
| Backend Senior (2x) | 5 meses | R$ 25.000/mes | R$ 250.000 |
| Frontend Developer | 5 meses | R$ 22.000/mes | R$ 110.000 |
| ML Engineer (50%) | 5 meses | R$ 15.000/mes | R$ 75.000 |
| DevOps (50%) | 5 meses | R$ 12.500/mes | R$ 62.500 |
| QA (50%, 3 meses) | 3 meses | R$ 10.000/mes | R$ 30.000 |
| **Subtotal Desenvolvimento** | | | **R$ 702.500** |

#### Infraestrutura (Mensal - Producao)

| Item | Especificacao | Custo/mes |
|------|---------------|-----------|
| Kubernetes (AKS/EKS) | 3 nodes, 8 vCPU, 32GB | R$ 3.500 |
| PostgreSQL Managed | 4 vCPU, 16GB, 500GB | R$ 1.500 |
| Qdrant Cloud | 2M vectors, 3 replicas | R$ 2.000 |
| Redis Managed | 4GB | R$ 500 |
| Storage (Blob/S3) | 1TB | R$ 200 |
| Network/CDN | - | R$ 300 |
| OpenAI/Azure OpenAI | Embeddings + LLM | R$ 2.500 |
| Monitoring (Datadog/Grafana) | - | R$ 500 |
| **Total Infraestrutura/mes** | | **R$ 11.000** |

#### Resumo de Custos

| Categoria | Valor |
|-----------|-------|
| Desenvolvimento (one-time) | R$ 702.500 |
| Infraestrutura (12 meses) | R$ 132.000 |
| Contingencia (15%) | R$ 125.175 |
| **Total Primeiro Ano** | **R$ 959.675** |
| **Custo Mensal Operacao (apos go-live)** | **~R$ 15.000** |

---

## 6. Estrategia de Qualidade

### 6.1 Piramide de Testes

```
                        ┌───────────┐
                        │    E2E    │  10%
                        │  (Cypress)│
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
| E2E | Happy paths | Cypress |
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

---

## 7. Analise de Riscos

### 7.1 Matriz de Riscos

| ID | Risco | Probabilidade | Impacto | Score | Mitigacao |
|----|-------|---------------|---------|-------|-----------|
| R1 | Sistema Sense sem API disponivel | Alta | Alto | **9** | RPA/Scraping como fallback; priorizar outras fontes |
| R2 | ELO sem integracao automatica | Media | Medio | **6** | Exportacao manual como MVP; renegociar com fornecedor |
| R3 | Performance inadequada em grande volume | Media | Alto | **6** | PoC de carga; arquitetura escalavel desde inicio |
| R4 | Qualidade dos embeddings insuficiente | Media | Alto | **6** | Testes com subset; fine-tuning se necessario |
| R5 | Latencia de rede com file servers | Media | Medio | **4** | Cache local; sync incremental |
| R6 | Resistencia dos usuarios | Baixa | Alto | **4** | Change management; treinamento; quick wins |
| R7 | Mudancas de escopo frequentes | Media | Medio | **4** | Sprints curtos; backlog priorizado |
| R8 | Problemas de acesso/permissoes | Media | Baixo | **3** | Validacao previa; ambiente sandbox |

### 7.2 Plano de Contingencia

| Risco | Trigger | Acao |
|-------|---------|------|
| R1 - Sense sem API | Confirmacao na discovery | Implementar RPA; escopo reduzido inicial |
| R3 - Performance | P95 > 10s em testes | Escalar infra; otimizar queries; cache agressivo |
| R4 - Qualidade RAG | Recall < 70% | Fine-tuning embeddings; ajuste chunking; hybrid search |

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
- [ ] Integracao com pelo menos 2 fontes de dados (FileServer + SharePoint)
- [ ] Controle de acesso via AD funcionando
- [ ] Interface de busca e visualizacao de resultados
- [ ] Exportacao de pacote de documentos (ZIP)
- [ ] Tempo de resposta < 5s (P95)
- [ ] Documentacao tecnica e de usuario

---

## 9. Proximos Passos

### 9.1 Acoes Imediatas (Antes de GO)

| # | Acao | Responsavel | Prazo |
|---|------|-------------|-------|
| 1 | Levantar metricas atuais (tempo, volume) | ThyssenKrupp Juridico | 1 semana |
| 2 | Validar API do sistema ELO | ThyssenKrupp TI | 1 semana |
| 3 | Analise tecnica do sistema Sense | Equipe Tecnica | 2 semanas |
| 4 | Workshop de regras de negocio (docs obrigatorios) | Juridico + Equipe | 1 semana |
| 5 | Validar ambiente cloud (Azure/AWS) | ThyssenKrupp TI | 1 semana |
| 6 | Definir ambiente de desenvolvimento/sandbox | TI | 1 semana |

### 9.2 Condicoes para GO

A recomendacao passa de **CONDITIONAL** para **GO** quando:

1. **Confirmado:** Metricas baseline (tempo atual, volume de processos)
2. **Definido:** Estrategia de integracao com Sense (API ou alternativa)
3. **Validado:** Disponibilidade de API ELO (ou aceite de fallback manual)
4. **Documentado:** Regras de documentos obrigatorios por tipo de processo
5. **Aprovado:** Ambiente cloud e conectividade de rede

### 9.3 Roadmap Pos-MVP

| Fase | Features | Estimativa |
|------|----------|------------|
| **MVP** | Busca, 2 conectores, exportacao | 5 meses |
| **V1.1** | Conector Sense, regras automaticas | +6 semanas |
| **V1.2** | Integracao ELO, notificacoes | +4 semanas |
| **V2.0** | Analytics, ML para predicao | +8 semanas |

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

### Anexo B: Referencias Tecnicas

- OpenAI Embeddings: https://platform.openai.com/docs/guides/embeddings
- Qdrant Documentation: https://qdrant.tech/documentation/
- Microsoft Graph API: https://learn.microsoft.com/en-us/graph/
- RAG Best Practices: https://docs.anthropic.com/en/docs/build-with-claude/retrieval-augmented-generation

### Anexo C: Perguntas Pendentes

1. Qual o tempo medio atual para localizar todos os documentos de um processo?
2. Quantos processos trabalhistas sao tratados por mes?
3. O sistema ELO possui API REST ou SOAP?
4. Qual a estrutura de pastas/nomenclatura no sistema Sense?
5. Existem documentos em formatos nao-texto (imagens, videos)?
6. Qual o tamanho medio dos documentos em cada repositorio?
7. Ha requisitos de retencao/expurgo de documentos?

---

**Documento gerado em:** 21 de Janeiro de 2026
**Versao:** 1.0
**Autor:** Solution Architect Agent
**Revisao pendente:** ThyssenKrupp + Equipe Tecnica
