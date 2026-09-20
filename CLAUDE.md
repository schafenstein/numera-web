# Numera Website

Die vier statischen Seiten zur iOS-App **Numera** (Kopfrechentrainer, `~/Numera`).
Kein Build-Schritt, keine Abhängigkeiten, **nichts wird von außen nachgeladen** —
keine Schriften, kein CDN, kein Analytics. Läuft auf jedem Webserver gleich.

```
index.html        Startseite
support.html      Support + FAQ   → die Support-URL für App Store Connect
datenschutz.html  Datenschutz     → die Datenschutz-URL für App Store Connect
impressum.html    Impressum
style.css         die Farbtokens der App (aus Theme.swift übernommen)
img/              drei Screenshots, auf 386 px Breite verkleinert (dargestellt bei 210 px)
```

## Hosting

GitHub Pages aus diesem Repo (`schafenstein/numera-web`, **öffentlich** — Pages braucht aus
einem privaten Repo einen bezahlten Plan, deshalb liegt die Website getrennt von der App).

Live: https://schafenstein.github.io/numera-web/

Die zwei **Pflicht-URLs für App Store Connect** sind `…/support.html` und
`…/datenschutz.html`. Beide sind in ASC eingetragen und dürfen ihre Dateinamen nicht ändern.

## ⚠️ Die Website driftet still

**Kein Test, kein Compiler und kein Skript prüft diese vier Dateien.** Sie behaupten Dinge
über die App — Modulzahl, Modusnamen, Modullisten —, und niemand merkt, wenn sie falsch
werden. Belegte Fälle:

- 21.08.: „25 Module" (es waren 29), beworben wurde **„Time Attack gegen 60 Sekunden"** —
  ein Modus, der seit Monaten entfernt war; „binomische Formeln" und „Dualzahlen" als
  Modulnamen; die Datenschutzseite listete die **Wochen-Challenge**.
- Bis zur Repo-Trennung am 20.09. lag die Website **zweimal** vor (hier und als `web/`
  im App-Repo) und musste von Hand nachgezogen werden — sie lief dabei auseinander:
  live stand „29 Module", die Arbeitskopie sagte 45.

**Daraus die Regel: wer in der App Modulnamen, Modi oder Zählungen ändert, greppt dieses
Repo mit.** Und umgekehrt: dieses Repo ist seit dem 20.09. die *einzige* Fassung — es gibt
keine zweite mehr, die nachzuziehen wäre.

## Die Vorschaubilder

`img/` kommt aus `~/Numera/release/screenshots/iphone-6.9/` (`home` ← 01, `play` ← 04,
`progress` ← 05, auf 420 px Breite gerechnet). Sie veralten mit jedem Umbau der App genauso
still wie der Text.

## Inhaltliche Zusagen, die hier stehen

Die Seiten werben mit **„dauerhaft kostenlos, kein Abo, keine In-App-Käufe, keine Werbung,
ohne Konto, ohne Internetverbindung"** und die Datenschutzseite sagt **„keine
Datenerhebung"**. Das sind dieselben Zusagen wie im App Store und in der App selbst —
sie sind öffentlich, und sie zurückzunehmen wäre ein Wortbruch, keine Textänderung.
Die Datenschutzseite spricht durchgehend in der **Ich-Form** (ein namentlich genannter
Verantwortlicher plus „der Anbieter dieser App" läse sich wie zwei Personen).

Kontakt: **schafensteinapp@posteo.com** — eine eigene App-Adresse, nicht die private.
Sie steht in drei Dateien je zweimal (Linktext + `mailto:`) und muss dieselbe sein wie die
Support-Kontaktadresse im ASC-Formular.

## Offen

- **Der Live-Stand sagt „29 Module", der Repo-Stand 45.** Der nächste Push bringt die
  Seite auf 45 — das ist gewollt, aber es ist ein sichtbarer Sprung.
- Die Store-Texte der App bewerben die Daily als „TÄGLICHER ANKER", die Startseite hier
  nicht mehr (Straffung vom 31.08.). Kein Drift, sondern zwei Kanäle mit verschiedener
  Gewichtung — wer sie angleicht, entscheidet das bewusst.
