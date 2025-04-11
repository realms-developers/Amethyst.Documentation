# Introduction

Welcome to the Amethyst documentation homepage!

Amethyst is a powerful and optimized core for Terraria servers.

## How to Start the Server?

First, understand what server profiles are:

- Profiles allow storing multiple datasets in a single directory, organized by server name.
- This simplifies server management, eliminates the need to repeatedly update plugins, modules, or the core across multiple servers, and reduces disk clutter.

If this is still unclear: Amethyst does not use direct configuration like vanilla servers or TShock (`tshock/...`). Instead, it uses server names (`data/<profile>/...`).

To start the server, first decide on a server name, e.g., `MyServer`:

### Windows
Open a command prompt in the root folder and run:
```bat
start.bat -profile MyServer
```

### Linux
Open a terminal in the root directory and run:
```sh
./start.sh -profile MyServer
```

## Launch Arguments
| Argument            | Values / Parameters                                | Description                                                            |
|---------------------|----------------------------------------------------|------------------------------------------------------------------------|
| `-profile`          | `<profile>` (Required)                            | Load a server profile.                                                 |
| `-deflang`          | `ru-RU`, `en-US`                                  | Set the server's default language.                                      |
| `-genevil`          | `-1` (random), `0` (Corruption), `1` (Crimson) [Default: -1] | World evil type during generation.                                     |
| `-gengamemode`      | `0` (Classic), `1` (Expert), `2` (Master), `3` (Creative) [Default: 0] | World game mode during generation.                               |
| `-genwidth`         | `4200`, `6400`, `8400`                             | World width during generation.                                         |
| `-genheight`        | `1200`, `1800`, `2400`                             | World height during generation.                                        |
| `-worldpath`        | `<worldpath>`                                      | Path to load a world file.                                             |
| `-worldrecreate`    | (Flag)                                            | Generate a new world on server startup.                                |
| `-netport`          | `<netport>` [Default: `7777`]                     | Server port.                                                           |
| `-netslots`         | `<netslots>` [Default: `8`]                       | Maximum number of player slots.                                        |
| `-debugmode`        | (Flag)                                            | Enable debug mode (extended logging and test commands).                |
| `-ssc`              | (Flag)                                            | Enable server-side characters (store player data on the server).       |
| `--version`         | (Flag)                                            | Show version information.                                              |
| `-?`, `-h`, `--help`| (Flag)                                            | Show help and usage information.                                       |