# Lightbox – DSLR film szkennelés

Egyoldalas progresszív web app (PWA), ami a telefon kijelzőjét átvilágító paddá
(lightbox) alakítja analóg film DSLR-es szkenneléséhez. A képernyő nem alszik el,
a menü 3 másodperc tétlenség után eltűnik, így a kijelző teljesen homogén világító
felület marad.

## Funkciók

- **Kijelző ébrentartása** – két réteg egyszerre: Screen Wake Lock API (iOS 16.4+),
  és mellette egy néma, 3 px-es, hurokban futó videó, ami akkor is ébren tartja a
  kijelzőt, ha a Wake Lock nem elérhető vagy elbukik. Mindkettő újraindul, ha az app
  visszakerül előtérbe, érintésre, és 10 másodpercenként ellenőrizve is.
- **Teljes képernyő** – Fullscreen API gombbal. iPhone-on a Safari nem támogatja;
  ott a főképernyőre kitett ikonról indítva eleve sávok nélkül nyílik meg.
- **Színprofilok**
  - Fekete-fehér: tiszta fehér, RGB 255 / 255 / 255
  - Színes negatív: hideg ciánkék, RGB 181 / 219 / 255 – ellensúlyozza a film narancs maszkját
  - Dia: enyhén meleg fehér, RGB 255 / 245 / 224
- **Fényerő** – közös csúszka 5–100%, alapból maximumon. Arányosan skálázza mind a három
  csatornát, így a színegyensúly nem változik. (A kijelző hardveres fényerejét egy weblap
  nem tudja állítani, ez a megjelenített színt halványítja.)
- **RGB finomhangolás** – három csúszka, 0–100%, mellette a fényerővel együtt kiszámolt
  tényleges 0–255 érték, hogy egy beállítás később pontosan visszaállítható legyen.
- **Auto-hide** – 3 mp tétlenség után a teljes UI kihalványodik; bármilyen érintésre
  azonnal visszajön. Rejtett menü mellett az első koppintás csak előhozza a menüt,
  gombot nem nyom meg.
- **Offline** – service worker gyorsítótárazza az appot, telepítés után net nélkül is megy.
- **Diagnosztika** – a panel alján látszik a megjelenítési mód (standalone / browser),
  a felső biztonsági zóna magassága, a Wake Lock és a videós tartalék állapota, az
  ablak- és képernyőméret, valamint a build verziója.

A beállítások a telefonon maradnak (localStorage), újranyitáskor visszajönnek.

## Telepítés iPhone-ra

Nyisd meg a linket Safariban, majd Megosztás → **Főképernyőhöz adás**.
Onnantól ikonról indul, böngészősáv nélkül, offline is.

## A felső státuszsáv (óra, térerő, akku)

iOS nem ad rá API-t, hogy egy webapp elrejtse a státuszsávot – ezt csak natív app tudja.
Amit az app megtesz:

- `apple-mobile-web-app-status-bar-style: black-translucent` – a tartalom a státuszsáv alá
  fut, nincs külön sáv, és a sáv ikonjai világosak;
- nincs `theme-color`, hogy semmi ne írja felül ezt a viselkedést;
- a manifestben `"display": "fullscreen"` (`display_override`-fal) – Androidon ettől tényleg
  eltűnik a sáv, iOS jelenleg standalone-ként kezeli.

A panel alján lévő diagnosztika megmondja, mi a helyzet: ha a **Mód** nem `standalone`,
akkor az app böngésző módban fut (ilyenkor mindig van sáv); ha a **felső biztonsági zóna 0px**,
akkor az iOS külön sávterületet tart fenn, tehát a `black-translucent` nem érvényesült.

Ha iOS-en így is látszik az óra és a térerő, a bevált megoldás az **Irányított hozzáférés**
(Beállítások → Kisegítő lehetőségek → Irányított hozzáférés). Bekapcsolás után az appban
az oldalgomb háromszori nyomására indul, ilyenkor a rendszer elrejti a státuszsávot, és
egyben az érintéseket is letiltja, tehát szkennelés közben nem lehet véletlenül kilépni.

Fontos: iOS a manifestet a **főképernyőhöz adás pillanatában** olvassa be. Ha frissült az
app, töröld a főképernyőről az ikont, és add hozzá újra, különben a régi beállításokkal indul.

## Szkenneléshez

- Kapcsold ki az **Automatikus fényerőt**, a **True Tone-t** és a **Night Shiftet**
  (Beállítások → Kijelző és fényerő, illetve Kisegítő lehetőségek → Kijelző és szövegméret),
  különben a fehér színhőmérséklete menet közben elcsúszik.
- A rendszerfényerőt vidd maximumra, a fényerőt az RGB csúszkákkal szabályozd,
  így a színegyensúly nem változik.
- Ha a kijelző mégis elalszik: kapcsold ki az **Alacsony fogyasztású módot** (az lerövidíti
  az automatikus zárolást), és ellenőrizd a panel alján a diagnosztikát – ott látszik,
  hogy a Wake Lock aktív-e, illetve fut-e a videós tartalék.
- Végső megoldásként a Beállítások → Kijelző és fényerő → Automatikus zárolás → **Soha**.

## Fájlok

- `index.html` – az egész app egyetlen fájlban (HTML + CSS + JS)
- `manifest.webmanifest` – PWA manifest
- `sw.js` – service worker, offline gyorsítótár
- `icons/` – app ikonok

## Hosztolás

GitHub Pages, a `main` branch gyökeréből. Minden útvonal relatív, így alkönyvtárból is működik.
