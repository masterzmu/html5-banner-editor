# HTML5-bannerieditori

Työkalu IAB-standardien mukaisten HTML5-display-mainosten luomiseen selaimessa. Ei vaadi asennusta, npm:iä eikä ulkoisia palveluita — pelkkä HTML-tiedosto.

## Avaa paikallisesti

### Vaihtoehto 1: Suora file://-avaus
1. Avaa `index.html` kaksoisklikkaamalla tiedostoa selaimessa.
2. Tai käytä komentoriviä:
   ```bash
   open index.html
   ```

### Vaihtoehto 2: Paikallinen palvelin (suositeltu)
```bash
# Python 3:
cd ~/Documents/html5-banner-editor/
python3 -m http.server 8000

# Avaa sitten http://localhost:8000
```

Tai Node.js:
```bash
npx serve .
```

## Julkaise GitHub Pagesiin

1. Luo uusi repo GitHubissa (esim. `banner-editor`).
2. Pushaa tiedostot:
   ```bash
   git init
   git add .
   git commit -m "Initial: bannerieditori"
   git branch -M main
   git remote add origin https://github.com/KÄYTTÄJÄTUNNUS/banner-editor.git
   git push -u origin main
   ```
3. Mene repo → Settings → Pages → Source: `main branch /docs folder` (tai `/` root).
4. Sivusto on saatavilla osoitteessa `https://KÄYTTÄJÄTUNNUS.github.io/banner-editor/`.

## Käyttöohje

1. **Valitse koko** vasemmasta valikosta: 300×250, 728×90, 160×600, 320×50, 300×600 tai 970×250.
2. **Määritä sisältö**: brändi, otsikko, alaotsikko, hyödyt, hinta, CTA-teksti ja URL-osoite.
3. **Säädä värejä**: otsikon akcenttiväri, CTA:n tausta- ja tekstimväri.
4. **Valitse tausta**: yksivärinen, CSS-asteikko (kahta väriä) tai oma kuva.
   - **1:1-neliökuva suositellaan** — sama kuva cropataan `object-fit: cover` / `object-position: center` -tilassa kaikkiin IAB-kokoihin (ei venytystä).
   - Kuvat tallennetaan automaattisesti data-URI-muodossa vieissä HTML-tiedostoissa.
   - Esimerkkikuva (auringonlaskun hiekkadyynit) löytyy projektista (`assets/desert-golden.png`).
5. **Säädä hämärtyystä** (scrim): päälle/pois ja hämärtyksen voimakkuus.
6. **Animaatiot**: päälle/pois, syklin kesto 4–20 sekuntia.
7. **Vie**:
   - **Vie mainos**: luo itsestään riippumaton `.html`-tiedosto (tarkka IAB-koko, meta `ad.size`, koko-yksikkö-linkki).
   - **Vie esikatselu**: luo HTML-tiedosto, joka näyttää bannerin skaalattuna mihin tahansa näkymään. Sisältää bannerin markkinoinnin sisällään (ei iframe viereiseen tiedostoon).


## Size-adaptive layout + 1:1 cover

Bannerin sisältö adaptoituu kiinteisiin IAB-kokoihin CSS-luokilla `.size-WxH` (ja `data-size="WxH"` exportissa):

| Koko | Moodi |
|------|--------|
| 300×250, 300×600 | Pystystack (brand → center → price+CTA) |
| 728×90, 970×250 | Vaakasuuntainen (brand \| headline \| price+CTA) |
| 160×600 | Kapea pystystack, CTA täysleveä |
| 320×50 | Ultra-kompakti vaakasuuntainen (perks/legal piilotettu) |

Taustakuva: yksi data-URI → `<img class="bg-img">` absoluuttisesti `inset:0` + `object-fit:cover` — cover-crop jokaiseen kokoon ilman distortionia.

## Esimerkit

Kansiossa `examples/` on valmis AurinkoMatkat-esimerkki (inspiroitu AurinkoMatkat_Greece.html):

- `banner_300x250_AurinkoMatkat.html` — itse mainos 300×250
- `banner_300x250_AurinkoMatkat_preview.html` — esikatselusivu, jossa mainos skaalautuu näkymään

## Rakenteen esikuva

```
html5-banner-editor/
├── index.html              # Editori (Finnish UI)
├── css/                    # (Inline CSS index.html:ssä)
├── js/                     # (Inline JS index.html:ssä)
├── assets/
│   └── desert-golden.png   # Esimerkkikuva (auringonlasku)
├── examples/
│   ├── banner_300x250_AurinkoMatkat.html
│   └── banner_300x250_AurinkoMatkat_preview.html
└── README.md               # Tämä tiedosto
```

## Tekniset tiedot

- **Ei ulkoisia riippuvuujia**: kaikki CSS ja JS on sisällytetty.
- **Järjestelmäfontit**: fontit `system-ui, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif`.
- **Tarkka IAB-koko**: mainos on aina tiukasti valittu koko (300×250, 728×90 jne.).
- **Data-URI**: ladatut kuvat koodataan automaattisesti base64:ksi vieissä tiedostoissa.
- **Cover-crop**: kuvatausta käyttää `object-fit: cover` (ei venytystä eri IAB-koissa).
- **Size-adaptive DOM/CSS**: `.size-300x250` … `.size-970x250` + `data-size`.
- **Animaatiot**: CSS `@keyframes` — staggered fade ja slide-up -liikkeet, 8–12s luuppi.
- **`<meta name="ad.size">`**: automaattisesti asetettu vieissä tiedostoissa.
- **Koko-yksikkö-linkki**: koko banneri on `<a>`-elementti.

## Tekijänoikeudet

Kaikki esimerkit on merkitty **kuvitteellisiksi demoiksi** (`Kuvitteellinen demo`). Tuotannossa käytettävissä mainoksissa poista tämä huomio ja lisää oikeat tiedot.
