# Jak web MATES nasadit (deploy)

Web je **statický** — žádný server, žádná databáze. Jsou to jen soubory,
které se sestaví samy z `obsah.js`. Proto ho může hostovat cokoli, co
umí posílat soubory přes HTTPS. Níže jsou tři cesty — od nejjednodušší
po „udělej si sám“.

| | GitHub Pages | **Cloudflare Pages** ⭐ | Raspberry Pi doma |
|---|---|---|---|
| Cena | 0 Kč navždy | 0 Kč navždy | 0 Kč + elektrika |
| HTTPS | ✅ automaticky | ✅ automaticky | nastavit ručně (Let's Encrypt) |
| Security headers | ❌ jen meta tagy | ✅ plné HTTP hlavičky (_headers) | nastavit v nginx |
| CDN | ✅ GitHub | ✅ globální CDN | ❌ jen lokálně |
| Údržba | žádná | žádná | aktualizace, běh doma |
| Editování postů | tužka na GitHubu | tužka na GitHubu | SSH / FTP |
| Riziko výpadku | prakticky žádné | prakticky žádné | vypadne domácí internet/proud |

**Doporučení: Cloudflare Pages** (Možnost B). Je to zdarma, rychlejší než
GitHub Pages (CDN), umí vlastní HTTP hlavičky pro zabezpečení (CSP, HSTS)
a nasazuje se přes GitHub — stejně snadno jako GitHub Pages. GitHub Pages
(Možnost A) funguje taky, ale nepodporuje custom HTTP headers.

---

## Možnost A — GitHub Pages (doporučeno)

### 1. Repozitář
1. Založ si účet na [github.com](https://github.com) (nebo použij existující).
2. Klikni **New repository** → název např. `mates` → **Public** → **Create repository**.
   (Public je důležité — Pages pro soukromé repozitáře funguje jen s placeným účtem;
   po aktivaci GitHub Education Packu ti to nevadí ani u soukromého.)

   💡 **Na Edu Pack nemusíš čekat** — web rozjedeš hned na Public repozitáři
a úplně zadarmo. Až se Pack aktivuje (za ~2 dny), můžeš repozitář přepnout
na Private (GitHub Pro to umí) a sjednat si první doménu zdarma.
3. **Add file → Upload files** → přetáhni **všechny soubory a složky z `site/`**
   do okna → **Commit changes**.

   ⚠️ Soubory musí být **v kořeni repozitáře** (ne v podsložce):
   `index.html`, `obsah.js`, `main.js`, `editor.js`, `style.css`,
   všechny `*.html` a celá složka `assets/`. Všechny cesty na webu jsou
   relativní, takže to pak funguje na jakékoli adrese.

### 2. Zapnout Pages
1. V repozitáři jdi do **Settings → Pages**.
2. **Source: Deploy from a branch** → branch `main` → složka `/ (root)` → **Save**.
3. Po minutě je web na adrese `https://tvuj-ucet.github.io/mates/` — s **HTTPS automaticky**.

### 3. Editování (to nejlepší)
- **Způsob 1 — tužka na GitHubu (nejjednodušší):** otevři `obsah.js` na github.com
  → ikona tužky (pencil) → uprav podle NAVOD.md → **Commit changes**.
  Za necelou minutu je web aktualizovaný. Postupy z NAVOD.md fungují stejně —
  jen místo „ulož soubor“ uděláš Commit.
- **Způsob 2 — bez psaní kódu:** na webu 5× stiskni `E` (nebo přidej `#editor`
  do adresy), přidej post, **Stáhnout obsah.js**, pak stažený soubor přetáhni
  na github.com přes **Add file → Upload files** (nahradí starý) → Commit.
- Když někdo rozbije syntaxi v `obsah.js`, web nezhasne — nahoře se objeví
  červený pruh (stejně jako u lokální verze). Stačí opravit poslední řádek.

### 4. Vlastní doména (až bude budget / Edu Pack)
1. Kdekoli kup doménu (Edu Pack dává první rok zdarma — např. `.me`, `.tech`).
2. V repozitáři **Settings → Pages → Custom domain** → zadej doménu → **Save**.
3. U poskytovatele domény vytvoř **CNAME** záznam na `tvuj-ucet.github.io`.
   HTTPS zůstává zdarma (certifikát vydá GitHub automaticky).

> GitHub Education Pack (aktivuje se za ~2 dny): GitHub Pages je zdarma i bez něj,
> ale Pack přidá GitHub **Pro** (soukromé repozitáře, Actions) a hlavně kredity,
> ze kterých se dá koupit první doména — viz krok 4.

---

## Možnost B — Cloudflare Pages (doporučeno)

Cloudflare Pages je zdarma, rychlejší než GitHub Pages (CDN po celém světě)
a **umí vlastní HTTP hlavičky** (`_headers` soubor) — tím pádem má web
plné zabezpečení (CSP, HSTS, X-Frame-Options) bez placeného hostingu.

### 1. Připoj GitHub repozitář
1. Jdi na [dash.cloudflare.com](https://dash.cloudflare.com) → přihlas se
   (nebo si založ účet — je zdarma).
2. V levém menu klikni **Workers & Pages**.
3. Klikni **Create application → Pages → Connect to Git**.
4. Vyber GitHub účet → autorizuj Cloudflare → vyber repozitář **`mates-web`**.
5. Nastavení buildu:
   - **Production branch:** `main`
   - **Build command:** (ponech prázdné — žádný build)
   - **Build output directory:** `/` (kořen)
6. Klikni **Save and Deploy**.

Po ~30 vteřinách je web na adrese `https://tvuj-projekt.pages.dev`.

### 2. Vlastní doména (matesgympolicka.me)
1. V Cloudflare dashboardu jdi **Websites → + Add site**.
2. Zadej doménu `matesgympolicka.me` → klikni **Add site**.
3. Cloudflare ti ukáče 2 nameservery (např. `anna.ns.cloudflare.com`…).
4. Jdi k poskytovateli domény (kde ji máš kupenou) a **nahrad nameservery**
   za ty od Cloudflare. (Na Endoře/Hostingeru: DNS → Nameservery → Změnit.)
5. Počkej ~15 minut (max. 24h), než se změna propaguje.
6. V Cloudflare dashboardu jdi zpátky na **Workers & Pages → tvůj projekt**
   → **Custom domains → Set up a custom domain**.
7. Zadej `matesgympolicka.me` → Cloudflare ověří DNS a automaticky
   vydá HTTPS certifikát.

### 3. Přesměrování z www (volitelné)
Pokud chceš, aby `www.matesgympolicka.me` přesměrovalo na `matesgympolicka.me`:
1. V Cloudflare dashboardu jdi na doménu → **Rules → Configuration Rules**.
2. Vytvoř pravidlo: `www.matesgympolicka.me/*` → **Dynamic URL redirect**
   na `https://matesgympolicka.me/${1}` (status 301).

### 4. Bezpečnostní hlavičky
Soubor `_headers` už je součástí webu — Cloudflare Pages ho automaticky
načte a nastaví HTTP hlavičky (CSP, HSTS, X-Frame-Options atd.).
Nic dalšího nastavovat nemusíš.

### 5. Editování (stejné jako GitHub Pages)
- **Tužka na GitHubu:** otevři `obsah.js` → ikona tužky → uprav → Commit.
  Za ~30 sekund je web aktualizovaný (Cloudflare automaticky deployne z GitHubu).
- **Editor na webu:** 5× stiskni `E` → heslo → přidej post → Stáhnout obsah.js
  → nahraj na GitHub (Add file → Upload files → Commit).

---

## Možnost C — Raspberry Pi Zero WH (self-hosting doma)

Dá se to, ale počítej s tím, co to obnáší.

### Co budeš potřebovat
- **Veřejnou IP adresu** doma (zavolej ISP, ať ti ji nezablokuje).
- **Port forwarding** na routeru: porty `80` a `443` → IP Raspberry Pi.
- **Dynamickou DNS** (domácí IP se mění): zdarma např. [DuckDNS](https://duckdns.org)
  → založíš si `mojedomena.duckdns.org` a na routeru/Pi nastavíš update skript.
- **Let's Encrypt certifikát** pro HTTPS (funguje i na DuckDNS doménách).

### Postup
```bash
# na Raspberry Pi (Raspberry Pi OS Lite)
sudo apt update && sudo apt install -y nginx certbot python3-certbot-nginx

# nahraj soubory webu (tuto složku) do /var/www/mates/
# např. přes scp:  scp -r site/* pi@ip:/var/www/mates/

# nginx konfigurace (jednoduchá; soubor nginx-mates.conf už ve webu není)
sudo tee /etc/nginx/sites-available/mates > /dev/null <<'EOF'
server {
    listen 80;
    server_name _;
    root /var/www/mates;
    index index.html;
}
EOF
sudo ln -s /etc/nginx/sites-available/mates /etc/nginx/sites-enabled/mates
sudo nginx -t && sudo systemctl reload nginx

# HTTPS (doména musí mířit na tvoji veřejnou IP!)
sudo certbot --nginx -d mojedomena.duckdns.org
```

### Upozornění
- Web stojí, když vypadne domácí **internet nebo proud** — pro školní web je
  to riziko. Nikdo ti to neopraví za tebe.
- Pi Zero WH je pomalé, ale pro statické soubory to stačí (nginx to zvládne).
- Musíš hlídat **bezpečnostní aktualizace** (`sudo apt upgrade`).
- Let's Encrypt certifikát se sám obnovuje, ale jen když je Pi zapnuté.
- Editování probíhá přes SSH (rsync/scp) nebo editovací režim na webu
  (Stáhnout obsah.js → nahrát). Není to tak pohodlné jako tužka na GitHubu.

### Šikovný hybrid (doporučeno, když už Pi máš)
Hlavní web běží na GitHub Pages, Pi si z GitHubu **sám tahá aktualizace**:
```bash
# na Pi: jednou nastav, pak každých 10 minut
sudo apt install -y git
mkdir -p /var/www/mates && cd /var/www/mates
git init && git remote add origin https://github.com/tvuj-ucet/mates.git
git pull origin main
# cron:  */10 * * * *  cd /var/www/mates && git pull -q origin main
```
Tak máš lokální kopii webu jako zálohu a testovací prostředí — bez ručního nahrávání.

---

## Rychlá kontrola po nasazení
- [ ] Otevře se úvodní stránka a vidí se hero s fotkou
- [ ] HTTPS zámek v adresním řádku
- [ ] `obsah.js` se načte (když chybí, je nahoře červený pruh)
- [ ] Fungují PDF odkazy v Archivu a na stránce Aktuální zadání
- [ ] Editovací režim: 5× `E` → heslo → přidat post → stáhnout obsah.js
- [ ] (Cloudflare) Security headers: zkontroluj na [securityheaders.com](https://securityheaders.com) — mělo by být A nebo A+
- [ ] (Cloudflare) Vlastní doména funguje přes HTTPS
