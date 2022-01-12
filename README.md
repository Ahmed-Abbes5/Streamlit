# Streamlit


import streamlit as st
import pandas as pd
from PIL import Image
import numpy as np
import plotly.express as px
import matplotlib.pyplot as plt
import matplotlib
import altair as alt
import webbrowser


im = Image.open('Desktop/MSDPY.png')
st.title('Dashboard Réunions Publiques MSD')
st.image(im)

url = 'https://github.com/Ahmed-Abbes5/psbx'
if st.sidebar.button("MY GITHUB"):
    webbrowser.open_new_tab(url)

url2 = 'https://www.msd.com/stories/'
if st.sidebar.button("About US : MSD"):
    webbrowser.open_new_tab(url2)
    

df= pd.read_excel('Downloads/Data_1712.xlsx')
df['Type RP / Catégorie']=df['Type RP / Catégorie'].fillna('Vide')


fig = px.pie(df,values= df['Type'].value_counts() ,names=df['Type'].unique(),title='Camembert Type de contrats')
st.plotly_chart(fig)



fig1 = px.bar(df,x=df['Type RP / Catégorie'].unique(),y= df['Type RP / Catégorie'].value_counts(), title='Graphique de Types de RP',labels={'x':'Type de Réunions Publiques','y':"Nombre d'occurrences"},height=600,width=800)
st.plotly_chart(fig1)






df['Aire Thérapeutique']=df['Aire Thérapeutique'].fillna('inconnu') 
line_grp = alt.Chart(df).mark_line().encode(x='Aire Thérapeutique',y='count()').properties(width = 900, height = 900)
st.altair_chart(line_grp)
#nombres de RP par Air thérapeutique 


st.write('Veuillez choisir une Aire Thérapeutique pour afficher ses données')
choix_aire= st.multiselect("Aire Thérapeutique",df['Aire Thérapeutique'].unique())
filter_df = df[df['Aire Thérapeutique'].isin(choix_aire)]
st.dataframe(filter_df)

#filtre intéractif
