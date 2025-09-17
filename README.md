## Hi there 👋
👋 Olá, eu sou Crislan

🎓 Estudante de Linguagem de Python na na Fábrica de Programadores.
💻 Interessado em programação, tecnologia e aprendizado contínuo
🚀 Atualmente aprendendo: Python.

🛠️ Tecnologias e Ferramentas

Linguagens: Python 

Ferramentas: GitHub | VS Code 

# make_dog_matrix_gif.py
# Gera um GIF com "rain" de caracteres ao fundo e um cachorro no centro.
# Requisitos: pillow, imageio
# pip install pillow imageio

import os
import random
from PIL import Image, ImageDraw, ImageFont, ImageEnhance
import imageio

# Configurações
W, H = 900, 500               # tamanho do GIF
FRAMES = 50                   # número de frames da animação
COL_WIDTH = 18                # largura de cada coluna de texto (px)
FONT_SIZE = 18
SPEED = 5                     # quão rápido as colunas deslizam (pixels/frame)
OUT_NAME = "dog_matrix.gif"
DOG_FILE = "dog.png"          # coloque sua imagem aqui (preferencialmente PNG com alpha)

# Fonte monoespaçada (tente instalar DejaVu Sans Mono ou Edite o caminho da fonte)
try:
    FONT = ImageFont.truetype("DejaVuSansMono.ttf", FONT_SIZE)
except Exception:
    FONT = ImageFont.load_default()

# caracteres a usar (números + letras maiúsculas e minúsculas)
CHARS = list("0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz@#$%&*()-+=<>?")

# número de colunas
cols = W // COL_WIDTH

# cria posições iniciais random para cada coluna (y offset)
offsets = [random.randint(-H, 0) for _ in range(cols)]
speeds = [random.randint(2, SPEED) for _ in range(cols)]  # velocidade por coluna

frames = []

# carrega e redimensiona o cachorro
if not os.path.exists(DOG_FILE):
    raise FileNotFoundError(f"Arquivo {DOG_FILE} não encontrado. Coloque uma imagem chamada {DOG_FILE} na pasta.")

dog = Image.open(DOG_FILE).convert("RGBA")
# redimensiona o cachorro para caber bem
max_dog_w = int(W * 0.45)
scale = min(max_dog_w / dog.width, (H * 0.65) / dog.height, 1.0)
dog = dog.resize((int(dog.width * scale), int(dog.height * scale)), Image.LANCZOS)

dog_x = (W - dog.width) // 2
dog_y = (H - dog.height) // 2 + 20  # ligeiramente abaixo do centro

for f in range(FRAMES):
    # imagem de fundo preta
    img = Image.new("RGBA", (W, H), (0, 0, 0, 255))
    draw = ImageDraw.Draw(img)

    # desenha cada coluna de caracteres
    for c in range(cols):
        x = c * COL_WIDTH + 4
        y = offsets[c]

        # desenha uma "cauda" de caracteres por coluna
        tail_len = random.randint(8, 20)
        for i in range(tail_len):
            ch = random.choice(CHARS)
            yy = y + i * (FONT_SIZE + 2)

            # efeito de brilho: último caractere mais claro
            if i == tail_len - 1:
                fill = (180, 255, 180, 255)  # verde-claro
            else:
                # gradação de verde (mais transparente conforme se afasta do topo da cauda)
                alpha = max(10, 220 - i * 12)
                fill = (0, 180, 0, alpha)

            # só desenha se dentro da tela
            if 0 <= yy < H:
                draw.text((x, yy), ch, font=FONT, fill=fill)

        # atualiza offset da coluna
        offsets[c] += speeds[c]
        # reinicia coluna quando sai da tela (para looping)
        if offsets[c] - tail_len * (FONT_SIZE + 2) > H:
            offsets[c] = random.randint(-H//2, 0)
            speeds[c] = random.randint(2, SPEED)

    # coloca uma leve camada semitransparente para dar profundidade (opcional)
    overlay = Image.new("RGBA", (W, H), (0, 0, 0, 100))
    img = Image.alpha_composite(img, overlay)

    # cola o cachorro em cima
    img.paste(dog, (dog_x, dog_y), dog)

    # opcional: brilho suave no cachorro (pode comentar)
    # enhancer = ImageEnhance.Brightness(img)
    # img = enhancer.enhance(1.02 if (f % 10) < 5 else 0.98)

    # converte para RGB para reduzir tamanho
    frames.append(img.convert("P", palette=Image.ADAPTIVE))

# salva GIF
imageio.mimsave(OUT_NAME, frames, duration=0.06)  # duration em segundos por frame
print(f"GIF gerado: {OUT_NAME} ({len(frames)} frames)")


<!--
**Crislan-input/Crislan-input** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
