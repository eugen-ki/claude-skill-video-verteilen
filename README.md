# Video verteilen: ein Video, fünf Plattformen

Ein Claude-Code-Skill, der aus einem eigenen Video passende Texte für YouTube Shorts, Instagram Reels, TikTok, LinkedIn und X macht, für jede Plattform eigene. Auf Wunsch plant er die Posts über Blotato ein. Er postet nie ohne dein ausdrückliches Ja.

## Was du bekommst

- Eigene Texte je Plattform: Titel, Caption, Hashtags.
- Standard: nur die Texte zum Kopieren.
- Optional: Einplanen über Blotato, frühestens 2 Stunden in der Zukunft. So kannst du jeden Post vorher noch prüfen oder löschen.
- Nur für deine eigenen Videos.

## Installieren

1. Oben rechts auf **Code** und dann **Download ZIP** klicken. ZIP entpacken.
2. Im entpackten Ordner liegt der Ordner `video-verteilen`. Kopiere genau diesen Ordner in deinen Skills-Ordner:
   - Windows: `C:\Users\<dein-name>\.claude\skills\`
   - Mac: `~/.claude/skills/`

   Gibt es den Ordner `skills` noch nicht, leg ihn an. Danach liegt die Datei unter `.claude/skills/video-verteilen/SKILL.md`.
3. Claude Code neu starten.

## So benutzt du ihn

Tippe `/video-verteilen` oder sag es einfach in deinen Worten, zum Beispiel:

> Verteil mein Reel auf YouTube, Instagram und LinkedIn. Es liegt unter C:\Users\ich\Videos\reel.mp4.

## Tipp: zuerst dein Profil

Der Skill klingt nach dir, wenn du vorher einmal [mein-profil](https://github.com/eugen-ki/claude-skill-mein-profil) installierst und `/mein-profil` startest. Ohne Profil funktioniert er auch, er stellt dir dann ein paar kurze Fragen.

## Was du brauchst

- [Claude Code](https://claude.com/claude-code).
- Nur zum Einplanen: ein [Blotato](https://blotato.com)-Konto mit verbundenen Social-Media-Konten. Die Verbindung erklärt dir der Skill Schritt für Schritt, mit Anmeldung im Browser und ohne API-Schlüssel in Dateien.

## Gut zu wissen

- Der Skill erfindet keine Zahlen oder Ergebnisse. Was er nicht weiß, steht in `[eckigen Klammern]`. Füll diese Stellen vor dem Veröffentlichen mit echten Angaben.
- Das Ergebnis speichert er als Datei in dem Ordner, in dem du Claude Code gestartet hast.

## Lizenz

MIT, frei nutzbar und veränderbar. Siehe [LICENSE](LICENSE).
