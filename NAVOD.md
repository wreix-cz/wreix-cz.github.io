# Jak upravovat web MATES

Ahoj! Tady je všechno, co potřebuješ vědět, když chceš na webu něco změnit.
**90 % změn se dělá v jednom jediném souboru: `obsah.js`.**

---

## Co kde je

| Soubor | K čemu je | Měníš ho? |
|---|---|---|
| `obsah.js` | **VŠECHEN obsah** – posty, série, archiv, texty, fotky | ✅ Ano, skoro pořád |
| `index.html`, `aktualni-zadani.html`, … | kostra stránek (menu, hlavička) | ❌ Ne, jen výjimečně |
| `style.css` | barvy, písma, vzhled | ❌ Ne, jen když měníš design |
| `main.js` | skripty, vykreslování | ❌ Ne, nikdy |

Stránky se negenerují – obsah z `obsah.js` se načte sám. Po každé změně
stačí v prohlížeči zmáčknout F5.

---

## 1. Přidat nový příspěvek na úvodní stránku

Na úvodní stránce se ukazují 3 nejnovější příspěvky. Všechny příspěvky
najdeš na stránce `vsechny-prispevky.html`.

1. Otevři `obsah.js` (klikni pravým → Otevřít v Poznámkovém bloku / VS Code).
2. Najdi sekci `PŘÍSPĚVKY` (hledej slovo `aktuality:`).
3. Zkopíruj tenhle blok:

```js
{
  tag: "Novinka",
  datum: "1. 9. 2026",
  nadpis: "Nadpis vaší zprávy",
  text: "Krátký popis, 2–3 věty.",
  odkaz: "assets/pdf/2026-2027/vysledky.pdf",
  odkazText: "Stáhnout výsledky"
},
```

4. Vlož ho hned za `aktuality: [` (na začátek seznamu).
5. Dopln svoje údaje, ulož, obnov stránku. Hotovo.

**Tipy:**
- `odkaz` + `odkazText` = tlačítko ke stažení pod textem. Bez tlačítka? Napiš `odkaz: ""`.
- Za každým blokem musí být čárka – i za posledním nevadí.

---

## 2. Změnit fotku / pozadí

1. Nahraj fotku do `assets/images/` jako **WebP** (převedeš na [squoosh.app](https://squoosh.app) nebo v GIMPu).
2. V `obsah.js` najdi sekci `VZHLED` (`vzhled:`).
3. Změň název souboru:

```js
vzhled: {
  heroFotka: "assets/images/back1.webp",
  strankyFotka: "assets/images/back2.webp",
  poznamka: "Řešení zasílejte do termínu! ;)"
},
```

Pozor: cesta musí končit na `.webp`. Když nahraješ `.jpg` a necháš `.webp`, fotka se nezobrazí.

---

## 3. Nahrát novou sérii (Aktuální zadání)

1. Nahraj PDF do `assets/pdf/` (klidně do podsložky podle ročníku).
2. V `obsah.js` najdi `aktualniSerie:`.
3. Uprav:

```js
aktualniSerie: {
  label: "4. série",
  nadpis: "… 4. série 2025/2026",
  termin: "Termín odevzdání: 30. 4. 2026",
  popis: "Text pro řešitele…",
  pdfNazev: "4-serie-2025-2026.pdf",
  pdf: "assets/pdf/2025-2026/4-serie.pdf",
  jakOdevzdat: "Řešení zasílejte … {email} …"
},
```

4. Ulož a obnov stránku Aktuální zadání.

---

## 4. Doplnit archiv

1. V `obsah.js` najdi `archiv:`.
2. Každý ročník vypadá takhle:

```js
{
  rocnik: "2025/2026",
  serie: [
    { nazev: "1. série", pdf: "assets/pdf/2025-2026/1-serie.pdf" },
    { nazev: "2. série", pdf: "" }   // prázdné = řádek "brzy"
  ]
},
```

3. Přidej řádek nebo doplň cestu k PDF. Ulož, obnov.

**Vzorová řešení (od ročníku 2025/2026):** nahraj soubor do
`assets/pdf/<ročník>/vzorove/` a přidej pole `vzorove`:

```js
{ nazev: "1. série", pdf: "assets/pdf/2025-2026/1-serie.pdf",
  vzorove: "assets/pdf/2025-2026/vzorove/vzorove-1-serie.pdf" }
```

---

## 5. Změnit texty, kontakt, čísla

- **Kontakt** (e-mail, kontaktní osoba, telefon, škola, adresa, ročník) → sekce `KONTAKT` v `obsah.js`. Telefon se v patičce sám udělá klikatý.
- **Text o soutěži** → sekce `O SOUTĚŽI` (`oNas:`). Odstavce odděl prázdným řádkem.
- **Čísla pod titulkem** → sekce `ÚVODNÍ STRÁNKA` (`hero:`).

---

## 6. Vyhledávání

Vyhledávání se otevírá lupou v menu. Na mobilu je přímo v hamburger menu.
Hledá v příspěvcích, archivu i textech O soutěži (diakritika nevadí).
Nic se v `obsah.js` nenastavuje — funguje to samo.

---

## 7. Editovací režim přímo na webu

Na webu je schovaný editovací režim:

1. Otevři úvodní stránku a 5× rychle stiskni E (nebo přidej `#editor` do adresy).
2. Zadej heslo. Výchozí: `mates2026`.
3. Vyplň formulář a klikni Přidat aktualitu — post se hned objeví.
4. Klikni Stáhnout obsah.js a nahraj ho na server.

**Změna hesla:** v editovacím režimu rozbal „Změnit heslo", zadej nové heslo,
nech vygenerovat hash. Ten pak v `editor.js` nahraď za `HESLO_HASH = "..."`.

---

## 8. Záhlaví a zápatí

Texty v patičce (e-mail, telefon) se mění v `obsah.js` (sekce `KONTAKT`).

Struktura menu a patičky je v každém HTML souboru zvlášť.
Když přidáváš/ubíráš stránku, nezapomeň ji přidat i do ostatních.

---

## 9. Jak to nahrát

Viz [DEPLOY.md](DEPLOY.md) — tam je postup krok za krokem pro GitHub Pages,
Cloudflare Pages i Raspberry Pi.
