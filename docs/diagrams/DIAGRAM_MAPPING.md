# Mapeamento: Diagramas C4 ↔ Documentação

Este documento mapeia a relação entre os diagramas C4 e as seções da [Proposta de Solução](../SOLUTIONS_PROPOSAL.md).

## Mapa de Correlação

### Diagrama de Contexto → Seções da Proposta

**Diagrama:** [c4-context.puml](./c4-context.puml)

| Elemento no Diagrama | Seção da Proposta | Descrição |
|----------------------|-------------------|-----------|
| **Advogado Interno** | 1.1 Personas | Usuário principal, recebe demandas de processos |
| **Analista Jurídico** | 1.1 Personas | Apoia na análise e organização de documentos |
| **Active Directory** | RF03 - Controle de Acesso | Autenticação LDAP/SSO |
| **Sistema ELO** | RF06 - Integração com ELO | Upload de documentos (condicional) |
| **File Server** | RF02 - Indexação Multi-Fonte | Pastas de rede SMB/CIFS |
| **SharePoint Online** | RF02 - Indexação Multi-Fonte | Graph API integration |
| **Sistema Sense** | 1.6 Gaps Identificados | Sistema legado (API a definir) |

**Referências:**
- Seção 1.1: Personas
- Seção 1.2: Requisitos Funcionais RF01-RF06
- Seção 2.2: Diagrama de Contexto (ASCII art)

---

### Diagrama de Container → Arquitetura Proposta

**Diagrama:** [c4-container.puml](./c4-container.puml)

| Container | Seção da Proposta | Detalhes Técnicos |
|-----------|-------------------|-------------------|
| **Frontend SPA** | 2.5 Stack Tecnológica | React 18 + TypeScript + TailwindCSS |
| **API Gateway** | 2.5 Stack Tecnológica | Kong OSS ou AWS API Gateway |
| **Search Service** | 2.3 Diagrama de Container | Python/FastAPI, query parsing, hybrid search |
| **Indexer Service** | 2.3 Diagrama de Container | Python/Celery, crawlers, embeddings |
| **Export Service** | RF05 - Exportação | Package builder, PDF generation |
| **PostgreSQL** | 2.5 Stack Tecnológica | Metadata, usuários, auditoria |
| **Qdrant** | ADR-002 | Vector database escolhido |
| **Redis** | 2.5 Stack Tecnológica | Cache, sessões, filas Celery |
| **FileServer Connector** | 4.3 Conector File Server | SMB/CIFS integration |
| **SharePoint Connector** | 4.2 Conector SharePoint | Graph API integration |
| **Sense Connector** | 4.4 Conector Sense | A definir (API/RPA) |

**Referências:**
- Seção 2.3: Diagrama de Container (ASCII art)
- Seção 2.5: Stack Tecnológica
- Seção 4: Integração de Sistemas

---

### Diagrama de Componentes → Search Service

**Diagrama:** [c4-component-search.puml](./c4-component-search.puml)

| Componente | Seção da Proposta | Funcionalidade |
|------------|-------------------|----------------|
| **Query Parser** | 3.3 Fluxo de Processamento | Extrai entidades: nome, data, tipo doc |
| **Query Embedder** | 3.4 Estratégia de Embeddings | text-embedding-3-large (3072d) |
| **Hybrid Search Engine** | ADR-001 | Vetorial (Qdrant) + Keyword (BM25) |
| **Access Control Filter** | RF03 - Controle de Acesso | Validação de permissões AD/LDAP |
| **Reranker** | 3.2 Modelos e Custos | Cohere Rerank v3 ou Cross-encoder |
| **Result Formatter** | RF01 - Busca Inteligente | JSON com highlights e scores |
| **Cache Controller** | 2.5 Stack Tecnológica | Redis cache de queries |

**Referências:**
- Seção 3.3: Fluxo de Processamento - Busca
- ADR-001: Busca Híbrida
- Seção 3.4: Estratégia de Embeddings

---

### Diagrama Dinâmico - Busca → Pipeline RAG

**Diagrama:** [c4-dynamic-search.puml](./c4-dynamic-search.puml)

| Passo | Seção da Proposta | Detalhe |
|-------|-------------------|---------|
| **1-3: Request flow** | 2.1 Visão Geral | Usuário → SPA → Gateway → Service |
| **4: Cache check** | 2.5 Stack - Redis | Verificação por hash da query |
| **5-6: Query parsing** | 3.3 Pipeline de Busca (1) | Extração de entidades |
| **7-8: Embedding** | 3.4 Estratégia de Embeddings | OpenAI text-embedding-3-large |
| **9-12: Hybrid search** | ADR-001 | Qdrant (vetorial) + PostgreSQL (keyword) |
| **13: Fusion** | ADR-001 | RRF - Reciprocal Rank Fusion |
| **14: Access filter** | RF03 | Validação ACL do usuário |
| **15: Reranking** | 3.2 Modelos | Cross-encoder reordena top 20 |
| **16: Cache save** | RNF01 Performance | TTL 1h |
| **17-19: Response** | RF01 Critérios de Aceite | < 5s P95, ranqueado por relevância |

**Referências:**
- Seção 2.4: Pipeline RAG Detalhado
- Seção 3.3: Pipeline de Busca
- ADR-001: Busca Híbrida

---

### Diagrama Dinâmico - Indexação → Pipeline RAG

**Diagrama:** [c4-dynamic-indexing.puml](./c4-dynamic-indexing.puml)

| Passo | Seção da Proposta | Detalhe |
|-------|-------------------|---------|
| **1-2: Scheduling** | 2.5 Stack - Celery | Celery Beat + Redis queue |
| **3-6: Crawling** | RF02 - Indexação Multi-Fonte | Detecta novos/alterados |
| **7-8: Deduplication** | RNF02 Performance | Hash SHA256, indexação incremental |
| **9-10: Download** | 4.2, 4.3 Conectores | SMB/Graph API |
| **11-13: Parsing + OCR** | 3.5 Estratégia de Chunking | PyPDF2, python-docx, Azure Doc Intelligence |
| **14-15: Chunking** | ADR-003 | 512 tokens, 50 tokens overlap |
| **16-17: Embeddings** | 3.4 Estratégia de Embeddings | Batch OpenAI API |
| **18: Vector storage** | ADR-002 | Qdrant batch insert |
| **19: Metadata storage** | 2.5 Stack - PostgreSQL | doc_id, path, hash, indexed_at |

**Referências:**
- Seção 2.4: Pipeline RAG - Indexação
- Seção 3.3: Fluxo de Processamento - Indexação
- ADR-003: Chunking por Documento
- Seção 3.5: Estratégia de Chunking

---

### Diagrama de Deployment → Infraestrutura

**Diagrama:** [c4-deployment.puml](./c4-deployment.puml)

| Componente Infra | Seção da Proposta | Especificação |
|------------------|-------------------|---------------|
| **AKS Cluster** | 5.4 Custos - Infra | 3 nodes app + 2 workers, Standard_D4s_v3 |
| **PostgreSQL Managed** | 5.4 Custos - Infra | 4 vCPU, 16GB, 500GB SSD |
| **Redis Managed** | 5.4 Custos - Infra | 4GB Standard C1 |
| **Qdrant Cloud** | 5.4 Custos - Infra | 2M vectors, 3 replicas |
| **Blob Storage** | 5.4 Custos - Infra | 1TB Hot Tier |
| **ExpressRoute/VPN** | 1.4 Restrições | Conectividade rede corporativa |
| **Corporate Network** | 4.1 Mapa de Integrações | AD, File Server, ELO on-premises |

**Referências:**
- Seção 5.4: Estimativa de Custos - Infraestrutura
- Seção 1.4: Restrições
- Seção 4.1: Mapa de Integrações

---

## Decisões Arquiteturais (ADRs) Refletidas nos Diagramas

### ADR-001: Busca Híbrida
- **Diagramas:** c4-component-search, c4-dynamic-search
- **Evidência:** Hybrid Search Engine combina Qdrant (vetorial) + PostgreSQL FTS (keyword)

### ADR-002: Qdrant como Vector Database
- **Diagramas:** c4-container, c4-deployment
- **Evidência:** ContainerDb(qdrant) em todos os diagramas de dados

### ADR-003: Chunking com Overlap
- **Diagramas:** c4-dynamic-indexing
- **Evidência:** Passo 14 - "Chunking: divide em chunks de 512 tokens com overlap de 50"

### ADR-004: Arquitetura de Conectores Plugável
- **Diagramas:** c4-container
- **Evidência:** Boundary "Conectores de Fontes" com FileServer, SharePoint, Sense isolados

---

## Gaps e Pendências Refletidas

| Gap (Seção 1.6) | Representação nos Diagramas |
|------------------|------------------------------|
| **API do sistema ELO** | c4-context: Rel "API REST/HTTPS TBD" |
| **Estrutura do sistema Sense** | c4-container: SenseConnector "API TBD" |
| **Tempo médio atual** | Não representado (métrica de negócio) |
| **Volume mensal** | Não representado (métrica de negócio) |

---

## Navegação Recomendada

**Para entender a arquitetura completa:**
1. Leia [SOLUTIONS_PROPOSAL.md](../SOLUTIONS_PROPOSAL.md) - Seção 2 (Arquitetura)
2. Visualize [c4-context.puml](./c4-context.puml) - Visão geral
3. Visualize [c4-container.puml](./c4-container.puml) - Containers e tecnologias
4. Visualize [c4-dynamic-search.puml](./c4-dynamic-search.puml) - Fluxo de busca
5. Visualize [c4-deployment.puml](./c4-deployment.puml) - Infraestrutura

**Para entender o pipeline RAG:**
1. Leia [SOLUTIONS_PROPOSAL.md](../SOLUTIONS_PROPOSAL.md) - Seção 3 (Componentes IA/ML)
2. Visualize [c4-dynamic-indexing.puml](./c4-dynamic-indexing.puml) - Como documentos são processados
3. Visualize [c4-component-search.puml](./c4-component-search.puml) - Como buscas funcionam
4. Visualize [c4-dynamic-search.puml](./c4-dynamic-search.puml) - Fluxo completo

**Para entender integrações:**
1. Leia [SOLUTIONS_PROPOSAL.md](../SOLUTIONS_PROPOSAL.md) - Seção 4 (Integrações)
2. Visualize [c4-context.puml](./c4-context.puml) - Sistemas externos
3. Visualize [c4-container.puml](./c4-container.puml) - Conectores específicos

---

**Última atualização:** 21 de Janeiro de 2026
**Versão:** 1.0
**Autor:** C4 Diagram Architect
