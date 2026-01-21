# Diagramas de Arquitetura C4 - JurisFinder

Este diretório contém os diagramas de arquitetura do sistema JurisFinder seguindo o modelo C4 (Context, Container, Component, Code) em formato PlantUML.

## Índice de Diagramas

### Nível 1 - Contexto do Sistema
- **[c4-context.puml](./c4-context.puml)** - Visão geral do sistema, atores e sistemas externos
  - Mostra usuários (Advogado Interno, Analista Jurídico)
  - Sistemas externos (Active Directory, ELO, File Server, SharePoint, Sense)
  - Interações de alto nível

### Nível 2 - Containers
- **[c4-container.puml](./c4-container.puml)** - Arquitetura de containers/serviços
  - Frontend SPA (React + TypeScript)
  - API Gateway (Kong)
  - Search Service (Python/FastAPI)
  - Indexer Service (Python/Celery)
  - Export Service (Python/FastAPI)
  - Bancos de dados (PostgreSQL, Qdrant, Redis)
  - Conectores de fontes de dados

### Nível 3 - Componentes
- **[c4-component-search.puml](./c4-component-search.puml)** - Componentes internos do Search Service
  - Query Parser
  - Query Embedder
  - Hybrid Search Engine
  - Access Control Filter
  - Reranker
  - Result Formatter
  - Cache Controller

### Diagramas Dinâmicos
- **[c4-dynamic-search.puml](./c4-dynamic-search.puml)** - Fluxo de uma busca completa
  - Desde a digitação da query pelo usuário
  - Processamento e parsing
  - Busca híbrida (vetorial + keyword)
  - Filtragem de acesso
  - Reranking
  - Retorno dos resultados

- **[c4-dynamic-indexing.puml](./c4-dynamic-indexing.puml)** - Fluxo de indexação de documentos
  - Agendamento de tarefas (Celery Beat)
  - Crawling de fontes
  - Parsing e extração de texto
  - Chunking de documentos
  - Geração de embeddings
  - Armazenamento em Qdrant e PostgreSQL

### Deployment
- **[c4-deployment.puml](./c4-deployment.puml)** - Infraestrutura e deployment
  - Azure Kubernetes Service (AKS)
  - PostgreSQL Managed
  - Redis Managed
  - Qdrant Cloud
  - Conectividade híbrida (VPN/ExpressRoute)
  - Rede corporativa ThyssenKrupp

## Como Visualizar os Diagramas

### Opção 1: VS Code com Extensão PlantUML
1. Instale a extensão [PlantUML](https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml)
2. Instale o Java Runtime (necessário para o PlantUML)
3. Abra qualquer arquivo `.puml`
4. Pressione `Alt+D` para preview

### Opção 2: PlantUML Online
1. Acesse [PlantUML Online Editor](https://www.plantuml.com/plantuml/uml/)
2. Copie o conteúdo de qualquer arquivo `.puml`
3. Cole no editor
4. Visualize o diagrama renderizado

### Opção 3: CLI (Linha de Comando)
```bash
# Instalar PlantUML
sudo apt install plantuml  # Linux
brew install plantuml      # macOS

# Gerar PNG
plantuml c4-context.puml

# Gerar SVG (melhor para documentação)
plantuml -tsvg c4-context.puml

# Gerar todos os diagramas
plantuml -tsvg *.puml
```

### Opção 4: Docker
```bash
# Renderizar todos os diagramas usando Docker
docker run -v $(pwd):/data plantuml/plantuml -tsvg "/data/*.puml"
```

## Convenções Utilizadas

### Cores do Modelo C4
- **Azul**: Sistemas internos (JurisFinder)
- **Cinza**: Sistemas externos
- **Verde**: Pessoas/Atores
- **Laranja**: Bancos de dados

### Tecnologias Específicas
Todos os containers incluem a tecnologia específica utilizada:
- React 18 + TypeScript (Frontend)
- Python 3.12 + FastAPI (Backend Services)
- PostgreSQL 16 (Metadata)
- Qdrant (Vector Database)
- Redis 7 (Cache/Queue)
- Kong OSS (API Gateway)

### Protocolos e Portas
Todas as relações especificam o protocolo e, quando relevante, a porta:
- HTTPS (TLS 1.3)
- LDAP/LDAPS (389/636)
- SMB/CIFS (445)
- PostgreSQL (5432)
- Redis (6379)
- gRPC (Qdrant)

## Integração com Documentação

Estes diagramas complementam a documentação existente:
- **[SOLUTIONS_PROPOSAL.md](../SOLUTIONS_PROPOSAL.md)** - Proposta de solução completa
- **[ARCHITECTURE.md](../architecture/ARCHITECTURE.md)** - Documentação de arquitetura (se existir)

## Manutenção

Ao modificar a arquitetura do sistema:
1. Atualize os diagramas correspondentes
2. Mantenha IDs de elementos consistentes entre diferentes níveis
3. Atualize este README se novos diagramas forem adicionados
4. Re-gere os arquivos PNG/SVG para visualização rápida

## Referências

- [Modelo C4](https://c4model.com/) - Documentação oficial do C4 Model
- [C4-PlantUML](https://github.com/plantuml-stdlib/C4-PlantUML) - Biblioteca PlantUML para C4
- [PlantUML](https://plantuml.com/) - Documentação do PlantUML

---

**Última atualização:** 21 de Janeiro de 2026
**Versão:** 1.0
**Autor:** C4 Diagram Architect
