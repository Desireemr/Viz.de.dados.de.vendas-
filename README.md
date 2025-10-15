# Projeto de Análise de Dados em Python

Este projeto foi desenvolvido com a finalidade de **praticar as competências adquiridas na especialização em Data Science & Analytics**, aplicando técnicas de análise exploratória de dados, limpeza de dados e visualização utilizando Python.

## Descrição do projeto

O objetivo deste projeto é analisar um conjunto de dados fictício de vendas de uma empresa de varejo, extraindo insights sobre:

- Volume de vendas por produto e região
- Tendências de vendas ao longo do tempo
- Produtos mais rentáveis
- Distribuição de vendas por categorias

## Tecnologias e bibliotecas utilizadas

- Python 
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook

## Estrutura do projeto

- `data/` → Contém o dataset utilizado.
- `notebooks/` → Contém o notebook de análise exploratória.
- `scripts/` → Scripts Python para limpeza e visualização de dados.
- `images/` → Plots gerados durante a análise.
- `README.md` → Documentação do projeto.

## Como executar

1. Clone o repositório:
```bash
git clone https://github.com/seu-usuario/data-analysis-project.git

pip install -r requirements.txt
jupyter notebook notebooks/analysis.ipynb

---

## **3️⃣ requirements.txt**


---

## **4️⃣ data/sales_data.csv**

O arquivo `sales_data.csv` será gerado pelo script abaixo. Ele contém **200 registros fictícios** com as colunas:

- Order ID
- Product Name
- Category
- Sales
- Quantity
- Order Date
- Region

**Código para gerar o CSV (execute antes de usar o notebook):**

```python
import pandas as pd
import numpy as np
import random
from datetime import datetime, timedelta

# Configurações
num_records = 200
categories = ['Eletrônicos', 'Móveis', 'Papelaria', 'Roupas', 'Alimentos']
products = {
    'Eletrônicos': ['Smartphone', 'Notebook', 'Fone de Ouvido', 'Monitor', 'Teclado'],
    'Móveis': ['Cadeira', 'Mesa', 'Estante', 'Sofá', 'Guarda-Roupa'],
    'Papelaria': ['Caderno', 'Caneta', 'Agenda', 'Papel A4', 'Marcador'],
    'Roupas': ['Camisa', 'Calça', 'Jaqueta', 'Vestido', 'Sapato'],
    'Alimentos': ['Arroz', 'Feijão', 'Macarrão', 'Óleo', 'Café']
}
regions = ['Norte', 'Sul', 'Leste', 'Oeste']

def random_date(start, end):
    delta = end - start
    random_days = random.randint(0, delta.days)
    return start + timedelta(days=random_days)

data = []
start_date = datetime(2024, 1, 1)
end_date = datetime(2024, 12, 31)

for i in range(1, num_records + 1):
    category = random.choice(categories)
    product = random.choice(products[category])
    sales = round(random.uniform(20, 2000), 2)
    quantity = random.randint(1, 10)
    order_date = random_date(start_date, end_date).strftime('%Y-%m-%d')
    region = random.choice(regions)
    data.append([f'ORD{i:04d}', product, category, sales, quantity, order_date, region])

df = pd.DataFrame(data, columns=['Order ID', 'Product Name', 'Category', 'Sales', 'Quantity', 'Order Date', 'Region'])
df.to_csv('data/sales_data.csv', index=False)
print("Arquivo 'sales_data.csv' gerado com sucesso!")

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

sns.set_style("whitegrid")
plt.rcParams["figure.figsize"] = (10,6)

# Carregando os dados
data = pd.read_csv('../data/sales_data.csv')

# Visualizando dados
display(data.head())
display(data.describe())
print(data.isnull().sum())

# Limpeza simples
data.dropna(inplace=True)

# Vendas por categoria
sales_by_category = data.groupby('Category')['Sales'].sum().sort_values(ascending=False)
sales_by_category.plot(kind='bar', color='skyblue', title='Vendas por Categoria')
plt.ylabel('Total de Vendas')
plt.show()

# Vendas ao longo do tempo
data['Order Date'] = pd.to_datetime(data['Order Date'])
sales_over_time = data.groupby(data['Order Date'].dt.to_period('M'))['Sales'].sum()
sales_over_time.plot(title='Vendas Mensais', marker='o')
plt.ylabel('Total de Vendas')
plt.show()

# Top 10 produtos mais vendidos
top_products = data.groupby('Product Name')['Sales'].sum().sort_values(ascending=False).head(10)
sns.barplot(x=top_products.values, y=top_products.index, palette='viridis')
plt.title('Top 10 Produtos Mais Vendidos')
plt.show()

import pandas as pd

data = pd.read_csv('../data/sales_data.csv')
data.dropna(inplace=True)
data.to_csv('../data/sales_data_clean.csv', index=False)
print("Dados limpos salvos com sucesso!")

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

sns.set_style("whitegrid")
plt.rcParams["figure.figsize"] = (10,6)

data = pd.read_csv('../data/sales_data_clean.csv')

# Vendas por categoria
sales_by_category = data.groupby('Category')['Sales'].sum()
sales_by_category.plot(kind='bar', color='skyblue', title='Vendas por Categoria')
plt.ylabel('Total de Vendas')
plt.show()
