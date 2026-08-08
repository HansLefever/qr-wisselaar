# QR-code met 3 wisselende afbeeldingen + Firebase teller

## Wat doet dit?

Er is één vaste QR-code die naar je GitHub Pages URL verwijst.

Elke keer dat de URL wordt geopend:

1. De pagina leest de centrale teller in Firebase Firestore.
2. De teller wordt veilig met 1 verhoogd via een Firestore-transactie.
3. Scan 1 toont afbeelding 1.
4. Scan 2 toont afbeelding 2.
5. Scan 3 toont afbeelding 3.
6. Scan 4 toont opnieuw afbeelding 1.
7. Enzovoort.

De teller is centraal. Het maakt dus niet uit welke gsm scant.

---

## Stap 1 — Firebase project maken

1. Ga naar https://console.firebase.google.com/
2. Kies "Create a project".
3. Geef het bijvoorbeeld de naam: `de-mol-qr`
4. Google Analytics is voor dit project niet nodig.

---

## Stap 2 — Firestore activeren

1. Open je Firebase-project.
2. Ga naar **Build > Firestore Database**.
3. Kies **Create database**.
4. Kies bij voorkeur een Europese locatie.
5. Start in **Production mode**.

---

## Stap 3 — Teller maken

Maak handmatig:

Collection:
`qrGame`

Document ID:
`main`

Field:
`count`

Type:
`number`

Value:
`0`

Dus:

qrGame
└── main
    └── count: 0

BELANGRIJK:
Gebruik exact de namen `qrGame`, `main` en `count`.

---

## Stap 4 — Security Rules plaatsen

Open in Firebase:

**Firestore Database > Rules**

Vervang de bestaande regels door de inhoud van:

`firestore.rules`

Klik daarna op **Publish**.

Deze regels laten alleen toe dat de bestaande teller exact met 1 wordt verhoogd.
Andere Firestore-documenten blijven geblokkeerd.

---

## Stap 5 — Web-app registreren

1. Firebase Console > tandwiel > **Project settings**
2. Bij **Your apps** kies je het `</>` Web-icoon.
3. Geef de app bijvoorbeeld de naam `QR spel`.
4. Firebase Hosting hoef je NIET te activeren.
5. Na registratie krijg je een blok met ongeveer:

const firebaseConfig = {
  apiKey: "...",
  authDomain: "...",
  projectId: "...",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "..."
};

Kopieer die waarden naar `index.html`.

Zoek in index.html naar:

`const firebaseConfig = {`

en vervang de tijdelijke waarden.

De Firebase webconfig mag in client-side JavaScript staan; de beveiliging wordt bepaald door je Firestore Security Rules.

---

## Stap 6 — Eigen afbeeldingen plaatsen

In de map:

`images/`

staan:

- afbeelding1.png
- afbeelding2.png
- afbeelding3.png

Vervang deze drie bestanden door je eigen afbeeldingen en behoud dezelfde bestandsnamen.

Volgorde:

scan 1  -> afbeelding1.png
scan 2  -> afbeelding2.png
scan 3  -> afbeelding3.png
scan 4  -> afbeelding1.png
scan 5  -> afbeelding2.png
scan 6  -> afbeelding3.png

---

## Stap 7 — Op GitHub zetten

Bijvoorbeeld repository:

`qr-wisselaar`

Plaats de volledige inhoud van deze map in de repository.

Daarna:

1. GitHub repository > **Settings**
2. **Pages**
3. Source: **Deploy from a branch**
4. Branch: `main`
5. Folder: `/ (root)`
6. Save

Je URL wordt dan bijvoorbeeld:

`https://JOUWNAAM.github.io/qr-wisselaar/`

Dat is de URL die in de QR-code moet komen.

---

## Teller opnieuw op nul zetten

Firebase Console:

Firestore Database > Data > qrGame > main

Wijzig:

`count: 27`

bijvoorbeeld terug naar:

`count: 0`

De volgende scan toont dan opnieuw afbeelding 1.

Je kunt ook een andere startpositie instellen:

count = 0 -> volgende scan = afbeelding 1
count = 1 -> volgende scan = afbeelding 2
count = 2 -> volgende scan = afbeelding 3

---

## Testen

Open de GitHub Pages URL op:

- gsm A -> afbeelding 1
- gsm B -> afbeelding 2
- gsm C -> afbeelding 3
- gsm D -> afbeelding 1

Test bij voorkeur ook twee bijna gelijktijdige scans.

---

## Belangrijk verschil tussen "scan" en "pagina openen"

Technisch telt Firebase elke keer dat de URL wordt geopend.

Dus ook:
- QR-code scannen
- browser verversen
- URL handmatig opnieuw openen

verhoogt de teller.

Voor een spel is dit meestal precies bruikbaar. Als deelnemers niet mogen kunnen valsspelen door te verversen, is een extra server-side controle of scan-token nodig.

---

## QR-code maken

Maak de QR-code pas wanneer de definitieve GitHub Pages URL bekend is.

De QR-code bevat alleen die URL. De QR-code zelf hoeft daarna nooit meer te veranderen.
