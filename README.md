# AzerothCore Module: mod-ollama-chat

## Overview

mod-ollama-chat is an AzerothCore module that enhances the built-in Player Bots system by integrating external language model (LLM) support via the Ollama API. This module enables player bots to generate dynamic, in-character chat responses using advanced natural language processing. Bots are enriched with personality traits, random chatter triggers, and context-aware replies that mimic the language and lore of World of Warcraft.

## Features

- Ollama LLM Integration:
  Bots generate chat responses by querying an external Ollama API endpoint. This enables natural and contextually appropriate in-game dialogue.

- Player Bot Personalities:
  When enabled, each bot is assigned a personality type (e.g., Gamer, Roleplayer, Trickster) that modifies its chat style. Personalities influence prompt generation and result in varied, immersive responses.

- Context-Aware Prompt Generation:
  The module gathers extensive context about both the bot and the interacting player—including class, race, role, faction, guild, and more—to generate prompts for the LLM. A comprehensive WoW cheat sheet is appended to every prompt to ensure the LLM replies with accurate lore, terminology, and in-character language spanning Vanilla WoW, The Burning Crusade, and Wrath of the Lich King.

- Random Chatter:
  Bots periodically initiate random, environment-based chat when a real player is nearby. This feature adds an extra layer of immersion to the game world.

- Blacklist for Playerbot Commands:
  A configurable blacklist prevents bots from responding to chat messages that start with common playerbot command prefixes, ensuring that administrative commands are not inadvertently processed.
  Additional commands can be appended via the configuration.

- Asynchronous Response Handling:
  Chat responses are generated on separate threads to avoid blocking the main server loop, ensuring smooth server performance.

## Installation

1. Prerequisites:
   - Ensure you have liyunfan1223's AzerothCore (https://github.com/liyunfan1223/azerothcore-wotlk) installation with the Player Bots (https://github.com/liyunfan1223/mod-playerbots) module enabled.
   - The module depends on:
       - cURL (https://curl.se/libcurl/)
       - fmtlib (https://github.com/fmtlib/fmt)
       - nlohmann/json (https://github.com/nlohmann/json)
       - Ollama LLM support – set up a local instance of the Ollama API server with the model of your choice. More details at https://ollama.com

2. Clone the Module:
   Clone the repository into your AzerothCore modules directory:
   cd /path/to/azerothcore/modules
   git clone https://github.com/DustinHendrickson/mod-ollama-chat.git

3. Recompile AzerothCore:
   Navigate to your AzerothCore root directory and compile:
   cd /path/to/azerothcore
   mkdir build && cd build
   cmake ..
   make -j$(nproc)

4. Configuration:
   Copy the default configuration file to your server configuration directory and change to match your setup (if not already done):
   cp /path/to/azerothcore/modules/mod-ollama-chat/mod-ollama-chat.conf.dist /path/to/azerothcore/etc/config/mod-ollama-chat.conf

5. Restart the Server:
   Restart your worldserver to load the new module:
   ./worldserver

## Configuration Options

All configuration options for mod-ollama-chat are defined in mod-ollama-chat.conf. Key settings include:

- OllamaChat.SayDistance:
  Description: Maximum distance (in game units) a bot must be within to reply on a Say message.
  Default: 30.0

- OllamaChat.YellDistance:
  Description: Maximum distance for Yell messages.
  Default: 100.0

- OllamaChat.GeneralDistance:
  Description: Maximum distance for custom chat channels.
  Default: 600.0

- OllamaChat.PlayerReplyChance:
  Description: Percentage chance that a bot replies when a real player speaks.
  Default: 90

- OllamaChat.BotReplyChance:
  Description: Percentage chance that a bot replies when another bot speaks.
  Default: 10

- OllamaChat.MaxBotsToPick:
  Description: Maximum number of bots randomly chosen to reply when no bot is directly mentioned.
  Default: 2

- OllamaChat.Url:
  Description: URL of the Ollama API endpoint.
  Default: http://localhost:11434/api/generate

- OllamaChat.Model:
  Description: The model identifier for the Ollama API query.
  Default: llama3.2:1b

- OllamaChat.EnableRandomChatter:
  Description: Enable or disable random chatter from bots.
  Default: 1 (true)

- OllamaChat.MinRandomInterval:
  Description: Minimum interval (in seconds) between random bot chat messages.
  Default: 45

- OllamaChat.MaxRandomInterval:
  Description: Maximum interval (in seconds) between random bot chat messages.
  Default: 180

- OllamaChat.EnableRPPersonalities:
  Description: Enable distinct roleplay personalities for bots.
  Default: 0 (false)

- OllamaChat.RandomChatterRealPlayerDistance:
  Description: Maximum distance (in game units) a real player must be within to trigger random chatter.
  Default: 40.0

- OllamaChat.RandomChatterBotCommentChance:
  Description: Percentage chance that a bot adds a random comment when random chatter is triggered.
  Default: 25

- OllamaChat.BlacklistCommands:
  Description: Comma-separated list of command prefixes that should be ignored by bots.
  Default: 

## How It Works

1. Chat Handling:
   When a player sends a chat message, the module determines the channel type and checks if the message starts with any blacklisted command prefix. If it does, the bot will not respond.

2. Prompt Generation:
   For eligible messages, the module gathers detailed context about both the bot and the player (including class, race, faction, guild, and more) and appends a comprehensive WoW cheat sheet to the prompt. The cheat sheet covers lore, terminology, key locations, and communication style from Vanilla WoW, The Burning Crusade, and Wrath of the Lich King.

3. LLM Query:
   The generated prompt is sent to the Ollama API asynchronously. The API’s response is then routed back through the appropriate chat channel by the bot.

4. Personality & Random Chatter:
   Bots have assigned personality types that affect their responses. Additionally, bots may randomly generate environment-based chatter when real players are nearby.

## Debugging

For detailed logs of bot responses, prompt generation, and LLM interactions, enable debug mode via your server logs or module-specific settings.

## License

This module is released under the GNU GPL v2 license, consistent with AzerothCore's licensing.

## Contribution

Developed by Dustin Hendrickson

Pull requests, bug reports, and feature suggestions are welcome. Please adhere to AzerothCore's coding standards and guidelines when submitting contributions.
