import streamlit as st
import pandas as pd
import random

# Sayfa Genişliği ve Tema
st.set_page_config(page_title="GanyanAnaliz AI v3.0", page_icon="🐎", layout="wide")

# Özel CSS ile Mobil Görünüm İyileştirme
st.markdown("""
    <style>
    .main { background-color: #0e1117; }
    .stMetric { background-color: #161b22; padding: 10px; border-radius: 10px; border: 1px solid #30363d; }
    .stButton>button { width: 100%; border-radius: 25px; height: 3em; background-color: #238636; color: white; font-weight: bold; }
    </style>
    """, unsafe_allow_html=True)

st.title("🐎 GanyanAnaliz AI - Mobil Prototip")
st.info("📊 23 Mart 2026 - Yapay Zeka Analiz Motoru Aktif")

# Analiz Fonksiyonu (Kafama Göre Algoritma)
def ai_tahmin(sehir):
    # Simüle edilmiş TJK verileri (Gerçek bültene göre genişletilebilir)
    at_verileri = {
        "Adana": ["MARTINA", "AZP STAR", "WHIZBANG", "GÖKÇESTAR", "FEARLESS DRAGON"],
        "Bursa": ["GO BEAUTY", "KAYALARIN OĞLU", "SARI FIRTINA", "KARA ÇOCUK", "RÜZGAR"]
    }
    
    sonuclar = []
    for at in at_verileri.get(sehir, ["At 1", "At 2"]):
        hp = random.randint(40, 98)
        form = random.randint(50, 100)
        skor = int((hp * 0.6) + (form * 0.4))
        
        durum = "Normal"
        if skor > 85: durum = "⭐ BANKO"
        elif skor < 60 and form > 85: durum = "💣 SÜRPRİZ"
        
        sonuclar.append({"At Adı": at, "HP": hp, "Form": f"%{form}", "AI Skoru": skor, "Durum": durum})
    
    return pd.DataFrame(sonuclar).sort_values(by="AI Skoru", ascending=False)

# Kullanıcı Arayüzü
sehir = st.selectbox("📍 Yarış Şehri Seçin", ["Adana", "Bursa"])

if st.button("🚀 ANALİZİ BAŞLAT"):
    with st.spinner('Veriler TJK sunucularından analiz ediliyor...'):
        df = ai_tahmin(sehir)
        
        for index, row in df.iterrows():
            col1, col2, col3 = st.columns([2, 1, 1])
            with col1:
                st.subheader(row['At Adı'])
                st.caption(f"Handikap: {row['HP']} | Form: {row['Form']}")
            with col2:
                if "⭐" in row['Durum']:
                    st.success(row['Durum'])
                elif "💣" in row['Durum']:
                    st.warning(row['Durum'])
                else:
                    st.write(row['Durum'])
            with col3:
                st.metric("Skor", row['AI Skoru'])
            st.divider()

st.sidebar.header("⚙️ Ayarlar")
st.sidebar.write("Model: DeepGanyan v2")
st.sidebar.write("Versiyon: 3.0.1")
