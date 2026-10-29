# Gorski Kotar Info

Osobni blog o ljepoti, životu i događajima u Gorskom kotaru.

## Kako objaviti na GitHub Pages

1. Napravi novi public repozitorij na GitHubu (npr. `moj-gorski-kotar`).
2. Uploadaj sve datoteke iz ove mape u root repozitorija.
3. U Settings → Pages odaberi branch `main` i folder `/ (root)`.
4. Stranica će biti dostupna na `https://tvoje-ime.github.io/moj-gorski-kotar`.

## Kako botovi dodaju članke

Novi članci se dodaju kao Markdown datoteke u mapu `_posts/` u formatu:

```
YYYY-MM-DD-naslov-clanka.md
```

Primjer:

```markdown
---
layout: post
title: Naslov članka
date: 2026-10-22 11:40:00 +0200
location: Delnice
type: Događanja
categories: [Delnice, Događanja]
excerpt: Kratki opis...
image: /assets/images/naziv-slike.jpg
image_alt: "Opis slike"
image_credit: "Fotografija: Autor / [Izvor](https://...), licenca"
source: "[Naziv izvora](https://...)"
---

Tekst članka...
```

### Najave događaja (stranica Događaji)

Najava događaja koji tek slijedi dobiva i polja s datumom događaja:

```markdown
---
layout: post
title: Predstavljanje knjige u Delnicama
date: 2026-10-23 13:28:56 +0200
location: Delnice
type: Događanja
categories: [Delnice, Događanja]
event_date: 2026-10-23 17:00:00 +0200   # početak događaja (obavezno za najavu)
event_end: 2026-10-23 19:00:00 +0200    # kraj (neobavezno)
event_place: Gradska knjižnica Janet Majnarich, Delnice   # mjesto (neobavezno)
excerpt: ...
---
```

- Stranica `/dogadaji/` (izbornik **Događaji**) prikazuje samo najave koje još nisu prošle, najbliža prva. Događaj je prošao kad prođe `event_end`, a ako njega nema, 3 sata nakon `event_date`. Provjera se radi pri izradi stranice i u pregledniku, pa prošli događaji nestaju i bez novog pusha. Na samoj najavi tada piše „Ovaj događaj je prošao.“
- Izvještaji s već održanih događaja **nemaju** `event_date`.
- Kategorija Događanja (filteri, bočna traka, `/kategorije/`) i dalje prikazuje sve članke te vrste, i prošle i buduće.
- Stranica `/povijest/` (izbornik **Povijest**) prikazuje sve članke vrste Povijest i serijala POVIJEST GK, najnoviji prvi.
- **Stranice kategorija (`/kategorije/<slug>/`):** donji izbornik **Kategorije** vodi na `/kategorije/` – pločice svih mjesta, vrsta i serijala s brojem članaka i najnovijim naslovom. Svaka pločica otvara stranicu te kategorije (layout `_layouts/kategorija.html`, stub datoteke u `kategorije/<slug>.md`), npr. `/kategorije/mrkopalj/`, `/kategorije/sport/`. Na stranici: naslov, broj članaka, filtar-čipovi (na stranici mjesta po vrsti, na ostalima po mjestu), prva vijest veća, ostale kompaktne (mala slika lijevo), datum riječima (`_includes/datum-hr.html`). Prikazuje se **prvih 10 članaka**, a gumb **VIŠE** otkriva sljedećih 10 (bez ponovnog učitavanja); gumb nestaje kad više nema članaka, bez JavaScripta vide se svi. Popis je jedan zajednički include `_includes/clanci-lista.html` (`posts=`, `po=10`) + `assets/js/kategorije.js`; isti se koristi i na `/vijesti/` i `/povijest/`. Oznake mjesta/vrste u člancima vode na te stranice, a stare poveznice `/kategorije/#slug` automatski otvaraju `/kategorije/slug/`. Zaposlenje vodi na `/zaposlenje/`. **Novo mjesto ili vrstu** dodaj u `_data/kategorije.yml` i napravi `kategorije/<slug>.md` (kopiraj postojeći stub, promijeni `title`, `kategorija`, `grupa`, `permalink`); bez stuba pločica vodi na filtar naslovnice.
- **Mobilna naslovnica (< 900 px):** zaglavlje „GORSKI KOTAR INFO“ (logo lijevo, hamburger desno) i ispod njega vodoravni meni rubrika (chips: kategorijski blokovi, 💼 Zaposlenje, mjesta). Redoslijed: prijelomna traka (ako postoji) → glavna vijest (hero, ista logika: `featured: true` ili najnovija) → **Najnovije** (5 najnovijih, bez glavne vijesti i Zaposlenja) → oglasni banner → Izdvojeno → **kategorijski blokovi** → blok Zaposlenje → filteri → gumb „Prikaži sve članke“ (mreža svih članaka na mobitelu se prikazuje tek uz filter ili taj gumb). Blokovi se podešavaju u `_data/kategorije.yml` pod `naslovnica_blokovi` (naslov, id, `vrste` ili `ostale: true`, `vise` = poveznica „Više“): Vijesti (sve vrste koje nisu u drugim blokovima – sad Aktualno, Promet, Priroda; „Više“ → `/vijesti/`), Poslovi općine (Općinske vijesti), Zanimljivosti, Događanja (→ `/dogadaji/`), Sport, Crna kronika, Kultura, Društvo, Povijest GK (vrsta Povijest, → `/povijest/`); ostali „Više“ vode na naslovnicu s filterom `?kategorija=<slug>`. Svaki blok: 1 veća vijest + do 3 kompaktne; blok bez članaka se ne prikazuje, glavna vijest se ne ponavlja. Isti redoslijed imaju i gumbi u vodoravnom meniju. Desktop (≥ 900 px) je nepromijenjen.
- **Mali oglasi:** u `_data/oglasi.yml` oglas s `mali: true` (i `active: true`) prikazuje se kao kompaktna traka (logo ~56 px, naziv · mjesto, kratka rečenica `text`, oznaka „Oglas“; include `oglas-mali.html`). Na mobilnoj naslovnici stoji između kategorijskih blokova, iza bloka čiji je `id` u `mobilno_iza`; na desktopu i na ostalim stranicama u bočnoj traci – bez `sidebar` ispod velikog oglasa, a sa `sidebar: dolje` niže, ispod okvira „Mjesta“ (da mali oglasi ne stoje jedan uz drugi). `banner: false` ga drži izvan velikog vodoravnog bannera. Poveznica se otvara u novoj kartici (`rel="noopener sponsored"`). Trenutno: Caffe bar Tiffany Delnice → Instagram (mobilno iza Poslova općine, desktop ispod „Mjesta“; logo = profilna s Instagrama) i Shamrock Pub Ravna Gora → Instagram (mobilno iza Sporta, desktop ispod velikog oglasa).
- **Veliki oglasi:** oglas bez `mali` – okvir u bočnoj traci (`oglas-veliki.html`, opcionalno `foto` + `logo_sirok`) i vodoravni banner (`oglas-banner.html`). Gornji banner na naslovnici je prvi aktivni oglas bez `banner: false` (Goring). Veliki oglas s `banner: false` i `mobilno_iza` (Breza, Ravna Gora) na mobilnoj naslovnici dolazi kao banner među blokovima (iza Događanja), a na desktop naslovnici u bočnoj traci. Na ostalim stranicama bočna traka prikazuje jedan veliki oglas, izmjenično po stranici. Redoslijed na mobilnoj naslovnici: banner Goring → … Poslovi općine → Tiffany → Zanimljivosti → Događanja → Breza → Sport → Shamrock → Crna kronika.
- **Ankete:** u front matter dodaj `anketa:` (`id`, `pitanje`, `opcije`), a u tekst `{% include anketa.html %}`. Glasovi se broje anonimno preko besplatnih javnih brojača bez prijave i ključeva, podešenih u `_config.yml` pod `ankete:` – `servis` (glavni, trenutno countapi.mileshilliard.com, ključ `mojgk-<id>-o<broj odgovora>`) i `rezerva` (Abacus, abacus.jasoncameron.dev, ključ `mojgk.github.io/<id>-o<broj>`), uz `timeout_ms` 4000 ms (rezerva Abacus ima vlastiti `timeout_ms: 8000` jer odgovara sporo). Ako glavni servis ne odgovori na vrijeme, koristi se rezerva; ako ne rade oba, anketa ispiše „Glasanje trenutno nije dostupno, pokušajte kasnije.“ Jedan glas po pregledniku (localStorage pamti odgovor i servis). Brojevi s dva servisa se ne zbrajaju. Stari glasovi iz Abacusa (glavni servis do 28. 10. 2026.) mogu se pročitati, kad Abacus radi, s `https://abacus.jasoncameron.dev/get/mojgk.github.io/<id>-o<broj>` (npr. `.../demografija-gk-2026-o1`).
- **Prijelomna vijest:** `prijelomna: true` u front matteru prikazuje crvenu traku „⚠ PRIJELOMNA VIJEST“ s naslovom i poveznicom na vrhu naslovnice (odmah ispod zaglavlja, iznad glavne vijesti), crvenu oznaku iznad naslova na stranici članka i crvenu oznaku na kartici članka (Izdvojeno, mreža, hero, Najnovije). Ako više članaka ima zastavicu, traka prikazuje najnoviji. Neobavezno `prijelomna_do: 2026-10-28 19:00:00 +0100` – nakon tog trenutka preglednik sakrije traku i oznake, ali zastavicu (i eventualni `istaknuto: true`) treba ukloniti iz front mattera kad prođe rok. Za udarnu vijest u Izdvojenom uz to stavi `istaknuto: true` (udarna = najnoviji članak s `istaknuto: true` koji nije hero).
- **ZAPOSLENJE** je zasebna rubrika (`type: Zaposlenje`, stranica `/zaposlenje/`, izbornik **Zaposlenje**). Ti članci **ne ulaze** u opći tok vijesti (hero, Izdvojeno, mreža članaka bez filtera, Najnovije u bočnoj traci); na naslovnici ih zamjenjuje poseban blok **💼 Zaposlenje** s 1–2 izdvojena oglasa iz najnovijeg članka. Filter Zaposlenje i stranica `/kategorije/` i dalje ih prikazuju.
  - Naslov: „ZAPOSLENJE: <poslodavac> traži <istaknuto radno mjesto> u <mjesto> i još N oglasa u Gorskom kotaru“.
  - Front matter: `location: Gorski kotar`, `type: Zaposlenje`, `categories: [Gorski kotar, Zaposlenje]`, `broj_oglasa: N`, zajednička slika `/assets/images/zaposlenje-naslovna.jpg` (AI ilustracija, `image_credit: "Ilustracija: generirano umjetnom inteligencijom (AI)"`) i `istaknuti_oglasi:` – popis (1–2) s poljima `pozicija`, `poslodavac`, `mjesto`, `rok` (GGGG-MM-DD ili tekst), `link`, neobavezno `opis`. Iz toga se rade blok na naslovnici, okvir na vrhu članka i istaknuti oglasi na `/zaposlenje/`; oglas s prošlim rokom automatski se zasivi.
  - Svi oglasi idu u front matter kao lista `oglasi:` (polja: `poslodavac`, `pozicija`, `mjesto`, `rok` (GGGG-MM-DD ili tekst), `izvori` – popis `naziv` + `url`, prvi je gumb „Više / prijava”; neobavezno `broj`, `vrsta`, `oznaka` (fusnota uz rok, npr. `"*"`), `opis`). U tekstu članka na mjestu popisa stoji `{% include oglasi-lista.html %}`, koji generira kartice: poslodavac (istaknuto), radno mjesto (podebljano), mjesto, rok, broj radnika, ugovor/uvjeti i gumb na izvorni oglas. Mobitel: jedan stupac; ≥ 700 px: dva stupca. Oglas s prošlim rokom (datum) u pregledniku se zasivi s oznakom „Rok istekao“ (ne briše se – istekle oglase iz liste ukloni ručno i smanji `broj_oglasa` i broj u naslovu).
  - Tijelo: uvod, `{% include oglasi-lista.html %}`, fusnote (`\*`), popis izvora i napomena da se istekli oglasi uklanjaju. Predložak: `_drafts/predlozak-zaposlenje.md`.

### Datum i vrijeme

- Za svaki novi članak **uvijek upiši stvarno vrijeme objave** sa zonom: `date: YYYY-MM-DD HH:MM:SS +0200` (ljeti, CEST) ili `+0100` (zimi, CET). Dan mora odgovarati datumu u nazivu datoteke. Ne izmišljaj vrijeme.
- Prikazuje se kao „22. 10. 2026. u 11:40“, a u traci „Najnovije“ samo „11:40“ ako je objavljeno danas.
- Stariji članci bez poznatog vremena imaju samo datum (`date: YYYY-MM-DD`) i `show_time: false` – za njih se prikazuje samo „18. 10. 2026.“ (u traci „18. 10.“). Isto vrijedi za svaki datum s vremenom 00:00:00.
- Vrijeme ne smije biti u budućnosti – Jekyll inače preskače članak dok to vrijeme ne prođe.
- Glavni članak na naslovnici je najnoviji; za ručni odabir dodaj `featured: true` (samo jednom članku).

### Kategorije

- `location` – jedno mjesto: Delnice, Fužine, Lokve, Mrkopalj, Čabar, Skrad, Ravna Gora, Vrbovsko, Brod Moravice ili Gorski kotar (za cijelu regiju).
- `type` – jedna vrsta: Događanja, Crna kronika, Sport, Priroda, Promet, Kultura, Zanimljivosti, Povijest, Općinske vijesti (natječaji, javni pozivi, savjetovanja, prodaja/zakup čestica, komunalne naknade, prostorni planovi, proračun, odluke vijeća), Društvo (stanovništvo, demografija, iseljavanje, socijalne teme i analize) ili Zaposlenje (oglasi za posao – zasebna rubrika, vidi gore).
- `istaknuto: true` – (neobavezno) članak postaje **udarna vijest** u rubrici Izdvojeno na naslovnici (vrijedi najnoviji takav članak).
- `categories` – uvijek `[location, type]`, istim redom i istim nazivima.
- Serijal: članak ulazi u serijal (npr. `POVIJEST GK`) ako mu `categories` sadrži naziv serijala, npr. `categories: [Fužine, Povijest, POVIJEST GK]`. Članak se broji i u svakoj dodatnoj vrsti/mjestu navedenom u `categories`.
- Pripremljeni, još neobjavljeni članci serijala su u `_drafts/povijest-gk/` (vidi tamošnji README).
- Nazivi moraju točno odgovarati popisu u `_data/kategorije.yml` (s dijakriticima). Novo mjesto ili vrstu dodaj u tu datoteku (`name` + `slug` bez dijakritika) i odmah će se pojaviti u filterima, bočnoj traci i na stranici `/kategorije/` (za vlastitu stranicu kategorije dodaj i `kategorije/<slug>.md`).

Nakon pusha na GitHub, stranica se automatski ažurira.
