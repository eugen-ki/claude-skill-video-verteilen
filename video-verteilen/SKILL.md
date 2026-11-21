---
name: video-verteilen
description: Use when the member wants to reuse one of their own short videos on other platforms (Video verteilen, Repurpose, Crossposting, Reel auch auf YouTube Shorts, TikTok, LinkedIn oder X, Captions für mehrere Plattformen), with or without posting through Blotato.
---

# Video verteilen

Macht aus **einem eigenen Video** passende Texte für YouTube Shorts, Instagram Reels, TikTok, LinkedIn und X. Auf Wunsch plant es die Posts über Blotato ein.

Ohne Blotato funktioniert der Skill auch: Dann bekommt das Mitglied nur die fertigen Texte.

## Die drei Sicherheitsregeln

1. **Nur eigene Videos.** Videos anderer Leute werden nicht heruntergeladen und nicht neu gepostet.
2. **Nichts geht ohne ausdrückliches Ja raus.** Das Mitglied bestätigt Text, Plattformen und Art (B einplanen oder C sofort), bevor irgendetwas gepostet wird. "Schreib mir Texte" ist keine Erlaubnis zum Posten. "Passt alles" gilt als Ja zu allen gezeigten Texten. Sagt es dabei nicht, ob eingeplant oder sofort ("hau raus", "mach"), frag genau einmal: "Einplanen (wann?) oder sofort?"
3. **Standard ist "einplanen", nicht "sofort".** Geplante Posts liegen frühestens 2 Stunden in der Zukunft. So bleibt Zeit, sie im Blotato-Kalender zu prüfen oder zu löschen. Sofort posten nur, wenn das Mitglied es genau so sagt.

## Schritt 1: Was soll passieren? (eine Nachricht)

Lies `~/.claude/mein-profil.md` (Stimme, Zielgruppe, Plattformen, Standard-Aufruf). Frag dann zusammen:
- Wo liegt das Video? (Dateipfad auf dem PC, oder Link zu deinem eigenen Video)
- Auf welche Plattformen soll es? (Vorschlag: die Plattformen aus dem Profil)
- **A** nur Texte, **B** in Blotato einplanen (Datum und Uhrzeit?), oder **C** sofort posten?

Keine Antwort zu A, B, C heißt **A**.

## Schritt 2: Inhalt des Videos holen

In dieser Reihenfolge, das erste, was klappt:
1. Liegt neben dem Video eine Untertitel-Datei (`.srt`, `.vtt`, `.txt`), lies sie.
2. Ist Blotato verbunden und es gibt einen Link: `blotato_create_source` mit dem Link, dann `blotato_get_source_status` abfragen, bis es fertig ist (höchstens 12 Mal, je etwa 10 Sekunden).
3. Sonst: bitte das Mitglied, das Transkript oder die automatischen Untertitel einzufügen, oder in 3 Sätzen zu sagen, was im Video passiert.

Nie raten, was im Video gesagt wird.

## Schritt 3: Texte je Plattform

Jede Plattform bekommt **eigenen** Text, nicht denselben kopiert. Konkret, mit echten Details aus dem Video. Keine Gedankenstriche als Satzzeichen. Hashtags auf Deutsch, wenn die Zielgruppe deutsch sucht.

| Plattform | Was | Regeln |
|---|---|---|
| YouTube Shorts | Titel und Beschreibung | Titel unter 60 Zeichen, Suchbegriff vorne. Beschreibung 2 bis 3 Sätze mit Suchbegriffen, 3 Hashtags |
| Instagram Reels | Caption | Erste Zeile ist ein Hook (unter 125 Zeichen, so viel sieht man vor "mehr"). 2 bis 3 kurze Zeilen, ein Aufruf, 3 bis 5 Hashtags (breit, Nische, Zielgruppe, Thema) |
| TikTok | Caption | 1 bis 2 Sätze, Suchbegriff drin, 3 bis 5 Hashtags |
| LinkedIn | Beitrag | Die ersten 2 Zeilen tragen den Hook (vor "mehr anzeigen"). 3 bis 5 kurze Absätze: was gebaut oder gelernt wurde, was es bringt. Höchstens 3 Hashtags |
| X | Post | Höchstens 280 Zeichen inklusive allem. Zähle nach. Berichtender Ton: "Habe X gemacht. Das ist passiert." |

Versprich im Aufruf nur, was es laut Profil wirklich gibt. Nennt das Video etwas anderes als die Gratis-Hilfe im Profil, nimm die Worte aus dem Video und frag kurz nach, ob es das gibt. Fehlt ein Link, schreib `[Link]`.

Frag einmal nach, ob im Video KI-erzeugte, echt wirkende Personen oder Szenen vorkommen. Wenn ja: Die Plattformen verlangen dafür eine KI-Kennzeichnung, weise darauf hin.

Speichere alles in `verteilen-<thema>.md` im aktuellen Ordner und zeig die Texte im Chat. Frag: "Passt das so? Sag mir pro Plattform ja oder was sich ändern soll."

Bei **A** ist hier Schluss: Pfad nennen, fertig.

## Schritt 4: Blotato verbinden (nur B und C)

Teste mit `blotato_list_accounts`. Klappt das nicht, gib dem Mitglied diese Anleitung und warte:

1. Im Terminal: `claude mcp add --transport http blotato https://mcp.blotato.com/mcp`
2. Claude Code neu starten, `/mcp` eingeben, Blotato wählen, im Browser bei Blotato anmelden.
3. Danach sag mir Bescheid, dann prüfe ich die Verbindung.

Den Browser-Login bevorzugen. Bittet das Mitglied um die Variante mit API-Schlüssel, soll es den Schlüssel selbst im Terminal eintragen. Den Schlüssel nie in den Chat, in eine Datei im Projekt oder in Git schreiben lassen.

Ordne die Konten aus `blotato_list_accounts` den gewünschten Plattformen zu. Fehlt eine Plattform, sag es und lass sie weg.

## Schritt 5: Video hochladen (nur B und C)

Blotato braucht eine öffentliche Adresse für das Video, keinen Dateipfad.
1. `blotato_create_presigned_upload_url` mit dem Dateinamen samt Endung (zum Beispiel `reel.mp4`).
2. Datei hochladen: `curl -X PUT "<presignedUrl>" -H "Content-Type: video/mp4" --data-binary "@<pfad-zur-datei>"`. In Windows PowerShell `curl.exe` statt `curl` schreiben.
3. Die zurückgegebene `publicUrl` für alle Plattformen verwenden.

Keine fremden Datei-Hoster benutzen.

## Schritt 6: Einplanen oder posten (nur B und C, nur bestätigte Plattformen)

Reihenfolge: YouTube zuerst (braucht am längsten), dann Instagram, TikTok, LinkedIn, X.

`blotato_create_post` je Plattform mit `accountId`, `platform`, `text`, `mediaUrls: [publicUrl]` und:
- **B:** `scheduledTime` als ISO-Zeit mit Zeitzone, mindestens 2 Stunden in der Zukunft.
- **YouTube:** zusätzlich `title` und `privacyStatus` (bei B `public`, bei C vorher fragen: öffentlich, nicht gelistet oder privat).
- **Instagram:** `mediaType: "reel"`.

Danach jede Abgabe mit `blotato_get_post_status` prüfen (höchstens 6 Mal, je etwa 10 Sekunden). Eine Abgabe allein heißt nicht, dass der Post angekommen ist.

## Schritt 7: Ergebnis melden

```
Video verteilt: [Thema]
YouTube Shorts: eingeplant für [Zeit] / veröffentlicht [Link] / fehlgeschlagen ([Grund])
Instagram: ...
TikTok: ...
LinkedIn: ...
X: ...
Texte gespeichert in: [Pfad]
```

Bei **B**: "Du kannst jeden Post bis zur geplanten Zeit im Blotato-Kalender ändern oder löschen."
Ist etwas fehlgeschlagen: Grund in einfachen Worten, und biete einen neuen Versuch nur für diese Plattform an.

## Häufige Fehler

| Problem | Lösung |
|---|---|
| Blotato meldet 401 oder "nicht berechtigt" | `/mcp` öffnen, Blotato neu anmelden |
| Upload schlägt fehl | Mit `--data-binary` und `PUT` hochladen, nicht als Formular |
| Status bleibt "in-progress" | Nach 6 Abfragen aufhören und den Blotato-Kalender prüfen lassen, nicht neu posten (sonst doppelt) |
| X-Text zu lang | Zeichen zählen, Hashtags zuerst kürzen |
