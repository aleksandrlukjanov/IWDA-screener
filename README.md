# IWDA · 8-mėn. SMA screeneris

Gyvas, automatiškai atsinaujinantis timing screeneris IWDA (iShares Core MSCI World, EUR).
Dabartinė kaina lyginama su paskutinių 8 užbaigtų mėnesių uždarymo kainų vidurkiu:

- kaina **virš** SMA → **HOLD / BUY**
- kaina **žemiau** SMA → **SELL CORE / DCA**

## Failai

- `index.html` — pats screeneris (čia ir gyvena dizainas + DATA blokas).
- `build.py` — parsisiunčia šviežius duomenis ir perrašo DATA bloką į `site/index.html`.
- `.github/workflows/update.yml` — kasdien (darbo dienomis 18:00 UTC) paleidžia `build.py` ir perdeploy'ina.

---

## Greitas startas — be automatikos (1 min.)

Jei nori tiesiog paskelbti dabar:

1. Eik į **app.netlify.com/drop**.
2. Nutempk `index.html` į langą → gauni viešą HTTPS adresą.
3. Tą adresą įklijuoji į Substack (žr. žemiau).

Skaičius atnaujinsi rankiniu būdu — arba pačiame puslapyje (laukai redaguojami),
arba pakeisdamas `DATA` bloką `index.html` ir perkeldamas failą iš naujo.

---

## Automatinis atsinaujinimas — GitHub Pages

1. Sukurk **viešą** GitHub repo ir įkelk visus failus (išlaikant `.github/workflows/` struktūrą).
2. Repo **Settings → Pages → Build and deployment → Source: GitHub Actions**.
3. Eik į **Actions** skirtuką → pasirink *„Atnaujinti screenerį"* → **Run workflow** (pirmas paleidimas rankomis).
4. Po kelių sekundžių tavo adresas: `https://NAUDOTOJAS.github.io/REPO/`
5. Toliau viskas vyksta savaime — kiekvieną darbo dieną po biržos uždarymo skriptas
   atnaujina dabartinę kainą, o mėnesio gale į vidurkį įsirita naujas mėnuo.

> Duomenų šaltinis: `yfinance` (Yahoo, tikeris `IWDA.AS`). Jei kada Yahoo blokuotų,
> `build.py` galima nukreipti į kitą šaltinį (pvz. Stooq `iwda.nl` CSV) — logika ta pati.
> Norint **USD** versijos (Londono kotiruotė): `build.py` pakeisk `TICKER = "IWDA.L"`.

---

## Integracija į Substack

⚠️ Substack **nepalaiko** gyvo HTML/iframe įdėjimo įraše. Todėl integruojama per nuorodą:

**Variantas A — automatinė kortelė (rekomenduojama).**
Įraše naujoje eilutėje įklijuok savo adresą (`https://NAUDOTOJAS.github.io/REPO/`).
Substack pats pasitrauks `og:title` / `og:description` ir pavers gražia kortele, kurią paspaudus
screeneris atsidaro naujame skirtuke.

**Variantas B — mygtukas.**
Editoriaus įrankių juostoje pridėk **Button** elementą, įrašyk to paties adreso URL ir tekstą,
pvz. „Atidaryti IWDA screenerį".

**Kortelės paveikslėlis (nebūtina, bet gražiau).**
Įkelk į repo `preview.png` (1200×630) ir `index.html` head'e atkomentuok bei užpildyk:

```html
<meta property="og:image" content="https://NAUDOTOJAS.github.io/REPO/preview.png">
```

Taip pat `index.html` pakeisk `og:url` reikšmę į tikrą savo adresą.

---

## Atsakomybės atsisakymas

Tai edukacinė priemonė, paremta mechaniniu slankiojo vidurkio algoritmu. Tai nėra individuali
investavimo rekomendacija. Istoriniai rezultatai negarantuoja ateities grąžos; galima prarasti
dalį ar visą investuotą kapitalą.
