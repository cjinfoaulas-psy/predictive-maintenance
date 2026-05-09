# Manutenção Preditiva de Equipamentos Industriais

> Projeto de Machine Learning para previsão de falhas em equipamentos industriais com base em dados de sensores operacionais.

---

## Contexto e Problema de Negócio

Falhas não planejadas em equipamentos industriais geram paradas de produção, custos elevados de manutenção corretiva e riscos operacionais. A manutenção preditiva surge como alternativa à manutenção reativa, utilizando dados de operação para antecipar falhas antes que aconteçam.

Este projeto simula esse cenário com dados reais de uma linha de produção industrial, onde sensores monitoram continuamente variáveis como temperatura, rotação, torque e desgaste de ferramenta.

---

## Objetivo

Desenvolver um modelo de classificação capaz de prever se um equipamento vai falhar com base nas variáveis operacionais coletadas por sensores, identificando o modo de falha mais crítico e as variáveis com maior poder preditivo.

---

## Dataset

- **Nome:** AI4I 2020 Predictive Maintenance Dataset
- **Fonte:** [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/601/ai4i+2020+predictive+maintenance+dataset)
- **Registros:** 10.000
- **Variáveis:** 14
- **Target:** `Machine failure` (0 = sem falha, 1 = falha)

### Variáveis do dataset

| Variável | Descrição |
|---|---|
| `Air temperature [K]` | Temperatura do ar em Kelvin |
| `Process temperature [K]` | Temperatura do processo em Kelvin |
| `Rotational speed [rpm]` | Velocidade rotacional da máquina |
| `Torque [Nm]` | Torque aplicado |
| `Tool wear [min]` | Desgaste acumulado da ferramenta |
| `Machine failure` | Target: falha (1) ou operação normal (0) |
| `TWF / HDF / PWF / OSF / RNF` | Tipos específicos de falha |

---

## Principais Insights da Análise Exploratória

- **Taxa de falha:** 3.39% dos registros, dataset altamente desbalanceado
- **Modo de falha mais comum:** HDF (Heat Dissipation Failure), falha por dissipação de calor
- **Variáveis com maior poder preditivo:** Torque e Tool wear apresentaram maior separação entre registros com e sem falha
- **Features de engenharia criadas:**
  - `temp_diff`: diferença entre temperatura do processo e do ar, diretamente relacionada ao modo HDF
  - `power`: produto entre rotação e torque, representando a potência estimada do equipamento

---

## Tecnologias Utilizadas

- Python 3.12
- Pandas 3.0
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn 1.8

---

## Estrutura do Repositório

```
predictive-maintenance/
├── data/
│   └── ai4i2020.csv
├── notebooks/
│   └── 01_exploratory_analysis.ipynb
│   └── 02_model_training.ipynb
├── .gitignore
└── README.md
```

---

## Como Executar

```bash
# Clone o repositório
git clone https://github.com/cjinfoaulas-psy/predictive-maintenance.git
cd predictive-maintenance

# Crie e ative o ambiente virtual
python -m venv venv
venv\Scripts\activate  # Windows

# Instale as dependências
pip install pandas numpy matplotlib seaborn scikit-learn jupyter ipykernel

# Abra o Jupyter no VS Code e execute os notebooks em ordem
```

---

## Modelo e Resultados

**Algoritmo:** Random Forest Classifier (100 árvores, class_weight=balanced)

**Divisão dos dados:** 80% treino / 20% teste com estratificação

### Métricas no conjunto de teste

| Métrica | Sem Falha | Com Falha |
|---|---|---|
| Precision | 99% | 96% |
| Recall | 100% | 66% |
| F1-Score | 99% | 78% |
| Acurácia geral | 99% | |

### Importância das features

| Ranking | Feature | Origem |
|---|---|---|
| 1 | Torque [Nm] | Original |
| 2 | Rotational speed [rpm] | Original |
| 3 | power | Feature de engenharia |
| 4 | Tool wear [min] | Original |
| 5 | temp_diff | Feature de engenharia |
| 6 | Air temperature [K] | Original |
| 7 | Process temperature [K] | Original |
| 8 | Type | Original |

### Interpretação dos resultados

- Variáveis mecânicas (torque e rotação) dominam a previsão de falhas
- A feature de engenharia `power` superou variáveis originais do dataset, validando o uso de conhecimento de domínio na construção do modelo
- O Recall de 66% indica que o modelo detecta 2 em cada 3 falhas reais, resultado relevante considerando que nenhuma técnica de balanceamento foi aplicada

---

## Próximos Passos

- Aplicar técnicas de balanceamento de classes (SMOTE) para melhorar o Recall
- Testar outros algoritmos (XGBoost, LightGBM)
- Interpretar o modelo com SHAP values
- Desenvolver pipeline de predição em produção

---

## Autor

**Caio Henrique Vieira**
Engenheiro Mecânico | Analista de Inteligência de Produto | Transição para Data Science

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Caio%20Henrique-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/caiohenrique-vieira/)

---

## Mentoria e Desenvolvimento

Este projeto foi desenvolvido com orientação de **Claude (Anthropic)**, utilizado como mentor técnico no processo de estruturação do portfólio, explicação de conceitos e boas práticas de Data Science aplicado à engenharia industrial.