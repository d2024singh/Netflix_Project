# Netflix_Project ready-to-upload structure

# 1️⃣ netflix_analysis.ipynb (EDA Notebook)
# Include all STEP 1-3 codes, visualizations, and insights

# 2️⃣ netflix_dashboard.py (Interactive Dashboard)
import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt

st.set_page_config(page_title="Netflix Dashboard", layout="wide")
st.title("🎬 Netflix Movies & TV Shows Dashboard")
st.markdown("### Data Analysis using Python & Streamlit")

df = pd.read_csv("netflix_titles.csv")

# Preprocessing
import numpy as np

df['date_added'] = pd.to_datetime(df['date_added'], errors='coerce')
df['year_added'] = df['date_added'].dt.year
df['main_country'] = df['country'].fillna('Unknown').apply(lambda x: x.split(',')[0])
df['main_genre'] = df['listed_in'].apply(lambda x: x.split(',')[0])
df['duration_value'] = df['duration'].str.extract('(\d+)').astype(float)
df['director'] = df['director'].fillna('Not Available')
df['cast'] = df['cast'].fillna('Not Available')

# KPI Cards
col1, col2, col3 = st.columns(3)
col1.metric("Total Titles", df.shape[0])
col2.metric("Total Movies", df[df['type']=="Movie"].shape[0])
col3.metric("Total TV Shows", df[df['type']=="TV Show"].shape[0])

# Sidebar Filters
st.sidebar.header("Filter Options")
type_filter = st.sidebar.selectbox("Select Type", options=df['type'].unique())
country_filter = st.sidebar.selectbox("Select Country", options=sorted(df['main_country'].unique()))
year_filter = st.sidebar.slider("Select Release Year", int(df['release_year'].min()), int(df['release_year'].max()))

filtered_df = df[(df['type'] == type_filter) & (df['main_country'] == country_filter) & (df['release_year'] == year_filter)]

# Rating Distribution Chart
st.subheader("Rating Distribution")
fig, ax = plt.subplots()
filtered_df['rating'].value_counts().plot(kind='bar', ax=ax)
plt.xlabel("Rating")
plt.ylabel("Count")
st.pyplot(fig)

# Genre Chart
st.subheader("Top Genres")
fig, ax = plt.subplots()
filtered_df['main_genre'].value_counts().head(10).plot(kind='bar', ax=ax)
plt.xlabel("Genre")
plt.ylabel("Count")
st.pyplot(fig)

# 3️⃣ netflix_titles.csv (Dataset)
# Source:Kaggle - Netflix Movies and TV Shows Dataset

# 4️⃣ requirements.txt
# pandas
# matplotlib
# seaborn
# streamlit

# 5️⃣ README.md
readme_content = '''
# Netflix Movies & TV Shows Analysis Dashboard

## Project Overview
This project analyzes Netflix's Movies & TV Shows dataset and provides an interactive dashboard for insights. Built using Python and Streamlit.

## Features
- Interactive dashboard with KPIs
- Filter by type (Movie/TV Show), country, and release year
- Visualizations: Movies vs TV Shows, Top countries, Top genres, Rating distribution, Content growth

## Dataset
Netflix Movies and TV Shows dataset from Kaggle

## Tech Stack
- Python
- Pandas
- Matplotlib & Seaborn
- Streamlit

## How to Run
1. Clone the repo: git clone <repo-link>
2. Install dependencies: pip install -r requirements.txt
3. Run the dashboard: streamlit run netflix_dashboard.py

## Insights
- Netflix has more Movies than TV Shows.
- Most content is from USA and India.
- Drama and International genres are popular.
- Recent years saw rapid content growth.
'''

with open("README.md", "w") as f:
    f.write(readme_content)
## 📌 Business Recommendations
- Netflix can further invest in international and regional content to expand global reach.
- Focus on Drama and mature-rated content aligns well with current audience preferences.
- Increasing TV show production may help improve long-term user engagement.
