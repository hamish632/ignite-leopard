# ignite-leopard
from pathlib import Path
import shutil, zipfile

site = Path("/mnt/data/ignite-leopard-final-site")
assets = site / "assets"
assets.mkdir(parents=True, exist_ok=True)

src = Path("/mnt/data/Screenshot 2026-08-12 at 11.00.12 pm(2).png")
dst = assets / "xe-sport.png"
shutil.copy2(src, dst)

index = site / "index.html"
html = index.read_text(encoding="utf-8")

# XE Sport card now uses the newly uploaded image.
old = '<article class="model"><div class="num">01 — XE SPORT</div><h3>XE Sport</h3><div class="mission">Light trail / entry performance</div><img src="assets/pro-s.png"'
new = '<article class="model"><div class="num">01 — XE SPORT</div><h3>XE Sport</h3><div class="mission">Light trail / entry performance</div><img src="assets/xe-sport.png"'
html = html.replace(old, new)

index.write_text(html, encoding="utf-8")

zip_path = Path("/mnt/data/ignite-leopard-final-website-v7.zip")
with zipfile.ZipFile(zip_path, "w", zipfile.ZIP_DEFLATED) as z:
    for p in site.rglob("*"):
        if p.is_file():
            z.write(p, p.relative_to(site))

print(zip_path)
