# Vahinkohallinta (vahingot)

Vakuutusmeklarin aineistonhallinta **AJarmo Oy · J Sailing Tmi:lle** — vahinkotapausten, asiakkaiden ja liitetiedostojen kirjaus. Nykyinen versio **v9.6**.

## Arkkitehtuuri — huom!

Sovellus **ei tallenna dataa mihinkään palvelimelle**. Kaikki asiakkaat, vahingot ja liitetiedostot tallentuvat pelkästään selaimen `localStorage`- ja `IndexedDB`-muistiin (kuvat IndexedDB:hen 5MB-rajan kiertämiseksi). Firebase on käytössä **vain kirjautumiseen** (Google-kirjautuminen, rajattu osoitteeseen `jacke.seilaa@gmail.com`, sama `jsailing-f716c`-projekti kuin muissakin tilin sovelluksissa) — ei Firestore-tallennusta.

Tämän vuoksi data on sidottu yhteen laitteeseen/selaimeen, ja mukana on kokonainen työkalupakki manuaaliseen varmuuskopiointiin ja siirtoon laitteiden välillä.

## Tietomalli

- **Asiakas**: nimi, y-tunnus, toimiala, kontakti, puhelin, sähköposti, muistiinpanot.
- **Vahinko** (per asiakas): päivämäärä, kohde (esim. rekisterinumero), tyyppi (Ajoneuvo-/Liikenne-/Omaisuus-/Vastuuvahinko/Muu), tila (Avoin/Käsittelyssä/Käsitelty/Arkistoitu), vahinko-/vakuutustunnus, muistiinpanot, liitteet (max 2 kpl / 200 KB, kuvat pakataan JPEG:ksi automaattisesti).

## Sivut

| Tiedosto | Tarkoitus |
|---|---|
| `index.html` | Pääsovellus: kirjautuminen → asiakasnäkymä (hakukortit, tilastot) → vahinkorekisteri per asiakas (suodatus, muokkaus, liitteiden raahaus, JSON-vienti/tuonti). Automaattitallennus 500 ms viiveellä. |
| `data-update.html` | Pikakorjauslomake yksittäisen vahingon tietojen muokkaamiseen ilman koko sovellusta. |
| `export-csv.html` | CSV-vienti (per asiakas / kaikki / yhteenveto / "vakuutusraportti" karkealla riskitason arviolla) — `;`-erotin + BOM, toimii ilman kirjastoja. |
| `export-excel.html` | Sama neljä raporttityyppiä oikeana `.xlsx`-tiedostona (SheetJS). |
| `import.html` | "Älykäs" JSON-palautus: raahaa/valitse tiedosto, validoi rakenteen, tilapalkki, tarjoaa vanhan datan tyhjennyksen jos muisti täynnä. |
| `import-no-files.html` | Kevyt JSON-tuonti ilman kuvia — iPadin localStorage-rajaa varten. |
| `storage-optimizer.html` | Näyttää localStorage-käytön, siirtää irralliset kuva-avaimet IndexedDB:hen, pakkaa vanhoja tietueita. |
| `sync-icloud.html` | Manuaalinen varmuuskopiointi/synkronointiohje iCloud Drive -kansion kautta laitteiden välillä (Mac/iPad/iPhone). Automaattinen iCloud-synkka merkitty "tulossa". |

## Kirjautuminen

Google-kirjautuminen (Firebase Auth), sallittu vain `jacke.seilaa@gmail.com`. Aiemmin (v9.5 asti) käytössä oli PIN-koodi, korvattiin v9.6:ssa.

## Deploy

GitHub Pages (branch-pohjainen, ei erillistä Actions-workflowia). Live: https://jackeseilaa.github.io/vahingot/
