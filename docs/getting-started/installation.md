# Installation

You only need two things to run Pipeline: a JRE and the `pipeline.jar` launcher.

## Requirements

- Java 21 or newer
- Internet access on first run

Pipeline will download and patch the vanilla Minecraft server automatically. You don't need to grab anything else.

## Download and Run

1. Get `pipeline.jar`. The source code is not public yet, so ask the maintainer for a build. If you have the source, build it with `./gradlew build` (the JAR lands in `build/libs/pipeline.jar`).
2. Put it in an empty folder.
3. Run:

```bash
java -jar pipeline.jar
```

On first launch, Pipeline downloads the Mojang server bundle into `cache/`, extracts the real server JAR and libraries, patches `MinecraftServer.class`, and starts the vanilla server. After that, launches are faster because everything is cached locally.

## Choosing a Minecraft Version

Pipeline defaults to `26.1.2`. To pick another Minecraft version, pass `--version <mc version>`:

```bash
java -jar pipeline.jar --version 26.1.2
```

You can also pass a path to an already-downloaded server JAR as the first argument.

## What Gets Created

`run/` below is just the folder you launched from; Pipeline uses your current working directory.

```
run/
├── pipeline.jar                 # the launcher
├── pipeline.json                # optional server config (storage backend)
├── cache/
│   ├── mojang_26.1.2.jar        # downloaded bundle
│   ├── versions/26.1.2/
│   │   ├── server-26.1.2.jar          # extracted vanilla server JAR
│   │   └── patched-server-26.1.2.jar  # patched copy that actually runs
│   └── libraries/               # extracted libraries
├── pipeline/
│   ├── permissions.json         # persistent permission grants
│   └── permission-requests.json # pending/resolved approval requests
├── storage/                     # pack storage (JSON backend default)
├── resourcepacks/               # generated from registry items
├── world/datapacks/             # your Pipeline packs go here
└── logs/
    └── latest.log               # vanilla server log + Pipeline output
```

Pipeline does not write a separate log file; its messages are merged into `logs/latest.log`.

## Adding Packs

Drop your Pipeline packs into `run/world/datapacks/` just like normal data packs. Pipeline scans them after the world is created, reads each `pipeline.json`, and loads the `.pipe` files.

If the server is already running, reload with:

```
/pipeline reload
```

Reload picks up events, schedules, storage schemas, permissions, inventories, registry entries, and network packets. New or changed commands are only registered at startup, so restart the server for those. Vanilla's own `/reload` also triggers a Pipeline reload.

## Minimum Versions

- Pipeline: `0.3.0`
- Minecraft: `26.1` (the first unobfuscated release)
