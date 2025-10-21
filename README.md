# LOVE-P-DEL
APP LOVE PÁDEL
import streamlit as st
import random

st.set_page_config(page_title="LOVE PÁDEL", layout="centered")

# Imagen de bienvenida
st.image("https://your-image-url.com/lovepadel.jpg")  # Reemplaza con tu imagen subida

st.title("🎾 LOVE PÁDEL")

# Base de datos simulada
if "jugadores" not in st.session_state:
    st.session_state.jugadores = {}
if "partidos" not in st.session_state:
    st.session_state.partidos = []

# Panel de administrador
st.sidebar.header("🔐 Acceso administrador")
admin_user = st.sidebar.text_input("Usuario")
admin_pass = st.sidebar.text_input("Clave", type="password")

if admin_user in ["admin1", "admin2", "admin3"] and admin_pass == "tu_clave":
    st.sidebar.success("Acceso concedido")

    st.subheader("👥 Crear jugador")
    nombre = st.text_input("Nombre del jugador")
    emoji = st.text_input("Emoji (elige en emojipedia.org)")
    if st.button("Crear jugador"):
        clave = str(random.randint(1000, 9999))
        st.session_state.jugadores[nombre] = {"emoji": emoji, "clave": clave}
        st.success(f"Jugador creado: {nombre} {emoji} - Clave: {clave}")

    st.subheader("📅 Crear partido")
    dia = st.date_input("Fecha del partido")
    pista = st.text_input("Número de pista")
    if st.button("Crear partido"):
        st.session_state.partidos.append({"fecha": dia, "pista": pista, "jugadores": []})
        st.success("Partido creado")

# Panel de jugador
st.subheader("🙋‍♂️ Acceso jugador")
nombre_j = st.text_input("Tu nombre")
emoji_j = st.text_input("Tu emoji")
clave_j = st.text_input("Tu clave", type="password")

if st.button("Entrar"):
    jugador = st.session_state.jugadores.get(nombre_j)
    if jugador and jugador["emoji"] == emoji_j and jugador["clave"] == clave_j:
        st.success("Bienvenido al sistema")
        st.subheader("📋 Partidos disponibles")
        for i, partido in enumerate(st.session_state.partidos):
            st.write(f"{partido['fecha']} - Pista {partido['pista']}")
            if len(partido["jugadores"]) < 4:
                if st.button(f"Apuntarse al partido {i}"):
                    partido["jugadores"].append(nombre_j)
                    st.success("¡Apuntado!")
            else:
                st.warning("Partido completo")
    else:
        st.error("Datos incorrectos")
