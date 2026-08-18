# Návod pro editory webu MATES

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

Stránky se **negenerují** – obsah z `obsah.js` se načte sám. Po každé změně
stačí v prohlížeči zmáčknout **F5** (obnovit stránku)n.

---

## 1. Jak přidat nový příspěvek na úvodní stránku

Na úvodní stránce se ukazují **3 nejnovější příspěvky**: první celý,
další dva jen s nadpisem a krátkým úryvkem (tlačítko „Rozbalit“ /
„Zabalit“). **Všechny příspěvky** najdou návštěvníci na stránce
`vsechny-prispevky.html` (klik na nadpis „Příspěvky“ na úvodní stránce,
nebo přes vyhledávání — lupa v menu).

1. Otevři soubor `obsah.js` (klikni pravým → Otevřít v Poznámkovém bloku / VS Code).
2. Najdi sekci `PŘÍSPĚVKY` (hledej slovo `aktuality:`).
3. Zkopíruj celý tento blok:

```js
{
  tag: "Novinka",
  datum: "1. 9. 2026",
  nadpis: "Nadpis vaší zprávy",
  text: "Krátký popis, 2–3 věty. Co se děje a proč to lidi zajímá.",
  odkaz: "assets/pdf/2026-2027/vysledky.pdf",
  odkazText: "Stáhnout výsledky"
},
```

4. Vlož ho **hned za `aktuality: [`** (na začátek seznamu).
5. Do uvozovek doplň svoje údaje.
6. Ulož soubor a obnov stránku. Hotovo! 🎉

**Tipy:**
- `odkaz` + `odkazText` = tlačítko ke stažení pod textem (např. „Stáhnout
  výsledky“). Textová tlačítka jako „Číst více“ už nepoužíváme.
- Nechceš tlačítko? Napiš `odkaz: ""` a `odkazText: ""`.
- **Důležité:** za každým blokem musí být čárka `,` – i za tím posledním nevadí.

## 2. Jak změnit fotku / pozadí

1. Nahraj fotku do složky `assets/images/` a ulož ji jako **WebP**
   (formát `.webp` – web se tak načítá rychleji). Fotku převedeš např.
   na [squoosh.app](https://squoosh.app) nebo v GIMPu (Exportovat jako… → WebP).
2. V `obsah.js` najdi sekci `VZHLED` (hledej `vzhled:`).
3. Změň název souboru:

```js
vzhled: {
  heroFotka: "assets/images/back1.webp",    // velká fotka na úvodní stránce
  strankyFotka: "assets/images/back2.webp", // jemná fotka na ostatních stránkách
  poznamka: "Řešení zasílejte do termínu! ;)" // ručně psaný vzkaz pod tlačítky
},
```

   > Staré fotky `back1.jpg`, `back2.jpg`, … jsou stále ve složce jako
   > originál. Na webu se používají pouze soubory `.webp` – proto musí
   > v `obsah.js` být cesta končící na `.webp`.
   > Pozor: když fotku nahraješ jako `.jpg` a cestu necháš `.webp`,
   > fotka se nezobrazí.

4. Ulož a obnov. Nechceš fotku? Napiš `""`.

## 3. Jak nahrát novou sérii (Aktuální zadání)

1. Nahraj PDF do `assets/pdf/` (klidně do podsložky podle ročníku).
2. V `obsah.js` najdi sekci `AKTUÁLNÍ ZADÁNÍ` (hledej `aktualniSerie:`).
3. Uprav:

```js
aktualniSerie: {
  label: "4. série",                    // která série to je
  nadpis: "… 4. série 2025/2026",       // celý název
  termin: "Termín odevzdání: 30. 4. 2026",
  popis: "Text pro řešitele…",
  pdfNazev: "4-serie-2025-2026.pdf",    // jméno souboru k zobrazení
  pdf: "assets/pdf/2025-2026/4-serie.pdf", // CESTA k PDF (důležité!)
  jakOdevzdat: "Řešení zasílejte … {email} …"
},
```

4. Ulož a obnov stránku Aktuální zadání.

## 4. Jak doplnit archiv

1. Najdi v `obsah.js` sekci `ARCHIV` (hledej `archiv:`).
2. Každý ročník vypadá takhle:

```js
{
  rocnik: "2025/2026",
  serie: [
    { nazev: "1. série", pdf: "assets/pdf/2025-2026/1-serie.pdf" },
    { nazev: "2. série", pdf: "" }   // prázdné pdf = řádek "brzy"
  ]
},
```

3. Přidej řádek pro novou sérii nebo doplň cestu k PDF.
4. Ulož a obnov.

**Vzorová řešení (od ročníku 2025/2026):** k sérii můžeš přidat i vzorové
řešení — nahraj soubor do podsložky `assets/pdf/<ročník>/vzorove/` a doplň
pole `vzorove`:

```js
{ nazev: "1. série", pdf: "assets/pdf/2025-2026/1-serie.pdf",
  vzorove: "assets/pdf/2025-2026/vzorove/vzorove-1-serie.pdf" }
```

Prázdné `vzorove: ""` = odkaz se nezobrazí.

## 5. Jak změnit texty, kontakt, čísla

- **Kontakt** (e-mail, kontaktní osoba, telefon, škola, adresa, ročník) → sekce `KONTAKT` v `obsah.js`.
  Mění se tím i patička a stránka O soutěži. Telefon se v patičce sám
  udělá klikatý — stačí přepsat číslo u položky `telefon:`.
- **Text o soutěži** → sekce `O SOUTĚŽI` (`oNas:`). Odstavce odděl prázdným řádkem.
- **Čísla pod titulkem** (série ročně, třída, rok vzniku) → sekce `ÚVODNÍ STRÁNKA` (`hero:`).

## 5d. Vyhledávání

Vyhledávání se otevírá **lupou v menu** (ikona nahoře vpravo) — funguje na
všech stránkách. Na mobilu je vyhledávací pole přímo v menu (hamburger).
Hledá v příspěvcích, archivu i textech O soutěži (diakritika nevadí —
„serie“ najde i „série“). Příspěvky vedou na stránku se všemi příspěvky
(`vsechny-prispevky.html`), slovo „příspěvky“ najde přímo tuto stránku.
Nic se v `obsah.js` nenastavuje — funguje to samo.

---

## 5b. Editovací režim přímo na webu (bez ručního kódu)

Na webu je schovaný editovací režim, kterým se dá přidat aktualita
bez otevírání obsah.js:

1. Otevři úvodní stránku webu a **5× rychle za sebou stiskni klávesu E**
   (nebo přidej na konec adresy `#editor`, třeba `index.html#editor`).
2. Zadej heslo. **Výchozí heslo je `mates2026`** — po prvním použití si ho
   změň (viz níže).
3. Vyplň formulář (štítek, datum, nadpis, text, odkaz) a klikni
   **Přidat aktualitu** — post se hned objeví na stránce.
4. Klikni **Stáhnout obsah.js** a stažený soubor nahraj na server
   (nahradí ten starý).

**Změna hesla:** v editovacím režimu rozbal sekci „Změnit heslo“, zadej
nové heslo a nech vygenerovat hash. Ten pak v souboru `editor.js`
nahraď na řádku `HESLO_HASH = "..."` a soubor nahraj na server.
Heslo jde vygenerovat i na Raspberry Pi: `echo -n "nove-heslo" | sha256sum`.

⚠ **Důležité:** web je statický, takže ochrana heslem je jen orientační
(zábrana pro návštěvníky). Kdokoli s technickými znalostmi si kód může
přečíst — pro pořádné zabezpečení by byl potřeba backend (viz README.md).

---

## 5c. Jak upravit záhlaví a zápatí (menu, patička)

**Texty v patičce (e-mail, kontaktní osoba, telefon) se mění v `obsah.js`**
(sekce `KONTAKT`) — stačí přepsat hodnotu a patička se na všech stránkách
změní sama. Telefon se sám udělá klikatý.

**Struktura záhlaví a zápatí je ale v každém HTML souboru zvlášť**
(`index.html`, `archiv.html`, `aktualni-zadani.html`, `o-soutezi.html`):

- **Záhlaví (menu, logo)** → blok `<header class="site-header">`.
  Položky menu jsou řádky `<li><a href="stranka.html">Název</a></li>`
  uvnitř seznamu — přidat odkaz = zkopírovat řádek a změnit název/odkaz.
- **Zápatí (sloupce, nadpisy, odkazy, logo školy)** → blok
  `<footer class="site-footer">`.
- **Vzhled** (barvy, velikosti, mezery) → `style.css` (třídy `.site-header`,
  `.nav__*`, `.site-footer`, `.footer-*`).

⚠ Změny HTML a CSS se dělají **ve všech 4 souborech**, protože každá stránka
má hlavičku i patičku vlastní. Když přidáváš/ubíráš stránku, nezapomeň na to.

---

## 6. Jak to nahrát na server (Raspberry Pi)

1. Kopíruj **vždy celou složku webu** – ne jen jednotlivé soubory!
   Soubor `obsah.js` MUSÍ být ve stejné složce jako HTML stránky,
   jinak se stránky načtou bez obsahu.
2. Přes FTP/SCP nahraj do `/var/www/mates/` (případně tam, kde je nginx nastavený).
3. Hotovo. Není potřeba nic restartovat.

## 7. Když se něco rozbije ⚠️

Nahoře na stránce se objeví **červený pruh** s hláškou. Nejčastější příčiny:

1. **Chybí čárka** za blokem v `obsah.js` → dopiš `,`.
2. **Chybí uvozovka** → každý text musí začínat a končit `"`.
3. **Chybí závorka** `}` nebo `]` → zkontroluj, že jsi nic nesmazal.
4. **Chybí soubor `obsah.js` na serveru** → nahraj celou složku (bod 6).

Nejjednodušší oprava: podívej se na řádek, který jsi naposledy měnil,
a porovnej ho s okolními řádky. Když si nevíš rady, vrať se k záloze
(`backup-site-v2/`) a zkopíruj původní soubor.

---

*Soubor `style.css` a HTML stránky upravuj jen tehdy, když víš, co děláš.
Obsah webu se mění v `obsah.js`.*
