# MATES – statický web (verze 3)

Web matematické korespondenční soutěže MATES pro žáky 6. a 7. tříd,
organizovaný studenty Gymnázia Polička.

**Pro editory webu je tu soubor [`NAVOD.md`](NAVOD.md)** – návod, jak přidávat
posty, série, měnit fotky a texty. Veškerý obsah se edituje v jednom souboru:
`obsah.js`.

**Jak web nasadit (GitHub Pages / Cloudflare / Raspberry Pi)?** → [`DEPLOY.md`](DEPLOY.md)

## Jak to funguje

```
index.html … o-soutezi.html   Kostry stránek (menu, hlavička, patička)
obsah.js                      VŠECHEN OBSAH – posty, série, archiv, texty,
                              kontakt, fotky (editují ho editoři webu)
main.js                       Načte obsah.js a vykreslí ho do stránek
style.css                     Vzhled (barvy, písma, rozložení)
NAVOD.md                      Návod pro editory (česky, krok za krokem)
assets/
  images/                     Fotky (back1.webp, back2.webp; originály .jpg)
  pdf/                        PDF zadání a výsledky
```

Stránky se při otevření samy sestaví z `obsah.js` – nic se negeneruje,
není potřeba žádný build, funguje to na statickém nginxu i otevřením
souboru z disku. Pokud `obsah.js` chybí nebo je v něm chyba, stránky
zobrazí červený chybový pruh místo prázdné stránky (viz NAVOD.md).

## Nasazení

Doporučená varianta je **GitHub Pages** – zdarma, HTTPS automaticky
(dokonce i s vlastní doménou) a editování postů tužkou přímo na GitHubu.
Alternativy: Cloudflare Pages (zdarma, CDN), Raspberry Pi Zero WH doma.
Všechny postupy krok za krokem: [`DEPLOY.md`](DEPLOY.md).

## Editovací režim (skrytý, zaheslovaný)

Web obsahuje skrytý editovací režim, kterým editoři přidají aktualitu
přímo v prohlížeči a stáhnou aktualizovaný `obsah.js` (nahraje se na server).

- Otevření: 5× rychle stisknout klávesu `E` nebo adresa s `#editor`.
- Výchozí heslo: `mates2026` (změna — viz `editor.js` a NAVOD.md).
- Heslo se ukládá jako SHA-256 hash (čistý JS, funguje i na obyčejném http).
- Stažený soubor se generuje z původního `obsah.js` vložením nového bloku
  (zachovávají se komentáře); když se soubor nepodaří načíst (např. otevření
  ze souboru), použije se záložní generování z paměti bez komentářů.

⚠ Bezpečnostní upozornění: web je statický, ochrana heslem je proto jen
orientační zábrana, ne skutečné zabezpečení. Kdokoli si může přečíst kód
v prohlížeči. Pro skutečné zabezpečení by bylo potřeba backend (např.
PHP skript na Raspberry Pi, který by heslo ověřoval a soubor ukládal
na serveru).

## Změny oproti verzi 2

- Veškerý obsah je centralizovaný v `obsah.js` – přidávání postů, sérií
  a změna fotek je otázka jednoho souboru (určeno pro editory 15–18 let).
- Sdílené `style.css` + `main.js` místo duplikovaného inline CSS/JS
  v každé stránce.
- Obsah vychází z původního webu matesgympolicka.tode.cz (vzkaz týmu,
  příběh Mat a Tes, matematické dopoledne, soustředění Balda, archiv ročníků).
- Vizuál: zachovaný motiv tabule a pracovních listů, bez generických
  template efektů (glassmorphism, scroll-reveal animace, Inter).

Původní verze (inline CSS v každé stránce) je zálohovaná ve složce
`../backup-site-v2/` vedle této složky.
