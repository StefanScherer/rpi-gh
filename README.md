# rpi-gh
## Raspberry Pi am Gymnasium Höchstadt

Ein kleiner Vortrag beim Technischen Werken. 
## Vortrag starten

### Direkt im Internet

* http://bit.do/rpi-gh

oder per [Remarkise](https://stefanscherer.github.io/remark/remarkise?url=https%3A%2F%2Frawgit.com%2FStefanScherer%2Frpi-gh%2Fmaster%2FSlides.md#1)


### Lokal

```bash
open http://localhost:8000
python -m SimpleHTTPServer 8000
```
## Folien bearbeiten

### Direkt im Internet

Die Folien werden immer direkt aus der  Markdown-Datei `Slides.md` im Browser ausgewertet.

Diese Datei lässt sich auch direkt per Browser auf GitHub bearbeiten und speichern. Nach kurzer Zeit erscheint die Änderung dann auch über den Remarkise (GitHub synchronisiert normalerweise ihre Server bzw. CDN innerhalb weniger Minuten).

### Lokal
Zum Bearbeiten sollte daher die lokale Methode bevorzugt werden.

```bash
git clone https://github.com/StefanScherer/rpi-gh
cd rpi-gh
atom .
```

### Beschreibung zu Remark Markdown

* https://github.com/gnab/remark/wiki/Markdown

### Kleine Tücken

Die CSS Styles habe ich nicht in die Markdown-Datei bekommen, obwohl es scheinbar funktionieren sollte.

Deshalb habe ich den remarkise auf meine GitHub Pages mit eingespielt und dort die Styles für meine Folien eingetragen, die sonst hier in der `index.html` zu finden sind.

In der Kürze der Zeit habe ich das Problem aber nicht weiter untersucht.
