# Proposta Tecnica Comercial - JurisFinder

**Cliente:** ThyssenKrupp Brasil
**Projeto:** JurisFinder - Sistema Inteligente de Busca e Recuperacao de Documentos Juridicos
**Versao:** 1.0
**Data:** 21 de Janeiro de 2026

---

## 3.1 Visao Geral do Projeto

### O Desafio

A gestao de documentos para processos trabalhistas representa um dos maiores desafios operacionais enfrentados pelos departamentos juridicos corporativos. Na ThyssenKrupp, esse cenario se traduz em:

**Fragmentacao de Fontes de Dados**

Os documentos necessarios para a defesa em processos trabalhistas estao dispersos em multiplas plataformas: pastas de rede corporativas, SharePoint Online, e o sistema Sense. Essa fragmentacao exige que advogados e analistas realizem buscas manuais em cada sistema, consumindo tempo valioso e aumentando o risco de documentos criticos nao serem localizados.

**Risco Juridico Concreto**

A experiencia recente demonstra que a dificuldade em localizar documentos dentro dos prazos legais ja resultou em prejuizos processuais. Cada processo trabalhista perdido por ausencia de documentacao comprobatoria representa nao apenas custos financeiros diretos, mas tambem precedentes que podem impactar futuras acoes.

**Ineficiencia Operacional**

O processo atual de busca manual consome horas de trabalho de profissionais altamente qualificados em tarefas repetitivas e de baixo valor agregado. Esse tempo poderia ser direcionado para analise estrategica e tomada de decisoes.

### A Solucao: JurisFinder

O **JurisFinder** e um sistema inteligente de busca e recuperacao de documentos que utiliza tecnologia de ponta em Inteligencia Artificial para transformar completamente a forma como o departamento juridico acessa e gerencia documentos trabalhistas.

**Busca em Linguagem Natural**

Em vez de navegar por estruturas de pastas ou lembrar nomes exatos de arquivos, os usuarios podem simplesmente perguntar: *"Encontre os documentos de EPI do funcionario Joao Silva entre 2020 e 2023"*. O sistema compreende o contexto da solicitacao e retorna os documentos mais relevantes, ranqueados por pertinencia.

**Unificacao de Fontes**

O JurisFinder consolida todas as fontes de documentos em um indice unico e pesquisavel. Independentemente de onde o documento esteja armazenado - pasta de rede, SharePoint ou Sense - uma unica busca retorna todos os resultados relevantes.

**Inteligencia de Compliance**

O sistema conhece os tipos de documentos obrigatorios para cada categoria de processo trabalhista. Ao iniciar a preparacao de uma defesa, o JurisFinder identifica automaticamente quais documentos sao necessarios e alerta quando algum documento obrigatorio nao foi localizado.

**Seguranca Corporativa**

O JurisFinder respeita integralmente as politicas de acesso da ThyssenKrupp. A integracao com Active Directory garante que cada usuario visualize apenas os documentos para os quais possui permissao, mantendo a conformidade com LGPD e politicas internas.

### Beneficios Esperados

| Indicador | Situacao Atual | Com JurisFinder | Impacto |
|-----------|----------------|-----------------|---------|
| Tempo de busca por processo | ~4 horas | ~30 minutos | **Reducao de 87%** |
| Documentos nao localizados | Frequente | < 5% dos casos | **Reducao drastica de risco** |
| Processos perdidos por falta de docs | Ocorre | Zero | **Eliminacao de prejuizos** |
| Produtividade da equipe | Baseline | +70% em tarefas de busca | **Reorientacao estrategica** |

### Retorno sobre Investimento (ROI)

O investimento no JurisFinder se justifica tanto pela **reducao de custos operacionais** quanto pela **prevencao de perdas processuais**:

**Economia Operacional Direta**
- Reducao de 3+ horas por processo na fase de busca documental
- Considerando o volume mensal de processos, representa centenas de horas/ano de trabalho especializado liberadas

**Prevencao de Perdas**
- Cada processo trabalhista perdido por falha documental pode representar valores significativos em condenacoes
- A eliminacao desse risco representa um beneficio que supera o investimento no primeiro ano

**ROI Projetado:** 6 a 12 meses para recuperacao total do investimento, considerando apenas a reducao de tempo operacional - sem contabilizar a prevencao de perdas processuais.

---

## 3.2 Requisitos Tecnicos e Descricao do Sistema

### 3.2.1 Requisitos Funcionais

O JurisFinder foi especificado para atender as necessidades operacionais do departamento juridico da ThyssenKrupp, com funcionalidades que transformam a gestao documental.

#### RF01 - Busca Inteligente em Linguagem Natural

**Descricao:** O sistema permite que usuarios realizem buscas utilizando linguagem natural, sem necessidade de conhecer a estrutura de pastas ou nomenclaturas especificas.

**Capacidades:**
- Compreensao de consultas em portugues brasileiro
- Interpretacao de entidades: nomes de funcionarios, datas, tipos de documentos
- Busca semantica que encontra documentos relevantes mesmo sem correspondencia exata de palavras-chave
- Ranqueamento inteligente por relevancia

**Exemplo de Uso:**
> *"Preciso dos documentos de treinamento de seguranca do funcionario Maria Santos que trabalhou na planta de Sao Paulo entre 2019 e 2022"*

O sistema interpreta:
- Funcionario: Maria Santos
- Tipo de documento: Treinamento de seguranca
- Localidade: Planta de Sao Paulo
- Periodo: 2019-2022

E retorna todos os documentos que atendem a esses criterios, ordenados por relevancia.

**Criterios de Aceite:**
- Tempo de resposta inferior a 5 segundos para 95% das consultas
- Precisao de busca superior a 85% (documentos relevantes entre os 10 primeiros resultados)
- Suporte a filtros adicionais apos busca inicial

#### RF02 - Indexacao Multi-Fonte Unificada

**Descricao:** O sistema consolida documentos de todas as fontes corporativas em um indice unico, eliminando a necessidade de buscas separadas em cada sistema.

**Fontes Suportadas:**
| Fonte | Protocolo | Caracteristicas |
|-------|-----------|-----------------|
| Pastas de Rede | SMB/CIFS | File servers Windows corporativos |
| SharePoint Online | Microsoft Graph API | Sites e bibliotecas de documentos |
| Sistema Sense | API/Custom | Sistema legado de gestao documental |

**Processo de Indexacao:**
- Varredura inicial completa de todas as fontes configuradas
- Atualizacao incremental diaria (apenas documentos novos ou modificados)
- Extracao de texto de multiplos formatos: PDF, DOCX, XLSX, imagens (via OCR)
- Preservacao de metadados: autor, data de criacao, data de modificacao, permissoes

**Criterios de Aceite:**
- Indexacao incremental concluida em menos de 1 hora/dia
- Suporte a pelo menos 1 milhao de documentos indexados
- Deteccao automatica de documentos duplicados

#### RF03 - Controle de Acesso Integrado

**Descricao:** O sistema respeita integralmente as permissoes de acesso definidas no Active Directory e nas fontes originais, garantindo conformidade com politicas de seguranca e LGPD.

**Mecanismo de Seguranca:**
- Autenticacao via SSO/SAML integrado ao Active Directory corporativo
- Verificacao de permissoes em tempo real antes de exibir resultados
- Documentos sao indexados com suas ACLs (Access Control Lists)
- Usuario visualiza apenas documentos para os quais possui acesso na fonte original

**Auditoria:**
- Registro de todas as buscas realizadas (usuario, query, timestamp)
- Registro de todos os documentos visualizados
- Registro de todas as exportacoes
- Retencao de logs conforme politica corporativa

**Criterios de Aceite:**
- Nenhum documento exibido para usuarios sem permissao
- Tempo de verificacao de permissao < 500ms
- Logs de auditoria completos e consultaveis

#### RF04 - Identificacao de Documentos Obrigatorios

**Descricao:** O sistema conhece os tipos de documentos obrigatorios para cada categoria de processo trabalhista e auxilia na verificacao de completude.

**Funcionalidades:**
- Cadastro de checklists por tipo de processo (ex: acidente de trabalho, horas extras, assedio)
- Ao preparar documentacao para um processo, o sistema indica:
  - Documentos obrigatorios encontrados
  - Documentos obrigatorios nao localizados (alerta)
  - Documentos complementares sugeridos
- Relatorio de gaps documentais

**Exemplo de Checklist - Processo de Acidente de Trabalho:**
- [ ] CAT (Comunicacao de Acidente de Trabalho)
- [ ] Ficha de registro do funcionario
- [ ] Cartoes de ponto do periodo
- [ ] Documentos de treinamento de seguranca
- [ ] Fichas de EPI assinadas
- [ ] ASO (Atestado de Saude Ocupacional)
- [ ] Documentos do PPRA/PCMSO

**Criterios de Aceite:**
- Cadastro flexivel de regras por tipo de processo
- Alerta visual claro para documentos faltantes
- Exportacao de relatorio de gaps

#### RF05 - Preparacao e Exportacao de Pacotes

**Descricao:** O sistema permite organizar documentos selecionados em pacotes estruturados para uso em processos.

**Funcionalidades:**
- Selecao de multiplos documentos dos resultados de busca
- Organizacao em pastas logicas dentro do pacote
- Geracao automatica de indice/sumario
- Exportacao em formatos padrao: ZIP, PDF consolidado
- Numeracao sequencial de documentos (Bates numbering)

**Criterios de Aceite:**
- Pacotes gerados em menos de 2 minutos para ate 100 documentos
- Sumario automatico com links para cada documento
- Suporte a pacotes de ate 1GB

#### RF06 - Integracao com Sistema ELO (Condicional)

**Descricao:** Envio automatizado de documentos para o sistema ELO, otimizando o fluxo de trabalho.

**Nota:** Esta funcionalidade depende da disponibilidade de API no sistema ELO. Em caso de indisponibilidade, sera implementado o fluxo de exportacao manual.

**Cenario com API Disponivel:**
- Upload automatico de pacotes para o ELO
- Confirmacao de recebimento
- Vinculacao automatica ao processo correspondente

**Cenario sem API (Fallback):**
- Exportacao de pacote em formato compativel com upload manual
- Instrucoes de upload incluidas no pacote
- Checklist de verificacao pos-upload

### 3.2.2 Requisitos Nao-Funcionais

Os requisitos nao-funcionais garantem que o sistema opere com a qualidade, seguranca e performance esperadas em ambiente corporativo.

#### Performance

| Requisito | Meta | Justificativa |
|-----------|------|---------------|
| Tempo de busca | < 5 segundos (P95) | Experiencia de usuario fluida |
| Indexacao incremental | < 1 hora/dia | Dados sempre atualizados |
| Disponibilidade | 99.5% (horario comercial) | Criticidade operacional |
| Usuarios simultaneos | 50+ | Equipe juridica completa |

#### Seguranca

| Requisito | Especificacao | Conformidade |
|-----------|---------------|--------------|
| Autenticacao | SSO/SAML com AD corporativo | Politica TI ThyssenKrupp |
| Criptografia em transito | TLS 1.3 | LGPD, melhores praticas |
| Criptografia em repouso | AES-256 | LGPD, melhores praticas |
| Auditoria | Log de todas as operacoes | LGPD, compliance interno |
| Retencao de dados | Conforme politica corporativa | LGPD |

#### Escalabilidade

| Requisito | Meta | Horizonte |
|-----------|------|-----------|
| Volume de documentos | 1M+ documentos | Inicial |
| Crescimento anual | +20% | 3 anos |
| Armazenamento de vetores | 2M+ vetores | Considerando chunking |

#### Usabilidade

| Requisito | Meta |
|-----------|------|
| Tempo de aprendizado | < 30 minutos para funcoes basicas |
| Interface | Responsiva, compativel com navegadores corporativos |
| Acessibilidade | WCAG 2.1 nivel AA |

### 3.2.3 Restricoes Tecnicas

O projeto opera dentro das seguintes restricoes definidas pela ThyssenKrupp:

**Ambiente de Hospedagem:**
- Cloud (Azure ou AWS) aprovado, desde que atenda aos criterios de seguranca corporativos
- Dados devem permanecer em data centers localizados no Brasil ou com conformidade LGPD

**Conectividade:**
- Acesso a pastas de rede requer conectividade via VPN ou ExpressRoute
- SharePoint acessivel via internet com autenticacao OAuth2
- Sistema Sense pode requerer conectividade especifica (a validar)

**Integracao:**
- Autenticacao obrigatoriamente via AD corporativo
- Nao e permitido armazenar credenciais de usuarios
- APIs devem seguir padroes REST com autenticacao OAuth2/JWT

**Compliance:**
- Conformidade com LGPD obrigatoria
- Dados pessoais (nomes, CPFs) devem ser tratados com protecao adicional
- Logs de auditoria devem ser imutaveis

---

## 3.3 Arquitetura da Solucao

### 3.3.1 Visao Geral da Arquitetura

A arquitetura do JurisFinder foi projetada seguindo principios de **escalabilidade**, **seguranca** e **manutenibilidade**, utilizando tecnologias modernas e comprovadas em ambientes corporativos.

O sistema adota a arquitetura **RAG (Retrieval-Augmented Generation)**, que combina o poder da busca semantica com inteligencia artificial para entregar resultados precisos e contextualizados.

### 3.3.2 Diagrama de Contexto

O diagrama abaixo apresenta o JurisFinder no contexto dos sistemas e usuarios que interagem com ele:

```
                                  USUARIOS
                    ┌─────────────────────────────────────┐
                    │                                     │
           ┌────────▼────────┐              ┌─────────────▼─────────────┐
           │    Advogados    │              │    Analistas Juridicos   │
           │    Internos     │              │                          │
           └────────┬────────┘              └─────────────┬─────────────┘
                    │                                     │
                    │  Busca documentos                   │  Organiza pacotes
                    │  Visualiza resultados               │  Exporta documentos
                    │                                     │
                    └──────────────┬──────────────────────┘
                                   │
                                   ▼
    ┌──────────────────────────────────────────────────────────────────────┐
    │                                                                      │
    │                          JURISFINDER                                 │
    │                                                                      │
    │    Sistema Inteligente de Busca e Recuperacao de Documentos          │
    │                                                                      │
    └──────────────────────────────────────────────────────────────────────┘
                                   │
         ┌─────────────────────────┼─────────────────────────┐
         │                         │                         │
         ▼                         ▼                         ▼
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   ACTIVE        │     │   FONTES DE     │     │   SISTEMAS      │
│   DIRECTORY     │     │   DOCUMENTOS    │     │   DESTINO       │
│                 │     │                 │     │                 │
│ - Autenticacao  │     │ - File Server   │     │ - Sistema ELO   │
│ - Permissoes    │     │ - SharePoint    │     │ - Email/Teams   │
│ - Grupos        │     │ - Sense         │     │                 │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

### 3.3.3 Arquitetura de Componentes

O sistema e composto por camadas bem definidas, cada uma com responsabilidades especificas:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                            CAMADA DE APRESENTACAO                           │
│                                                                             │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                    FRONTEND WEB (SPA)                                  │  │
│  │                                                                        │  │
│  │    React 18 + TypeScript + TailwindCSS                                │  │
│  │                                                                        │  │
│  │    Modulos:                                                            │  │
│  │    - Interface de Busca: Campo de busca, filtros, sugestoes           │  │
│  │    - Visualizacao de Resultados: Lista, preview, metadados            │  │
│  │    - Gestao de Pacotes: Selecao, organizacao, exportacao              │  │
│  │    - Administracao: Configuracoes, regras, monitoramento              │  │
│  │                                                                        │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                      │                                      │
│                                      │ HTTPS / REST API                     │
│                                      ▼                                      │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │                      API GATEWAY                                       │  │
│  │                                                                        │  │
│  │    Kong OSS ou AWS API Gateway                                        │  │
│  │                                                                        │  │
│  │    Funcoes:                                                            │  │
│  │    - Autenticacao JWT (validacao de tokens)                           │  │
│  │    - Rate Limiting (protecao contra abuso)                            │  │
│  │    - Logging centralizado (auditoria)                                 │  │
│  │    - Roteamento para microservicos                                    │  │
│  │                                                                        │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                            CAMADA DE SERVICOS                               │
│                                                                             │
│  ┌─────────────────────┐  ┌─────────────────────┐  ┌─────────────────────┐  │
│  │   SEARCH SERVICE    │  │  INDEXER SERVICE    │  │  EXPORT SERVICE     │  │
│  │                     │  │                     │  │                     │  │
│  │  Python + FastAPI   │  │  Python + Celery    │  │  Python + FastAPI   │  │
│  │                     │  │                     │  │                     │  │
│  │  Responsabilidades: │  │  Responsabilidades: │  │  Responsabilidades: │  │
│  │  - Query parsing    │  │  - Crawling fontes  │  │  - Montagem pacotes │  │
│  │  - Busca hibrida    │  │  - Extracao texto   │  │  - Geracao PDF      │  │
│  │  - Reranking        │  │  - Chunking         │  │  - Integracao ELO   │  │
│  │  - Filtragem ACL    │  │  - Embeddings       │  │  - Notificacoes     │  │
│  │                     │  │  - Metadados        │  │                     │  │
│  └──────────┬──────────┘  └──────────┬──────────┘  └─────────────────────┘  │
│             │                        │                                      │
└─────────────┼────────────────────────┼──────────────────────────────────────┘
              │                        │
              ▼                        ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                            CAMADA DE DADOS                                  │
│                                                                             │
│  ┌─────────────────────┐  ┌─────────────────────┐  ┌─────────────────────┐  │
│  │     POSTGRESQL      │  │       QDRANT        │  │       REDIS         │  │
│  │                     │  │                     │  │                     │  │
│  │  Banco Relacional   │  │   Vector Database   │  │   Cache + Queue     │  │
│  │                     │  │                     │  │                     │  │
│  │  Armazena:          │  │  Armazena:          │  │  Armazena:          │  │
│  │  - Metadados docs   │  │  - Embeddings       │  │  - Cache de queries │  │
│  │  - Usuarios/roles   │  │  - Vetores 3072d    │  │  - Sessoes          │  │
│  │  - Audit logs       │  │  - Payload filtros  │  │  - Filas de tarefas │  │
│  │  - Configuracoes    │  │                     │  │                     │  │
│  │  - Regras negocio   │  │                     │  │                     │  │
│  │                     │  │                     │  │                     │  │
│  └─────────────────────┘  └─────────────────────┘  └─────────────────────┘  │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                         CAMADA DE CONECTORES                                │
│                                                                             │
│  ┌─────────────────────┐  ┌─────────────────────┐  ┌─────────────────────┐  │
│  │  FILE SERVER        │  │  SHAREPOINT         │  │  SENSE              │  │
│  │  CONNECTOR          │  │  CONNECTOR          │  │  CONNECTOR          │  │
│  │                     │  │                     │  │                     │  │
│  │  Protocolo: SMB     │  │  Protocolo: REST    │  │  Protocolo: TBD     │  │
│  │  Auth: Kerberos     │  │  API: Graph API     │  │  (A ser definido)   │  │
│  │                     │  │  Auth: OAuth2       │  │                     │  │
│  └─────────────────────┘  └─────────────────────┘  └─────────────────────┘  │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 3.3.4 Pipeline de Inteligencia Artificial

O coracao do JurisFinder e seu pipeline de IA, que combina tecnicas de **busca semantica** e **busca por palavras-chave** para entregar resultados precisos.

#### Pipeline de Indexacao

```
DOCUMENTO        EXTRACAO         DIVISAO          VETORIZACAO       ARMAZENAMENTO
ORIGINAL         DE TEXTO         EM CHUNKS        (EMBEDDING)
   │                │                │                  │                  │
   ▼                ▼                ▼                  ▼                  ▼
┌──────┐       ┌──────────┐    ┌───────────┐     ┌───────────┐     ┌───────────┐
│ PDF  │──────►│ Extrair  │───►│ Dividir   │────►│ Gerar     │────►│ Salvar    │
│ DOCX │       │ texto    │    │ em chunks │     │ vetores   │     │ em Qdrant │
│ XLSX │       │ (OCR se  │    │ de 512    │     │ de 3072   │     │           │
│ IMG  │       │ necessario)   │ tokens    │     │ dimensoes │     │           │
└──────┘       └──────────┘    └───────────┘     └───────────┘     └───────────┘
                    │                │                  │
                    ▼                ▼                  ▼
              Azure Document   Overlap de 50      OpenAI text-
              Intelligence     tokens para        embedding-3-large
              (para PDFs       preservar
              escaneados)      contexto
```

**Detalhes Tecnicos:**

| Etapa | Tecnologia | Configuracao |
|-------|------------|--------------|
| Extracao de texto | PyPDF2, python-docx, Azure Document Intelligence | Suporte a PDF, DOCX, XLSX, imagens |
| OCR | Azure Document Intelligence | Para documentos escaneados |
| Chunking | LangChain Text Splitter | 512 tokens, overlap 50 tokens |
| Embeddings | OpenAI text-embedding-3-large | 3072 dimensoes, alta precisao |
| Armazenamento | Qdrant | Busca vetorial otimizada |

#### Pipeline de Busca

```
QUERY DO         ANALISE          BUSCA            REORDENACAO       FILTRAGEM
USUARIO          DA QUERY         HIBRIDA          (RERANKING)       DE ACESSO
   │                │                │                  │                 │
   ▼                ▼                ▼                  ▼                 ▼
┌──────────┐   ┌──────────┐    ┌───────────┐     ┌───────────┐    ┌───────────┐
│"Docs EPI │──►│ Extrair  │───►│ Busca     │────►│ Reordenar │───►│ Filtrar   │
│ do Joao  │   │ entidades│    │ vetorial  │     │ por       │    │ por       │
│ Silva    │   │ e intent │    │     +     │     │ relevancia│    │ permissoes│
│ 2020-23" │   │          │    │ keywords  │     │           │    │ do user   │
└──────────┘   └──────────┘    └───────────┘     └───────────┘    └───────────┘
                    │                │                  │                 │
                    ▼                ▼                  ▼                 ▼
              GPT-4o-mini      Qdrant (vetorial)   Cohere Rerank    Verificacao
              para parsing    + BM25 (keyword)    ou Cross-encoder  ACL em tempo
              de entidades    com RRF fusion                        real
```

**Por que Busca Hibrida?**

Documentos juridicos possuem caracteristicas unicas que exigem uma abordagem combinada:

- **Termos tecnicos precisos:** Siglas como "ASO", "PPRA", "PCMSO", "CAT" precisam de correspondencia exata
- **Contexto semantico:** O usuario pode pedir "documentos de seguranca" quando na verdade procura "fichas de EPI"
- **Variacao linguistica:** Diferentes formas de expressar a mesma necessidade

A **busca hibrida** combina:
- **Busca vetorial:** Encontra documentos semanticamente similares
- **Busca BM25:** Garante match exato de termos tecnicos
- **RRF Fusion:** Combina os resultados de forma balanceada

### 3.3.5 Stack Tecnologica

A selecao de tecnologias foi realizada considerando **maturidade**, **suporte corporativo**, **custo-beneficio** e **adequacao ao problema**.

| Camada | Tecnologia | Justificativa |
|--------|------------|---------------|
| **Frontend** | React 18 + TypeScript + TailwindCSS | Framework maduro, tipagem estatica, produtividade |
| **API Gateway** | Kong OSS ou AWS API Gateway | Rate limiting, auth, observabilidade nativa |
| **Backend** | Python 3.12 + FastAPI | Ecossistema ML/AI completo, performance async |
| **Task Queue** | Celery + Redis | Processamento assincrono robusto |
| **Vector DB** | Qdrant | Open-source, performance, filtragem nativa |
| **Relational DB** | PostgreSQL 16 | Confiabilidade, features avancadas |
| **Cache** | Redis | Velocidade, versatilidade |
| **Embeddings** | OpenAI text-embedding-3-large | Estado da arte em qualidade semantica |
| **LLM** | GPT-4o-mini | Parsing de queries, custo otimizado |
| **Reranker** | Cohere Rerank | Melhoria significativa em precisao |
| **OCR** | Azure Document Intelligence | Qualidade superior para docs escaneados |
| **Container** | Docker + Kubernetes | Escalabilidade, portabilidade |
| **CI/CD** | GitHub Actions | Automacao completa |
| **Observabilidade** | Prometheus + Grafana + Loki | Metricas, logs, alertas |

### 3.3.6 Decisoes Arquiteturais (ADRs)

#### ADR-001: Busca Hibrida (Semantica + Keyword)

**Contexto:** Documentos juridicos contem termos tecnicos que exigem match exato, alem de contexto semantico.

**Decisao:** Implementar busca hibrida combinando busca vetorial (Qdrant) com BM25, usando RRF para fusao de resultados.

**Consequencias:**
- (+) Alta precisao em termos tecnicos
- (+) Captura contexto semantico
- (-) Maior complexidade de infraestrutura

#### ADR-002: Qdrant como Vector Database

**Contexto:** Necessidade de armazenar embeddings de 1M+ documentos com filtragem por metadados.

**Decisao:** Usar Qdrant em vez de Pinecone, Weaviate ou pgvector.

**Justificativa:**
- Open-source (possibilidade de deploy on-premises se necessario)
- Performance superior em grandes volumes
- Filtragem nativa por payload (essencial para ACLs)
- Custo controlado

#### ADR-003: Chunking com Overlap

**Contexto:** Documentos variam de 1 a 100+ paginas. Chunks muito grandes perdem precisao; muito pequenos perdem contexto.

**Decisao:** Chunks de 512 tokens com 50 tokens de overlap, respeitando limites de secao.

**Consequencias:**
- (+) Contexto preservado entre chunks
- (+) Busca precisa em documentos longos
- (-) Volume de vetores ~3x maior que numero de documentos

### 3.3.7 Seguranca da Arquitetura

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        CAMADAS DE SEGURANCA                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  1. PERIMETRO                                                               │
│     - WAF (Web Application Firewall)                                        │
│     - DDoS Protection                                                       │
│     - IP Whitelisting (opcional)                                            │
│                                                                             │
│  2. AUTENTICACAO                                                            │
│     - SSO/SAML com Active Directory                                         │
│     - MFA (se habilitado no AD)                                             │
│     - Tokens JWT com expiracao curta (15 min)                               │
│                                                                             │
│  3. AUTORIZACAO                                                             │
│     - RBAC (Role-Based Access Control)                                      │
│     - Verificacao de ACL por documento                                      │
│     - Principio do menor privilegio                                         │
│                                                                             │
│  4. CRIPTOGRAFIA                                                            │
│     - TLS 1.3 em todas as comunicacoes                                      │
│     - AES-256 para dados em repouso                                         │
│     - Chaves gerenciadas em Key Vault                                       │
│                                                                             │
│  5. AUDITORIA                                                               │
│     - Logs imutaveis de todas as operacoes                                  │
│     - Rastreabilidade completa por usuario                                  │
│     - Retencao conforme politica corporativa                                │
│                                                                             │
│  6. COMPLIANCE                                                              │
│     - LGPD: Dados pessoais protegidos                                       │
│     - Direito ao esquecimento: Processo definido                            │
│     - Data residency: Brasil ou conformidade                                │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 3.4 Fases do Projeto

O projeto JurisFinder sera executado em **6 fases** bem definidas, cada uma com entregas especificas e criterios de aceite claros. Essa estrutura permite validacoes incrementais e ajustes ao longo do desenvolvimento.

### Fase 1: Discovery e Setup (3 semanas)

**Objetivo:** Estabelecer bases solidas para o projeto atraves de levantamento detalhado, configuracao de ambiente e alinhamento de expectativas.

**Atividades:**

| Atividade | Descricao | Entregavel |
|-----------|-----------|------------|
| Kick-off | Reuniao de alinhamento com stakeholders | Ata de kick-off |
| Levantamento tecnico | Analise detalhada dos sistemas Sense, ELO, estrutura de pastas | Documento de analise tecnica |
| Workshop de regras | Definicao de documentos obrigatorios por tipo de processo | Matriz de documentos |
| Definicao de metricas | Baseline atual para medir ROI | Documento de metricas baseline |
| Setup de ambiente | Configuracao de ambiente de desenvolvimento e staging | Ambientes funcionais |
| Arquitetura detalhada | Refinamento da arquitetura com base no levantamento | Documento de arquitetura v2 |

**Criterios de Saida:**
- Ambientes de desenvolvimento e staging configurados
- Documentacao tecnica dos sistemas legados completa
- Regras de negocio documentadas e aprovadas
- Metricas baseline definidas

### Fase 2: Core RAG Pipeline (6 semanas)

**Objetivo:** Desenvolver o motor de busca inteligente, que e o coracao do sistema.

**Atividades:**

| Semana | Foco | Entregavel |
|--------|------|------------|
| 1-2 | Infraestrutura de dados | PostgreSQL, Qdrant, Redis configurados |
| 2-3 | Pipeline de indexacao | Extracao de texto, chunking, embeddings |
| 3-4 | Pipeline de busca | Busca hibrida (vetorial + BM25) |
| 4-5 | Reranking e refinamento | Integracao com Cohere/Cross-encoder |
| 5-6 | Testes e otimizacao | Metricas de qualidade (Recall, MRR) |

**Marcos Tecnicos:**
- Indexacao de dataset de teste (10.000 documentos)
- Recall@10 > 85% em queries de teste
- Latencia P95 < 3 segundos

**Criterios de Saida:**
- Pipeline de indexacao funcional
- Pipeline de busca com metricas atingidas
- Testes automatizados implementados

### Fase 3: Conectores de Fontes (5 semanas)

**Objetivo:** Integrar todas as fontes de documentos corporativas.

**Atividades:**

| Semana | Foco | Entregavel |
|--------|------|------------|
| 1-2 | Conector File Server | Acesso SMB/CIFS funcional |
| 2-3 | Conector SharePoint | Integracao Graph API funcional |
| 3-4 | Conector Sense | Integracao customizada (a definir) |
| 4-5 | Sincronizacao incremental | Deteccao de alteracoes, delta sync |

**Desafios Conhecidos:**
- Sistema Sense pode requerer abordagem customizada (API, database, ou RPA)
- Conectividade de rede com file servers pode exigir configuracao especial

**Criterios de Saida:**
- Todos os conectores funcionais em ambiente de staging
- Sincronizacao incremental operacional
- Documentacao de configuracao completa

### Fase 4: Frontend e UX (5 semanas)

**Objetivo:** Desenvolver interface intuitiva e eficiente para os usuarios finais.

**Atividades:**

| Semana | Foco | Entregavel |
|--------|------|------------|
| 1 | Design System e prototipos | Wireframes, componentes base |
| 2-3 | Interface de busca | Campo de busca, filtros, sugestoes |
| 3-4 | Visualizacao de resultados | Lista, preview, metadados |
| 4-5 | Gestao de pacotes | Selecao, organizacao, exportacao |

**Principios de UX:**
- **Simplicidade:** Usuario realiza busca em menos de 3 cliques
- **Feedback imediato:** Indicadores de progresso, mensagens claras
- **Produtividade:** Atalhos de teclado, workflows otimizados

**Criterios de Saida:**
- Interface completa e funcional
- Testes de usabilidade realizados
- Documentacao de usuario (help inline)

### Fase 5: Integracoes e Seguranca (3 semanas)

**Objetivo:** Integrar com sistemas corporativos e garantir conformidade de seguranca.

**Atividades:**

| Semana | Foco | Entregavel |
|--------|------|------------|
| 1 | Integracao AD/SSO | Autenticacao corporativa funcional |
| 1-2 | Controle de acesso | ACLs, auditoria |
| 2-3 | Integracao ELO (se API disponivel) | Upload automatico ou fallback |
| 3 | Notificacoes | Alertas email/Teams |

**Criterios de Saida:**
- SSO funcional com AD corporativo
- Auditoria completa de operacoes
- Integracao ELO ou fallback definido

### Fase 6: Testes e Go-Live (3 semanas)

**Objetivo:** Garantir qualidade, performance e preparar equipe para operacao.

**Atividades:**

| Semana | Foco | Entregavel |
|--------|------|------------|
| 1 | Testes de integracao | Cenarios end-to-end validados |
| 1-2 | Testes de performance | Carga, stress, baseline |
| 2 | Testes de seguranca | Pentest, vulnerabilidades |
| 2-3 | Treinamento | Capacitacao de usuarios e TI |
| 3 | Go-live | Sistema em producao |

**Criterios de Go-Live:**
- [ ] Todos os testes passando (unit, integracao, e2e)
- [ ] Performance dentro das metas (P95 < 5s)
- [ ] Sem vulnerabilidades criticas ou altas
- [ ] Usuarios treinados (minimo 80% da equipe)
- [ ] Runbook de operacao documentado
- [ ] Suporte pos-go-live definido

---

## 4. Cronograma

### 4.1 Visao Geral do Cronograma

O projeto tem duracao estimada de **20 semanas (5 meses)**, com inicio previsto apos aprovacao e resolucao dos gaps identificados.

```
                         MES 1        MES 2        MES 3        MES 4        MES 5
                    ┌────────────┬────────────┬────────────┬────────────┬────────────┐
 FASE 1             │████████████│            │            │            │            │
 Discovery & Setup  │   3 sem    │            │            │            │            │
                    ├────────────┼────────────┼────────────┼────────────┼────────────┤
 FASE 2             │       █████│████████████│████████    │            │            │
 Core RAG Pipeline  │            │   6 sem    │            │            │            │
                    ├────────────┼────────────┼────────────┼────────────┼────────────┤
 FASE 3             │            │       █████│████████████│████        │            │
 Conectores         │            │            │   5 sem    │            │            │
                    ├────────────┼────────────┼────────────┼────────────┼────────────┤
 FASE 4             │            │            │       █████│████████████│████        │
 Frontend & UX      │            │            │            │   5 sem    │            │
                    ├────────────┼────────────┼────────────┼────────────┼────────────┤
 FASE 5             │            │            │            │       █████│████████    │
 Integracoes        │            │            │            │            │   3 sem    │
                    ├────────────┼────────────┼────────────┼────────────┼────────────┤
 FASE 6             │            │            │            │            │   ████████ │
 Testes & Go-Live   │            │            │            │            │   3 sem    │
                    └────────────┴────────────┴────────────┴────────────┴────────────┘

 MARCOS PRINCIPAIS:

 [M1] Ambiente configurado ────────────────────────► Semana 3
 [M2] Pipeline RAG funcional ──────────────────────► Semana 9
 [M3] Conectores integrados ───────────────────────► Semana 14
 [M4] MVP completo (interno) ──────────────────────► Semana 17
 [M5] Go-Live ─────────────────────────────────────► Semana 20
```

### 4.2 Cronograma Detalhado

| Semana | Fase | Atividades Principais | Entregaveis |
|--------|------|----------------------|-------------|
| 1 | Discovery | Kick-off, levantamento inicial | Ata, plano detalhado |
| 2 | Discovery | Analise tecnica sistemas, workshop regras | Doc analise, matriz docs |
| 3 | Discovery | Setup ambientes, arquitetura v2 | Ambientes, arquitetura |
| 4-5 | RAG Core | Infraestrutura dados, pipeline indexacao | Databases, indexador v1 |
| 6-7 | RAG Core | Pipeline busca, busca hibrida | Motor de busca v1 |
| 8-9 | RAG Core | Reranking, testes, otimizacao | Pipeline completo |
| 10-11 | Conectores | File Server, SharePoint | Conectores funcionais |
| 12-13 | Conectores | Sense, sync incremental | Todos conectores |
| 14 | Conectores | Integracao final, validacao | Fontes unificadas |
| 15-16 | Frontend | Design, busca, resultados | Interface v1 |
| 17 | Frontend | Pacotes, refinamentos | Interface completa |
| 18 | Integracoes | AD/SSO, ACLs, ELO | Seguranca completa |
| 19 | Testes | QA, performance, seguranca | Relatorios de teste |
| 20 | Go-Live | Treinamento, deploy, suporte | Sistema em producao |

### 4.3 Dependencias Criticas

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          CAMINHO CRITICO                                    │
└─────────────────────────────────────────────────────────────────────────────┘

  Discovery ──► RAG Core ──► Conectores ──► Integracoes ──► Go-Live
      │             │             │              │              │
      │             │             │              │              │
      ▼             ▼             ▼              ▼              ▼
  [3 sem]       [6 sem]       [5 sem]        [3 sem]        [3 sem]


  PARALELISMO POSSIVEL:

  RAG Core ────────────────────────►
                                    │
  Frontend (inicio semana 14) ──────┼──────────►
                                    │
  Conectores ───────────────────────►
```

**Dependencias Externas (ThyssenKrupp):**
- Acesso a ambientes e sistemas (Discovery)
- Validacao de regras de negocio (Discovery)
- Homologacao de usuarios (Testes)
- Aprovacao de go-live (Go-Live)

---

## 5. Gestao do Projeto

*[Secao mantida conforme documento existente]*

---

## 6. Produtos a Serem Entregues

O projeto JurisFinder resulta na entrega de um conjunto abrangente de produtos, organizados em **software**, **documentacao** e **capacitacao**.

### 6.1 Software

#### 6.1.1 Sistema JurisFinder (Aplicacao Principal)

| Componente | Descricao | Tecnologia |
|------------|-----------|------------|
| **Frontend Web** | Interface de usuario responsiva | React, TypeScript |
| **API Backend** | Servicos REST para todas as operacoes | Python, FastAPI |
| **Search Service** | Motor de busca inteligente | Python, Qdrant |
| **Indexer Service** | Processamento e indexacao de documentos | Python, Celery |
| **Export Service** | Geracao de pacotes e integracao ELO | Python |

**Funcionalidades Incluidas:**
- Busca em linguagem natural com resultados ranqueados
- Visualizacao de documentos com preview
- Filtros avancados (data, tipo, fonte, autor)
- Selecao e organizacao de documentos em pacotes
- Exportacao em ZIP e PDF consolidado
- Identificacao de documentos obrigatorios por tipo de processo
- Dashboard de metricas de uso

#### 6.1.2 Conectores de Fontes

| Conector | Protocolo | Funcionalidades |
|----------|-----------|-----------------|
| **File Server** | SMB/CIFS | Varredura recursiva, deteccao de alteracoes |
| **SharePoint Online** | Microsoft Graph API | Listagem de sites, bibliotecas, documentos |
| **Sistema Sense** | Customizado | Conforme analise tecnica |

#### 6.1.3 Infraestrutura como Codigo

| Item | Descricao |
|------|-----------|
| **Terraform/Pulumi** | Scripts de provisionamento de infraestrutura cloud |
| **Kubernetes Manifests** | Configuracoes de deploy, services, ingress |
| **Docker Compose** | Ambiente local de desenvolvimento |
| **CI/CD Pipelines** | GitHub Actions para build, test, deploy |

### 6.2 Documentacao

#### 6.2.1 Documentacao Tecnica

| Documento | Descricao | Audiencia |
|-----------|-----------|-----------|
| **Arquitetura do Sistema** | Diagramas C4, decisoes arquiteturais (ADRs), fluxos | Desenvolvedores, Arquitetos |
| **API Reference** | Especificacao OpenAPI 3.0 de todas as APIs | Desenvolvedores |
| **Guia de Desenvolvimento** | Setup local, padroes de codigo, contribuicao | Desenvolvedores |
| **Guia de Deploy** | Procedimentos de deploy, rollback, configuracao | DevOps, TI |
| **Runbook de Operacao** | Procedimentos para incidentes, monitoramento, manutencao | TI, Suporte |

#### 6.2.2 Documentacao de Usuario

| Documento | Descricao | Audiencia |
|-----------|-----------|-----------|
| **Manual do Usuario** | Guia completo de uso do sistema | Usuarios finais |
| **Guia Rapido** | Quick start de 5 minutos | Novos usuarios |
| **FAQ** | Perguntas frequentes e solucoes | Todos |
| **Videos Tutoriais** | Demonstracoes das principais funcionalidades | Usuarios finais |

#### 6.2.3 Documentacao de Gestao

| Documento | Descricao |
|-----------|-----------|
| **Plano de Projeto** | Escopo, cronograma, recursos, riscos |
| **Relatorios de Status** | Atualizacoes semanais durante o projeto |
| **Documento de Aceite** | Criterios e validacao formal de entrega |

### 6.3 Capacitacao

#### 6.3.1 Treinamentos

| Treinamento | Duracao | Audiencia | Conteudo |
|-------------|---------|-----------|----------|
| **Usuario Final** | 4 horas | Advogados, Analistas | Busca, visualizacao, pacotes |
| **Administrador** | 8 horas | TI | Configuracao, monitoramento, troubleshooting |
| **Avancado** | 4 horas | Power users | Filtros avancados, regras, otimizacao |

#### 6.3.2 Materiais de Treinamento

- Apresentacoes em formato reutilizavel
- Exercicios praticos com cenarios reais
- Ambiente de treinamento com dados de exemplo
- Certificados de conclusao (opcional)

### 6.4 Resumo de Entregas por Fase

| Fase | Entregas Principais |
|------|---------------------|
| **Discovery** | Documento de arquitetura v2, Matriz de documentos obrigatorios, Ambientes configurados |
| **RAG Core** | Pipeline de indexacao, Pipeline de busca, Metricas de qualidade |
| **Conectores** | Conectores File Server, SharePoint, Sense integrados |
| **Frontend** | Aplicacao web completa, Guia rapido do usuario |
| **Integracoes** | SSO/AD funcional, Auditoria, Integracao ELO |
| **Go-Live** | Sistema em producao, Documentacao completa, Usuarios treinados |

---

## 7. Responsabilidades

A execucao bem-sucedida do projeto depende da colaboracao entre a **Equipe de Desenvolvimento** e a **ThyssenKrupp**. Abaixo estao definidas as responsabilidades de cada parte.

### 7.1 Matriz RACI

| Atividade | Dev Team | TK Juridico | TK TI | TK Gestao |
|-----------|:--------:|:-----------:|:-----:|:---------:|
| Desenvolvimento do sistema | **R** | I | C | A |
| Definicao de regras de negocio | C | **R** | I | A |
| Fornecimento de acesso a sistemas | I | C | **R** | A |
| Validacao de entregas | I | **R** | C | A |
| Infraestrutura cloud | **R** | I | C | A |
| Integracao com AD | C | I | **R** | A |
| Treinamento de usuarios | **R** | C | C | A |
| Suporte pos-go-live (90 dias) | **R** | C | C | A |
| Operacao continuada | C | I | **R** | A |

**Legenda:** R = Responsavel, A = Aprova, C = Consultado, I = Informado

### 7.2 Responsabilidades da Equipe de Desenvolvimento

#### Gestao do Projeto
- Planejamento detalhado e acompanhamento de cronograma
- Comunicacao proativa de riscos e impedimentos
- Relatorios semanais de status
- Gestao de mudancas de escopo

#### Desenvolvimento
- Arquitetura e design do sistema
- Codificacao seguindo melhores praticas
- Testes unitarios, integracao e e2e
- Code review e garantia de qualidade
- Documentacao tecnica

#### Infraestrutura
- Provisionamento de ambientes cloud
- Configuracao de CI/CD
- Monitoramento e observabilidade
- Backup e disaster recovery

#### Entrega
- Instalacao e configuracao em producao
- Treinamento de usuarios e administradores
- Documentacao de usuario e operacao
- Suporte pos-go-live (90 dias)

### 7.3 Responsabilidades da ThyssenKrupp

#### Gestao
- Designar Product Owner com autoridade de decisao
- Aprovar entregas dentro dos prazos acordados
- Garantir disponibilidade dos stakeholders para reunioes
- Aprovar mudancas de escopo e prioridades

#### Juridico
- Definir regras de documentos obrigatorios por tipo de processo
- Validar relevancia dos resultados de busca
- Participar de testes de aceitacao
- Fornecer feedback sobre usabilidade

#### TI
- Fornecer acesso aos sistemas (Sense, File Servers, SharePoint)
- Configurar conectividade de rede necessaria (VPN, ExpressRoute)
- Integrar autenticacao com Active Directory
- Fornecer ambiente de staging com dados anonimizados
- Assumir operacao do sistema apos go-live

#### Seguranca
- Validar arquitetura de seguranca proposta
- Aprovar fluxos de autenticacao e autorizacao
- Revisar resultados de testes de seguranca
- Aprovar conformidade com LGPD

### 7.4 Composicao da Equipe de Desenvolvimento

| Papel | Qtd | Dedicacao | Responsabilidades |
|-------|-----|-----------|-------------------|
| **Tech Lead / Arquiteto** | 1 | 100% | Arquitetura, decisoes tecnicas, code review |
| **Backend Developer Senior** | 2 | 100% | RAG pipeline, APIs, conectores |
| **Frontend Developer** | 1 | 100% | Interface React, UX |
| **ML Engineer** | 1 | 50% | Embeddings, metricas de qualidade |
| **DevOps/SRE** | 1 | 50% | Infraestrutura, CI/CD, monitoramento |
| **QA Engineer** | 1 | 50%* | Testes, automacao |

*QA com maior dedicacao nas fases finais do projeto.

### 7.5 Governanca do Projeto

#### Reunioes Recorrentes

| Reuniao | Frequencia | Participantes | Objetivo |
|---------|------------|---------------|----------|
| Daily Standup | Diaria | Equipe Dev | Alinhamento diario |
| Sprint Review | Quinzenal | Todos | Demo de entregas |
| Sprint Planning | Quinzenal | Todos | Planejamento |
| Steering Committee | Mensal | Gestao | Decisoes estrategicas |

#### Canais de Comunicacao

| Tipo | Canal | Uso |
|------|-------|-----|
| Comunicacao diaria | Teams/Slack | Duvidas, alinhamentos rapidos |
| Documentacao | Confluence/Notion | Documentos, decisoes |
| Codigo | GitHub | Repositorio, code review |
| Incidentes | Email + Telefone | Urgencias, escalacoes |

---

## 8. Premissas Tecnicas

O sucesso do projeto depende da validacao das seguintes premissas. Caso alguma premissa nao se confirme, ajustes de escopo, prazo ou custo poderao ser necessarios.

### 8.1 Premissas de Infraestrutura

| ID | Premissa | Impacto se Invalida |
|----|----------|---------------------|
| P01 | Ambiente cloud (Azure ou AWS) aprovado para uso | Necessario redesenhar para on-premises |
| P02 | Conectividade de rede entre cloud e file servers corporativos disponivel (VPN ou ExpressRoute) | Impossibilita integracao com pastas de rede |
| P03 | Ambiente de staging com dados anonimizados sera fornecido | Atraso no desenvolvimento e testes |
| P04 | Recursos de GPU nao sao necessarios (embeddings via API) | Aumento de custo de infraestrutura |

### 8.2 Premissas de Integracao

| ID | Premissa | Impacto se Invalida |
|----|----------|---------------------|
| P05 | SharePoint Online acessivel via Microsoft Graph API | Necessario abordagem alternativa para SharePoint |
| P06 | Sistema Sense possui alguma forma de integracao (API, database, ou export) | Escopo do conector Sense pode aumentar significativamente |
| P07 | Active Directory corporativo suporta integracao SAML/OIDC | Necessario implementar autenticacao alternativa |
| P08 | Permissoes de documentos nas fontes estao corretamente configuradas | Sistema pode exibir documentos incorretamente |

### 8.3 Premissas de Negocio

| ID | Premissa | Impacto se Invalida |
|----|----------|---------------------|
| P09 | Regras de documentos obrigatorios serao definidas com equipe juridica | Funcionalidade RF04 nao podera ser implementada |
| P10 | Volume de documentos e inferior a 2 milhoes | Necessario redimensionar infraestrutura |
| P11 | Maioria dos documentos esta em formato texto pesquisavel (nao escaneado) | Aumento significativo em custos de OCR |
| P12 | Product Owner tera disponibilidade para validacoes em ate 3 dias uteis | Atrasos no cronograma |

### 8.4 Premissas de Qualidade de Dados

| ID | Premissa | Impacto se Invalida |
|----|----------|---------------------|
| P13 | Documentos possuem metadados basicos (nome, data) | Dificuldade em filtragem e organizacao |
| P14 | Estrutura de pastas possui alguma logica organizacional | Impacto na precisao de busca |
| P15 | Nomes de arquivos contem informacoes relevantes | Necessario maior dependencia de conteudo |

### 8.5 Premissas de Seguranca

| ID | Premissa | Impacto se Invalida |
|----|----------|---------------------|
| P16 | Dados podem ser armazenados em cloud com criptografia | Necessario redesenhar arquitetura |
| P17 | Nao ha requisito de air-gap ou ambiente desconectado | Aumento significativo de complexidade |
| P18 | APIs de LLM (OpenAI/Azure) sao permitidas pela politica de seguranca | Necessario usar modelos on-premises |

### 8.6 Validacao de Premissas

As premissas serao validadas durante a **Fase 1 (Discovery)**, com os seguintes mecanismos:

| Premissa | Metodo de Validacao | Responsavel |
|----------|---------------------|-------------|
| P01, P04 | Reuniao com TI, analise de politicas | TK TI |
| P02 | Teste de conectividade | TK TI + Dev |
| P03 | Solicitacao formal de ambiente | TK TI |
| P05 | Teste de integracao Graph API | Dev |
| P06 | Analise tecnica do sistema Sense | Dev + TK TI |
| P07 | Reuniao com TI, teste de integracao | TK TI + Dev |
| P09 | Workshop com equipe juridica | TK Juridico |
| P10, P11 | Analise de amostra de documentos | Dev + TK TI |

---

## 9. Riscos

A gestao proativa de riscos e essencial para o sucesso do projeto. Abaixo apresentamos os riscos identificados, sua classificacao e estrategias de mitigacao.

### 9.1 Matriz de Riscos

| ID | Risco | Prob. | Impacto | Score | Categoria |
|----|-------|:-----:|:-------:|:-----:|-----------|
| R01 | Sistema Sense sem API disponivel | Alta | Alto | **9** | Tecnico |
| R02 | ELO sem integracao automatica | Media | Medio | **6** | Tecnico |
| R03 | Performance inadequada em grande volume | Media | Alto | **6** | Tecnico |
| R04 | Qualidade dos embeddings insuficiente | Media | Alto | **6** | Tecnico |
| R05 | Latencia de rede com file servers | Media | Medio | **4** | Infraestrutura |
| R06 | Resistencia dos usuarios a adocao | Baixa | Alto | **4** | Organizacional |
| R07 | Mudancas de escopo frequentes | Media | Medio | **4** | Gestao |
| R08 | Indisponibilidade de stakeholders | Media | Medio | **4** | Gestao |
| R09 | Problemas de permissoes/acesso | Media | Baixo | **3** | Infraestrutura |
| R10 | Dados sensiveis expostos inadvertidamente | Baixa | Alto | **3** | Seguranca |

**Classificacao de Probabilidade:** Baixa (1), Media (2), Alta (3)
**Classificacao de Impacto:** Baixo (1), Medio (2), Alto (3)
**Score:** Probabilidade x Impacto

### 9.2 Detalhamento dos Riscos Criticos

#### R01 - Sistema Sense sem API Disponivel (Score: 9)

**Descricao:** O sistema Sense pode nao possuir API de integracao, dificultando a indexacao de seus documentos.

**Indicadores de Alerta:**
- Documentacao do Sense nao menciona API
- Fornecedor nao responde sobre integracao
- Sistema legado sem manutencao

**Estrategia de Mitigacao:**
1. **Prevencao:** Analise tecnica detalhada na fase Discovery
2. **Alternativas:**
   - Acesso direto ao banco de dados (se permitido)
   - Export periodico de dados para processamento
   - RPA/scraping como ultimo recurso
3. **Contingencia:** Priorizar outras fontes, Sense como fase posterior

**Impacto no Projeto se Materializar:**
- Atraso de 2-4 semanas na fase de conectores
- Possivel aumento de custo para desenvolvimento customizado

#### R03 - Performance Inadequada em Grande Volume (Score: 6)

**Descricao:** O sistema pode nao atingir as metas de performance (< 5s P95) com o volume real de documentos.

**Indicadores de Alerta:**
- Testes de carga em staging mostram degradacao
- Tempo de indexacao acima do esperado
- Queries complexas excedem timeout

**Estrategia de Mitigacao:**
1. **Prevencao:**
   - Arquitetura escalavel desde o inicio
   - PoC com volume representativo na fase Discovery
   - Monitoramento continuo de metricas
2. **Alternativas:**
   - Escalar verticalmente (mais recursos)
   - Otimizar queries e indices
   - Implementar cache agressivo
3. **Contingencia:** Reduzir escopo de indexacao inicial

#### R04 - Qualidade dos Embeddings Insuficiente (Score: 6)

**Descricao:** Os embeddings gerados podem nao capturar adequadamente a semantica dos documentos juridicos, resultando em busca imprecisa.

**Indicadores de Alerta:**
- Recall@10 < 70% nos testes
- Usuarios reportam resultados irrelevantes
- Termos tecnicos nao sao bem compreendidos

**Estrategia de Mitigacao:**
1. **Prevencao:**
   - Usar modelo de embeddings de alta qualidade (text-embedding-3-large)
   - Busca hibrida (vetorial + keyword) desde o inicio
   - Testes extensivos com queries reais
2. **Alternativas:**
   - Ajustar parametros de chunking
   - Implementar fine-tuning de embeddings
   - Aumentar peso do BM25 na fusao
3. **Contingencia:** Fallback para busca puramente keyword

### 9.3 Plano de Contingencia

| Risco | Trigger | Acao de Contingencia | Responsavel |
|-------|---------|---------------------|-------------|
| R01 | Confirmacao de ausencia de API | Implementar RPA; escopo reduzido inicial | Tech Lead |
| R02 | ELO sem API | Exportacao manual como MVP | Tech Lead |
| R03 | P95 > 10s em staging | Escalar infra; otimizar queries | DevOps |
| R04 | Recall < 70% | Ajustar chunking; aumentar BM25 | ML Engineer |
| R06 | Adocao < 50% em 1 mes | Treinamento adicional; ajustes UX | PO |

### 9.4 Monitoramento de Riscos

Os riscos serao monitorados continuamente atraves de:

**Reunioes de Risco:**
- Revisao quinzenal em Sprint Review
- Atualizacao de status e scores
- Identificacao de novos riscos

**Dashboard de Riscos:**
- Status visual de cada risco
- Historico de evolucao
- Alertas automaticos para triggers

**Metricas de Monitoramento:**

| Risco | Metrica | Threshold |
|-------|---------|-----------|
| R03 | Tempo de busca P95 | > 5s = alerta |
| R04 | Recall@10 em testes | < 80% = alerta |
| R06 | % usuarios ativos | < 50% = alerta |

### 9.5 Gestao de Issues

Quando um risco se materializa, o seguinte processo e seguido:

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  DETECTAR   │────►│  AVALIAR    │────►│   DECIDIR   │────►│  EXECUTAR   │
│             │     │             │     │             │     │             │
│ Identificar │     │ Impacto em  │     │ Escolher    │     │ Implementar │
│ o problema  │     │ escopo/prazo│     │ estrategia  │     │ solucao     │
│             │     │ /custo      │     │             │     │             │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
       │                   │                   │                   │
       ▼                   ▼                   ▼                   ▼
   Comunicar          Quantificar         Steering            Monitorar
   ao PO              impacto             Committee           resultado
                                          se necessario
```

---

## Conclusao

O **JurisFinder** representa uma oportunidade de transformacao significativa na gestao de documentos juridicos da ThyssenKrupp. Atraves da aplicacao de tecnologias de ponta em Inteligencia Artificial, o sistema promete:

**Reducao Drastica de Tempo:** De horas para minutos na localizacao de documentos criticos.

**Eliminacao de Riscos:** Zero processos perdidos por falta de documentacao.

**Aumento de Produtividade:** Equipe juridica focada em analise estrategica, nao em tarefas manuais.

**Retorno Mensuravel:** ROI projetado em 6-12 meses, considerando apenas economia operacional.

A proposta apresentada foi elaborada com base em melhores praticas de mercado, tecnologias comprovadas e uma arquitetura pensada para escalabilidade e seguranca. Estamos confiantes de que, com a parceria e comprometimento de ambas as partes, o JurisFinder se tornara uma ferramenta indispensavel para o departamento juridico.

**Proximos Passos:**
1. Resolucao dos gaps identificados (metricas baseline, API ELO, sistema Sense)
2. Aprovacao formal da proposta
3. Kick-off do projeto

---

**Documento elaborado por:** Equipe de Solucoes
**Data:** 21 de Janeiro de 2026
**Versao:** 1.0
**Status:** Aguardando aprovacao

---

*Este documento e confidencial e destinado exclusivamente a ThyssenKrupp Brasil. A reproducao ou distribuicao nao autorizada e proibida.*
