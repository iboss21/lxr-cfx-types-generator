```
    ██╗     ██╗  ██╗██████╗        ██████╗ ██████╗ ██████╗ ███████╗
    ██║     ╚██╗██╔╝██╔══██╗      ██╔════╝██╔═══██╗██╔══██╗██╔════╝
    ██║      ╚███╔╝ ██████╔╝█████╗██║     ██║   ██║██████╔╝█████╗
    ██║      ██╔██╗ ██╔══██╗╚════╝██║     ██║   ██║██╔══██╗██╔══╝
    ███████╗██╔╝ ██╗██║  ██║      ╚██████╗╚██████╔╝██║  ██║███████╗
    ╚══════╝╚═╝  ╚═╝╚═╝  ╚═╝       ╚═════╝ ╚═════╝ ╚═╝  ╚═╝╚══════╝
```

<div align="center">

# 🐺 LXR Cfx Lua Type Generator

### **Automatically Generate Lua Type Definitions for FiveM/RedM Server Exports**

**The Land of Wolves | Georgian RP 🇬🇪 | მგლების მიწა - რჩეულთა ადგილი!**
*ისტორია ცოცხლდება აქ!* (History Lives Here!)

[![License](https://img.shields.io/badge/License-MIT-green.svg)](license)
[![Version](https://img.shields.io/badge/Version-1.0.0-blue.svg)](package.json)
[![Node](https://img.shields.io/badge/Node-%3E%3D16.0.0-brightgreen.svg)](https://nodejs.org)
[![RedM](https://img.shields.io/badge/RedM-Compatible-orange.svg)](https://redm.net)
[![FiveM](https://img.shields.io/badge/FiveM-Compatible-orange.svg)](https://fivem.net)

[💬 Discord](https://discord.gg/CrKcWdfd3A) • [🌐 Website](https://www.wolves.land) • [💻 GitHub](https://github.com/iBoss21) • [🛒 Store](https://theluxempire.tebex.io)

</div>

---

## 📋 Overview

Automatically generate Lua type definitions for your FiveM/RedM server resource exports. This tool scans your server files and creates IntelliSense type definitions for resource exports, Cfx GlobalState, LocalPlayer and Player state variables, giving you autocomplete and type checking in VS Code.

### 🎯 Built For

- **RedM Servers** — Full support for RedM resources and frameworks
- **FiveM Servers** — Full support for FiveM resources and frameworks
- **Multi-Framework** — Works with LXR-Core, RSG-Core, VORP Core, and more

---

## ✨ Features

<table>
<tr>
<td width="50%">

### 📝 **Type Generation**
- ✅ Scans all Lua files in your server
- ✅ Parses LuaDoc comments (`---@param`, `---@return`, etc.)
- ✅ Detects function-based and inline exports
- ✅ Separates client, server, and shared exports
- ✅ Generates type definitions for GlobalState
- ✅ Generates type definitions for Player and LocalPlayer state bags

</td>
<td width="50%">

### ⚡ **Developer Experience**
- ✅ Full VS Code IntelliSense support
- ✅ Autocomplete for resource exports
- ✅ Configurable via JSON config file
- ✅ Exclude patterns for fine-grained control
- ✅ Verbose mode for debugging
- ✅ Zero runtime dependencies on your server

</td>
</tr>
</table>

### 🔧 Multi-Framework Support

| Framework | Support Level | Status |
|-----------|---------------|--------|
| **LXR-Core** | Primary | ✅ Full Support |
| **RSG-Core** | Primary | ✅ Full Support |
| **VORP Core** | Supported | ✅ Compatible |
| **QBCore** | Optional | 📦 If Detected |
| **Standalone** | Fallback | ✅ Basic Features |

---

## 🚀 Installation

```bash
npm install
```

---

## ⚙️ Basic Usage

1. Configure `inputDir` in `config.json` to your server resources path
2. Run the type generator:

```bash
npm start
```

This will scan all Lua files and generate type definitions in the `./types` directory.

### Configuration

Edit the `config.json` file in your project root:

```json
{
  "inputDir": "./resources",
  "outputDir": "./types",
  "excludePatterns": [
    "node_modules/**",
    "types/**"
  ],
  "verbose": false
}
```

#### Configuration Options

- **inputDir**: Directory to scan for Lua files *(normally your server resources folder)*
- **outputDir**: Where to output generated type files
- **excludePatterns**: Glob patterns to exclude from scanning
- **verbose**: Show detailed output during generation

### Using Generated Types

Now that we generated the types, we need to add them to your VS Code settings:

1. Install the following extensions
    - [Lua](https://marketplace.visualstudio.com/items?itemName=sumneko.lua)
    - [CfxLua IntelliSense](https://marketplace.visualstudio.com/items?itemName=communityox.cfxlua-vscode-cox)
2. Open your VS Code settings (JSON)
3. Add the types directory to `Lua.workspace.library`:

```json
{
  "Lua.workspace.library": [
      // EXISTING LIBRARIES HERE
      "C:/path/to/type-gen/types",
  ],
}
```

---

## 📖 Supported Patterns

### Export Patterns

#### Function-Based Export

```lua
--- Adds or updates multiple jobs in shared/jobs.lua.
--- @param newJobs table<string, table> A table where keys are job names
--- @param commitToFile boolean Whether to commit the job data
--- @return boolean success Whether all jobs were successfully created
--- @return string? message An optional message
function CreateJobs(newJobs, commitToFile)
    -- Implementation
end

exports('CreateJobs', CreateJobs)
```

#### Inline Export

```lua
exports('CreateJobs', function(newJobs, commitToFile)
    -- Implementation
end)
```

### State Patterns

#### GlobalState

```lua
GlobalState.weather = "sunny"
GlobalState.policeOnDuty = 5
GlobalState.heistCooldown = true
```

#### Player States

```lua
--SERVER
Player(source).state:set("isLoggedIn", true, true)
Player(playerId).state.invBusy = false

--CLIENT
LocalPlayer.state:set("inv_busy", false, true)
LocalPlayer.state.dead = true
```

---

## 📤 Example Output

The generator creates type definition files for both exports and state bags:

### Export Types

**types/my_resource/server.lua**
```lua
---@meta

---**`server`**
---Adds or updates multiple jobs in shared/jobs.lua.
---@param newJobs table<string, table> A table where keys are job names
---@param commitToFile boolean Whether to commit the job data
---@return boolean success Whether all jobs were successfully created
---@return string? message An optional message
function exports.my_resource:CreateJobs(newJobs, commitToFile) end
```

---

## 🔍 How It Works

1. **Scanning**: Recursively finds all `.lua` files in the input directory (skipping folders matching the excludePatterns config)
2. **Parsing**: Extracts `exports()` calls, GlobalState assignments, and Player/LocalPlayer state operations
3. **Documentation**: Parses LuaDoc comments (`---@param`, `---@return`, etc.) for exports
4. **Type Inference**: Infers types from assigned values for state variables
5. **Context Detection**: Determines if exports are client, server, or shared based on file paths
6. **State Aggregation**: Combines state definitions from all resources into unified interfaces
7. **Generation**: Creates properly formatted Lua type definition files

---

## 💡 Tips

- Use clear LuaDoc comments for best results with exports
- Follow consistent naming conventions
- Place client-side code in files containing "client" in the name
- Place server-side code in files containing "server" in the name
- Shared code goes in files containing "shared" or neither
- See [qbx_core/server/functions.lua](https://github.com/Qbox-project/qbx_core/blob/main/server/functions.lua) for an example of "good" LuaDoc documentation

---

## 🏗️ Architecture

```
lxr-cfx-types-generator/
├── config.json             # Configuration file
├── package.json            # Node.js package manifest
├── src/
│   ├── index.js            # Main entry point
│   ├── parser.js           # Lua file parser
│   ├── generator.js        # Type definition generator
│   └── state_generator.js  # StateBag type generator
└── types/                  # Generated output (gitignored)
```

---

## 💡 Support

Need help? We're here for you!

- 💬 **Discord:** [Join our community](https://discord.gg/CrKcWdfd3A)
- 🐛 **Issues:** [Report bugs](https://github.com/iboss21/lxr-cfx-types-generator/issues)
- 🛒 **Store:** [The Lux Empire Store](https://theluxempire.tebex.io)

---

## 🐺 The Land of Wolves

<div align="center">

**Server Information**

🌍 **The Land of Wolves 🐺**
Georgian RP 🇬🇪 | მგლების მიწა - რჩეულთა ადგილი!
*ისტორია ცოცხლდება აქ!* (History Lives Here!)

**Type:** Serious Hardcore Roleplay
**Access:** Discord & Whitelisted

---

**Links**

[🌐 Website](https://www.wolves.land) •
[💬 Discord](https://discord.gg/CrKcWdfd3A) •
[🎮 Server](https://servers.redm.net/servers/detail/8gj7eb) •
[🛒 Store](https://theluxempire.tebex.io) •
[💻 GitHub](https://github.com/iBoss21)

</div>

---

## 📜 Credits

**Developer:** iBoss21 / The Lux Empire
**Original Author:** ihyajb
**Inspired by:** [ox_types](https://github.com/overextended/ox_types)

---

## 🏷️ Tags

`RedM` `FiveM` `LXR-Core` `RSG-Core` `VORP` `Lua` `Types` `IntelliSense` `LuaDoc` `TypeGeneration` `wolves.land`

---

<div align="center">

## 📄 License

MIT

© 2026 iBoss21 / The Lux Empire | wolves.land | All Rights Reserved

**Made with ❤️ by The Lux Empire for The Land of Wolves 🐺**

</div>
