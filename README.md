![deheadline](https://github.com/user-attachments/assets/e4423579-3784-40d0-a083-6c394dd95b42)

from PIL import Image, ImageDraw, ImageFont
import os
import random

w = 980
h = 220
frames = 48
bg = (8, 10, 16)
fg = (233, 238, 252)

os.makedirs("assets", exist_ok=True)

title = "DataDestroyerLab"
tag = "Breaking systems   Building tools   Shipping chaos responsibly"

ascii_lines = [
"   ____        _        ____            _                     _           _     ",
"  |  _ \\  __ _| |_ __ _|  _ \\  ___  ___| |_ _ __ ___  _   _  | |    __ _ | |__  ",
"  | | | |/ _` | __/ _` | | | |/ _ \\/ __| __| '__/ _ \\| | | | | |   / _` || '_ \\ ",
"  | |_| | (_| | || (_| | |_| |  __/\\__ \\ |_| | | (_) | |_| | | |__| (_| || |_) |",
"  |____/ \\__,_|\\__\\__,_|____/ \\___||___/\\__|_|  \\___/ \\__, | |_____\\__,_||_.__/ ",
"                                                      |___/                     ",
]

try:
    font = ImageFont.truetype("DejaVuSansMono.ttf", 16)
    font_big = ImageFont.truetype("DejaVuSansMono.ttf", 22)
except:
    font = ImageFont.load_default()
    font_big = ImageFont.load_default()

def jitter(s, strength):
    out = []
    for ch in s:
        if ch == " ":
            out.append(" ")
            continue
        if random.random() < strength:
            out.append(random.choice(["#", "@", "%", "&", "*", "+", "X"]))
        else:
            out.append(ch)
    return "".join(out)

imgs = []
for i in range(frames):
    im = Image.new("RGB", (w, h), bg)
    d = ImageDraw.Draw(im)

    glow = (70, 140, 255) if (i // 6) % 2 == 0 else (255, 80, 200)

    d.text((26, 14), title, font=font_big, fill=glow)
    d.text((26, 46), tag, font=font, fill=(180, 190, 220))

    y = 78
    strength = 0.03 + 0.02 * (i % 6 == 0)
    for line in ascii_lines:
        line2 = jitter(line, strength)
        x_shift = random.randint(0, 2) if i % 8 == 0 else 0
        d.text((22 + x_shift, y), line2, font=font, fill=fg)
        y += 20

    if i % 10 == 0:
        for _ in range(6):
            yb = random.randint(76, h - 20)
            d.rectangle([20, yb, w - 20, yb + 2], fill=(40, 55, 95))

    imgs.append(im)

out_path = os.path.join("assets", "ascii.gif")
imgs[0].save(
    out_path,
    save_all=True,
    append_images=imgs[1:],
    duration=70,
    loop=0,
    optimize=False
)

print("made", out_path)

</h4>

