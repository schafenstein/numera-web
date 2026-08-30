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

## Kontaktdaten — seit 2026-08-31 eingetragen

Die GROSSBUCHSTABEN-Platzhalter sind **weg**; `grep -rn "EINTRAGEN" web/` findet nichts
mehr. Eingetragen sind (auf Nutzerangabe): **André Schäfer**, Sonnenstraße 10,
66917 Wallhalben, **schafensteinapp@posteo.com**.

Die Mailadresse ist eine **eigene App-Adresse**, nicht das private Postfach — sie steht
öffentlich auf drei Seiten und wird von Spam-Sammlern abgegriffen, das trifft so nur ein
Postfach, das dafür da ist. (Am 31.08. stand dort kurz die private Adresse; sie ist noch
am selben Tag ersetzt worden.)

⚠️ **Die Angaben stehen öffentlich im Netz**, auch die Anschrift. Wer etwas ändert, ändert
es an **vier** Stellen: Fuß jeder Seite (Name), `support.html` (Mail), `impressum.html`
(alles) und `datenschutz.html` unter „Verantwortlich" (alles) — Mail und Anschrift jeweils
zweimal je Datei (Linktext und `mailto:`). Die Anschrift im Impressum und die unter
„Verantwortlich" sind **dieselbe** und gehören zusammen gepflegt. Zum Prüfen:

```bash
grep -rn "posteo\|Sonnenstraße\|André" web/*.html
```

**Impressum: bleibt** (Nutzerentscheid 2026-08-24). Die Frage war, ob eine kostenlose App
ohne Einnahmen als privat oder geschäftsmäßig gilt — der Grenzfall ist zugunsten des
Impressums entschieden. Das ist keine Rechtsberatung.

⚠️ **Die Datenschutzseite spricht in der ICH-Form** (seit 31.08.): sie sagt „ich" statt
„der Anbieter dieser App", seit der Verantwortliche namentlich dort steht. Wer einen
Abschnitt ergänzt, bleibt dabei — ein Text, der zwischen beidem wechselt, liest sich, als
wären es zwei verschiedene Personen.

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
