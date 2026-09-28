# Boulder-Firmware

Firmware-Releases für die Pebble Time 2 (obelix), gebaut aus einem Fork von
[PebbleOS](https://github.com/coredevices/PebbleOS) (Apache-2.0) ab v4.38.1.
Die Begleit-App Boulder bietet die Releases dieses Repos als Update an.

Was diese Firmware anders macht als die offizielle:

- eigene Schlafanalyse mit Phasen (Tief, Leicht, REM, Wach), auf der Uhr gerechnet
- HRV-Messung in der Nacht, einstellbare Messdichte (Precision)
- erkannte Alltagsbewegung heisst „Bewegung“ und ist kein Training
- Kieselsport als eingebaute Trainings-App statt der Workout-App
- eigene Health-App: Heute, Schlaf, Herz & SpO2, Woche

Versionen heissen `v4.38.1-bN`: die PebbleOS-Fassung, auf der sie beruhen, und
die laufende Nummer der Boulder-Firmware.

Jedes Release enthält `normal_obelix_pvt_<version>.pbz` mit beiden Slots.
Zurück zur offiziellen Firmware geht es über die Pebble-App.
