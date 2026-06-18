## ENGLISH
# Arens Plugin

<div align="center">

![Version](https://img.shields.io/badge/version-1.0--SNAPSHOT-blue)
![Spigot](https://img.shields.io/badge/Spigot-1.20.1%2B-green)
![Java](https://img.shields.io/badge/Java-17%2B-orange)
![License](https://img.shields.io/badge/license-MIT-yellow)

**A comprehensive arena plugin for Paper/Spigot 1.20.1+**

Features queue system, GUI rewards, ranking, combat check integration, multi-language support, and multiple arena instances.

[Features](#features) • [Installation](#installation) • [Commands](#commands) • [Configuration](#configuration) • [Setup Guide](#setup-guide)

</div>

---

## Table of Contents

- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Commands](#commands)
  - [Player Commands](#player-commands)
  - [Admin Commands](#admin-commands)
- [Configuration](#configuration)
  - [Language System](#language-system)
  - [Database Configuration](#database-configuration)
  - [Combat Check Integration](#combat-check-integration)
  - [GUI Settings](#gui-settings)
  - [Allowed Commands](#allowed-commands)
- [Multiple Arena Instances](#multiple-arena-instances)
- [PlaceholderAPI Placeholders](#placeholderapi-placeholders)
- [Setup Guide](#setup-guide)
- [Permissions](#permissions)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

---

## Features

### Core Features

- **🎯 Arena Queue System** - Players can join arena queues and wait for opponents
- **⚔️ Match System** - Automatic matchmaking with countdown, teleportation, and freezing
- **🎁 Rewards GUI** - Paginated GUI for collecting won items with navigation and clear options
- **🗺️ Multiple Arena Instances** - Create multiple maps under one arena name for variety
- **🌍 Multi-Language Support** - Full Polish and English language support with easy switching
- **🛡️ Command Blocking** - Restricts commands during arena matches (only /msg, /r, /tell, /reply allowed)
- **⚔️ Combat Check Integration** - Integrates with anti-logout plugins via PlaceholderAPI
- **💾 Database Support** - SQLite and MySQL support for player stats and rewards
- **📊 PlaceholderAPI Expansion** - Placeholders for wins, losses, ranking, and top players
- **📢 Global Messages** - All arena events broadcast to all players
- **🎨 Pretty Help Centers** - Beautifully formatted help menus in both languages
- **🔄 Hot Reload** - Reload configuration without server restart

### Advanced Features

- **Random Map Selection** - When multiple instances exist, players are randomly assigned to available maps
- **Item Serialization** - Won items are safely stored in the database and can be collected later
- **Player Statistics** - Track wins, losses, total matches, and win rates
- **Ranking System** - Automatic ranking based on total wins
- **Combat Tag Protection** - Prevents players from joining arenas while in combat

---

## Requirements

- **Server**: Paper/Spigot 1.20.1 or higher
- **Plugins**: PlaceholderAPI (required)
- **Java**: Java 17 or higher
- **Database**: SQLite (default) or MySQL

---

## Installation

### From Source

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/arens.git
   cd arens
   ```

2. **Build the plugin using Maven**
   ```bash
   mvn clean package
   ```

3. **Install the plugin**
   - Copy the generated JAR file from `target/arens-1.0-SNAPSHOT.jar`
   - Place it in your server's `plugins` folder

4. **Restart the server**

5. **Configure the plugin**
   - Edit `plugins/Arens/lang.yml` to set your preferred language
   - Run `/aareny reload` to generate the appropriate config file
   - Configure settings in `plugins/Arens/config.yml`

### From Release

1. Download the latest release from the [Releases](https://github.com/yourusername/arens/releases) page
2. Place the JAR file in your server's `plugins` folder
3. Restart the server
4. Configure as described above

---

## Commands

### Player Commands

| Command | Description | Aliases |
|---------|-------------|---------|
| `/arena <name>` | Join the queue for a specific arena | - |
| `/arena leave` | Leave the current arena queue | - |
| `/arena help` | Show arena command help | - |
| `/areny collect` | Open the rewards GUI to collect won items (EN) | `/areny odbierz` (PL) |

### Admin Commands

| Command | Description | Permission |
|---------|-------------|------------|
| `/aareny create <name> [instance]` | Create a new arena (with optional instance/map) | `arens.admin` |
| `/aareny pos1 <name> [instance]` | Set position 1 (spawn for player 1) | `arens.admin` |
| `/aareny pos2 <name> [instance]` | Set position 2 (spawn for player 2) | `arens.admin` |
| `/aareny setpawn <name> [instance]` | Set the respawn position after match end | `arens.admin` |
| `/aareny list` | List all arenas with their status | `arens.admin` |
| `/aareny delete <name> [instance]` | Delete an arena | `arens.admin` |
| `/aareny reload` | Reload configuration based on language setting | `arens.admin` |
| `/aareny help` | Show admin command help | `arens.admin` |

---

## Configuration

### Language System

The plugin supports both Polish and English languages. To change the language:

1. Edit `plugins/Arens/lang.yml`:
   ```yaml
   # PL: Język PL | EN
   # EN: Language PL | EN
   lang: "EN"  # Change to "PL" for Polish
   ```

2. Run `/aareny reload` to regenerate the config file with the selected language

The plugin will automatically:
- Generate `config.yml` with messages in the selected language
- Display help menus in the selected language
- Show all system messages in the selected language

### Database Configuration

#### SQLite (Default)

```yaml
database:
  type: sqlite
  sqlite:
    file: arens.db
```

#### MySQL

```yaml
database:
  type: mysql
  mysql:
    host: localhost
    port: 3306
    database: arens
    username: root
    password: password
```

### Combat Check Integration

Integrates with anti-logout plugins to prevent players from joining arenas while in combat:

```yaml
combat_check:
  enabled: true
  placeholder: "%antylogout_status%"
  blocked_value: "true"
```

When enabled, players with the placeholder value matching `blocked_value` cannot join arena queues.

### GUI Settings

Configure the rewards GUI appearance:

```yaml
gui:
  rewards_size: 54
  next_page_item: ARROW
  prev_page_item: ARROW
  clear_page_item: RED_DYE
  fill_item: GRAY_STAINED_GLASS_PANE
```

### Allowed Commands During Arena

Commands that players can use while in an arena match:

```yaml
allowed_commands:
  - msg
  - r
  - tell
  - reply
```

All other commands are blocked during arena matches.

### Countdown Settings

```yaml
countdown:
  seconds: 3
```

The countdown duration before the match starts after both players are teleported.

---

## Multiple Arena Instances

The plugin supports creating multiple maps (instances) under a single arena name. This allows for variety in gameplay.

### Creating Multiple Instances

```bash
# Create the base arena
/aareny create duel

# Create additional maps
/aareny create duel map1
/aareny create duel map2
/aareny create duel map3
```

### Configuring Each Instance

```bash
# Configure map1
/aareny pos1 duel map1
/aareny pos2 duel map1
/aareny setpawn duel map1

# Configure map2
/aareny pos1 duel map2
/aareny pos2 duel map2
/aareny setpawn duel map2
```

### How It Works

- Players join using `/arena duel` (without specifying the instance)
- The plugin randomly selects an available instance/map
- This provides variety and prevents players from always playing on the same map
- Use `/aareny list` to see all instances and their configuration status

---

## PlaceholderAPI Placeholders

The plugin provides the following placeholders for use in other plugins:

| Placeholder | Description |
|-------------|-------------|
| `%arens_wins%` | Player's total wins |
| `%arens_losses%` | Player's total losses |
| `%arens_total_matches%` | Player's total matches (wins + losses) |
| `%arens_win_rate%` | Player's win rate percentage |
| `%arens_ranking%` | Player's ranking position (1 = highest) |
| `%arens_top_player%` | Name of the top player |
| `%arens_top_wins%` | Wins of the top player |

### Example Usage

```
Scoreboard:
- §aWins: %arens_wins%
- §cLosses: %arens_losses%
- §eWin Rate: %arens_win_rate%%
- §6Rank: #%arens_ranking%
```

---

## Setup Guide

### Basic Setup

1. **Create an arena**
   ```bash
   /aareny create myarena
   ```

2. **Set position 1** (stand at player 1 spawn)
   ```bash
   /aareny pos1 myarena
   ```

3. **Set position 2** (stand at player 2 spawn)
   ```bash
   /aareny pos2 myarena
   ```

4. **Set respawn position** (stand at the respawn location)
   ```bash
   /aareny setpawn myarena
   ```

5. **Test the arena**
   ```bash
   /arena myarena
   ```

### Advanced Setup with Multiple Maps

1. **Create the base arena**
   ```bash
   /aareny create duel
   ```

2. **Create multiple map instances**
   ```bash
   /aareny create duel desert
   /aareny create duel forest
   /aareny create duel ice
   ```

3. **Configure each map**
   ```bash
   # Desert map
   /aareny pos1 duel desert
   /aareny pos2 duel desert
   /aareny setpawn duel desert
   
   # Forest map
   /aareny pos1 duel forest
   /aareny pos2 duel forest
   /aareny setpawn duel forest
   
   # Ice map
   /aareny pos1 duel ice
   /aareny pos2 duel ice
   /aareny setpawn duel ice
   ```

4. **Verify setup**
   ```bash
   /aareny list
   ```

5. **Players can now join**
   ```bash
   /arena duel
   ```
   The plugin will randomly select one of the configured maps.

---

## Permissions

| Permission | Description | Default |
|-----------|-------------|---------|
| `arens.admin` | Access to all admin arena commands | OP |

---

## Troubleshooting

### Plugin not loading

- Ensure you're running Paper/Spigot 1.20.1 or higher
- Check that PlaceholderAPI is installed
- Verify Java 17+ is installed
- Check the server console for error messages

### Arena not found

- Use `/aareny list` to see all configured arenas
- Verify the arena name is spelled correctly
- If using instances, check the instance name

### Players can't join arena

- Verify the arena is fully configured (pos1, pos2, spawn set)
- Check if combat check is blocking the player
- Ensure the player is not already in a queue or match

### Rewards not saving

- Check database connection in config.yml
- Verify the database file exists (for SQLite)
- Check server logs for database errors

### Language not changing

- Verify `lang.yml` has the correct language setting
- Run `/aareny reload` after changing the language
- Check that `config_pl.yml` and `config_en.yml` exist in the plugin folder

---

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Setup

1. Clone the repository
2. Import into your IDE (IntelliJ IDEA recommended)
3. Build with Maven: `mvn clean package`
4. Test on a local Paper server

---

## License

This plugin is created for educational purposes.

---

## Support

- **Issues**: Report bugs and feature requests on [GitHub Issues](https://github.com/yourusername/arens/issues)
- **Discord**: Join our Discord server for support (coming soon)
- **Documentation**: Check this README and the inline code comments

---

## Acknowledgments

- Built with [Paper API](https://papermc.io/)
- Uses [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/)
- Database support via [SQLite JDBC](https://github.com/xerial/sqlite-jdbc) and [MySQL Connector](https://dev.mysql.com/downloads/connector/j/)

---

<div align="center">

**Made with ❤️ for the Minecraft community**

[⬆ Back to Top](#arens-plugin)

# Polish
# Wtyczka Arens

<div align="center">

![Version](https://img.shields.io/badge/wersja-1.0--SNAPSHOT-blue)
![Spigot](https://img.shields.io/badge/Spigot-1.20.1%2B-green)
![Java](https://img.shields.io/badge/Java-17%2B-orange)
![License](https://img.shields.io/badge/licencja-MIT-yellow)

**Kompleksowa wtyczka arena dla Paper/Spigot 1.20.1+**

Funkcje: system kolejek, GUI nagród, ranking, integracja z systemem walki, obsługa wielu języków i wiele instancji aren.

[Funkcje](#funkcje) • [Instalacja](#instalacja) • [Komendy](#komendy) • [Konfiguracja](#konfiguracja) • [Przewodnik konfiguracji](#przewodnik-konfiguracji)

</div>

---

## Spis treści

- [Funkcje](#funkcje)
- [Wymagania](#wymagania)
- [Instalacja](#instalacja)
- [Komendy](#komendy)
  - [Komendy gracza](#komendy-gracza)
  - [Komendy administratora](#komendy-administratora)
- [Konfiguracja](#konfiguracja)
  - [System językowy](#system-językowy)
  - [Konfiguracja bazy danych](#konfiguracja-bazy-danych)
  - [Integracja z systemem walki](#integracja-z-systemem-walki)
  - [Ustawienia GUI](#ustawienia-gui)
  - [Dozwolone komendy](#dozwolone-komendy)
- [Wiele instancji aren](#wiele-instancji-aren)
- [Placeholdery PlaceholderAPI](#placeholdery-placeholderapi)
- [Przewodnik konfiguracji](#przewodnik-konfiguracji)
- [Uprawnienia](#uprawnienia)
- [Rozwiązywanie problemów](#rozwiązywanie-problemów)
- [Współpraca](#współpraca)
- [Licencja](#licencja)

---

## Funkcje

### Funkcje podstawowe

- **🎯 System kolejek aren** - Gracze mogą dołączać do kolejek aren i czekać na przeciwników
- **⚔️ System meczów** - Automatyczne dobieranie graczy z odliczaniem, teleportacją i zamykaniem
- **🎁 GUI nagród** - Paginowane GUI do zbierania wygranych przedmiotów z nawigacją i opcją czyszczenia
- **🗺️ Wiele instancji aren** - Twórz wiele map pod jedną nazwą areny dla różnorodności
- **🌍 Obsługa wielu języków** - Pełna obsługa języka polskiego i angielskiego z łatwym przełączaniem
- **🛡️ Blokowanie komend** - Ogranicza komendy podczas meczów aren (tylko /msg, /r, /tell, /reply dozwolone)
- **⚔️ Integracja z systemem walki** - Integracja z wtyczkami anti-logout przez PlaceholderAPI
- **💾 Obsługa bazy danych** - Obsługa SQLite i MySQL dla statystyk graczy i nagród
- **📊 Rozszerzenie PlaceholderAPI** - Placeholdery dla wygranych, przegranych, rankingu i najlepszych graczy
- **📢 Globalne wiadomości** - Wszystkie wydarzenia aren są transmitowane do wszystkich graczy
- **🎨 Ładne centra pomocy** - Pięknie sformatowane menu pomocy w obu językach
- **🔄 Gorące przeładowanie** - Przeładuj konfigurację bez restartowania serwera

### Funkcje zaawansowane

- **Losowy wybór mapy** - Gdy istnieje wiele instancji, gracze są losowo przydzielani do dostępnych map
- **Serializacja przedmiotów** - Wygrane przedmioty są bezpiecznie przechowywane w bazie danych i można je odebrać później
- **Statystyki graczy** - Śledzenie wygranych, przegranych, łącznej liczby meczów i wskaźnika wygranych
- **System rankingowy** - Automatyczny ranking na podstawie łącznej liczby wygranych
- **Ochrona przed tagiem walki** - Zapobiega dołączaniu graczy do aren podczas walki

---

## Wymagania

- **Serwer**: Paper/Spigot 1.20.1 lub wyższy
- **Wtyczki**: PlaceholderAPI (wymagane)
- **Java**: Java 17 lub wyższa
- **Baza danych**: SQLite (domyślna) lub MySQL

---

## Instalacja

### Ze źródła

1. **Sklonuj repozytorium**
   ```bash
   git clone https://github.com/twojanazwa/arens.git
   cd arens
   ```

2. **Zbuduj wtyczkę używając Maven**
   ```bash
   mvn clean package
   ```

3. **Zainstaluj wtyczkę**
   - Skopiuj wygenerowany plik JAR z `target/arens-1.0-SNAPSHOT.jar`
   - Umieść go w folderze `plugins` swojego serwera

4. **Zrestartuj serwer**

5. **Skonfiguruj wtyczkę**
   - Edytuj `plugins/Arens/lang.yml` aby ustawić preferowany język
   - Uruchom `/aareny reload` aby wygenerować odpowiedni plik konfiguracyjny
   - Skonfiguruj ustawienia w `plugins/Arens/config.yml`

### Z wydania

1. Pobierz najnowsze wydanie ze strony [Releases](https://github.com/twojanazwa/arens/releases)
2. Umieść plik JAR w folderze `plugins` swojego serwera
3. Zrestartuj serwer
4. Skonfiguruj zgodnie z powyższym opisem

---

## Komendy

### Komendy gracza

| Komenda | Opis | Aliasy |
|---------|-------------|---------|
| `/arena <nazwa>` | Dołącz do kolejki dla określonej areny | - |
| `/arena leave` | Opuść bieżącą kolejkę areny | - |
| `/arena help` | Pokaż pomoc komend areny | - |
| `/areny odbierz` | Otwórz GUI nagród aby odebrać wygrane przedmioty (PL) | `/areny collect` (EN) |

### Komendy administratora

| Komenda | Opis | Uprawnienie |
|---------|-------------|------------|
| `/aareny create <nazwa> [instacja]` | Utwórz nową arenę (z opcjonalną instancją/mapą) | `arens.admin` |
| `/aareny pos1 <nazwa> [instacja]` | Ustaw pozycję 1 (spawn dla gracza 1) | `arens.admin` |
| `/aareny pos2 <nazwa> [instacja]` | Ustaw pozycję 2 (spawn dla gracza 2) | `arens.admin` |
| `/aareny setpawn <nazwa> [instacja]` | Ustaw pozycję respawnu po zakończeniu meczu | `arens.admin` |
| `/aareny list` | Lista wszystkich aren ze statusem | `arens.admin` |
| `/aareny delete <nazwa> [instacja]` | Usuń arenę | `arens.admin` |
| `/aareny reload` | Przeładuj konfigurację na podstawie ustawienia języka | `arens.admin` |
| `/aareny help` | Pokaż pomoc komend administratora | `arens.admin` |

---

## Konfiguracja

### System językowy

Wtyczka obsługuje język polski i angielski. Aby zmienić język:

1. Edytuj `plugins/Arens/lang.yml`:
   ```yaml
   # PL: Język PL | EN
   # EN: Language PL | EN
   lang: "EN"  # Zmień na "PL" dla języka polskiego
   ```

2. Uruchom `/aareny reload` aby zregenerować plik konfiguracyjny z wybranym językiem

Wtyczka automatycznie:
- Wygeneruje `config.yml` z wiadomościami w wybranym języku
- Wyświetli menu pomocy w wybranym języku
- Pokaże wszystkie wiadomości systemowe w wybranym języku

### Konfiguracja bazy danych

#### SQLite (Domyślne)

```yaml
database:
  type: sqlite
  sqlite:
    file: arens.db
```

#### MySQL

```yaml
database:
  type: mysql
  mysql:
    host: localhost
    port: 3306
    database: arens
    username: root
    password: password
```

### Integracja z systemem walki

Integruje z wtyczkami anti-logout aby zapobiec dołączaniu graczy do aren podczas walki:

```yaml
combat_check:
  enabled: true
  placeholder: "%antylogout_status%"
  blocked_value: "true"
```

Gdy włączone, gracze z wartością placeholdera pasującą do `blocked_value` nie mogą dołączać do kolejek aren.

### Ustawienia GUI

Skonfiguruj wygląd GUI nagród:

```yaml
gui:
  rewards_size: 54
  next_page_item: ARROW
  prev_page_item: ARROW
  clear_page_item: RED_DYE
  fill_item: GRAY_STAINED_GLASS_PANE
```

### Dozwolone komendy podczas areny

Komendy, które gracze mogą używać podczas meczu areny:

```yaml
allowed_commands:
  - msg
  - r
  - tell
  - reply
```

Wszystkie inne komendy są zablokowane podczas meczów aren.

### Ustawienia odliczania

```yaml
countdown:
  seconds: 3
```

Czas trwania odliczania przed rozpoczęciem meczu po teleportacji obu graczy.

---

## Wiele instancji aren

Wtyczka obsługuje tworzenie wielu map (instancji) pod jedną nazwą areny. Pozwala to na różnorodność w rozgrywce.

### Tworzenie wielu instancji

```bash
# Utwórz bazową arenę
/aareny create duel

# Utwórz dodatkowe mapy
/aareny create duel map1
/aareny create duel map2
/aareny create duel map3
```

### Konfiguracja każdej instancji

```bash
# Skonfiguruj map1
/aareny pos1 duel map1
/aareny pos2 duel map1
/aareny setpawn duel map1

# Skonfiguruj map2
/aareny pos1 duel map2
/aareny pos2 duel map2
/aareny setpawn duel map2
```

### Jak to działa

- Gracze dołączają używając `/arena duel` (bez określania instancji)
- Wtyczka losowo wybiera dostępną instancję/mapę
- Zapewnia to różnorodność i zapobiega graniu zawsze na tej samej mapie
- Użyj `/aareny list` aby zobaczyć wszystkie instancje i ich status konfiguracji

---

## Placeholdery PlaceholderAPI

Wtyczka udostępnia następujące placeholdery do użytku w innych wtyczkach:

| Placeholder | Opis |
|-------------|-------------|
| `%arens_wins%` | Łączna liczba wygranych gracza |
| `%arens_losses%` | Łączna liczba przegranych gracza |
| `%arens_total_matches%` | Łączna liczba meczów gracza (wygrane + przegrane) |
| `%arens_win_rate%` | Procent wskaźnika wygranych gracza |
| `%arens_ranking%` | Pozycja rankingu gracza (1 = najwyższa) |
| `%arens_top_player%` | Nazwa najlepszego gracza |
| `%arens_top_wins%` | Wygrane najlepszego gracza |

### Przykład użycia

```
Tablica wyników:
- §aWygrane: %arens_wins%
- §cPrzegrane: %arens_losses%
- §eWskaźnik wygranych: %arens_win_rate%%
- §6Rank: #%arens_ranking%
```

---

## Przewodnik konfiguracji

### Podstawowa konfiguracja

1. **Utwórz arenę**
   ```bash
   /aareny create myarena
   ```

2. **Ustaw pozycję 1** (stań na spawnie gracza 1)
   ```bash
   /aareny pos1 myarena
   ```

3. **Ustaw pozycję 2** (stań na spawnie gracza 2)
   ```bash
   /aareny pos2 myarena
   ```

4. **Ustaw pozycję respawnu** (stań w lokalizacji respawnu)
   ```bash
   /aareny setpawn myarena
   ```

5. **Przetestuj arenę**
   ```bash
   /arena myarena
   ```

### Zaawansowana konfiguracja z wieloma mapami

1. **Utwórz bazową arenę**
   ```bash
   /aareny create duel
   ```

2. **Utwórz wiele instancji map**
   ```bash
   /aareny create duel desert
   /aareny create duel forest
   /aareny create duel ice
   ```

3. **Skonfiguruj każdą mapę**
   ```bash
   # Mapa pustynna
   /aareny pos1 duel desert
   /aareny pos2 duel desert
   /aareny setpawn duel desert
   
   # Mapa leśna
   /aareny pos1 duel forest
   /aareny pos2 duel forest
   /aareny setpawn duel forest
   
   # Mapa lodowa
   /aareny pos1 duel ice
   /aareny pos2 duel ice
   /aareny setpawn duel ice
   ```

4. **Zweryfikuj konfigurację**
   ```bash
   /aareny list
   ```

5. **Gracze mogą teraz dołączać**
   ```bash
   /arena duel
   ```
   Wtyczka losowo wybierze jedną ze skonfigurowanych map.

---

## Uprawnienia

| Uprawnienie | Opis | Domyślne |
|-----------|-------------|---------|
| `arens.admin` | Dostęp do wszystkich komend administratora aren | OP |

---

## Rozwiązywanie problemów

### Wtyczka nie ładuje się

- Upewnij się, że używasz Paper/Spigot 1.20.1 lub wyższego
- Sprawdź, czy PlaceholderAPI jest zainstalowane
- Zweryfikuj, czy Java 17+ jest zainstalowana
- Sprawdź konsolę serwera pod kątem komunikatów o błędach

### Arena nie została znaleziona

- Użyj `/aareny list` aby zobaczyć wszystkie skonfigurowane areny
- Zweryfikuj, czy nazwa areny jest wpisana poprawnie
- Jeśli używasz instancji, sprawdź nazwę instancji

### Gracze nie mogą dołączyć do areny

- Zweryfikuj, czy arena jest w pełni skonfigurowana (pos1, pos2, spawn ustawione)
- Sprawdź, czy system walki blokuje gracza
- Upewnij się, że gracz nie jest już w kolejce lub meczu

### Nagrody nie są zapisywane

- Sprawdź połączenie z bazą danych w config.yml
- Zweryfikuj, czy plik bazy danych istnieje (dla SQLite)
- Sprawdź logi serwera pod kątem błędów bazy danych

### Język się nie zmienia

- Zweryfikuj, czy `lang.yml` ma poprawne ustawienie języka
- Uruchom `/aareny reload` po zmianie języka
- Sprawdź, czy `config_pl.yml` i `config_en.yml` istnieją w folderze wtyczki

---

## Współpraca

Współpraca jest mile widziana! Proszę postępować zgodnie z poniższymi krokami:

1. Fork repozytorium
2. Utwórz gałąź funkcji (`git checkout -b feature/amazing-feature`)
3. Zatwierdź zmiany (`git commit -m 'Add amazing feature'`)
4. Wypchnij do gałęzi (`git push origin feature/amazing-feature`)
5. Otwórz Pull Request

### Konfiguracja deweloperska

1. Sklonuj repozytorium
2. Zaimportuj do swojego IDE (IntelliJ IDEA zalecane)
3. Zbuduj używając Maven: `mvn clean package`
4. Przetestuj na lokalnym serwerze Paper

---

## Licencja

Ta wtyczka została stworzona do celów edukacyjnych.

---

## Wsparcie

- **Problemy**: Zgłaszaj błędy i prośby o funkcje na [GitHub Issues](https://github.com/twojanazwa/arens/issues)
- **Discord**: Dołącz do naszego serwera Discord dla wsparcia (wkrótce)
- **Dokumentacja**: Sprawdź ten README i komentarze w kodzie

---

## Podziękowania

- Zbudowano z [Paper API](https://papermc.io/)
- Używa [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/)
- Obsługa bazy danych przez [SQLite JDBC](https://github.com/xerial/sqlite-jdbc) i [MySQL Connector](https://dev.mysql.com/downloads/connector/j/)

---

<div align="center">

**Stworzono z ❤️ dla społeczności Minecraft**

[⬆ Powrót do góry](#wtyczka-arens)

</div>

</div>

