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

</div>

