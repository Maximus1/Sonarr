# Sonarr (TMDB-Version) als Docker-Container

Fertiges Docker-Setup für den Build mit TMDB-Metadaten-Erweiterung.

## Voraussetzungen (einmalig erledigt)

- `linuxsonarr\` – **self-contained** Linux-x64-Publish der **Console-App**
  (`dotnet publish src\NzbDrone.Console\Sonarr.Console.csproj -c Release -r linux-x64 -f net10.0 --self-contained true -p:RunAnalyzers=false -o linuxsonarr`)
- `_output\UI\` – Frontend-Bundle (`cd frontend && yarn build`)

## Image bauen

Von diesem Ordner (`docker-sonarr`) aus, da der Build-Kontext das Projekt-Root ist:

```bash
docker build -t sonarr-tmdb:local -f Dockerfile ..
```

Oder per Compose (empfohlen):

```bash
# Pfade in docker-compose.yml anpassen (Serien-Mount!)
docker compose up -d --build
```

## Starten

Die Compose-Datei startet den Container mit:
- Port **8989** (WebUI)
- Volume `./config` → `/config` (dauerhafte Daten)
- Volume `/pfad/zu/serien` → `/media/serien` (bitte anpassen!)

## In Sonarr aktivieren

1. WebUI: http://localhost:8989
2. **Einstellungen → Metadatenquelle** → TMDB-Key (`eyJ…`) + **TMDB aktivieren** → Speichern
3. **Einstellungen → UI → Sprache → Metadata Language** → z. B. **Deutsch**
4. Serien suchen/hinzufügen → Metadaten auf Deutsch

## Hinweise

- Das Image enthält die **komplette .NET-Runtime** (self-contained) → kein separater .NET-Install nötig.
- Gestartet wird der **AppHost `Sonarr`** (Console-App mit Entry Point) – nicht `Sonarr.Host.dll`.
- Bei späteren Code-Änderungen: `linuxsonarr` + `_output\UI` neu bauen, dann `docker compose up -d --build`.
