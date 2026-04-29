# 📊 Customer Churn Analysis

Análise exploratória de churn bancário com foco em identificar **quem cancela, por quê e quais ações tomar** — usando técnicas de EDA, WoE/IV e segmentação de risco.

---

## 🗂️ Arquivos do Projeto

| Arquivo | Descrição |
|---|---|
| `customer_churn_records.xlsx` | Base de dados com 10.000 registros de clientes de um banco fictício, contendo variáveis demográficas, financeiras e comportamentais |
| `customer_churn_records_analysis.ipynb` | Notebook principal com toda a análise: pré-processamento, EDA, cálculo de IV/WoE, correlações, visualizações e segmentação estratégica |

### Dicionário de Variáveis (`customer_churn_records.xlsx`)

| Coluna | Descrição |
|---|---|
| `CreditScore` | Pontuação de crédito do cliente |
| `Geography` | País de residência (France, Spain, Germany) |
| `Gender` | Gênero |
| `Age` | Idade |
| `Tenure` | Anos como cliente |
| `Balance` | Saldo médio na conta |
| `NumOfProducts` | Nº de produtos contratados |
| `HasCrCard` | Possui cartão de crédito (1=Sim) |
| `IsActiveMember` | Acessou a conta nos últimos 30 dias (1=Sim) |
| `EstimatedSalary` | Salário estimado |
| `Exited` | **Variável alvo** — cancelou a conta (1=Sim, 0=Não) |
| `Complain` | Registrou reclamação (1=Sim) |
| `SatisfactionScore` | Nível de satisfação declarado (1 a 5) |
| `CardType` | Tipo de cartão (DIAMOND, GOLD, SILVER, PLATINUM) |
| `PointEarned` | Pontos acumulados no programa de fidelidade |

---

## 📋 Estrutura da Análise

O notebook está organizado em 9 seções:

1. **Setup e Importações**
2. **Carregamento e Inspeção dos Dados**
3. **Limpeza e Pré-processamento**
4. **Bins — Faixas das Variáveis Numéricas**
5. **Weight of Evidence (WoE) e Information Value (IV)**
6. **Análise de Correlação**
7. **Resumo Estatístico por Grupo**
8. **Distribuição de Churn por Variável**
9. **Análise de Outliers (IQR)**
10. **Análises**

---

## 🔍 Principais Resultados

### Taxa de Churn Global
> **20,4%** dos clientes cancelaram — acima do típico setor bancário, o que justifica ação.

---

### 1. Quem cancela mais?

**Perfil demográfico:**
- **Mulheres** apresentam taxa de churn superior à dos homens
- Clientes entre **40 e 60 anos** concentram os maiores índices de cancelamento
- Clientes da **Alemanha** lideram o churn por país, com taxa significativamente acima de França e Espanha

**Hábitos financeiros:**
- Churners têm **saldo médio mais alto** que os clientes que permanecem — um sinal de alerta: o banco perde justamente quem tem mais dinheiro
- O **tipo de cartão não discrimina** o comportamento de churn (taxas similares entre DIAMOND, GOLD, SILVER e PLATINUM)
- Clientes com **3 ou 4 produtos** têm taxa de cancelamento acima de 80%, contra ~7% nos que têm 2

**Engajamento:**
- **Membros inativos** (`IsActiveMember = 0`) têm taxa de churn quase 2× maior que os ativos
- O tempo de relacionamento (`Tenure`) não apresenta padrão linear — nem clientes novos nem antigos são os que mais saem

---

### 2. O que melhor explica o churn?

Calculado via **Information Value (IV)**:

| Variável | IV | Poder Preditivo |
|---|---|---|
| `Complain` | > 3,0 | ⚠️ Dominante |
| `Age` | ~0,35 | ✅ Forte |
| `Balance` | ~0,15 | 🔵 Médio |
| `IsActiveMember` | ~0,15 | 🔵 Médio |
| `NumOfProducts` | ~0,12 | 🔵 Médio |
| `CreditScore` | ~0,02 | 🟡 Fraco |
| `SatisfactionScore` | ~0,02 | 🟡 Fraco |

> `Complain` domina com folga. Ter feito uma reclamação é o melhor preditor individual de cancelamento no dataset.

---

### 3. Padrões que indicam problemas internos

- **Crédito alto não retém**: correlação de Pearson entre `CreditScore` e `Exited` é próxima de zero — não há proteção
- **Muitos produtos, mais cancelamentos**: o padrão é contraintuitivo — mais produtos contratados está associado a mais churn, sugerindo complexidade ou insatisfação com o portfólio
- **Reclamações predizem saída quase certa**: independentemente do score de satisfação informado pelo cliente, quem registrou reclamação cancela em proporção altíssima — o score de satisfação isolado não é confiável como indicador de retenção

---

### 4. Decisões estratégicas

**Segmentação de risco:**
Clientes foram pontuados com um *Risk Score* composto (0 a 5) baseado nas variáveis de maior IV. Clientes com score ≥ 3 apresentam taxa de churn acima de 70%.

**Prioridade financeira:**
O cruzamento de *Risk Score* alto com receita estimada acima da mediana identifica o grupo de **maior impacto financeiro imediato** — clientes lucrativos que estão prestes a sair.

**Ações sugeridas:**
- Criar fluxo de retenção ativado no momento da reclamação, antes do cancelamento
- Revisar a proposta de valor para clientes com 3+ produtos (simplificação ou benefícios adicionais)
- Desenvolver programa específico para membros inativos com saldo alto
- Investigar a operação na Alemanha — churn estruturalmente superior às outras geografias

---

## 🛠️ Tecnologias

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?logo=pandas)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-1.x-F7931E?logo=scikit-learn&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-0.13-4C72B0)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

```
pandas · numpy · matplotlib · seaborn · scikit-learn
```

---

## ▶️ Como Executar

```bash
# 1. Clone o repositório
git clone https://github.com/seu-usuario/customer-churn-analysis.git
cd customer-churn-analysis

# 2. Instale as dependências
pip install pandas numpy matplotlib seaborn scikit-learn openpyxl jupyter

# 3. Abra o notebook
jupyter notebook customer_churn_records_analysis.ipynb
```

> O arquivo `customer_churn_records.xlsx` deve estar na mesma pasta do notebook.

---

## 📁 Estrutura de Pastas

```
customer-churn-analysis/
│
├── customer_churn_records.xlsx          # Dataset
├── customer_churn_records_analysis.ipynb  # Análise completa
└── README.md
```
