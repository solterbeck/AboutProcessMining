# All About Process Intelligence

Technische Dokumentation fuer die statische Webanwendung **All About Process Intelligence**. Das Projekt kuratiert Literatur, Videos, Praesentationen und weitere Quellen rund um Process Mining und Process Intelligence und stellt sie als durchsuchbare Bibliographie bereit.

## Projektueberblick

Die Anwendung ist eine reine Frontend-Webseite ohne Backend, Build-Prozess oder Paketmanager. Alle Seiten bestehen aus statischem HTML, CSS und clientseitigem JavaScript. Der Bibliographiedatensatz liegt als JSON-Datei im Repository und wird im Browser geladen.

Produktiv ist die Anwendung fuer GitHub Pages ausgelegt:

```text
https://solterbeck.github.io/AboutProcessMining/
```

## Technologiestack

- HTML5 fuer Seitenstruktur und Inhalte
- CSS3 fuer eigenes Styling in `styles.css`
- Bootstrap 5.3.0 ueber CDN fuer Layout, Navigation, Accordion und Buttons
- jQuery 3.6.4 ueber CDN fuer DOM-Manipulation und JSON-Laden in der Bibliographie
- GitHub Pages als statisches Hosting
- JSON als Datenformat fuer die Literaturdatenbank

Es gibt aktuell keinen Node-, Python-, PHP- oder sonstigen Serveranteil im Projekt.

## Dateistruktur

```text
.
|-- index.html                  # Startseite mit Hero-Banner und zuletzt hinzugefuegten Publikationen
|-- bibliography.html           # Durchsuchbare und paginierte Bibliographie
|-- imprint.html                # About, Datenschutz-, Haftungs- und Impressumsinformationen
|-- bibliography copy.html      # Aeltere/alternative Kopie der Bibliographieseite
|-- styles.css                  # Aktives globales Stylesheet fuer Startseite und Impressum
|-- styles_old.css              # Alte Stylesheet-Version, derzeit nicht eingebunden
|-- library_full_data.json      # Bibliographiedatenbestand
|-- banner-image.png            # Hero-Bild der Startseite
|-- logo-placeholder.png        # Logo in der Navigation
|-- readme.md                   # Diese technische Dokumentation
```

## Seiten und Funktionen

### `index.html`

Die Startseite enthaelt:

- responsive Bootstrap-Navigation
- Hero-Banner mit `banner-image.png`
- Bereich "Recently Added Publications"
- clientseitiges Laden von `library_full_data.json`
- Filterung auf Eintraege, deren `DateAdded` innerhalb der letzten 30 Tage liegt
- Darstellung der Treffer als Bootstrap-Accordion

Der Datensatz wird aktuell ueber eine absolute GitHub-Pages-URL geladen:

```javascript
fetch('https://solterbeck.github.io/AboutProcessMining/library_full_data.json')
```

### `bibliography.html`

Die Bibliographieseite enthaelt:

- Suchfeld fuer Titel, Autor, Editor, Typ, Publikationstitel, Verlag, Jahr, Seiten, Sprache, Tags, Abstract und URL
- Filterbuttons:
  - alle Eintraege
  - ab 2026
  - ab 2016
  - vor 2016
- alphabetische Sortierung nach Titel
- Pagination mit 10 Eintraegen pro Seite
- Accordion-Detailansicht pro Datensatz

Die Seite verwendet eigenes Inline-CSS und laedt die Daten mit:

```javascript
$.getJSON('https://solterbeck.github.io/AboutProcessMining/library_full_data.json', function (response) {
    data = sortDataByTitle(response);
    filteredData = [...data];
    currentPage = 1;
    applyFilterAndSearch();
});
```

### `imprint.html`

Die Impressumsseite dokumentiert:

- Projektbeschreibung
- Hosting- und Datenschutzkontext
- Verantwortliche Kontaktinformationen
- Haftung fuer Inhalte und Links
- Copyright-Hinweise

## Datenmodell

`library_full_data.json` ist ein Array aus Literatur- und Medienobjekten. Der aktuelle Datenbestand enthaelt 745 Eintraege.

Die verwendeten Felder sind:

| Feld | Beschreibung |
| --- | --- |
| `Title` | Titel des Eintrags |
| `Author` | Autorinnen/Autoren, falls vorhanden |
| `Editor` | Herausgeberinnen/Herausgeber, falls vorhanden |
| `ItemType` | Typ des Eintrags, z. B. Buch, Artikel, Video oder Praesentation |
| `PublicationTitle` | Titel der Zeitschrift, Konferenz oder Sammlung |
| `Publisher` | Verlag oder publizierende Organisation |
| `PublicationYear` | Erscheinungsjahr |
| `Pages` | Seitenangabe |
| `Language` | Sprache des Eintrags |
| `ManualTags` | Manuell gepflegte Schlagwoerter |
| `AbstractNote` | Abstract, Notiz oder Beschreibung |
| `Url` | Externer Link zur Quelle |
| `DateAdded` | Hinzufuegedatum im ISO-Format |
| `Medium` | Medium, falls gepflegt |
| `ConferenceName` | Konferenzname, falls gepflegt |

Nicht jeder Datensatz enthaelt Werte in allen Feldern. Die Oberflaeche zeigt fehlende Werte ueber Fallbacks wie `N/A` oder `-` an.

## Datenfluss

1. Der Browser laedt eine HTML-Seite.
2. Bootstrap, jQuery und Bootstrap JavaScript werden von CDNJS geladen.
3. Die Seite ruft `library_full_data.json` von GitHub Pages ab.
4. JavaScript filtert, sortiert und rendert die Daten im Browser.
5. Die Accordion-Komponenten werden durch Bootstrap JavaScript interaktiv.

Es findet keine serverseitige Verarbeitung und keine Speicherung von Nutzereingaben statt.

## Lokale Ausfuehrung

Da es keine Build-Schritte gibt, kann das Projekt direkt aus dem Repository heraus getestet werden.

Empfohlen ist ein lokaler statischer Server:

```powershell
python -m http.server 8000
```

Danach im Browser oeffnen:

```text
http://localhost:8000/
```

Alternativ koennen die HTML-Dateien direkt im Browser geoeffnet werden. Fuer realistische Tests ist ein lokaler Server jedoch stabiler, weil Browser beim Laden lokaler Dateien andere Sicherheitsregeln anwenden koennen.

Wichtig: `index.html` und `bibliography.html` laden den JSON-Datensatz aktuell von der produktiven GitHub-Pages-URL. Lokale Aenderungen an `library_full_data.json` erscheinen daher erst dann in der Oberflaeche, wenn die URLs auf eine relative Quelle wie `library_full_data.json` geaendert oder die produktive Datei aktualisiert wurde.

## Deployment

Das Deployment erfolgt ueber GitHub Pages. Fuer eine Aktualisierung muessen die statischen Dateien in den Branch/Ordner gepusht werden, der fuer GitHub Pages konfiguriert ist.

Typischer Ablauf:

```powershell
git status
git add .
git commit -m "Update website"
git push
```

Nach dem Push kann GitHub Pages einige Minuten benoetigen, bis neue Inhalte sichtbar sind.

## Wartung der Bibliographie

Beim Aktualisieren von `library_full_data.json` sollte Folgendes geprueft werden:

- Die Datei muss gueltiges JSON enthalten.
- Der Wurzelwert muss ein Array sein.
- `DateAdded` sollte als ISO-Datum vorliegen, damit die Startseite neue Publikationen korrekt erkennt.
- `PublicationYear` sollte numerisch interpretierbar sein, damit die Jahresfilter funktionieren.
- Externe URLs sollten mit `https://` beginnen, sofern moeglich.
- Sehr lange Abstracts koennen die Accordion-Darstellung deutlich vergroessern.

Eine einfache Validierung ist mit PowerShell moeglich:

```powershell
Get-Content -Raw library_full_data.json | ConvertFrom-Json | Measure-Object
```

## Externe Abhaengigkeiten

Die Anwendung ist zur Laufzeit von folgenden CDN-Ressourcen abhaengig:

- `https://cdnjs.cloudflare.com/ajax/libs/bootstrap/5.3.0/css/bootstrap.min.css`
- `https://cdnjs.cloudflare.com/ajax/libs/bootstrap/5.3.0/js/bootstrap.bundle.min.js`
- `https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.4/jquery.min.js`

Wenn CDNJS nicht erreichbar ist, funktionieren Layout und interaktive Komponenten nur eingeschraenkt.

## Bekannte technische Hinweise

- `bibliography.html` enthaelt Inline-CSS, waehrend `index.html` und `imprint.html` `styles.css` verwenden.
- `styles_old.css` ist aktuell nicht eingebunden und dient nur als historische Referenz.
- `bibliography copy.html` wirkt wie eine Sicherungs- oder Altversion und sollte vor weiteren Releases entweder geloescht, archiviert oder klar benannt werden.
- Die Daten-URL ist in `index.html` und `bibliography.html` hart auf GitHub Pages gesetzt. Fuer lokale Datenpflege waere eine relative URL wartungsfreundlicher.
- Dynamisch gerenderte Inhalte werden derzeit per Template-String direkt in `innerHTML` bzw. jQuery `append` eingefuegt. Bei nicht vertrauenswuerdigen Daten sollte HTML-Escaping ergaenzt werden.
- Die Seite sammelt selbst keine personenbezogenen Daten, bindet aber externe CDN- und Linkziele ein.

## Roadmap

Aktuell dokumentierte naechste Schritte:

- Zitier-Button fuer Bibliographieeintraege
- Barrierefreiheit pruefen und verbessern
- Aktuelle Forschung und Publikationen weiter ausbauen
- Interaktive Lernmaterialien
- LinkedIn-Interaktion
- Case Studies

## Projektstatus

MVP 1 ist fertiggestellt. Die Anwendung ist als statischer Prototyp produktiv nutzbar und wird weiterentwickelt.

## Autor

Sven Solterbeck  
Kontakt: `socialnetwork@solterbeck.net`
