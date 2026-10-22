# Boulder-Firmware

Firmware-Releases für die Pebble Time 2 (obelix), gebaut aus einem Fork von
[PebbleOS](https://github.com/coredevices/PebbleOS) (Apache-2.0).
Die Begleit-App Boulder bietet die Releases dieses Repos als Update an.

Was diese Firmware anders macht als die offizielle:

- eigene Schlafanalyse mit Phasen (Tief, Leicht, REM, Wach), auf der Uhr gerechnet
- HRV-Messung in der Nacht, einstellbare Messdichte (Precision)
- eigene Health-App: Today, Night, Heart, Activity und Settings
- Kieselsport als eingebaute Trainings-App auf Pebbles Workout-Dienst, mit Atemübung
- Kieselstrasse: der nächste Schritt einer OsmAnd-Navigation aus Boulder

Versionen heissen `vX.Y.Z-bN`: die PebbleOS-Fassung, auf der sie beruhen, und
die laufende Nummer der Boulder-Firmware.

Jedes Release enthält `normal_obelix_pvt_<version>.pbz` mit beiden Slots.
Zurück zur offiziellen Firmware geht es über die Pebble-App.

## Releases

1. Ein Release-Commit auf einem Zweig neben `main` legt `pakete/<tag>.pbz` und
   `pakete/<tag>.md` an: erste Zeile der Titel ohne Tag, darunter der Text mit genau einer Zeile
   `Mindest-App: X.Y.Z`. Älteren Boulder-Fassungen bietet die App das Release nicht an. `main`
   bleibt ohne Pakete, den Release-Commit hält das Tag.
2. Unter Actions → „Signieren und veroeffentlichen“ → „Run workflow“ diesen Zweig wählen und
   das Tag eingeben. Der Workflow prüft Paket (Manifeste mit Tag und `obelix_pvt`) und Text,
   signiert, legt das Release als Entwurf mit Paket und Signatur an, prüft Grösse und sha256 aus
   der API und schaltet es frei. Das Tag entsteht dabei auf dem Release-Commit.

Vorher muss ein App-Release ab der Mindest-App veröffentlicht sein: Eine App vor 0.50 liest die
Zeile nicht und böte die Firmware trotzdem an. Neuere Apps verwiesen auf ein Update, das es noch
nicht gibt. Der Workflow kann das nicht prüfen, weil das Repo der App privat ist.

Eine Beta ist ein gewöhnliches Release mit „Beta“ im Titel: Boulder bietet sie wie jedes andere
Release an.

Bricht der Lauf ab, bleibt höchstens ein Entwurf stehen, den Boulder nicht anbietet. Ursache
beheben, den Entwurf löschen und neu starten.

## Signatur

Neben dem Paket liegt `<paket>.sig`: 64 Byte Ed25519-Signatur über diese Aussage (ASCII, fünf
Zeilen, je mit LF):

```
boulder-firmware/v1
<tag>
<dateiname>
<grösse in byte>
sha256:<64 hex, klein>
```

Boulder baut die Aussage aus dem Release nach und prüft sie gegen die Schlüssel in
`FirmwareSignatur.kt`, bevor es das Paket auspackt. Ohne gültige Signatur gibt es kein Update.

- **Schlüssel:** Der Seed (32 Byte, 64 Hex-Zeichen) liegt im Repository-Secret
  `BOULDER_SIGNATUR_SEED`. Der öffentliche Schlüssel ist `signatur.pub`. Der Workflow signiert
  nur, wenn das Secret dazu passt. Den Seed an zwei Orten sichern: Passwortmanager und Papier.
- **Grenze:** Wer das GitHub-Konto übernimmt, kann mit dem Secret signieren. Dann den Schlüssel
  wechseln (unten).
- **Schlüsselwechsel:**
  1. Einen neuen Seed erzeugen (`openssl genpkey -algorithm ed25519`).
  2. Seinen öffentlichen Schlüssel in `FirmwareSignatur.SCHLUESSEL` aufnehmen und die App
     ausrollen.
  3. Secret und `signatur.pub` tauschen.
  4. Den alten Schlüssel später aus der App nehmen.
- **Zurückziehen:** Das Release als Vorabversion markieren (Boulder überspringt
  Vorabversionen). Die Korrektur kommt als neue Nummer.
