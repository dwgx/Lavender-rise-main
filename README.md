# Lavender (RISE)

<!-- dwgx-banner:BEGIN -->
<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="docs/assets/banner.svg?t=31bf577e5f5f" />
  <source media="(prefers-color-scheme: light)" srcset="docs/assets/banner-light.svg?t=31bf577e5f5f" />
  <img src="docs/assets/banner.svg?t=31bf577e5f5f" width="100%" alt="Lavender-rise-main — 很久以前做梦做的 Minecraft 客户端 · 早已停更，为了当年造它的人重新发布" />
</picture>

<br/>

Java · AGPL-3.0 · ★10

[issues](https://github.com/dwgx/Lavender-rise-main/issues)

</div>
<!-- dwgx-banner:END -->


**Minecraft 1.8.x 模块化作弊客户端 | Modular cheat client for Minecraft 1.8.x**

---

## Overview / 概述

Lavender is a modular Minecraft 1.8.x cheat client (internal codename `RISE`). Modules, Components, and Commands are auto-discovered via reflection at startup. The codebase is built on MCP-decompiled Minecraft source with ViaMCP for multi-version protocol support.

Lavender 是一个基于 Minecraft 1.8.x 的模块化作弊客户端（内部代号 `RISE`）。模块（Module）、组件（Component）、命令（Command）在启动时通过反射自动扫描注册。底层基于 MCP 反编译的 Minecraft 源码，集成 ViaMCP 实现多版本协议兼容。

这是作者早年想做的客户端，搁置多年后重新公开——留给当年一起做它的人，也留着当成长记录。

## Features / 功能

All modules come from actual classes in `src/java/com/alan/clients/module/impl/`:

所有功能均来自仓库中真实存在的模块类：

| Category | Modules |
|----------|---------|
| **Combat** | KillAura, TeleportAura, Criticals, Velocity, Regen, AutoWeapon, ArmorBreak, SuperKnockback, AntiBot, ProjectileAura |
| **Movement** | Flight, Speed, Strafe, Step, NoSlow, Phase, LongJump, Jesus, WallClimb, Teleport, InventoryMove |
| **Player** | Scaffold, AutoPot, AutoSoup, AutoTool, FastEat, NoFall, Blink, Breaker, Stealer, AntiVoid, AutoPearl |
| **Render** | ESP, NameTags, Tracers, Radar, ClickGUI, XRay, FreeCam, FreeLook, HUD (CPS/FPS/BPS counters, ArmorHud, InventoryHud, KeyStrokes, ScoreBoard) |
| **Ghost** | AimAssist, AutoClicker, Reach, HitBox, WTap, FastPlace, SafeWalk |
| **Exploit** | Crasher, Disabler, PingSpoof, NoRotate, KeepContainer, LightningTracker |
| **Other** | AntiCheat, ClientSpoofer, MusicPlayer, Translator, Spammer, FakePlayer, Nuker, Hypixel/Hyt helpers |

Other engineering features: custom ClickGUI with theme system (ThemeManager), config management (ConfigManager/ConfigFile), Alt account manager (AltManager), localization (default `EN_US`), EventBus, and integrated ViaMCP for multi-protocol version support.

其它工程能力：自定义 ClickGUI 与主题系统、配置文件管理、Alt 账号管理、本地化支持、事件总线、ViaMCP 多协议兼容。

## Tech Stack / 技术栈

- **Language:** Java
- **Platform:** Minecraft 1.8.x client
- **Base:** MCP (Mod Coder Pack) decompiled `net.minecraft.*` source
- **Dependencies:** Lombok, ViaMCP, LWJGL / OpenAL / JInput (native libs in `natives/`)
- **Native libraries:** `natives/windows/` and `natives/linux/` (LWJGL, OpenAL, JInput, Twitch SDK)

<!-- TODO: 仓库未包含 Gradle/Maven 构建脚本，第三方库的确切版本无法从清单确认。 -->

## Project Structure / 项目结构

```text
Lavender-rise-main/
├─ src/
│  ├─ java/
│  │  ├─ Start.java                 # Entry point (assembles MC launch args)
│  │  ├─ com/alan/clients/          # Client core
│  │  │  ├─ Client.java             # Main class (enum singleton), init & register
│  │  │  ├─ Loader.java / Type.java # Load entry / client type enum
│  │  │  ├─ module/                 # Module system (combat/movement/player/render/...)
│  │  │  ├─ component/              # Component system (HUD, render, event)
│  │  │  ├─ command/                # Command system
│  │  │  ├─ ui/ · manager/ · util/  # GUI/themes, managers, utilities
│  │  │  └─ domcer/ · hyt/          # Domcer packet handling, Hyt/Hypixel
│  │  ├─ com/diaoling/network/      # Network/Socket/user management
│  │  ├─ net/                       # Minecraft (net.minecraft.*) source
│  │  └─ util/
│  └─ resources/
│     ├─ assets/                    # Game assets
│     └─ META-INF/MANIFEST.MF       # Main-Class: net.minecraft.client.main.Main
├─ natives/                         # Windows & Linux native libraries
├─ long.jpeg · long1.jpeg · main.png # Historical screenshots
└─ LICENSE
```

## Getting Started / 快速开始

This repo has no Gradle/Maven build scripts. You set it up manually in an IDE.

仓库内没有构建脚本，需要在 IDE 里手动配置。

### Prerequisites / 前置条件

- JDK 8
- IntelliJ IDEA (recommended; launch configs use `$MODULE_DIR$` variable)
- Dependencies you provide yourself: Lombok, ViaMCP, Minecraft 1.8.x libraries & assets

### Build & Run / 构建与运行

1. Import project in IDE. Mark `src/java` as source root, `src/resources` as resource root.
2. Add dependencies manually: Lombok, ViaMCP, Minecraft 1.8.x libs and assets.
3. Run via `Start.java` (assembles launch args) or directly via `net.minecraft.client.main.Main`.
4. Set native library path:

```
-Djava.library.path="$MODULE_DIR$/natives/windows"
```

Launch args from `Start.java`:

```
--version Lavender --accessToken 0 --assetsDir assets --assetIndex 1.8 --userProperties {}
```

<!-- TODO: 完整的依赖列表与可复现的构建步骤未包含在仓库中。 -->

## Configuration / 配置

- Config saved to `latest.json` in client config directory (read/written by `ConfigManager` / `ConfigFile`, flushed on `Client#terminate`).
- Language controlled by `Locale`, defaults to `EN_US`.
- Alt accounts, Insults, etc. loaded from local files by their respective managers.

配置保存在客户端目录下的 `latest.json`，由 ConfigManager 管理，关闭时自动写回。语言默认 `EN_US`。Alt 账号等由对应 Manager 从本地文件加载。

<!-- TODO: 未发现从环境变量读取的显式配置项。 -->

## Status / 状态

Archived legacy project. Version constant: `1.0`, date `2 12, 2024`. Not actively maintained.

历史归档项目，版本号 `1.0`，日期 `2 12, 2024`。不再活跃维护。

## License / 许可证

AGPL-3.0. See [LICENSE](./LICENSE).

## Acknowledgements / 致谢

感谢当年一起做这个项目的朋友。仓库保留了那段历史。

Thanks to everyone who worked on this back in the day.
