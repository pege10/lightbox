# Lightbox – DSLR film szkennelés

Egyoldalas progresszív web app (PWA), ami a telefon kijelzőjét átvilágító paddá
(lightbox) alakítja analóg film DSLR-es szkenneléséhez. A képernyő nem alszik el,
a menü 3 másodperc tétlenség után eltűnik, így a kijelző teljesen homogén világító
felület marad.

## Funkciók

- **Kijelző ébrentartása** – Screen Wake Lock API, automatikus újrakérés, ha a telefon
  visszatér az apphoz. A státuszsor mutatja, sikerült-e.
- **Teljes képernyő** – Fullscreen API gombbal. iPhone-on a Safari nem támogatja;
  ott a főképernyőre kitett ikonról indítva eleve sávok nélkül nyílik meg.
- **Színprofilok**
  - Fekete-fehér: tiszta fehér, RGB 255 / 255 / 255
  - Színes negatív: hideg ciánkék, RGB 181 / 219 / 255 – ellensúlyozza a film narancs maszkját
  - Dia: enyhén meleg fehér, RGB 255 / 245 / 224
- **RGB finomhangolás** – három csúszka, 0–100%, mellette a tényleges 0–255 érték,
  hogy egy beállítás később pontosan visszaállítható legyen.
- **Auto-hide** – 3 mp tétlenség után a teljes UI kihalványodik; bármilyen érintésre
  azonnal visszajön. Rejtett menü mellett az első koppintás csak előhozza a menüt,
  gombot nem nyom meg.
- **Offline** – service worker gyorsítótárazza az appot, telepítés után net nélkül is megy.

A beállítások a telefonon maradnak (localStorage), újranyitáskor visszajönnek.

## Telepítés iPhone-ra

Nyisd meg a linket Safariban, majd Megosztás → **Főképernyőhöz adás**.
Onnantól ikonról indul, böngészősáv nélkül, offline is.

## Szkenneléshez

- Kapcsold ki az **Automatikus fényerőt**, a **True Tone-t** és a **Night Shiftet**
  (Beállítások → Kijelző és fényerő, illetve Kisegítő lehetőségek → Kijelző és szövegméret),
  különben a fehér színhőmérséklete menet közben elcsúszik.
- A rendszerfényerőt vidd maximumra, a fényerőt az RGB csúszkákkal szabályozd,
  így a színegyensúly nem változik.
- Ha az ébrentartás nem elérhető (iOS 16.4 alatt), állítsd az Automatikus zárolást „Soha”-ra.

## Fájlok

- `index.html` – az egész app egyetlen fájlban (HTML + CSS + JS)
- `manifest.webmanifest` – PWA manifest
- `sw.js` – service worker, offline gyorsítótár
- `icons/` – app ikonok

## Hosztolás

GitHub Pages, a `main` branch gyökeréből. Minden útvonal relatív, így alkönyvtárból is működik.
