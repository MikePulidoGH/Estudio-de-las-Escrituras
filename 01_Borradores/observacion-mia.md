---
tipo: "OBSERVACIÓN MÍA"
fecha: "2026-11-05"
pasaje: ""
versiculos: ""
nodo_laboratorio: ""
estado: "nueva"
origen: "captura-rapida-notas-vivas"
---

# OBSERVACIÓN MÍA

cat > ~/keep-it-markdown/widget-keep/captura_escrituras.py << 'EOF'
import json
import os
import re
import subprocess
from datetime import datetime

# Cambiá esta ruta por la de tu bóveda de Obsidian
VAULT = os.path.expanduser(
    "~/storage/shared/Estudio-de-las-Escrituras"
)

# Carpeta por defecto: el explorador arranca acá
DESTINO = os.path.join(VAULT, "Inbox")

HOME_STORAGE = os.path.expanduser("~/storage") + "/"

def limpiar(titulo):
    titulo = re.sub(r'[\\/:*?"<>|]', '-', titulo)
    return titulo.lstrip("# ").strip(" .")[:60].strip()

def pedir_titulo():
    try:
        salida = subprocess.check_output(
            ["termux-dialog", "text", "-t", "Título de la nota"],
            text=True,
            stderr=subprocess.PIPE
        )
        datos = json.loads(salida)
    except Exception:
        return ""
    if datos.get("code") != -1:
        return None  # canceló
    return datos.get("text", "").strip()

def pedir_nombre_carpeta():
    salida = subprocess.check_output(
        ["termux-dialog", "text", "-t", "Nombre de la nueva carpeta"],
        text=True,
        stderr=subprocess.PIPE
    )
    datos = json.loads(salida)
    if datos.get("code") != -1:
        return ""
    return limpiar(datos.get("text", "").strip())

def raices_almacenamiento():
    raices = [os.path.expanduser("~/storage/shared")]
    externo = os.path.expanduser("~/storage/external-1")
    if os.path.isdir(externo):
        raices.append(externo)
    return raices

def subcarpetas(ruta):
    try:
        nombres = sorted(os.listdir(ruta), key=str.lower)
    except Exception:
        return []
    return [
        os.path.join(ruta, n) for n in nombres
        if not n.startswith(".") and os.path.isdir(os.path.join(ruta, n))
    ]

def menu_radio(titulo, etiquetas):
    """Muestra un menú de opción única. Devuelve el índice o None si cancela."""
    etiquetas = [e.replace(",", " ") for e in etiquetas]
    salida = subprocess.check_output(
        ["termux-dialog", "radio", "-t", titulo, "-v", ",".join(etiquetas)],
        text=True,
        stderr=subprocess.PIPE
    )
    datos = json.loads(salida)
    if datos.get("code") != -1:
        return None
    indice = datos.get("index")
    if not isinstance(indice, int) or indice < 0 or indice >= len(etiquetas):
        return None
    return indice

def elegir_almacenamiento(raices):
    etiquetas = [r.replace(HOME_STORAGE, "") for r in raices]
    indice = menu_radio("Elegí el almacenamiento", etiquetas)
    return None if indice is None else raices[indice]

def punto_inicio(raices):
    for candidato in (DESTINO, VAULT, raices[0]):
        if os.path.isdir(candidato):
            return candidato
    return raices[0]

def elegir_carpeta():
    """Explorador de carpetas. Devuelve la carpeta elegida, None si cancela,
    o DESTINO si el menú falla."""
    try:
        raices = raices_almacenamiento()
        raices_norm = [os.path.normpath(r) for r in raices]
        actual = punto_inicio(raices)

        while True:
            opciones = [("✔ GUARDAR AQUÍ", "guardar", None)]
            if os.path.normpath(actual) not in raices_norm:
                opciones.append(("⬆ Subir", "subir", None))
            elif len(raices) > 1:
                opciones.append(("💾 Cambiar almacenamiento", "almacenamiento", None))
            opciones.append(("➕ Nueva carpeta aquí", "nueva", None))
            for sub in subcarpetas(actual):
                opciones.append(("📁 " + os.path.basename(sub), "entrar", sub))

            titulo = "📂 " + actual.replace(HOME_STORAGE, "")
            indice = menu_radio(titulo, [o[0] for o in opciones])
            if indice is None:
                return None  # canceló

            _, accion, valor = opciones[indice]
            if accion == "guardar":
                return actual
            elif accion == "subir":
                actual = os.path.dirname(actual)
            elif accion == "almacenamiento":
                nueva_raiz = elegir_almacenamiento(raices)
                if nueva_raiz:
                    actual = nueva_raiz
            elif accion == "nueva":
                nombre = pedir_nombre_carpeta()
                if nombre:
                    nueva = os.path.join(actual, nombre)
                    os.makedirs(nueva, exist_ok=True)
                    actual = nueva
            elif accion == "entrar":
                actual = valor
    except Exception:
        return DESTINO

def main():
    try:
        texto = subprocess.check_output(
            ["termux-clipboard-get"],
            text=True,
            stderr=subprocess.PIPE
        ).strip()
    except Exception as e:
        print(f"Error al leer el portapapeles: {e}")
        return

    if not texto:
        print("El portapapeles está vacío.")
        return

    ahora = datetime.now()
    sello = ahora.strftime("%Y-%m-%d_%H-%M-%S")

    elegido = pedir_titulo()
    if elegido is None:
        print("Cancelado.")
        return

    destino = elegir_carpeta()
    if destino is None:
        print("Cancelado.")
        return
    os.makedirs(destino, exist_ok=True)

    # Título elegido; si está vacío, primera línea del texto
    titulo = limpiar(elegido) or limpiar(texto.splitlines()[0]) or sello

    ruta = os.path.join(destino, f"{titulo}.md")
    if os.path.exists(ruta):
        ruta = os.path.join(destino, f"{titulo} {sello}.md")

    contenido = (
        "---\n"
        "tags:\n"
        "  - clipboard\n"
        "  - captura\n"
        f"created: {ahora.strftime('%Y-%m-%d %H:%M:%S')}\n"
        "---\n\n"
        f"{texto}\n"
    )

    with open(ruta, "x", encoding="utf-8") as archivo:
        archivo.write(contenido)

    print(f"Nota guardada: {ruta}")

if __name__ == "__main__":
    main()
EOF
