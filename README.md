# web/ — die Numera-Website

Vier statische Seiten, kein Build-Schritt, keine Abhängigkeiten, nichts wird von außen
nachgeladen (keine Schriften, kein CDN, kein Analytics). Damit läuft das auf jedem Webserver,
auf GitHub Pages, auf Netlify — überall gleich.

```
index.html        Startseite
support.html      Support + FAQ   → die Support-URL für App Store Connect
datenschutz.html  Datenschutz     → die Datenschutz-URL für App Store Connect
impressum.html    Vorlage, siehe unten
style.css         die Farbtokens der App (Theme.swift)
img/              drei Screenshots, auf 386px Breite verkleinert (dargestellt bei 210px)
```

## ⚠️ Vor dem Hochladen ausfüllen

Die Platzhalter stehen absichtlich in GROSSBUCHSTABEN, damit sie nicht versehentlich
online gehen. Alle finden:

```bash
grep -rn "EINTRAGEN" web/
```

- **`MAILADRESSE@EINTRAGEN`** — die Kontaktadresse. Steht in `support.html`,
  `datenschutz.html` und `impressum.html`. Überlege, ob du dafür eine eigene Adresse
  nimmst: sie steht öffentlich im Netz und wird von Spam-Sammlern gefunden.
- **`NAME EINTRAGEN`** — im Copyright-Fuß jeder Seite und als Verantwortlicher im
  Datenschutz.
- **`ANSCHRIFT EINTRAGEN`** / die Adresszeilen im Impressum.

**Impressum: bleibt** (Nutzerentscheid 2026-08-24). Die Frage war, ob eine kostenlose App
ohne Einnahmen als privat oder geschäftsmäßig gilt — der Grenzfall ist zugunsten des
Impressums entschieden. `impressum.html` und die drei Fußzeilen-Links bleiben also stehen;
die Anschrift dort und die unter „Verantwortlich" in `datenschutz.html` sind **dieselbe** und
gehören zusammen ausgefüllt. Das ist keine Rechtsberatung.

## Hochladen auf einen eigenen Server

```bash
rsync -avz --delete web/ user@server:/var/www/numly/
```

nginx-Block, falls du einen brauchst:

```nginx
server {
    listen 443 ssl http2;
    server_name numly.example;

    root /var/www/numly;
    index index.html;

    # Saubere URLs: /support statt /support.html
    location / {
        try_files $uri $uri.html $uri/ =404;
    }
}
```

Für TLS ist `certbot --nginx -d numly.example` der übliche Weg. Ohne HTTPS akzeptiert Apple
die URLs zwar meist trotzdem, aber eine Datenschutzseite ohne TLS ist kein guter Auftritt.

## Alternative: GitHub Pages

Aus dem privaten Hauptrepo braucht Pages einen bezahlten Plan. Der kostenlose Weg ist ein
zweites, **öffentliches** Repo, in das nur der Inhalt von `web/` kommt:

```bash
cd web && git init && git add -A && git commit -m "Numera website"
gh repo create numly-web --public --source=. --push
gh api -X POST repos/:owner/numly-web/pages -f source[branch]=main -f source[path]=/
```

Die URL ist dann `https://<user>.github.io/numly-web/` — für App Store Connect völlig
ausreichend.

## Screenshots erneuern

`img/` enthält verkleinerte Kopien aus `release/screenshots/iphone-6.9/` (die liegen im Git,
erzeugt von `tools/screenshot-pass.sh`):

```bash
sips -Z 386 release/screenshots/iphone-6.9/01-home.png       --out web/img/home.png
sips -Z 386 release/screenshots/iphone-6.9/04-ueben.png      --out web/img/play.png
sips -Z 386 release/screenshots/iphone-6.9/05-fortschritt.png --out web/img/progress.png
```

⚠️ **Diese Befehle standen bis zum 24.08. auf `build/appstore/` und auf einer Datei namens
`03a-play-timeattack.png`** — einem Ordner, der nicht mehr gefüllt wird, und einem Spielmodus,
den es seit Monaten nicht mehr gibt. Wer sie kopierte, bekam einen Fehler statt eines Bildes.

⚠️ **Die Seiten driften still — sie werden von nichts gebaut und von nichts getestet.** Am
21.08. standen hier „25 Module" und „Time Attack". **Wer Modulnamen, Modi oder Zählungen
ändert, greppt `web/` mit.**

## Örtlich anschauen

```bash
python3 -m http.server -d web 8000
```
