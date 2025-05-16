# TBL (Tele Bot Lang)

> A powerful JavaScript-based language for Telegram bot development

## What is TBL?

TBL (Tele Bot Lang) is a specialized programming language built on JavaScript that simplifies Telegram bot development. It provides built-in objects, methods, and libraries specifically designed for creating powerful Telegram bots with minimal code.

## Key Features

- **Simple JavaScript-based syntax** - If you know JavaScript, you already know TBL
- **Built-in Telegram API integration** - No need to manually implement API calls
- **Persistent property storage** - Store and retrieve data with simple methods
- **Command-based structure** - Organize your bot logic into modular commands
- **Powerful libraries** - Use pre-built libraries for common tasks
- **Session management** - Easily handle multi-step interactions
- **Fully hosted solution** - No server setup or deployment needed

## Quick Example

Here's a simple echo bot in TBL:

```javascript
// Command: /start
Bot.sendMessage("Welcome! Send me any message and I'll echo it back to you.");

// Command: *
// This handles any message
Bot.sendMessage(message);
```

## Core Components

TBL bots operate with these key components:

- **Commands** - Functions that respond to user inputs
- **Bot Object** - Methods for bot operations and global data
- **API Object** - Direct access to Telegram's Bot API
- **User Object** - User data management
- **Properties** - Persistent data storage
- **Libraries** - Reusable code modules

## Documentation Structure

Our documentation is organized to help you find what you need:

- [Getting Started](docs/getting-started.md) - Create your first bot
- [Bot Structure](docs/bot-structure.md) - Understand how TBL bots work
- [Core Objects](docs/bot-object.md) - Learn about Bot, API, User objects
- [Commands](docs/commands.md) - Master the command system
- [API Reference](docs/api-reference/messages/README.md) - Explore available API methods
- [Tutorials](docs/tutorials/echo-bot.md) - Follow step-by-step guides

## Installation

TBL is fully hosted, so there's no need to install anything. Simply:

1. Create a Telegram bot with [BotFather](https://t.me/BotFather)
2. Register on the TBL platform
3. Enter your bot token
4. Start coding right in the web interface

## Sample Bot Code

Here's a more complete example of a TBL bot:

```javascript
// Command: /start
// Answer: Welcome to my bot!
Bot.sendKeyboard("Random Number,User Info\nHelp", "Choose an option:");

// Command: Random Number
let number = Libs.Random.randomInt(1, 100);
Bot.sendMessage("Your random number is: " + number);

// Command: User Info
let text = "Your Information:\n";
text += "User ID: " + user.id + "\n";
text += "Name: " + user.first_name;
if(user.last_name){ text += " " + user.last_name; }
if(user.username){ text += "\nUsername: @" + user.username; }
Bot.sendMessage(text);

// Command: Help
Bot.sendMessage("This bot can generate random numbers and display your user information.\n\nCommands:\n/start - Main menu\nRandom Number - Get a random number\nUser Info - See your profile info");
```

## Next Steps

Ready to build your first Telegram bot with TBL?

- [Create your first bot](docs/getting-started.md)
- [Explore the sample bots](docs/tutorials/echo-bot.md)
- [Join our community](#) for help and inspiration

---

[Documentation](SUMMARY.md) | [Getting Started](docs/getting-started.md) | [API Reference](docs/api-reference/messages/README.md) 