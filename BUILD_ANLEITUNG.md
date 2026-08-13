# Build-Anleitung / Build Guide

Kompilieren von Sonarr mit TMDB-Metadaten-Erweiterung für **Windows** und **Linux**.
Compiling Sonarr (with the TMDB metadata extension) for **Windows** and **Linux**.

> Stand / Revision: 13.08.2026 – basierend auf Commit `58699ef52`
> Getestet mit: .NET SDK 10.0.400, Node.js + Yarn, Windows 11

---

## DE // Voraussetzungen

| Werkzeug | Version | Hinweis |
|----------|---------|---------|
| .NET SDK | **10.x** | `global.json` verlangt `10.0.302`; 10.0.400 getestet (mit `rollForward: latestFeature`) |
| Node.js | >= 18 (empfohlen 20 LTS) | Für das Frontend |
| Yarn | 1.x (classic) | Frontend-Paketmanager |
| (Git) | beliebig | Optional, für Repo-Handling |

Prüfen:
```powershell
dotnet --list-sdks
node --version
yarn --version
```

---

## EN // Prerequisites

| Tool | Version | Note |
|------|---------|------|
| .NET SDK | **10.x** | `global.json` requires `10.0.302`; 10.0.400 tested (with `rollForward: latestFeature`) |
| Node.js | >= 18 (LTS 20 recommended) | Used for the frontend |
| Yarn | 1.x (classic) | Frontend package manager |
| (Git) | any | Optional, for repository handling |

Verify:
```bash
dotnet --list-sdks
node --version
yarn --version
```

---

## 1. Abhängigkeiten wiederherstellen // Restore dependencies

```powershell
# Backend
dotnet restore src/Sonarr.sln --nologo

# Frontend
cd frontend
yarn install
cd ..
```

---

## 2. Windows kompilieren // Build on Windows

### Debug (Standard, wie in der Entwicklung)
```powershell
dotnet build src/Sonarr.sln --nologo -v minimal
```
Ausgabe: `_output\net10.0\` (Binaries) + `_output\UI\` (Frontend, sofern gebaut)

### Release
```powershell
dotnet build src/Sonarr.sln -c Release --nologo -v minimal
```

### Frontend-Bundle (nötig, damit die UI mit dem Backend mitgeliefert wird)
```powershell
cd frontend
yarn build
cd ..
```
Ausgabe: `_output\UI\` (index.html, JS, CSS)

### Hinweis: Sonarr-Prozess beendet das Kopieren
Wenn die DLLs laufen (z. B. `Sonarr.Console.exe`), schlägt der Build beim Kopieren fehl (MSB3021). Vorher stoppen:
```powershell
Get-Process -Name "Sonarr.Console" -ErrorAction SilentlyContinue | Stop-Process -Force
```

---

## EN // Build on Windows

### Debug (standard, like during development)
```powershell
dotnet build src/Sonarr.sln --nologo -v minimal
```
Output: `_output\net10.0\` (binaries) + `_output\UI\` (frontend, if built)

### Release
```powershell
dotnet build src/Sonarr.sln -c Release --nologo -v minimal
```

### Frontend bundle (required so the UI ships with the backend)
```powershell
cd frontend
yarn build
cd ..
```
Output: `_output\UI\` (index.html, JS, CSS)

### Note: A running Sonarr blocks the copy step
If the DLLs are in use (e.g. `Sonarr.Console.exe`), the build fails when copying (MSB3021). Stop it first:
```powershell
Get-Process -Name "Sonarr.Console" -ErrorAction SilentlyContinue | Stop-Process -Force
```

---

## 3. Linux kompilieren (cross-publish von Windows) // Linux build (cross-publish from Windows)

Das Projekt targetet `net10.0`. Für Linux werden **RID-spezifische Binaries** benötigt – nativen Dateien wie `libe_sqlite3.so` und `ffprobe` werden dabei mitgeliefert.

The project targets `net10.0`. For Linux, **RID-specific binaries** are required – native files like `libe_sqlite3.so` and `ffprobe` are included automatically.

### 3a. Linux x64 (framework-dependent, passt zur LinuxServer-Runtime)
```powershell
dotnet publish src\NzbDrone.Host\Sonarr.Host.csproj `
  -c Release -r linux-x64 --self-contained false -f net10.0 `
  -p:RunAnalyzers=false -o linuxsonarr
```
> **Wichtig // Important:** `-p:RunAnalyzers=false`, weil StyleCop-Analyzer im Release/Publish sonst in bestehenden Dateien fehlschlagen (SA1200 etc.). Der eigentliche Code ist davon nicht betroffen.
> **Hinweis // Note:** `--self-contained false` setzt voraus, dass die **gleiche .NET-Runtime** (10.x) auf dem Zielsystem/Container vorhanden ist. Falls der Zielcontainer nur .NET 8 hat, nicht kompatibel → siehe 3b.

### 3b. Linux x64 (self-contained, enthält die Runtime)
```powershell
dotnet publish src\NzbDrone.Host\Sonarr.Host.csproj `
  -c Release -r linux-x64 --self-contained true -f net10.0 `
  -p:RunAnalyzers=false -o linuxsonarr
```
> Die Runtime ist dann im Output enthalten – unabhängig vom Zielsystem. Größer (~80 MB+), aber robust.

### 3c. Frontend für den Container (separat bauen)
```powershell
cd frontend
yarn build
cd ..
```
Ausgabe: `_output\UI\` → für Container nach `/app/sonarr/bin/UI/` kopieren.

### EN
```bash
# 3a linux-x64 framework-dependent
dotnet publish src/NzbDrone.Host/Sonarr.Host.csproj \
  -c Release -r linux-x64 --self-contained false -f net10.0 \
  -p:RunAnalyzers=false -o linuxsonarr

# 3b linux-x64 self-contained
dotnet publish src/NzbDrone.Host/Sonarr.Host.csproj \
  -c Release -r linux-x64 --self-contained true -f net10.0 \
  -p:RunAnalyzers=false -o linuxsonarr

# 3c frontend
cd frontend && yarn build && cd ..
```

---

## 4. Tests & Linting

```powershell
# .NET-Tests (Core-Projekt)
dotnet test src/NzbDrone.Core.Test/NzbDrone.Core.Test.csproj -c Debug --nologo

# Frontend-Lint
cd frontend
yarn lint
cd ..
```

### EN
```bash
# .NET tests (Core project)
dotnet test src/NzbDrone.Core.Test/NzbDrone.Core.Test.csproj -c Debug --nologo

# Frontend lint
cd frontend && yarn lint && cd ..
```

---

## 5. Ausgaben / Outputs

| Ziel | Verzeichnis |
|------|-------------|
| Windows-Backend | `_output\net10.0\` |
| Windows-Frontend | `_output\UI\` |
| Linux-Backend (publish) | `linuxsonarr\` |
| Container-Zielpfade | `/app/sonarr/bin/`, `/app/sonarr/bin/UI/` (LinuxServer) |

---

## 6. Häufige Probleme // Common issues

| Fehler/Symptom | Ursache | Lösung |
|----------------|---------|--------|
| `MSB3021` / „Datei wird gesperrt“ | Sonarr läuft noch | Prozess beenden, dann bauen |
| `SA1200`/StyleCop-Fehler im Release | Analyzer greifen im Publish | `-p:RunAnalyzers=false` |
| `NETSDK1129` „multiple frameworks“ | Projekt targetet mehrere TFMs | `-f net10.0` angeben |
| `SDK not found“ (10.0.302) | Alte SDK-Version | SDK 10 installieren **oder** `global.json` `rollForward: latestFeature` setzen |
| Container startet, GUI tot | .NET-Runtime-Version mismatch | Ziel-Runtime prüfen (`dotnet --list-runtimes` im Container) → `--self-contained true` verwenden |

---

## EN // Common issues

| Error/Symptom | Cause | Fix |
|---------------|-------|-----|
| `MSB3021` / "file in use" | Sonarr is still running | Stop the process, then build |
| `SA1200`/StyleCop errors in Release | Analyzers run during publish | Add `-p:RunAnalyzers=false` |
| `NETSDK1129` "multiple frameworks" | Project targets multiple TFMs | Specify `-f net10.0` |
| `SDK not found (10.0.302)` | Old SDK version | Install SDK 10 **or** set `rollForward: latestFeature` in `global.json` |
| Container up, GUI dead | .NET runtime version mismatch | Check runtime in container (`dotnet --list-runtimes`) → use `--self-contained true` |

---

## 7. Kurzreferenz // Quick reference

```powershell
# Windows
dotnet restore src/Sonarr.sln
cd frontend; yarn install; yarn build; cd ..
dotnet build src/Sonarr.sln -c Release --nologo -v minimal

# Linux (cross-publish)
dotnet publish src\NzbDrone.Host\Sonarr.Host.csproj -c Release -r linux-x64 --self-contained true -f net10.0 -p:RunAnalyzers=false -o linuxsonarr
```

Damit sind alle Schritte abgedeckt – vom Restore über Windows- und Linux-Build bis zur Einbindung in den LinuxServer-Container. / That covers everything from restore over Windows & Linux builds to the LinuxServer container integration.