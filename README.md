# Hey das hier ist AI Slop... funktionierender AI Slop!!!

Endlich bekommst du deine Metadaten in deutsch oder französisch oder in welcher Sprache auch immer, vorausgesetzt die Quelle (TMDB) hat sie in deiner gewählten Sprache.
Mach mit dem Code was du willst, in einer Geschwindigkeit die dir passt und warte nicht darauf das irgendwer, irgendwann mal irgendeine Funktion einbaut. 

Aktuell habe ich die Windows Version am laufen, die Linux Version sollte auch funktionieren.

# Was genau macht das hier anders?

Es ruft die Metadaten von TMDB in deiner Sprache ab und speichert sie anstelle der Englischen.

# Warum das ganze?

NZBDrone was 2014 zu Sonarr wurde hat es in 15 Jahren nicht hinbekommen zu erkennen, dass es nicht nur Englischsprachige Länder gibt. Für den nächsten Milestone (2034?) wollen sie Multilingual werden...... 
Warten wir es ab!

# Willst du es dir mal ansehen? 

Forke das Projekt und gib deinem KI Agenten die Datei `BUILD_ANLEITUNG.md`. Der weiß schon was zu tun ist, oder du machst es anhand der Anleitung selbst.

# Warum TMDB?

Lies die Datei `METADATEN_SPRACHE_PLAN.md`

# zusätzliche Informationen zum verwenden des TMDB scrapers
1. Gehe zu https://www.themoviedb.org/
2. Erstelle dir ein Konto
3. Hole dir einen Kostenlosen API Key
4. Nutze für das scraping den langen `API Read Access Token`
5. Starte nach dem erstellen/kompilieren der Dateien die Datei `...\Sonarr\_output\net10.0\Sonarr.Console.exe`
6. Richte Sonarr ein
7. Bei `Einstellungen/UI/Sprache/Metadaten-Sprache` legst du die gewünschte Sprache fest.
8. Bei `Einstellungen/Metadatenquelle` gibst du den ganz langen API Key ein und machst das Häkchen an.
9. Füge deine erste Serie hinzu.

# <img width="24px" src="./Logo/256.png" alt="Sonarr"></img> Sonarr

[![Translated](https://translate.servarr.com/widget/servarr/sonarr/svg-badge.svg)](https://translate.servarr.com/engage/servarr/)
[![Backers on Open Collective](https://opencollective.com/Sonarr/backers/badge.svg)](#backers)
[![Sponsors on Open Collective](https://opencollective.com/Sonarr/sponsors/badge.svg)](#sponsors)
[![Mega Sponsors on Open Collective](https://opencollective.com/Sonarr/megasponsors/badge.svg)](#mega-sponsors)

~~Sonarr is a PVR for Usenet and BitTorrent users. It can monitor multiple RSS feeds for new episodes of your favorite shows and will grab, sort and rename them. It can also be configured to automatically upgrade the quality of files already downloaded when a better quality format becomes available.~~

~~## Getting Started~~

~~- [Download/Installation](https://sonarr.tv/#downloads-v3)~~
- [FAQ](https://wiki.servarr.com/sonarr/faq)
- [Wiki](https://wiki.servarr.com/Sonarr)
- [API Documentation](https://sonarr.tv/docs/api)

~~- [Donate](https://sonarr.tv/donate)~~

## Support

~~Note: GitHub Issues are for Bugs and Feature Requests Only~~

~~- [Forums](https://forums.sonarr.tv/)~~
~~- [Discord](https://discord.gg/M6BvZn5)~~
~~- [GitHub - Bugs and Feature Requests Only](https://github.com/Sonarr/Sonarr/issues)~~
~~- [IRC](https://web.libera.chat/?channels=#sonarr)~~
~~- [Reddit](https://www.reddit.com/r/sonarr)~~
~~- [Wiki](https://wiki.servarr.com/sonarr)~~

## Features

### Current Features

- Support for major platforms: Windows, Linux, macOS, Raspberry Pi, etc.
- Automatically detects new episodes
- Can scan your existing library and download any missing episodes
- Can watch for better quality of the episodes you already have and do an automatic upgrade. _eg. from DVD to Blu-Ray_
- Automatic failed download handling will try another release if one fails
- Manual search so you can pick any release or to see why a release was not downloaded automatically
- Fully configurable episode renaming
- Full integration with SABnzbd and NZBGet
- Full integration with Kodi, Plex (notification, library update, metadata)
- Full support for specials and multi-episode releases
- And a beautiful UI

~~## Contributing~~

~~### Development~~

~~This project exists thanks to all the people who contribute. [Contribute](CONTRIBUTING.md).~~

~~<a href="https://github.com/Sonarr/Sonarr/graphs/contributors"><img src="https://opencollective.com/Sonarr/contributors.svg?width=890&button=false" /></a>~~

~~### Supporters

~~This project would not be possible without the support of our users and software providers.~~
~~[**Become a sponsor or backer**](https://opencollective.com/sonarr) to help us out!~~

~~#### Mega Sponsors~~

~~[![Sponsors](https://opencollective.com/sonarr/tiers/mega-sponsor.svg?width=890)](https://opencollective.com/sonarr/contribute/mega-sponsor-21443/checkout)~~

~~#### Sponsors~~

~~[![Flexible Sponsors](https://opencollective.com/sonarr/sponsors.svg?width=890)](https://opencollective.com/sonarr/contribute/sponsor-21457/checkout)~~

~~#### Backers~~

~~[![Backers](https://opencollective.com/sonarr/backers.svg?width=890)](https://opencollective.com/sonarr/contribute/backer-21442/checkout)~~

~~#### JetBrains~~

~~Thank you to [<img src="https://resources.jetbrains.com/storage/products/company/brand/logos/jetbrains.png" alt="JetBrains" width="96">](http://www.jetbrains.com/) for providing us with free licenses to their great tools~~

~~[<img src="https://resources.jetbrains.com/storage/products/company/brand/logos/TeamCity.png" alt="TeamCity" width="64">](http://www.jetbrains.com/teamcity/)~~

~~[<img src="https://resources.jetbrains.com/storage/products/company/brand/logos/ReSharper.png" alt="ReSharper" width="64">](http://www.jetbrains.com/resharper/)~~

~~[<img src="https://resources.jetbrains.com/storage/products/company/brand/logos/dotTrace.png" alt="dotTrace" width="64">](http://www.jetbrains.com/dottrace/)~~

~~[<img src="https://resources.jetbrains.com/storage/products/company/brand/logos/Rider.png" alt="Rider" width="64">](http://www.jetbrains.com/rider/)~~

### Licenses

- [GNU GPL v3](http://www.gnu.org/licenses/gpl.html)
- Copyright 2010-2025
