import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt

st.set_page_config(page_title="Video Games Dashboard", layout="wide", page_icon="🎮")

@st.cache_data
def load_data():
    url = "https://www.kaggle.com/datasets/artermiloff/steam-games-dataset?select=games_march2025_full.csv"
    return pd.read_csv(url)

df = load_data()

# Sidebar

st.sidebar.title("🎮 Filtros")

year = st.sidebar.selectbox(
    "Año de lanzamiento",
    sorted(df["release_year"].unique(), reverse=True)
)

age = st.sidebar.multiselect(
    "Clasificación ESRB",
    df["required_age"].unique(),
    default=df["required_age"].unique()
)

price_range = st.sidebar.slider(
    "Rango de precio",
    float(df["price"].min()),
    float(df["price"].max()),
    (0.0, float(df["price"].max()))
)

# Filtrado

filtered = df[
    (df["release_year"] == year) &
    (df["required_age"].isin(age)) &
    (df["price"].between(price_range[0], price_range[1]))
]


# Métricas

st.title(" Dashboard de Videojuegos")

col1, col2, col3, col4 = st.columns(4)

col1.metric(" Juegos", len(filtered))
col2.metric(" Precio promedio", f"${filtered['price'].mean():.2f}")
col3.metric(" % Positivo promedio", f"{filtered['porcentaje_positive_total'].mean()*100:.1f}%")
col4.metric(" Tiempo promedio", f"{filtered['average_playtime_forever'].mean():.1f} hrs")


# Gráficas

st.subheader(" Precio vs Valoración")

fig, ax = plt.subplots()
ax.scatter(
    filtered["price"],
    filtered["porcentaje_positive_total"],
    alpha=0.5
)
ax.set_xlabel("Precio")
ax.set_ylabel("Porcentaje positivo")
st.pyplot(fig)



st.subheader(" Distribución por Clasificación ESRB")

fig2, ax2 = plt.subplots()
filtered["required_age"].value_counts().plot(
    kind="pie",
    autopct="%1.1f%%",
    ax=ax2
)
ax2.set_ylabel("")
st.pyplot(fig2)


st.subheader(" Juegos más populares")

top_games = (
    filtered.sort_values("total_num_reviews", ascending=False)
    .head(10)[["price", "total_num_reviews", "porcentaje_positive_total"]]
)

st.dataframe(top_games)
