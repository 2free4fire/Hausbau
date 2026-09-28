# Hausbegehung VR (Meta Quest 3)

Begehbares 3D-Modell des Hauses im Browser – mit der Quest 3 in VR, am PC mit Maus und Tastatur.

## Auf GitHub Pages veröffentlichen

1. Auf github.com ein neues Repository anlegen, z. B. `hausbegehung` (Public – GitHub Pages ist bei privaten Repos nur mit Pro-Account möglich).
2. **Den gesamten Inhalt dieses Ordners** hochladen (`index.html`, `haus.glb`, Ordner `lib/`, `.nojekyll`):
   - im Browser: „Add file → Upload files“, alles hineinziehen, „Commit changes“
   - oder per Git:
     ```
     git init && git add . && git commit -m "Hausbegehung"
     git branch -M main
     git remote add origin https://github.com/<DEIN-NAME>/hausbegehung.git
     git push -u origin main
     ```
3. Im Repo: **Settings → Pages → Source: „Deploy from a branch“, Branch `main`, Ordner `/ (root)`** → Save.
4. Nach 1–2 Minuten ist die Seite erreichbar unter `https://<DEIN-NAME>.github.io/hausbegehung/`.

## Auf der Quest 3 ansehen

1. Quest-Browser öffnen, die URL oben aufrufen (Tipp: als Lesezeichen speichern).
2. Warten, bis das Modell geladen ist (~12 MB), dann unten **„ENTER VR“** antippen.
3. Du startest vor der Haustür – einfach losgehen, die Tür öffnet sich.

### Steuerung

| Quest 3 | Funktion |
|---|---|
| Linker Stick | gehen (in Blickrichtung) |
| Linker Trigger halten | schneller gehen |
| Rechter Stick links/rechts | drehen in 30°-Schritten |
| A | Standort wechseln: Außen → EG → OG |
| B | Geistmodus an/aus (durch Wände gehen) |
| X | aktuellen Standort neu starten (schließt alle Türen) |
| Y | Hilfetafel am linken Controller ein/aus |

Du kannst dich auch real im Raum bewegen – der Kopf wird an Wänden gestoppt.
Treppen steigt man einfach hinauf, Türen öffnen sich beim Herangehen.

PC: ins Bild klicken, WASD + Maus, Shift = schneller, F = Standort, G = Geistmodus, R = neu starten.

## Anpassen

Oben in `index.html` im Block `CONFIG`:
- `walkSpeed` / `runSpeed` – Gehtempo
- `snapAngle` – Drehwinkel
- `vignette: false` – Tunnelblick beim Gehen abschalten (wer nicht zu Übelkeit neigt)
- `levels` – Startpunkte `[x, z, Blickrichtung°]`
- `sunAzimuth` / `sunElevation` – Sonnenstand (Lichteinfall durch die Fenster)

## Neues Modell aus SpacePlanner einspielen

Das Original-Export war 275 MB (GitHub erlaubt max. 100 MB pro Datei, die Quest wäre überfordert).
Es wurde mit gltf-transform auf ~12 MB komprimiert (Texturen WebP max. 1024 px, Geometrie meshopt).
Für einen neuen Export: Datei als `haus.glb` ersetzen, vorher komprimieren, z. B.:

```
npx @gltf-transform/cli optimize grundriss.glb haus.glb --compress meshopt --texture-compress webp --texture-size 1024 --join-named false --instance false
```

Hinweis: Die automatische Türerkennung braucht Knoten mit Namen `DOOR_<id>_leaf` / `DOOR_<id>_h0`.
Ohne diese Vorbereitung bleiben die Türen geschlossen – dann mit B (Geistmodus) durchgehen.
Flachdach, weiße EG-Decke und Treppenloch (`stairHole`) sind ergänzt, da sie im Export fehlen – bei Grundrissänderungen in `CONFIG` anpassen.

Bibliotheken (lokal in `lib/`): three.js r186 (MIT), three-mesh-bvh 0.8.3 (MIT).
