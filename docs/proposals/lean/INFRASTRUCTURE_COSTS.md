# Analise de Custos de Infraestrutura - JurisFinder

**Data:** 22 de Janeiro de 2026
**Versao:** 1.0
**Cambio Referencia:** USD 1.00 = R$ 5.50

---

## Sumario Executivo

| Cenario | Custo Total (5 meses) | Custo Medio/mes | Com Contingencia (15%) |
|---------|----------------------|-----------------|------------------------|
| **Minimo** | R$ 21.556 | R$ 4.311 | R$ 24.789 |
| **Medio** | R$ 46.885 | R$ 9.377 | R$ 53.918 |
| **Alto** | R$ 81.535 | R$ 16.307 | R$ 93.765 |

> **Nota:** Os custos incluem licencas do Claude Code para 5 desenvolvedores.
> Os custos variam significativamente entre as fases de desenvolvimento (meses 1-4) e producao (mes 5).

### Distribuicao de Custos (Cenario Medio)

| Categoria | Total 5 meses | % do Total |
|-----------|---------------|------------|
| Infraestrutura Cloud | R$ 33.135 | 71% |
| Claude Code (Licencas) | R$ 13.750 | 29% |

---

## 1. Premissas

### 1.1 Fases do Projeto

| Fase | Meses | Ambiente | Caracteristicas |
|------|-------|----------|-----------------|
| **Discovery/Setup** | 1 | Dev | Ambiente minimo, poucos recursos |
| **Desenvolvimento** | 2-4 | Dev/Staging | Ambiente medio, testes de carga |
| **Go-Live** | 5 | Producao | Ambiente completo, alta disponibilidade |

### 1.2 Volume de Dados Estimado

| Metrica | Desenvolvimento | Producao |
|---------|-----------------|----------|
| Documentos indexados | 10.000 - 50.000 | 500.000 - 1.000.000 |
| Vetores no Qdrant | 30.000 - 150.000 | 1.500.000 - 3.000.000 |
| Usuarios simultaneos | 5 - 10 | 50 - 100 |
| Queries/dia | 100 - 500 | 1.000 - 5.000 |

---

## 2. Detalhamento por Servico

### 2.1 Azure Kubernetes Service (AKS)

| Componente | Minimo | Medio | Alto |
|------------|--------|-------|------|
| **Control Plane** | Free | Standard ($72/mes) | Standard ($72/mes) |
| **Nodes App (Dev)** | 1x D2s_v5 (2vCPU, 8GB) | 2x D2s_v5 | 3x D4s_v5 (4vCPU, 16GB) |
| **Nodes App (Prod)** | 2x D4s_v5 | 3x D4s_v5 | 3x D8s_v5 (8vCPU, 32GB) |
| **Nodes Worker (Celery)** | 1x D2s_v5 | 2x D2s_v5 | 2x D4s_v5 |

**Precos de VMs Azure (Regiao Brazil South):**
- D2s_v5: ~$70/mes (~R$ 385)
- D4s_v5: ~$140/mes (~R$ 770)
- D8s_v5: ~$280/mes (~R$ 1.540)

| Fase | Minimo/mes | Medio/mes | Alto/mes |
|------|------------|-----------|----------|
| **Mes 1 (Dev)** | R$ 770 | R$ 1.540 | R$ 2.700 |
| **Mes 2-4 (Dev/Staging)** | R$ 1.155 | R$ 2.310 | R$ 3.850 |
| **Mes 5 (Producao)** | R$ 2.310 | R$ 3.850 | R$ 5.390 |

**Economia potencial:** Spot VMs para workers (ate 90% desconto), Reserved Instances 1 ano (ate 40% desconto).

---

### 2.2 PostgreSQL Flexible Server

| Tier | Config | Uso |
|------|--------|-----|
| **Burstable B2s** | 2 vCPU, 4GB | Dev/Testes |
| **General Purpose D4s** | 4 vCPU, 16GB | Staging |
| **General Purpose D8s** | 8 vCPU, 32GB | Producao |

**Precos (Brazil South):**
- Burstable B2s: ~$50/mes (~R$ 275)
- GP D4s: ~$250/mes (~R$ 1.375)
- GP D8s: ~$500/mes (~R$ 2.750)
- Storage: R$ 0.66/GB/mes
- Backup (100GB incluso, adicional ~R$ 0.55/GB/mes)

| Fase | Minimo/mes | Medio/mes | Alto/mes |
|------|------------|-----------|----------|
| **Mes 1 (Dev)** | R$ 330 | R$ 440 | R$ 550 |
| **Mes 2-4 (Staging)** | R$ 440 | R$ 825 | R$ 1.375 |
| **Mes 5 (Producao)** | R$ 1.375 | R$ 1.925 | R$ 3.025 |

> **Nota:** HA (High Availability) dobra o custo de compute.

---

### 2.3 Qdrant Cloud (Vector Database)

**Modelo de Pricing:** CPU + RAM + Disco

| Config | Capacidade | Uso |
|--------|------------|-----|
| **Starter** | 1GB RAM, 20GB disk | Dev (~100k vetores) |
| **Standard** | 4GB RAM, 100GB disk | Staging (~500k vetores) |
| **Production** | 16GB RAM, 500GB disk, 3 replicas | Prod (~2M vetores) |

**Precos estimados:**
- Starter: ~$50/mes (~R$ 275)
- Standard: ~$150/mes (~R$ 825)
- Production (3 replicas): ~$400-600/mes (~R$ 2.200 - 3.300)

| Fase | Minimo/mes | Medio/mes | Alto/mes |
|------|------------|-----------|----------|
| **Mes 1 (Dev)** | R$ 275 | R$ 385 | R$ 550 |
| **Mes 2-4 (Staging)** | R$ 550 | R$ 825 | R$ 1.100 |
| **Mes 5 (Producao)** | R$ 1.650 | R$ 2.200 | R$ 3.300 |

**Alternativa:** Qdrant self-hosted no AKS (incluso no custo de nodes).

---

### 2.4 Azure Cache for Redis

| Tier | Config | Uso |
|------|--------|-----|
| **Basic C0** | 250MB | Dev |
| **Standard C1** | 1GB, replicado | Staging |
| **Standard C2** | 2.5GB, replicado | Producao |

**Precos (Brazil South):**
- Basic C0: ~$16/mes (~R$ 88)
- Standard C1: ~$50/mes (~R$ 275)
- Standard C2: ~$100/mes (~R$ 550)

| Fase | Minimo/mes | Medio/mes | Alto/mes |
|------|------------|-----------|----------|
| **Mes 1 (Dev)** | R$ 88 | R$ 165 | R$ 275 |
| **Mes 2-4 (Staging)** | R$ 165 | R$ 275 | R$ 385 |
| **Mes 5 (Producao)** | R$ 385 | R$ 550 | R$ 825 |

---

### 2.5 OpenAI / Azure OpenAI (AI Services)

#### Embeddings (text-embedding-3-large)

| Volume | Tokens Estimados | Custo |
|--------|------------------|-------|
| 10k docs (dev) | ~50M tokens | ~$6.50 (~R$ 36) |
| 100k docs (staging) | ~500M tokens | ~$65 (~R$ 360) |
| 1M docs (prod) | ~5B tokens | ~$650 (~R$ 3.575) |
| Queries diarias (1000) | ~5M tokens/mes | ~$0.65/mes (~R$ 4) |

**Preco:** $0.13/1M tokens (Standard) ou $0.065/1M (Batch)

#### LLM para Query Parsing (GPT-4o-mini)

| Volume | Custo Estimado |
|--------|----------------|
| 100 queries/dia | ~$5/mes (~R$ 28) |
| 1000 queries/dia | ~$50/mes (~R$ 275) |
| 5000 queries/dia | ~$250/mes (~R$ 1.375) |

**Preco:** $0.15/1M input + $0.60/1M output

#### Reranking (Cohere Rerank v3)

| Volume | Custo Estimado |
|--------|----------------|
| 1000 queries/dia | ~$50/mes (~R$ 275) |
| 5000 queries/dia | ~$200/mes (~R$ 1.100) |

#### OCR (Azure Document Intelligence)

| Volume | Custo |
|--------|-------|
| 10k paginas | ~$15 (~R$ 83) |
| 100k paginas | ~$150 (~R$ 825) |
| 500k paginas | ~$750 (~R$ 4.125) |

**Preco:** ~$0.0015/pagina (Read model)

#### Resumo AI Services

| Fase | Minimo/mes | Medio/mes | Alto/mes |
|------|------------|-----------|----------|
| **Mes 1 (Setup)** | R$ 110 | R$ 275 | R$ 550 |
| **Mes 2-4 (Indexacao)** | R$ 550 | R$ 1.100 | R$ 2.200 |
| **Mes 5 (Producao)** | R$ 825 | R$ 1.650 | R$ 3.300 |

> **Importante:** O custo de indexacao inicial (Mes 2-3) pode ser significativo dependendo do volume de documentos. Considere usar Batch API (50% desconto).

---

### 2.6 Azure Blob Storage

| Volume | Custo (Hot Tier) |
|--------|------------------|
| 100GB | ~$2/mes (~R$ 11) |
| 500GB | ~$10/mes (~R$ 55) |
| 1TB | ~$20/mes (~R$ 110) |
| 5TB | ~$100/mes (~R$ 550) |

**Precos:**
- Hot tier: ~$0.02/GB/mes
- Cool tier: ~$0.01/GB/mes
- Archive: ~$0.002/GB/mes

| Fase | Minimo/mes | Medio/mes | Alto/mes |
|------|------------|-----------|----------|
| **Mes 1-4** | R$ 22 | R$ 55 | R$ 110 |
| **Mes 5** | R$ 55 | R$ 110 | R$ 275 |

---

### 2.7 Networking

| Componente | Custo Estimado |
|------------|----------------|
| VPN Gateway (Basic) | ~$27/mes (~R$ 150) |
| VPN Gateway (VpnGw1) | ~$140/mes (~R$ 770) |
| ExpressRoute (50 Mbps) | ~$55/mes (~R$ 303) |
| Bandwidth (saida, 100GB) | ~$8.70 (~R$ 48) |
| Load Balancer | ~$18/mes (~R$ 99) |

| Fase | Minimo/mes | Medio/mes | Alto/mes |
|------|------------|-----------|----------|
| **Mes 1-4 (VPN Basic)** | R$ 200 | R$ 350 | R$ 500 |
| **Mes 5 (VPN + LB)** | R$ 450 | R$ 770 | R$ 1.100 |

---

### 2.8 Monitoring & Observability

| Servico | Config | Custo |
|---------|--------|-------|
| **Azure Monitor** | Basic logs | ~$50/mes (~R$ 275) |
| **Log Analytics** | 5GB/dia | ~$100/mes (~R$ 550) |
| **Application Insights** | Included | - |
| **Grafana Cloud** | Free tier | R$ 0 |
| **Datadog (alternativa)** | Pro | ~$150/mes (~R$ 825) |

| Fase | Minimo/mes | Medio/mes | Alto/mes |
|------|------------|-----------|----------|
| **Mes 1-4** | R$ 110 | R$ 275 | R$ 550 |
| **Mes 5** | R$ 275 | R$ 550 | R$ 825 |

---

### 2.9 Claude Code (Licencas de Desenvolvimento)

Licencas da Anthropic para a equipe de desenvolvimento usar Claude Code como assistente de codificacao.

#### Planos Disponiveis

| Plano | Preco/usuario | Caracteristicas |
|-------|---------------|-----------------|
| **Claude Pro** | $20/mes (~R$ 110) | 5x uso do free, ideal para uso moderado |
| **Claude Max 5x** | $100/mes (~R$ 550) | 5x do Pro, para desenvolvedores ativos |
| **Claude Max 20x** | $200/mes (~R$ 1.100) | 20x do Pro, uso intensivo o dia todo |
| **Team Standard** | $30/mes (~R$ 165) | Colaboracao em equipe, admin controls |
| **Team Premium** | $150/mes (~R$ 825) | Inclui Claude Code environment completo |

#### API Anthropic (Alternativa ou Complemento)

Se preferir usar Claude via API para automacoes ou integracao:

| Modelo | Input/1M tokens | Output/1M tokens | Uso Recomendado |
|--------|-----------------|------------------|-----------------|
| **Haiku 4.5** | $1.00 (~R$ 5.50) | $5.00 (~R$ 27.50) | Tarefas rapidas, alto volume |
| **Sonnet 4.5** | $3.00 (~R$ 16.50) | $15.00 (~R$ 82.50) | Balanco custo/qualidade |
| **Opus 4.5** | $5.00 (~R$ 27.50) | $25.00 (~R$ 137.50) | Tarefas complexas |

> **Batch API:** 50% de desconto para processamento assincrono (24h).
> **Prompt Caching:** Cache read = 10% do preco de input.

#### Composicao da Equipe (conforme proposta)

| Papel | Qtd | Alocacao | Uso Claude Code |
|-------|-----|----------|-----------------|
| Tech Lead / Arquiteto | 1 | 100% | Intensivo |
| Backend Developer Senior | 2 | 100% | Intensivo |
| Frontend Developer | 1 | 100% | Intensivo |
| ML Engineer | 1 | 50% | Moderado |
| DevOps/SRE | 1 | 50% | Moderado |
| QA Engineer | 1 | 50% (fases finais) | Moderado |

**Total de usuarios ativos:** 5-6 desenvolvedores

#### Cenarios de Custo

| Cenario | Plano | Usuarios | Custo/mes |
|---------|-------|----------|-----------|
| **Minimo** | Claude Pro | 5 | $100 (~R$ 550) |
| **Medio** | Claude Max 5x | 5 | $500 (~R$ 2.750) |
| **Alto** | Claude Max 20x | 5 | $1.000 (~R$ 5.500) |

**Alternativa Team (recomendado para empresas):**

| Cenario | Plano | Usuarios | Custo/mes |
|---------|-------|----------|-----------|
| **Minimo** | Team Standard | 5 | $150 (~R$ 825) |
| **Medio** | Team Standard | 6 | $180 (~R$ 990) |
| **Alto** | Team Premium | 5 | $750 (~R$ 4.125) |

#### Estimativa de Uso via API (se aplicavel)

Para automacoes de code review, documentacao, ou agentes internos:

| Uso | Volume Estimado | Custo Mensal |
|-----|-----------------|--------------|
| Code review automatico | ~2M tokens/mes | ~R$ 165 (Sonnet) |
| Geracao de docs | ~1M tokens/mes | ~R$ 82 (Sonnet) |
| Agentes de suporte | ~5M tokens/mes | ~R$ 55 (Haiku) |

#### Resumo Claude Code

| Fase | Minimo/mes | Medio/mes | Alto/mes |
|------|------------|-----------|----------|
| **Mes 1-5** | R$ 550 | R$ 2.750 | R$ 5.500 |

> **Nota:** O custo de Claude Code e constante durante todo o projeto, pois a equipe esta ativa desde o inicio.

---

## 3. Consolidacao por Mes

### 3.1 Cenario MINIMO

| Servico | Mes 1 | Mes 2 | Mes 3 | Mes 4 | Mes 5 | Total |
|---------|-------|-------|-------|-------|-------|-------|
| AKS | R$ 770 | R$ 1.155 | R$ 1.155 | R$ 1.155 | R$ 2.310 | R$ 6.545 |
| PostgreSQL | R$ 330 | R$ 440 | R$ 440 | R$ 440 | R$ 1.375 | R$ 3.025 |
| Qdrant | R$ 275 | R$ 550 | R$ 550 | R$ 550 | R$ 1.650 | R$ 3.575 |
| Redis | R$ 88 | R$ 165 | R$ 165 | R$ 165 | R$ 385 | R$ 968 |
| AI Services | R$ 110 | R$ 550 | R$ 550 | R$ 550 | R$ 825 | R$ 2.585 |
| Storage | R$ 22 | R$ 22 | R$ 22 | R$ 22 | R$ 55 | R$ 143 |
| Network | R$ 200 | R$ 200 | R$ 200 | R$ 200 | R$ 450 | R$ 1.250 |
| Monitoring | R$ 110 | R$ 110 | R$ 110 | R$ 110 | R$ 275 | R$ 715 |
| **Claude Code** | R$ 550 | R$ 550 | R$ 550 | R$ 550 | R$ 550 | R$ 2.750 |
| **TOTAL** | **R$ 2.455** | **R$ 3.742** | **R$ 3.742** | **R$ 3.742** | **R$ 7.875** | **R$ 21.556** |

> **Reserva/Contingencia (15%):** R$ 3.233
> **TOTAL COM CONTINGENCIA:** R$ 24.789

---

### 3.2 Cenario MEDIO (Recomendado)

| Servico | Mes 1 | Mes 2 | Mes 3 | Mes 4 | Mes 5 | Total |
|---------|-------|-------|-------|-------|-------|-------|
| AKS | R$ 1.540 | R$ 2.310 | R$ 2.310 | R$ 2.310 | R$ 3.850 | R$ 12.320 |
| PostgreSQL | R$ 440 | R$ 825 | R$ 825 | R$ 825 | R$ 1.925 | R$ 4.840 |
| Qdrant | R$ 385 | R$ 825 | R$ 825 | R$ 825 | R$ 2.200 | R$ 5.060 |
| Redis | R$ 165 | R$ 275 | R$ 275 | R$ 275 | R$ 550 | R$ 1.540 |
| AI Services | R$ 275 | R$ 1.100 | R$ 1.100 | R$ 1.100 | R$ 1.650 | R$ 5.225 |
| Storage | R$ 55 | R$ 55 | R$ 55 | R$ 55 | R$ 110 | R$ 330 |
| Network | R$ 350 | R$ 350 | R$ 350 | R$ 350 | R$ 770 | R$ 2.170 |
| Monitoring | R$ 275 | R$ 275 | R$ 275 | R$ 275 | R$ 550 | R$ 1.650 |
| **Claude Code** | R$ 2.750 | R$ 2.750 | R$ 2.750 | R$ 2.750 | R$ 2.750 | R$ 13.750 |
| **TOTAL** | **R$ 6.235** | **R$ 8.765** | **R$ 8.765** | **R$ 8.765** | **R$ 14.355** | **R$ 46.885** |

> **Reserva/Contingencia (15%):** R$ 7.033
> **TOTAL COM CONTINGENCIA:** R$ 53.918

---

### 3.3 Cenario ALTO

| Servico | Mes 1 | Mes 2 | Mes 3 | Mes 4 | Mes 5 | Total |
|---------|-------|-------|-------|-------|-------|-------|
| AKS | R$ 2.700 | R$ 3.850 | R$ 3.850 | R$ 3.850 | R$ 5.390 | R$ 19.640 |
| PostgreSQL | R$ 550 | R$ 1.375 | R$ 1.375 | R$ 1.375 | R$ 3.025 | R$ 7.700 |
| Qdrant | R$ 550 | R$ 1.100 | R$ 1.100 | R$ 1.100 | R$ 3.300 | R$ 7.150 |
| Redis | R$ 275 | R$ 385 | R$ 385 | R$ 385 | R$ 825 | R$ 2.255 |
| AI Services | R$ 550 | R$ 2.200 | R$ 2.200 | R$ 2.200 | R$ 3.300 | R$ 10.450 |
| Storage | R$ 110 | R$ 110 | R$ 110 | R$ 110 | R$ 275 | R$ 715 |
| Network | R$ 500 | R$ 500 | R$ 500 | R$ 500 | R$ 1.100 | R$ 3.100 |
| Monitoring | R$ 550 | R$ 550 | R$ 550 | R$ 550 | R$ 825 | R$ 3.025 |
| **Claude Code** | R$ 5.500 | R$ 5.500 | R$ 5.500 | R$ 5.500 | R$ 5.500 | R$ 27.500 |
| **TOTAL** | **R$ 11.285** | **R$ 15.570** | **R$ 15.570** | **R$ 15.570** | **R$ 23.540** | **R$ 81.535** |

> **Reserva/Contingencia (15%):** R$ 12.230
> **TOTAL COM CONTINGENCIA:** R$ 93.765

---

## 4. Grafico de Evolucao de Custos (com Claude Code)

```
Custo Mensal (R$)
    │
24k │                                              ┌─────┐
    │                                              │█████│ Alto
22k │                                              │█████│
    │                                              │█████│
20k │                                              │█████│
    │                                              │█████│
18k │                                              │█████│
    │         ┌─────┬─────┬─────┐                  │█████│
16k │         │█████│█████│█████│                  │▓▓▓▓▓│
    │         │█████│█████│█████│                  │▓▓▓▓▓│
14k │         │█████│█████│█████│                  │▓▓▓▓▓│
    │  ┌─────┐│█████│█████│█████│                  │▓▓▓▓▓│
12k │  │█████││▓▓▓▓▓│▓▓▓▓▓│▓▓▓▓▓│                  │▓▓▓▓▓│
    │  │█████││▓▓▓▓▓│▓▓▓▓▓│▓▓▓▓▓│                  │▓▓▓▓▓│
10k │  │█████││▓▓▓▓▓│▓▓▓▓▓│▓▓▓▓▓│                  │▓▓▓▓▓│
    │  │▓▓▓▓▓││▓▓▓▓▓│▓▓▓▓▓│▓▓▓▓▓│                  │░░░░░│
 8k │  │▓▓▓▓▓││▓▓▓▓▓│▓▓▓▓▓│▓▓▓▓▓│                  │░░░░░│
    │  │▓▓▓▓▓││░░░░░│░░░░░│░░░░░│                  │░░░░░│
 6k │  │▓▓▓▓▓││░░░░░│░░░░░│░░░░░│                  │░░░░░│
    │  │░░░░░││░░░░░│░░░░░│░░░░░│                  │░░░░░│
 4k │  │░░░░░││░░░░░│░░░░░│░░░░░│                  │░░░░░│
    │  │░░░░░││░░░░░│░░░░░│░░░░░│                  │░░░░░│
 2k │  │░░░░░││░░░░░│░░░░░│░░░░░│                  │░░░░░│
    │  │░░░░░││░░░░░│░░░░░│░░░░░│                  │░░░░░│
 0k └──┴─────┴┴─────┴─────┴─────┴──────────────────┴─────┴──
         Mes 1   Mes 2   Mes 3   Mes 4            Mes 5

    ░░░░░ Minimo    ▓▓▓▓▓ Medio    █████ Alto

Valores por Mes:
┌─────────┬─────────┬─────────┬─────────┬─────────┬──────────┐
│ Cenario │  Mes 1  │  Mes 2  │  Mes 3  │  Mes 4  │  Mes 5   │
├─────────┼─────────┼─────────┼─────────┼─────────┼──────────┤
│ Minimo  │ R$ 2.5k │ R$ 3.7k │ R$ 3.7k │ R$ 3.7k │ R$ 7.9k  │
│ Medio   │ R$ 6.2k │ R$ 8.8k │ R$ 8.8k │ R$ 8.8k │ R$ 14.4k │
│ Alto    │ R$ 11.3k│ R$ 15.6k│ R$ 15.6k│ R$ 15.6k│ R$ 23.5k │
└─────────┴─────────┴─────────┴─────────┴─────────┴──────────┘
```

---

## 5. Comparativo com Proposta Original

| Item | Proposta Original | Cenario Medio Real | Diferenca |
|------|-------------------|-------------------|-----------|
| **Infra/mes (producao)** | R$ 8.000 - R$ 15.000 | R$ 11.605 | Dentro da faixa |
| **5 meses total (infra)** | R$ 40.000 - R$ 75.000 | R$ 33.135 | -17% a -56% |
| **Claude Code (5 meses)** | Nao previsto | R$ 13.750 | +R$ 13.750 |
| **TOTAL GERAL** | R$ 40.000 - R$ 75.000 | R$ 46.885 | Dentro da faixa |

**Conclusao:** O cenario MEDIO com Claude Code ainda esta dentro da faixa da proposta original.

### Impacto do Claude Code no Orcamento

| Cenario | Sem Claude Code | Com Claude Code | Aumento |
|---------|-----------------|-----------------|---------|
| Minimo | R$ 18.806 | R$ 21.556 | +15% |
| Medio | R$ 33.135 | R$ 46.885 | +41% |
| Alto | R$ 54.035 | R$ 81.535 | +51% |

> **Justificativa do investimento:** O Claude Code pode aumentar a produtividade da equipe em 30-50%, potencialmente reduzindo o tempo de desenvolvimento e o custo total de pessoal (que representa R$ 702.500 na proposta).

---

## 6. Oportunidades de Economia

### 6.1 Reserved Instances (1 ano)

| Servico | Economia | Impacto Anual |
|---------|----------|---------------|
| AKS VMs | 40% | ~R$ 24.000 |
| PostgreSQL | 40% | ~R$ 9.000 |
| Redis | 40% | ~R$ 3.000 |
| **Total** | | **~R$ 36.000** |

### 6.2 Spot VMs para Workers

| Uso | Economia |
|-----|----------|
| Celery workers (indexacao) | Ate 90% |
| Impacto mensal | ~R$ 500-1.500 |

### 6.3 Batch API para Embeddings

| Uso | Economia |
|-----|----------|
| OpenAI Batch (50% off) | R$ 500-1.000 durante indexacao |

### 6.4 Qdrant Self-Hosted

| Opcao | Impacto |
|-------|---------|
| Rodar no AKS | -R$ 1.650 a -R$ 3.300/mes |
| Trade-off | Maior complexidade operacional |

---

## 7. Recomendacoes

### Para o Mes 1 (Discovery)
- Usar **Free tier** do AKS
- PostgreSQL **Burstable** (Stop/Start quando nao usar)
- Qdrant **Free tier** ou local
- Foco em validar arquitetura com custo minimo

### Para Meses 2-4 (Desenvolvimento)
- Escalar conforme necessidade
- Usar **Spot VMs** para Celery workers
- **Batch API** para indexacao de documentos
- Monitorar custos semanalmente

### Para Mes 5 (Producao)
- Avaliar **Reserved Instances** se projeto continuar
- Implementar **auto-scaling** para otimizar custos
- Considerar **Qdrant self-hosted** se time tiver capacidade

### Para Claude Code (todos os meses)

| Opcao | Quando Usar | Custo/mes (5 devs) |
|-------|-------------|-------------------|
| **Claude Pro** | Equipe com uso ocasional | R$ 550 |
| **Team Standard** | Equipe colaborativa, admin controls | R$ 825 |
| **Claude Max 5x** | Desenvolvedores ativos full-time | R$ 2.750 |
| **Claude Max 20x** | Projetos criticos, prazo apertado | R$ 5.500 |

**Recomendacao:** Comecar com **Claude Max 5x** (cenario medio) e ajustar conforme uso real.

**Dicas de Economia:**
- Usar **Batch API** (50% off) para tarefas assincronas de code review/docs
- Aproveitar **Prompt Caching** (90% off em cache reads) para prompts repetitivos
- Monitorar uso semanal e ajustar planos se necessario
- Considerar **Haiku** para tarefas simples via API ($1/MTok vs $3-5)

---

## 8. Fontes e Referencias

### Infraestrutura Cloud
- [Azure AKS Pricing](https://azure.microsoft.com/en-us/pricing/details/kubernetes-service/)
- [Azure PostgreSQL Flexible Server Pricing](https://azure.microsoft.com/en-us/pricing/details/postgresql/flexible-server/)
- [Qdrant Cloud Pricing](https://qdrant.tech/pricing/)
- [OpenAI API Pricing](https://platform.openai.com/docs/pricing)
- [Azure Cache for Redis Pricing](https://azure.microsoft.com/en-us/pricing/details/cache/)
- [Azure Blob Storage Pricing](https://azure.microsoft.com/en-us/pricing/details/storage/blobs/)

### Claude Code / Anthropic
- [Claude Pricing (Subscriptions)](https://claude.com/pricing)
- [Anthropic API Pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- [Claude Code Pricing Guide](https://launchkit.tech/blog/claude-code-pricing-guide)
- [Claude API Cost Calculator](https://costgoat.com/pricing/claude-api)

---

**Documento gerado em:** 22 de Janeiro de 2026
**Proxima revisao:** Apos validacao dos requisitos de volume
