# 📊 Desafio de Engenharia de Dados - Airbnb

## Bem-vindos(as)

Este desafio faz parte da minha formação como Profissional de Dados, especificamente uma formação em engenharia de dados.

---

### 🎯 Etapas do Projeto

Neste projeto, vamos passar pelas seguintes etapas:

- **Preparação dos Dados**
- **Tratamento de Valores Ausentes**
- **Detecção e Tratamento de Outliers**
- **Transformação de Dados Categóricos**

> *No Notebook Jupyter constam todas as etapas acima citadas.*

---

### 📁 Dados do Projeto

[**ACESSAR OS DADOS**](https://drive.google.com/drive/folders/1xG7SX4eyZF7PQ17KN3BJ4qHosmDfqqhP?usp=sharing)

---

### 🔗 Contato

Conecte-se comigo no LinkedIn:

<a href="https://www.linkedin.com/in/icaroalmeidas/">
  <img src="https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=linkedin&logoColor=white">
</a>

---

## 📦 IMPORTANDO BIBLIOTECAS

```python
import pandas as pd

📂 CARREGANDO OS DADOS
Carregando o dataset de listings
df_listing = pd.read_csv(r"H:\Meu Drive\PESSOAL\PROJETOS\Desafio-Organizando-e-analisando-dados-de-um-Airbnb\listings_cleaned.csv")

Carregando o dataset de reviews
df_reviwes = pd.read_csv(r"H:\Meu Drive\PESSOAL\PROJETOS\Desafio-Organizando-e-analisando-dados-de-um-Airbnb\reviews.csv")

🧹 LIMPEZA E TRATAMENTO
Explorando o Dataset de Listings
--CONHECENDO OS DADOS--
df_listing.head()

--INFORMAÇÕES DOS DADOS--
df_listing.info()

--TIPOS DE DADOS--
df_listing.dtypes

--VERIFICANDO VALORES DUPLICADOS NA COLUNA ID--
df_listing["id"].value_counts()

--VERIFICANDO DIMENSÕES DO DATAFRAME--
df_listing.shape

Análise de Valores Nulos
--VERIFICANDO VALORES NULOS NO DATAFRAME--
df_listing.isnull().sum()

--PORCENTAGEM DE VALORES NULOS NO DATAFRAME--
(df_listing.isnull().sum() / df_listing.shape[0] * 100).sort_values(ascending=False)

--VERIFICANDO COLUNAS NUMÉRICAS COM VALORES NULOS--
df_listing[["bathrooms", "bedrooms", "beds", "price"]]

Preenchendo Dados Nulos
--PREENCHENDO VALORES NULOS NA COLUNA BEDROOMS COM O VALOR 1--
df_listing["bedrooms"].fillna(1, inplace=True)

--PREENCHENDO VALORES NULOS NA COLUNA BATHROOMS COM O VALOR 1--
df_listing["bathrooms"].fillna(1, inplace=True)

--PREENCHENDO VALORES NULOS NA COLUNA BEDS COM O VALOR 1--
df_listing["beds"].fillna(1, inplace=True)

--VERIFICANDO VALORES NULOS NO DATAFRAME--
df_listing.isnull().sum()

--PREENCHENDO VALORES NULOS NA COLUNA PRICE COM A MEDIANA DA COLUNA--
df_listing["price"].fillna(df_listing["price"].median(), inplace=True)

--VERIFICANDO VALORES NULOS NO DATAFRAME--
df_listing.isnull().sum()

--DADOS APÓS TRATAMENTO DE VALORES NULOS--
df_listing.head()

Explorando o Dataset de Reviews
--CONHECENDO OS DADOS DO DATAFRAME REVIEWS--
df_reviwes.head()

--INFORMAÇÕES DOS DADOS DO DATAFRAME REVIEWS--
df_reviwes.info()

--VERIFICANDO VALORES NULOS NO DATAFRAME REVIEWS--
df_reviwes.isnull().sum()

--VERIFICANDO VALORES DUPLICADOS NA COLUNA ID DO DATAFRAME REVIEWS--
df_reviwes["id"].value_counts()

Merge dos DataFrames
--REALIZANDO O MERGE ENTRE OS DOIS DATAFRAMES--
df_final = pd.merge(df_listing, df_reviwes, left_on='id', right_on='id', how='inner')

--DADOS DO DATAFRAME FINAL APÓS O MERGE--
df_final.head(10)

df_final.shape

df_final.dtypes

Conversão de Tipos de Dados
--CONVERTENDO A COLUNA ID PARA STRING--
df_final["id"] = df_final["id"].astype(str)

--CONVERTENDO A COLUNA BATHROOMS PARA INTEIRO--
df_final["bathrooms"] = df_final["bathrooms"].astype(int)

--CONVERTENDO A COLUNA BEDROOMS PARA INTEIRO--
df_final["bedrooms"] = df_final["bedrooms"].astype(int)

--CONVERTENDO A COLUNA BEDS PARA INTEIRO--
df_final["beds"] = df_final["beds"].astype(int)

--VERIFICANDO OS TIPOS DE DADOS DO DATAFRAME FINAL--
df_final.dtypes

df_final.shape

df_final.head()

🔍 IDENTIFICANDO E TRATANDO OUTLIERS
df_final.columns

colunas_numericas = ["accommodates", "bathrooms", "bedrooms", "beds", "price", "number_of_reviews", "review_scores_rating"]

--MANTENDO APENAS VALORES MAIORES QUE 0--
df_final = df_final[df_final["price"] > 0]

--REMOVENDO OUTLIERS MAIORES QUE 1000 NA COLUNA PRICE--
df_final[df_final["price"] < 0].sum()

--IDENTIFICANDO OUTLIERS NA COLUNA PRICE--
df_final['price'].plot(kind='box')

Cálculo do IQR (Intervalo Interquartil)
Q1 = df_final['price'].quantile(0.25)
Q3 = df_final['price'].quantile(0.75)
IQR = Q3 - Q1
limite_superior = Q1 + 1.5 * IQR

# Ajustar o limite inferior para zero
limite_inferior = max(0, Q1 - 1.5 * IQR)

limite_superior

limite_inferior

--IDENTIFICANDO OUTLIERS NA COLUNA PRICE--
outliers = df_final[(df_final['price'] < limite_inferior) | (df_final['price'] > limite_superior)]

--VERIFICANDO A QUANTIDADE DE OUTLIERS IDENTIFICADOS NA COLUNA PRICE--
outliers.shape

outliers

🗑️ REMOVENDO OUTLIERS
--REMOVENDO OUTLIERS DO DATAFRAME FINAL--
valores_outliers = outliers['price'].values
df_final_sem_outliers = df_final[~df_final['price'].isin(valores_outliers)]

df_final.shape

df_final_sem_outliers.shape

🔄 TRANSFORMANDO DADOS CATEGÓRICOS
--VERIFICANDO TIPOS DE DADOS APÓS REMOÇÃO DE OUTLIERS--
df_final_sem_outliers.dtypes

--VERIFICANDO VALORES ÚNICOS NA COLUNA ROOM TYPE--
df_final_sem_outliers['room_type'].value_counts()

--CONVERTENDO A COLUNA ROOM TYPE PARA CATEGÓRICA--
df_final_sem_outliers['room_type_number'] = df_final_sem_outliers['room_type'].astype('category').cat.codes

df_final_sem_outliers[['room_type', 'room_type_number']].value_counts()

df_final_sem_outliers.head()

df_final_sem_outliers.info()

df_final_sem_outliers.describe()

df_final_sem_outliers.columns

💾 SALVANDO O DATASET FINAL
df_final_sem_outliers.to_csv(r"H:\Meu Drive\PESSOAL\PROJETOS\Desafio-Organizando-e-analisando-dados-de-um-Airbnb\df_final_sem_outliers.csv", index=False)

🎉 Conclusão
Este notebook demonstra um pipeline completo de engenharia de dados, incluindo:

✅ Carregamento e exploração de dados
✅ Tratamento de valores ausentes
✅ Detecção e remoção de outliers usando IQR
✅ Transformação de variáveis categóricas
✅ Exportação do dataset limpo
Dataset final salvo com sucesso! 🚀

📊 Estrutura do Projeto
📁 Desafio-Organizando-e-analisando-dados-de-um-Airbnb/
│
├── 📄 listings_cleaned.csv          # Dataset original de listagens
├── 📄 reviews.csv                   # Dataset original de avaliações
├── 📄 df_final_sem_outliers.csv     # Dataset final processado
└── 📓 notebook.ipynb                # Notebook com análise completa

🛠️ Tecnologias Utilizadas

📈 Resultados
Total de registros originais: Verificar com df_final.shape
Total de registros após limpeza: Verificar com df_final_sem_outliers.shape
Outliers removidos: Diferença entre os dois datasets
Variáveis categóricas transformadas: room_type → room_type_number
🚀 Como Executar
Clone o repositório ou baixe os arquivos

Instale as dependências:

pip install pandas matplotlib

Execute o notebook Jupyter:
jupyter notebook

Ou execute via Python:
python seu_script.py

📝 Licença
Este projeto foi desenvolvido para fins educacionais como parte da formação em Engenharia de Dados.

✨ Autor
Ícaro Almeida de Souza

⭐ Se este projeto foi útil para você, considere dar uma estrela!