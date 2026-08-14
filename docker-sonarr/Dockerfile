# Dockerfile für Sonarr mit TMDB-Metadaten-Erweiterung
# Basis: .NET 10 Runtime-Image (für self-contained Publish reicht runtime-deps, hier runtime für Sicherheit)
FROM mcr.microsoft.com/dotnet/runtime:10.0

# Grundpakete: mediainfo für Medien-Erkennung, sqlite3/curl fürs Laufen
RUN apt-get update && apt-get install -y --no-install-recommends \
        sqlite3 \
        curl \
        libmediainfo0v5 \
    && rm -rf /var/lib/apt/lists/*

# AppData + App-Verzeichnis
RUN mkdir -p /config /opt/Sonarr

# Deine Artefakte: Backend (self-contained, inkl. .NET-Runtime + native .so-Dateien)
COPY linuxsonarr/ /opt/Sonarr/

# Frontend-Bundle (index.html, JS, CSS)
COPY _output/UI/ /opt/Sonarr/UI/

# Port für die WebUI
EXPOSE 8989

# Benutzer: root (einfach; für produktive Mounts empfehlenswert: eigener User)
USER root

# Start: AppHost "Sonarr" (Console-App mit Entry Point, nicht Sonarr.Host.dll!)
CMD ["/opt/Sonarr/Sonarr", "-nobrowser", "-data=/config"]