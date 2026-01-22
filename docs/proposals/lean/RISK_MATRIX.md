# Matriz de Riscos - JurisFinder

**Projeto:** JurisFinder - Sistema de Busca Inteligente em Documentos
**Cliente:** ThyssenKrupp
**Versao:** 1.0
**Data:** 22 de Janeiro de 2026
**Autor:** Nicksson Freitas

---

## 1. Metodologia de Avaliacao

### 1.1 Escala de Probabilidade

| Nivel | Descricao | Probabilidade |
|-------|-----------|---------------|
| **1 - Muito Baixa** | Improvavel de ocorrer | < 10% |
| **2 - Baixa** | Pode ocorrer em circunstancias excepcionais | 10-25% |
| **3 - Media** | Pode ocorrer em algum momento | 25-50% |
| **4 - Alta** | Provavelmente ocorrera | 50-75% |
| **5 - Muito Alta** | Quase certo de ocorrer | > 75% |

### 1.2 Escala de Impacto

| Nivel | Descricao | Impacto no Projeto |
|-------|-----------|-------------------|
| **1 - Muito Baixo** | Impacto minimo | Atraso < 1 semana, custo < 5% |
| **2 - Baixo** | Impacto menor | Atraso 1-2 semanas, custo 5-10% |
| **3 - Medio** | Impacto moderado | Atraso 2-4 semanas, custo 10-20% |
| **4 - Alto** | Impacto significativo | Atraso 1-2 meses, custo 20-40% |
| **5 - Muito Alto** | Impacto critico | Atraso > 2 meses, custo > 40%, ou inviabiliza projeto |

### 1.3 Matriz de Severidade

```
                    IMPACTO
              1    2    3    4    5
           ┌────┬────┬────┬────┬────┐
         5 │  5 │ 10 │ 15 │ 20 │ 25 │  MUITO ALTA
           ├────┼────┼────┼────┼────┤
         4 │  4 │  8 │ 12 │ 16 │ 20 │  ALTA
P          ├────┼────┼────┼────┼────┤
R        3 │  3 │  6 │  9 │ 12 │ 15 │  MEDIA
O          ├────┼────┼────┼────┼────┤
B        2 │  2 │  4 │  6 │  8 │ 10 │  BAIXA
           ├────┼────┼────┼────┼────┤
         1 │  1 │  2 │  3 │  4 │  5 │  MUITO BAIXA
           └────┴────┴────┴────┴────┘

Legenda:
  ■ Critico (20-25)  - Acao imediata obrigatoria
  ■ Alto (12-19)     - Plano de mitigacao prioritario
  ■ Medio (6-11)     - Monitoramento ativo
  ■ Baixo (1-5)      - Aceitar ou monitorar
```

---

## 2. Riscos Tecnicos

### 2.1 Integracao com Sistemas Legados

| ID | Risco | Prob | Imp | Score | Categoria |
|----|-------|------|-----|-------|-----------|
| T01 | **Sistema Sense sem API disponivel** - Necessidade de RPA/scraping como alternativa | 4 | 5 | **20** | Critico |
| T02 | **API do ELO inexistente ou limitada** - Impossibilita integracao automatica | 3 | 4 | **12** | Alto |
| T03 | **Conectividade com File Servers** - Latencia/instabilidade de rede corporativa | 3 | 3 | **9** | Medio |
| T04 | **Mudancas no SharePoint** - Alteracoes de estrutura/permissoes pela TI | 2 | 3 | **6** | Medio |
| T05 | **Incompatibilidade de protocolos SMB** - Versoes antigas de file server | 2 | 4 | **8** | Medio |

#### Detalhamento T01 - Sistema Sense sem API

**Descricao Completa:**
O sistema Sense e uma das tres fontes principais de documentos. Sem API disponivel, as alternativas sao:
- RPA/Web Scraping (fragil, quebra com atualizacoes)
- Acesso direto ao banco de dados (requer autorizacao, risco de seguranca)
- Extracao manual periodica (nao automatizado)

**Gatilhos de Ativacao:**
- Confirmacao do fornecedor sobre indisponibilidade de API
- Documentacao tecnica inexistente

**Plano de Mitigacao:**
1. Priorizar analise tecnica do Sense na fase de Discovery (semana 1-2)
2. Negociar com fornecedor desenvolvimento de API
3. Se inviavel, implementar conector RPA com Selenium/Playwright
4. Aumentar contingencia de prazo em 3 semanas

**Plano de Contingencia:**
- Excluir Sense do MVP e tratar como fase posterior
- Processo manual de upload para documentos do Sense

**Responsavel:** Tech Lead + TI ThyssenKrupp
**Prazo de Decisao:** Semana 2 do projeto

---

### 2.2 Pipeline RAG e Machine Learning

| ID | Risco | Prob | Imp | Score | Categoria |
|----|-------|------|-----|-------|-----------|
| T06 | **Qualidade de embeddings insuficiente para portugues juridico** - Recall abaixo de 80% | 3 | 5 | **15** | Alto |
| T07 | **OCR com baixa precisao em documentos escaneados** - Documentos de seguranca inutilizaveis | 4 | 4 | **16** | Alto |
| T08 | **Chunking inadequado fragmenta contexto** - Perda de informacao em documentos longos | 3 | 4 | **12** | Alto |
| T09 | **Latencia de busca acima de SLA** - Tempo > 5s em P95 | 3 | 3 | **9** | Medio |
| T10 | **Reranking nao melhora resultados** - Cross-encoder sem impacto mensuravel | 2 | 2 | **4** | Baixo |

#### Detalhamento T06 - Qualidade de Embeddings

**Descricao Completa:**
Modelos de embedding como `text-embedding-3-large` sao otimizados para ingles. Termos juridicos brasileiros e siglas especificas (PPRA, PCMSO, ASO, CIPA, NR-7) podem ter representacao vetorial inadequada, resultando em baixo recall.

**Gatilhos de Ativacao:**
- Recall@10 < 80% em testes com queries reais
- Usuarios reportam documentos nao encontrados

**Plano de Mitigacao:**
1. Criar dataset de avaliacao com 100+ queries reais antes do desenvolvimento
2. Testar embeddings multilingues (e5-multilingual, BGE-M3)
3. Implementar busca hibrida (vetorial + BM25) desde o inicio
4. Considerar fine-tuning de modelo de embedding

**Plano de Contingencia:**
- Aumentar peso de busca por keyword (BM25) na fusao
- Criar dicionario de sinonimos juridicos
- Expandir queries automaticamente

**Responsavel:** ML Engineer
**Prazo de Decisao:** Semana 4 (apos testes iniciais)

---

#### Detalhamento T07 - OCR com Baixa Precisao

**Descricao Completa:**
Documentos de seguranca do trabalho (fichas de EPI, ASOs, treinamentos) frequentemente sao escaneados com baixa resolucao ou contem carimbos/assinaturas que interferem no OCR.

**Gatilhos de Ativacao:**
- Precisao de OCR < 90% em amostras de documentos
- Campos criticos (datas, nomes) ilegiveids

**Plano de Mitigacao:**
1. Usar Azure Document Intelligence (melhor para formularios)
2. Implementar pre-processamento de imagem (deskew, binarizacao)
3. Criar pipeline de validacao humana para documentos criticos
4. Extrair metadados do nome do arquivo como fallback

**Plano de Contingencia:**
- Indexar apenas metadados de documentos problemáticos
- Manter link para documento original para visualizacao

**Responsavel:** ML Engineer + Backend Dev
**Prazo de Decisao:** Semana 6

---

### 2.3 Infraestrutura e Performance

| ID | Risco | Prob | Imp | Score | Categoria |
|----|-------|------|-----|-------|-----------|
| T11 | **Qdrant nao escala para 1M+ vetores** - Degradacao de performance | 2 | 4 | **8** | Medio |
| T12 | **Indexacao incremental excede janela de 1h** - Backlog de documentos | 3 | 3 | **9** | Medio |
| T13 | **Custos de cloud excedem orcamento** - Infraestrutura subdimensionada | 3 | 3 | **9** | Medio |
| T14 | **Indisponibilidade de servicos OpenAI** - Dependencia de API externa | 2 | 4 | **8** | Medio |
| T15 | **Vazamento de memoria em workers de indexacao** - Crashes em processamento longo | 3 | 2 | **6** | Medio |

---

## 3. Riscos de Projeto

### 3.1 Escopo e Requisitos

| ID | Risco | Prob | Imp | Score | Categoria |
|----|-------|------|-----|-------|-----------|
| P01 | **Escopo crescente (scope creep)** - Features adicionais nao planejadas | 4 | 4 | **16** | Alto |
| P02 | **Requisitos de documentos obrigatorios mal definidos** - Regras de negocio incompletas | 4 | 4 | **16** | Alto |
| P03 | **Metricas baseline inexistentes** - Impossivel medir ROI | 5 | 3 | **15** | Alto |
| P04 | **Mudanca de prioridades do cliente** - Reordenacao de backlog frequente | 3 | 3 | **9** | Medio |
| P05 | **Requisitos de compliance LGPD nao mapeados** - Redesign necessario | 2 | 4 | **8** | Medio |

#### Detalhamento P01 - Escopo Crescente

**Descricao Completa:**
O requisito RF04 "Identificacao Automatica de Documentos Obrigatorios" e vago e pode evoluir para um sistema especialista complexo com:
- Regras por tipo de reclamacao trabalhista
- Validacao de completude de dossiê
- Sugestoes baseadas em historico de processos
- Integracao com fluxos de aprovacao

**Gatilhos de Ativacao:**
- Mais de 3 mudancas de escopo em um sprint
- Novas features solicitadas apos inicio do desenvolvimento

**Plano de Mitigacao:**
1. Definir MVP minimo documentado e assinado
2. Todo novo requisito vai para backlog V2
3. Sprints de 2 semanas com demos frequentes
4. Change request formal para alteracoes significativas

**Plano de Contingencia:**
- Congelar escopo e entregar MVP basico
- Renegociar prazo e custo para features adicionais

**Responsavel:** Product Owner + Tech Lead
**Prazo de Decisao:** Continuo (revisao semanal)

---

#### Detalhamento P03 - Metricas Baseline Inexistentes

**Descricao Completa:**
A proposta promete "reducao de 70-85% no tempo de busca" e "ROI em 6-12 meses", mas nao existem metricas atuais para comparacao:
- Tempo medio para localizar documentos de um processo
- Numero de processos/mes
- Taxa de documentos nao encontrados
- Custo atual do processo manual

**Gatilhos de Ativacao:**
- Cliente nao fornece dados na fase de Discovery
- Metricas sao estimativas sem base em dados reais

**Plano de Mitigacao:**
1. Incluir levantamento de baseline como Gate obrigatorio para GO
2. Criar formulario de tracking para equipe juridica (2 semanas antes do inicio)
3. Analisar logs de sistemas existentes se disponiveis
4. Entrevistar usuarios para estimativas qualitativas

**Plano de Contingencia:**
- Definir metricas de sucesso baseadas em satisfacao do usuario
- Criar baseline a partir da primeira semana de uso do sistema

**Responsavel:** Product Owner (ThyssenKrupp)
**Prazo de Decisao:** Antes do GO do projeto

---

### 3.2 Recursos e Equipe

| ID | Risco | Prob | Imp | Score | Categoria |
|----|-------|------|-----|-------|-----------|
| P06 | **Alocacao parcial insuficiente (ML, DevOps)** - Gargalo em entregas | 4 | 3 | **12** | Alto |
| P07 | **Rotatividade de equipe** - Perda de conhecimento | 2 | 4 | **8** | Medio |
| P08 | **Indisponibilidade do PO cliente** - Decisoes atrasadas | 3 | 3 | **9** | Medio |
| P09 | **Dependencia de especialista unico (ML)** - Single point of failure | 3 | 4 | **12** | Alto |
| P10 | **Equipe de TI ThyssenKrupp sobrecarregada** - Suporte atrasado | 4 | 3 | **12** | Alto |

---

### 3.3 Cronograma

| ID | Risco | Prob | Imp | Score | Categoria |
|----|-------|------|-----|-------|-----------|
| P11 | **Prazo de 5 meses otimista** - Descobertas tecnicas nao planejadas | 4 | 3 | **12** | Alto |
| P12 | **Dependencia de terceiros (fornecedores Sense/ELO)** - Atrasos externos | 3 | 4 | **12** | Alto |
| P13 | **Ambiente de desenvolvimento atrasado** - Bloqueio de inicio | 3 | 3 | **9** | Medio |
| P14 | **Dados de teste/homologacao indisponiveis** - Desenvolvimento as cegas | 3 | 4 | **12** | Alto |

---

## 4. Riscos de Seguranca e Compliance

| ID | Risco | Prob | Imp | Score | Categoria |
|----|-------|------|-----|-------|-----------|
| S01 | **Vazamento de dados sensiveis via LLM** - Dados enviados para OpenAI | 2 | 5 | **10** | Medio |
| S02 | **Controle de acesso incorreto** - Usuario visualiza documento sem permissao | 2 | 5 | **10** | Medio |
| S03 | **Violacao de LGPD** - Dados pessoais sem consentimento/base legal | 2 | 5 | **10** | Medio |
| S04 | **Credenciais expostas em logs** - Vazamento de senhas/tokens | 2 | 4 | **8** | Medio |
| S05 | **Auditoria incompleta** - Impossivel rastrear acessos | 2 | 4 | **8** | Medio |
| S06 | **Dados nao anonimizados em ambiente de dev** - Exposicao de dados reais | 3 | 4 | **12** | Alto |

#### Detalhamento S01 - Vazamento via LLM

**Descricao Completa:**
Ao usar OpenAI API, dados de documentos podem ser enviados para servidores externos. Embora OpenAI tenha politica de nao treinar com dados de API, isso pode violar politicas internas da ThyssenKrupp.

**Plano de Mitigacao:**
1. Usar Azure OpenAI (dados em tenant proprio)
2. Nao enviar conteudo completo de documentos para LLM
3. LLM apenas para parsing de queries, nao para analise de documentos
4. Implementar DLP (Data Loss Prevention) na camada de API

**Responsavel:** Arquiteto + Seguranca TI
**Prazo de Decisao:** Semana 1

---

#### Detalhamento S02 - Controle de Acesso Incorreto

**Descricao Completa:**
O sistema indexa documentos de multiplas fontes com diferentes modelos de permissao:
- File Server: ACLs NTFS
- SharePoint: Permissoes de site/biblioteca
- Sense: Modelo proprio (desconhecido)

Sincronizar permissoes de forma consistente e complexo e pode haver janelas onde documentos ficam visiveis para usuarios nao autorizados.

**Plano de Mitigacao:**
1. Implementar verificacao de permissao em tempo real (nao apenas na indexacao)
2. Cache de permissoes com TTL curto (5 minutos)
3. Logging de todos os acessos para auditoria
4. Revisao de seguranca antes do go-live

**Responsavel:** Backend Dev + Seguranca TI
**Prazo de Decisao:** Antes de go-live

---

## 5. Riscos Financeiros

| ID | Risco | Prob | Imp | Score | Categoria |
|----|-------|------|-----|-------|-----------|
| F01 | **Custo de API OpenAI excede orcamento** - Volume de queries maior que previsto | 3 | 3 | **9** | Medio |
| F02 | **Infraestrutura subdimensionada** - Necessidade de upgrade inesperado | 3 | 3 | **9** | Medio |
| F03 | **Custo de desenvolvimento excede estimativa** - Complexidade subestimada | 3 | 4 | **12** | Alto |
| F04 | **ROI nao comprovavel** - Investimento sem retorno mensuravel | 3 | 4 | **12** | Alto |
| F05 | **Custos ocultos de licenciamento** - Ferramentas/bibliotecas comerciais | 2 | 2 | **4** | Baixo |

#### Detalhamento F01 - Custo de API OpenAI

**Descricao Completa:**
Estimativa atual: $550-1.200/mes. Variaveis que podem aumentar:
- Volume de queries diarias (estimativa: 100, pode ser 500+)
- Reprocessamento de embeddings apos mudancas
- Uso de GPT-4 para parsing de queries complexas
- OCR de alto volume de documentos escaneados

**Cenarios de Custo:**

| Cenario | Queries/dia | Embeddings | OCR | Total/mes |
|---------|-------------|------------|-----|-----------|
| Otimista | 100 | 10k docs | 1k pages | $550 |
| Esperado | 300 | 50k docs | 5k pages | $1.500 |
| Pessimista | 1000 | 100k docs | 20k pages | $4.000 |

**Plano de Mitigacao:**
1. Implementar cache agressivo de queries similares
2. Usar modelo menor (GPT-4o-mini) para tarefas simples
3. Batch processing de embeddings em horarios de baixo custo
4. Monitoramento de custos com alertas

**Responsavel:** DevOps + Finance
**Prazo de Decisao:** Mensal (revisao de custos)

---

## 6. Riscos de Adocao e Operacao

| ID | Risco | Prob | Imp | Score | Categoria |
|----|-------|------|-----|-------|-----------|
| A01 | **Resistencia dos usuarios** - Preferencia por processo manual | 3 | 4 | **12** | Alto |
| A02 | **Confianca abalada por falsos negativos** - Sistema nao encontra documento existente | 3 | 5 | **15** | Alto |
| A03 | **Dupla verificacao anula ROI** - Usuarios buscam manualmente apos sistema | 4 | 4 | **16** | Alto |
| A04 | **Treinamento insuficiente** - Usuarios nao sabem usar recursos avancados | 3 | 3 | **9** | Medio |
| A05 | **Suporte pos-go-live inadequado** - Problemas sem resolucao rapida | 2 | 4 | **8** | Medio |
| A06 | **Expectativas irreais** - Sistema vendido como "magico" | 3 | 4 | **12** | Alto |

#### Detalhamento A02 - Confianca Abalada

**Descricao Completa:**
Se um advogado buscar um documento que ele sabe existir e o sistema nao retornar, a confianca e imediatamente perdida. Em processos trabalhistas, a consequencia de nao encontrar um documento pode ser a perda do processo.

**Gatilhos de Ativacao:**
- Um unico caso de documento existente nao encontrado
- Comparacao manual mostra documentos faltando

**Plano de Mitigacao:**
1. Recall@10 > 95% como criterio de aceite (nao 90%)
2. Feedback loop: usuario pode marcar "documento nao encontrado"
3. Reindexacao automatica quando documento e reportado como faltante
4. Comunicacao clara: "sistema auxilia, nao substitui verificacao"

**Plano de Contingencia:**
- Modo hibrido: resultados do sistema + opcao de busca manual
- Auditoria periodica de recall com amostragem

**Responsavel:** Product Owner + QA
**Prazo de Decisao:** Criterio de aceite do MVP

---

#### Detalhamento A03 - Dupla Verificacao

**Descricao Completa:**
Mesmo com sistema funcionando, advogados podem continuar buscando manualmente "por seguranca", especialmente em processos de alto risco. Isso:
- Anula a reducao de tempo prometida
- Dificulta comprovacao de ROI
- Indica falta de confianca no sistema

**Plano de Mitigacao:**
1. Demonstrar recall atraves de testes comparativos
2. Permitir que usuario veja "cobertura" da busca
3. Gamification: mostrar economia de tempo
4. Quick wins: casos de sucesso divulgados

**Responsavel:** Product Owner + Change Management
**Prazo de Decisao:** Pos-go-live (monitoramento)

---

## 7. Consolidacao e Priorizacao

### 7.1 Top 10 Riscos Criticos

| Rank | ID | Risco | Score | Acao Prioritaria |
|------|-----|-------|-------|------------------|
| 1 | T01 | Sistema Sense sem API | **20** | Validar na Discovery (semana 1-2) |
| 2 | A03 | Dupla verificacao anula ROI | **16** | Plano de change management |
| 3 | P01 | Escopo crescente | **16** | MVP documentado e assinado |
| 4 | P02 | Regras de docs obrigatorios mal definidas | **16** | Workshop com juridico |
| 5 | T07 | OCR com baixa precisao | **16** | Testes com amostras reais |
| 6 | A02 | Confianca abalada por falsos negativos | **15** | Recall > 95% como criterio |
| 7 | T06 | Embeddings insuficientes para PT-BR | **15** | Testes com dataset juridico |
| 8 | P03 | Metricas baseline inexistentes | **15** | Gate obrigatorio para GO |
| 9 | T08 | Chunking fragmenta contexto | **12** | Estrategia de chunking semantico |
| 10 | T02 | API do ELO inexistente | **12** | Validar com fornecedor |

### 7.2 Riscos por Fase do Projeto

```
DISCOVERY (Semanas 1-3)
├── T01 - Sense sem API [CRITICO]
├── T02 - ELO sem API [ALTO]
├── P03 - Baseline inexistente [ALTO]
├── P02 - Regras mal definidas [ALTO]
└── S01 - Vazamento via LLM [MEDIO]

DESENVOLVIMENTO CORE (Semanas 4-12)
├── T06 - Embeddings insuficientes [ALTO]
├── T07 - OCR baixa precisao [ALTO]
├── T08 - Chunking inadequado [ALTO]
├── P06 - Alocacao parcial [ALTO]
└── P01 - Scope creep [ALTO]

INTEGRACAO (Semanas 13-17)
├── T03 - Conectividade File Server [MEDIO]
├── S02 - Controle de acesso [MEDIO]
├── P12 - Dependencia de terceiros [ALTO]
└── S06 - Dados nao anonimizados [ALTO]

GO-LIVE (Semanas 18-20)
├── A02 - Confianca abalada [ALTO]
├── A03 - Dupla verificacao [ALTO]
├── A01 - Resistencia usuarios [ALTO]
└── A06 - Expectativas irreais [ALTO]
```

---

## 8. Plano de Monitoramento

### 8.1 Indicadores de Risco (KRIs)

| KRI | Threshold Amarelo | Threshold Vermelho | Frequencia |
|-----|-------------------|-------------------|------------|
| Recall@10 em testes | < 90% | < 80% | Semanal |
| Latencia P95 | > 5s | > 10s | Diaria |
| Custo OpenAI/mes | > $1.500 | > $3.000 | Semanal |
| Bugs criticos abertos | > 3 | > 5 | Diaria |
| Mudancas de escopo/sprint | > 2 | > 4 | Por sprint |
| Disponibilidade PO | < 20% | < 10% | Semanal |
| Documentos nao indexados | > 5% | > 10% | Diaria |

### 8.2 Reunioes de Revisao de Riscos

| Reuniao | Participantes | Frequencia | Foco |
|---------|---------------|------------|------|
| Daily Standup | Equipe dev | Diaria | Bloqueios imediatos |
| Sprint Review | Equipe + PO | Quinzenal | Riscos de escopo/prazo |
| Risk Review | Tech Lead + Stakeholders | Mensal | Top 10 riscos |
| Steering Committee | Sponsors | Mensal | Riscos criticos, GO/NO-GO |

### 8.3 Escalation Matrix

| Severidade | Tempo de Resposta | Escalacao Para |
|------------|-------------------|----------------|
| Critico (20-25) | 4 horas | Steering Committee |
| Alto (12-19) | 24 horas | Tech Lead + PO |
| Medio (6-11) | 1 semana | Tech Lead |
| Baixo (1-5) | Proximo sprint | Equipe |

---

## 9. Acoes Imediatas Recomendadas

### 9.1 Antes do GO do Projeto

| # | Acao | Responsavel | Prazo | Status |
|---|------|-------------|-------|--------|
| 1 | Validar API/integracao do Sistema Sense | TI ThyssenKrupp | 2 semanas | Pendente |
| 2 | Confirmar API do ELO com fornecedor | TI ThyssenKrupp | 1 semana | Pendente |
| 3 | Levantar metricas baseline (tempo, volume) | Juridico ThyssenKrupp | 2 semanas | Pendente |
| 4 | Workshop de regras de documentos obrigatorios | Juridico + Equipe | 1 semana | Pendente |
| 5 | Definir politica de uso de LLM (OpenAI vs Azure) | Seguranca TI | 1 semana | Pendente |
| 6 | Obter amostras de documentos para testes de OCR | TI ThyssenKrupp | 1 semana | Pendente |
| 7 | Validar ambiente cloud (Azure/AWS) | TI ThyssenKrupp | 1 semana | Pendente |

### 9.2 Primeira Semana do Projeto (se GO)

| # | Acao | Responsavel | Entregavel |
|---|------|-------------|------------|
| 1 | PoC de embeddings com docs juridicos | ML Engineer | Relatorio de Recall |
| 2 | PoC de OCR com documentos escaneados | ML Engineer | Relatorio de precisao |
| 3 | Teste de conectividade com File Server | DevOps | Documentacao de rede |
| 4 | Configuracao de ambiente de dev | DevOps | Ambiente funcional |
| 5 | Documentar decisao sobre Sense | Tech Lead | ADR assinado |

---

## 10. Conclusao

### 10.1 Resumo Executivo de Riscos

- **Total de Riscos Identificados:** 38
- **Riscos Criticos (Score >= 20):** 1
- **Riscos Altos (Score 12-19):** 15
- **Riscos Medios (Score 6-11):** 17
- **Riscos Baixos (Score 1-5):** 5

### 10.2 Recomendacao

O projeto apresenta **riscos significativos mas gerenciaveis**, desde que:

1. **Validacoes criticas sejam feitas ANTES do GO:**
   - Integracao com Sense
   - API do ELO
   - Metricas baseline

2. **Escopo do MVP seja rigidamente controlado:**
   - Excluir "documentos obrigatorios automaticos" do MVP
   - Focar em busca + 2 conectores + exportacao

3. **PoC de 4-6 semanas seja considerado:**
   - Validar qualidade de embeddings
   - Testar OCR com documentos reais
   - Confirmar performance de busca

### 10.3 Condicoes para GO

A recomendacao de **CONDITIONAL** da proposta original permanece valida. Condicoes para mudar para **GO**:

- [ ] Sistema Sense: estrategia de integracao definida
- [ ] Sistema ELO: API confirmada OU fallback aceito
- [ ] Baseline: metricas atuais documentadas
- [ ] Regras de negocio: documentos obrigatorios mapeados
- [ ] Seguranca: politica de LLM aprovada
- [ ] Amostras: documentos para teste de OCR/embeddings disponíveis

---

**Proxima Revisao:** Apos conclusao das acoes da secao 9.1
**Responsavel pelo Documento:** Nicksson Freitas
**Aprovacao Pendente:** ThyssenKrupp + Equipe Tecnica
