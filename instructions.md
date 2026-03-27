# NorgeSMP – Nettside instruksjoner
## For Google Antigravity / GitHub Pages

---

## Oversikt

Lag en komplett, produksjonsklar nettside for **NorgeSMP** – en norsk Minecraft-server.
Nettsiden er hostet på **GitHub Pages** og skal fungere uten noe backend.
Alt innhold skal være på **norsk**. Designet skal være **mørkt (dark mode)**, minimalistisk og rent, med subtile Minecraft-inspirerte detaljer.

---

## Tekniske krav

- Ren **HTML + CSS + JavaScript** (ingen build-steg, ingen Node.js – GitHub Pages serverer statiske filer direkte)
- Én `index.html` fil med inline CSS og JS er greit, eller separate filer i samme repo
- All ekstern data hentes via **fetch()** mot offentlige read-only API-er (API-nøkler i kildekoden er OK siden det kun er read-access)
- Mobilresponsiv

---

## Server-API (live data på forsiden)

Bruk **mcsrvstat.us API** – gratis, ingen nøkkel nødvendig, read-only:

```
GET https://api.mcsrvstat.us/3/norgesmp.no
```

Responsen inneholder:
- `online` (boolean) – om serveren er oppe
- `players.online` (number) – antall spillere online nå
- `players.max` (number) – maks spillere

Eksempel fetch:
```javascript
async function fetchServerStatus() {
  try {
    const res = await fetch('https://api.mcsrvstat.us/3/norgesmp.no');
    const data = await res.json();
    if (data.online) {
      document.getElementById('status').textContent = '🟢 Online';
      document.getElementById('players').textContent = `${data.players.online} / ${data.players.max} spillere`;
    } else {
      document.getElementById('status').textContent = '🔴 Offline';
      document.getElementById('players').textContent = '–';
    }
  } catch {
    document.getElementById('status').textContent = '⚫ Ukjent';
  }
}
fetchServerStatus();
```

Oppdater hvert 60. sekund:
```javascript
setInterval(fetchServerStatus, 60000)
```

---

## Seksjoner / sider

Lag nettsiden som én scrollbar single-page med disse seksjonene (bruk anchor-lenker i navigasjonen):

### 1. Navigasjonsbar (fast/sticky øverst)
- Logo / servernavn: **NorgeSMP**
- Lenker: `Hjem` · `Om serveren` · `Regler` · `Hvordan koble til` · `Butikk`
- **Butikk-knappen** skal vise teksten "Kommer snart" (WIP) – deaktiver klikk og gi den et tydelig WIP-utseende (f.eks. grå, stiplet kant, eller liten "🚧"-badge)
- Plasser en **"Støtt oss"-knapp** øverst til høyre som lenker til: `https://buymeacoffee.com/emilianovec`

### 2. Hero-seksjon (forside)
- Stort servernavn: **NorgeSMP**
- Undertittel: *"Norges egen Minecraft-server"* eller noe lignende
- Live server-status vises her (se API-seksjon over):
  - Server-IP: `norgesmp.no` (kopierbar ved klikk)
  - Status: Online/Offline-indikator
  - Antall spillere online akkurat nå
- En stor "Koble til"-knapp som scroller ned til "Hvordan koble til"-seksjonen

### 3. Om serveren
Tekst du kan fylle inn selv, men foreslå en placeholder som:
> *"NorgeSMP er en norsk Minecraft-survival-server for alle som vil spille med andre nordmenn. Vi kjører Paper 1.21.1 med et balansert økonomi-system, vennlig community og stabile ytelser."*

### 4. Regler
Lag en ren, lesbar liste med standard SMP-regler som placeholder. Eksempel:
1. Vær respektfull mot alle spillere
2. Ingen griefing eller stjeling
3. Ingen jukseprogrammer eller hacks
4. Ikke spam i chatten
5. Følg instruksjonene fra admins og moderatorer
6. Ingen rasistiske, hatefulle eller støtende ytringer

*(Brukeren fyller inn egne regler her)*

### 5. Hvordan koble til
Steg-for-steg guide:
1. Åpne Minecraft Java Edition
2. Gå til **Multiplayer**
3. Trykk **Legg til server**
4. Skriv inn IP: `norgesmp.no`
5. Trykk **Ferdig** og koble til!

Vis IP-en stort og klikkbar/kopierbar (copy-to-clipboard ved klikk med en liten ✓-animasjon).
Anbefalt versjon: **Minecraft Java 1.21.1**

### 6. Footer
- © NorgeSMP 2025
- Lenke til Buy Me a Coffee igjen

---

## Donasjonsbanner (popup)

Vis et diskret banner/toast **nederst på skjermen** første gang brukeren besøker siden (og igjen ved refresh). Bruk **ikke** `localStorage` for å huske det – vis det hver gang siden lastes.

Banneret skal:
- Dukke opp etter **2–3 sekunder** (ikke umiddelbart)
- Inneholde tekst som:
  > *"☕ Liker du serveren? Vi setter stor pris på en liten donasjon – det koster mye å drifte en Minecraft-server på eget strømnett og hardware. Hver krone hjelper! 🙏"*
- Ha en **"Støtt oss"-knapp** som åpner `https://buymeacoffee.com/emilianovec` i ny fane
- Ha en **"✕ Lukk"-knapp** for å avvise banneret
- Glide inn fra bunnen med en CSS-animasjon
- Ikke blokkere innholdet (ikke en modal overlay)

---

## Design og stil

### Fargepalett (CSS-variabler)
```css
:root {
  --bg-primary: #0f0f0f;       /* Nesten svart bakgrunn */
  --bg-secondary: #1a1a1a;     /* Kort/seksjon bakgrunn */
  --bg-tertiary: #242424;      /* Hover-states, kanter */
  --accent: #4ade80;           /* Minecraft-grønn (gress) */
  --accent-dim: #166534;       /* Mørkere grønn for hover */
  --text-primary: #f0f0f0;     /* Hvit tekst */
  --text-secondary: #a0a0a0;   /* Grå undertekst */
  --danger: #ef4444;           /* Offline-indikator */
  --border: #2a2a2a;           /* Subtile kanter */
}
```

### Typografi
- **Overskrifter**: `'VT323'` fra Google Fonts – pixelert Minecraft-lignende font
- **Brødtekst**: `'IBM Plex Mono'` eller `'Inconsolata'` fra Google Fonts – lesbar monospace som gir et server-terminal-preg
- Importer begge fra Google Fonts i `<head>`

### Detaljer
- Legg til en subtil **grønn venstre-kant** (`border-left: 3px solid var(--accent)`) på seksjons-overskrifter for å gi et Minecraft "GUI"-preg
- Knapper skal ha **ingen border-radius** (firkantede kanter, Minecraft-stil) eller veldig liten (2px)
- Server-IP-boksen kan ligne på en Minecraft-serverinput: mørk bakgrunn, grønn kant, monospace font
- Bruk en subtil **pixel/noise-tekstur** som bakgrunn-overlay (CSS `background-image` med en SVG noise-filter eller et lite base64-encoded noise-mønster) for å gi dybde uten å være distraherende
- Seksjoner skilles med en tynn `1px solid var(--border)` linje

---

## Filstruktur (GitHub Pages)

```
norgesmp-website/
├── index.html          ← Hele nettsiden
├── style.css           ← (valgfritt, kan være inline)
├── script.js           ← (valgfritt, kan være inline)
└── README.md
```

Det enkleste for GitHub Pages er å ha alt i `index.html`.
Aktiver GitHub Pages på `main`-branchen fra repo-innstillingene.

---

## Oppsummering av lenker

| Formål | URL |
|--------|-----|
| Server IP | `norgesmp.no` |
| Donasjon | `https://buymeacoffee.com/emilianovec` |
| Server status API | `https://api.mcsrvstat.us/3/norgesmp.no` |
| Nettside | `https://norgesmp.no` (GitHub Pages / eget domene) |

---

## Notater til AI

- Siden er **kun norsk** – all UI-tekst, knapper, feilmeldinger og placeholder-innhold skal være på norsk
- Bruk `target="_blank" rel="noopener noreferrer"` på alle eksterne lenker
- Butikk-seksjonen skal **ikke** lages ennå – bare en deaktivert knapp i navbaren med WIP-indikator
- API-kallet til mcsrvstat.us er en offentlig read-only tjeneste, ingen autentisering nødvendig
- Vis en lasteindikator (f.eks. "Laster...") på server-status mens API-kallet pågår
- Siden trenger **ingen cookies, ingen localStorage, ingen tracking**
