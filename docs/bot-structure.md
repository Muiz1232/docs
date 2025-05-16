# Bot Structure

Understanding the structure of a TBL bot is essential for developing effective and maintainable bots. This document explains the key components and how they work together.

## Core Components

A TBL bot consists of several core components:

1. **Commands**: Functions that respond to user inputs
2. **Properties**: Persistent data storage
3. **Update Handler**: Processes incoming updates from Telegram
4. **Libraries**: Reusable code modules
5. **Webhook**: Endpoint for receiving messages from Telegram

### Visual Overview

```
┌─────────────────────────────────────────┐
│               TBL Bot                   │
│                                         │
│  ┌─────────────┐      ┌──────────────┐  │
│  │   Commands  │      │  Properties  │  │
│  └─────────────┘      └──────────────┘  │
│                                         │
│  ┌─────────────┐      ┌──────────────┐  │
│  │  Libraries  │      │  Webhook     │  │
│  └─────────────┘      └──────────────┘  │
│                                         │
│        ┌─────────────────────┐          │
│        │    Update Handler   │          │
│        └─────────────────────┘          │
└─────────────────────────────────────────┘
     ▲                           ▼
     │                           │
     │                           │
┌────┴───────────────────────────┴────┐
│            Telegram API             │
└────────────────────────────────────┘
```

## Execution Flow

When a user interacts with your bot, the following sequence occurs:

1. User sends a message or interaction to your bot
2. Telegram sends this as an update to your bot's webhook
3. The update handler processes the incoming update
4. The handler identifies which command should handle the update
5. Special commands (like @) are executed first if applicable
6. The matched command is executed
7. Finally commands (like @@) are executed after if applicable
8. The response is sent back to the user via the Telegram API

## Commands

Commands are the primary building blocks of your bot. Each command has:

- A name (e.g., "/start")
- Optional static text response (Answer)
- Optional TBL code
- Optional keyboard configuration
- Optional aliases
- Configuration options (need_reply, allow_only_group, etc.)

Commands are stored in your bot's database and loaded when needed.

## Properties

Properties provide persistent storage for your bot. There are three types:

1. **Bot Properties**: Global data for your bot
   ```javascript
   Bot.setProp("welcome_message", "Hello!");
   let message = Bot.getProp("welcome_message");
   ```

2. **User Properties**: Data specific to individual users
   ```javascript
   User.setProp("points", 100);
   let points = User.getProp("points");
   ```

3. **Chat Properties**: Data specific to chat groups
   ```javascript
   Chat.setProp("rules", "Be respectful");
   let rules = Chat.getProp("rules");
   ```

Properties are stored in a database and persist across bot restarts.

## Update Handler

The update handler is the core engine that processes incoming updates from Telegram. It:

1. Extracts information from the update (user, chat, message, etc.)
2. Determines which command should handle the update
3. Executes the command with the appropriate context
4. Handles any errors that might occur
5. Manages sessions for multi-step commands

This is mostly hidden from you as a developer, but understanding the flow is important for debugging and advanced usage.

## Libraries

Libraries are reusable code modules that provide specialized functionality. TBL includes several built-in libraries:

- **Random**: For generating random values
- **TgUtil**: Telegram-specific utilities
- **DateTimeFormat**: Date and time formatting

You can also create your own custom libraries.

Libraries are loaded on demand when first accessed:

```javascript
// This loads the Random library
let randomNumber = Libs.Random.randomInt(1, 10);
```

## Webhook

The webhook is the endpoint that receives updates from Telegram. When a user interacts with your bot, Telegram sends an update to your bot's webhook URL.

The webhook endpoint is automatically created and managed for you by the TBL platform. You don't need to configure this manually unless you're hosting your own TBL instance.

## Module Interaction

Here's how the different components interact:

1. Commands use Bot, User, and Chat objects to store and retrieve data
2. Commands use the Api object to interact with the Telegram API
3. Commands can use libraries for specialized functionality
4. Commands can run other commands using Bot.runCommand()
5. Special commands like @ and @@ can modify the behavior of other commands

## File Structure (for Custom Hosting)

If you're hosting your own TBL instance, here's the typical file structure:

```
my-bot/
├── app.js                  # Main application file
├── Bot.js                  # Bot object implementation
├── Api.js                  # Api object implementation
├── User.js                 # User object implementation
├── Message.js              # Message handling
├── commandHandeler.js      # Command execution
├── Libs/                   # Libraries
│   ├── libs.js             # Library loader
│   └── libs/               # Individual libraries
│       ├── random.js       # Random library
│       ├── TgUtil.js       # Telegram utilities
│       └── dateTimeformat.js  # Date formatting
└── package.json            # Dependencies
```

## Best Practices

1. **Organize commands logically**: Group related commands together
2. **Use libraries for common functionality**: Don't repeat code across commands
3. **Store configuration in properties**: Makes it easy to update without changing code
4. **Use the master command (@)** for setup and authorization checks
5. **Use the error command (!)** for consistent error handling
6. **Document your commands**: Makes maintenance easier

## Next Steps

Now that you understand the structure of a TBL bot, you're ready to:

1. [Create your first bot](getting-started.md)
2. Learn more about [commands](commands.md)
3. Explore the [Bot object](bot-object.md) and [API object](api-object.md)
4. Discover the available [libraries](libraries.md) 