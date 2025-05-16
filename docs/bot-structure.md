# Bot Structure

Understanding the structure of a TBL bot is essential for developing effective and maintainable bots. This comprehensive guide explains all components and their interactions in detail.

## Core Components

A TBL bot consists of several core components that work together:

1. **Commands**: Functions that respond to user inputs and messages
2. **Properties**: Persistent data storage for bot, user, and chat data
3. **Update Handler**: Processes incoming updates from Telegram API
4. **Libraries**: Reusable code modules for common functionality
5. **Webhook**: Endpoint for receiving messages from Telegram
6. **API Interface**: Direct access to Telegram Bot API methods

### Visual Overview

```
┌───────────────────────────────────────────────────┐
│                    TBL Bot                         │
│                                                   │
│  ┌─────────────┐      ┌──────────────┐      ┌───────────┐  │
│  │  Commands   │      │  Properties  │      │  Session  │  │
│  └─────────────┘      └──────────────┘      └───────────┘  │
│                                                   │
│  ┌─────────────┐      ┌──────────────┐      ┌───────────┐  │
│  │  Libraries  │      │  Webhook     │      │ Callbacks │  │
│  └─────────────┘      └──────────────┘      └───────────┘  │
│                                                   │
│        ┌─────────────────────────────────┐          │
│        │         Update Handler          │          │
│        └─────────────────────────────────┘          │
└───────────────────────────────────────────────────┘
     ▲                                     ▼
     │                                     │
     │                                     │
┌────┴─────────────────────────────────────┴────┐
│                 Telegram API                   │
└────────────────────────────────────────────────┘
```

## Execution Flow in Detail

When a user interacts with your bot, the following detailed sequence occurs:

1. User sends a message or interaction (button press, inline query, etc.) to your bot
2. Telegram processes this interaction and generates an update object
3. Telegram sends this update to your bot's webhook endpoint
4. The TBL update handler receives and processes the incoming update:
   - Extracts the update type (message, callback_query, inline_query, etc.)
   - Parses user, chat, and message data
   - Initializes context objects (Bot, User, API, etc.)
5. Special command handlers are executed if configured:
   - Master command (`@`) executes first for preprocessing, authentication, etc.
6. The matched command is executed based on:
   - Direct command match (/start, /help, etc.)
   - Alias match (alternative command triggers)
   - Awaiting reply status (for multi-step commands)
   - Default command (`*`) if no other match is found
7. Finally command (`@@`) executes after the main command for post-processing
8. Error command (`!`) executes if any errors occur during processing
9. The response is sent back to the user via the Telegram API
10. Session data and properties are saved to the database

## Commands in Detail

Commands are the primary building blocks of your bot. Each command has these components:

- **Name**: Unique identifier for the command (e.g., "/start", "menu", "help")
- **Answer**: Optional static text response sent when command executes
- **Code**: Optional TBL code to execute for dynamic responses
- **Keyboard**: Optional keyboard configuration for user interaction
- **Aliases**: Optional alternative triggers for the command
- **Configuration Options**:
  - `need_reply`: Set to true to make command await a user reply
  - `allow_only_group`: Command only works in group chats
  - `allow_only_private`: Command only works in private chats
  - `disable_notification`: Sends messages without notification
  - `on_result`: Command to run after this command completes

### Command Example

```javascript
// Command name: /start
// Answer: Welcome to my bot!
// Code:
Bot.sendKeyboard("Option 1, Option 2\nHelp", "Choose an option:");

// Store user's first interaction time
User.setProp("first_visit", (new Date()).toString());

// Track total users who started the bot
let totalUsers = Bot.getProp("total_users") || 0;
Bot.setProp("total_users", totalUsers + 1);
```

### Command Properties Table

| Property | Type | Description | Example |
|----------|------|-------------|---------|
| name | String | Command identifier | "/start" |
| answer | String | Static text response | "Welcome to my bot!" |
| code | String | TBL code to execute | "Bot.sendMessage('Hello');" |
| keyboard | String | Button layout | "Button 1, Button 2\nButton 3" |
| aliases | String | Alternative triggers | "start, begin, hello" |
| need_reply | Boolean | Whether to await reply | true |
| allow_only_group | Boolean | Group-only command | true |
| allow_only_private | Boolean | Private-only command | true |
| disable_notification | Boolean | Silent messages | true |
| on_result | String | Follow-up command | "process_result" |

Commands are stored in your bot's database and loaded when needed, providing efficient execution.

## Properties System in Detail

Properties provide persistent storage for your bot. There are three types of properties, each with specific uses:

1. **Bot Properties**: Global data for your entire bot
   ```javascript
   // Storing global settings
   Bot.setProp("welcome_message", "Hello and welcome!");
   Bot.setProp("maintenance_mode", false);
   Bot.setProp("version", "1.2.3");
   
   // Retrieving global settings
   let message = Bot.getProp("welcome_message");
   let inMaintenance = Bot.getProp("maintenance_mode");
   
   // Using default values
   let currency = Bot.getProp("currency", "USD");  // Returns "USD" if not set
   ```

2. **User Properties**: Data specific to individual users
   ```javascript
   // Storing user preferences
   User.setProp("language", "en");
   User.setProp("notifications", true);
   User.setProp("points", 100);
   
   // Tracking user activity
   let currentPoints = User.getProp("points", 0);
   User.setProp("points", currentPoints + 10);
   User.setProp("last_active", Date.now());
   
   // Retrieving user data
   let userLang = User.getProp("language");
   ```

3. **Chat Properties**: Data specific to chat groups
   ```javascript
   // Storing chat settings
   Chat.setProp("rules", "Be respectful to everyone.");
   Chat.setProp("welcome_new_members", true);
   Chat.setProp("spam_protection", "strict");
   
   // Moderation tracking
   let warningCount = Chat.getProp("user_" + user.id + "_warnings", 0);
   Chat.setProp("user_" + user.id + "_warnings", warningCount + 1);
   
   // Retrieving chat settings
   let chatRules = Chat.getProp("rules");
   ```

### Property Methods

Each property type (Bot, User, Chat) supports these methods:

| Method | Description | Example |
|--------|-------------|---------|
| setProp(name, value) | Store a property | Bot.setProp("mode", "active") |
| getProp(name, default) | Get a property with optional default | User.getProp("points", 0) |
| delProp(name) | Delete a property | Chat.delProp("temporary_data") |
| getAllProp() | Get all properties | let allUserProps = User.getAllProp() |

Properties are stored in a database and persist across bot restarts, server migrations, and code updates.

## Update Handler in Detail

The update handler is the core engine that processes incoming updates from Telegram. It handles:

1. **Update Parsing**: Extracts data from the Telegram update object
   - Message content, type, and metadata
   - User information (ID, name, username)
   - Chat information (ID, type, title)
   - Special update types (callback queries, inline queries)

2. **Command Routing**: Determines which command should handle the update
   - Direct command matches (/start, /help)
   - Text message matching for aliases
   - Callback query data routing
   - Awaiting reply status checks

3. **Context Creation**: Builds the execution context for commands
   - Initializes Bot, User, Chat, and API objects
   - Populates message data
   - Sets up the command environment

4. **Error Handling**: Manages exceptions during command execution
   - Catches and logs errors
   - Routes to error command (!) when configured
   - Prevents bot crashes from code errors

5. **Session Management**: Maintains state for multi-step commands
   - Tracks awaiting reply status
   - Manages command chains
   - Preserves context between interactions

The update handler is mostly hidden from developers but is critical to understand for debugging and advanced usage.

## Libraries in Detail

Libraries are reusable code modules that provide specialized functionality. TBL includes several built-in libraries:

### Random Library

Complete toolkit for generating random values:

```javascript
// Generate random numbers
let randomInteger = Libs.Random.randomInt(1, 100);    // Random integer 1-100
let randomDecimal = Libs.Random.randomFloat(0, 1);    // Random float 0-1

// Work with arrays
let fruits = ["apple", "banana", "orange", "grape"];
let randomFruit = Libs.Random.randomItem(fruits);     // Random array item

// Generate random strings
let password = Libs.Random.randomPassword(10);        // Random 10-char password
let uuid = Libs.Random.randomUUID();                  // Random UUID

// Shuffle arrays
let shuffled = Libs.Random.shuffle(fruits);           // Randomized array order
```

### TgUtil Library

Telegram-specific utilities for common tasks:

```javascript
// Format text
let boldText = Libs.TgUtil.bold("Important message");  // Bold text
let italicText = Libs.TgUtil.italic("Emphasized");     // Italic text
let monospaceCode = Libs.TgUtil.code("var x = 10;");   // Code block
let linkText = Libs.TgUtil.link("Google", "https://google.com");  // Hyperlink

// Parse entities
let entities = Libs.TgUtil.parseEntities(message.text);

// Create mentions
let mention = Libs.TgUtil.mention(user.id, user.first_name);

// HTML/Markdown conversion
let html = Libs.TgUtil.markdownToHtml(markdownText);
```

### DateTimeFormat Library

Comprehensive date and time formatting:

```javascript
// Format current date/time
let now = Libs.DateTimeFormat.format(new Date(), "YYYY-MM-DD HH:mm:ss");

// Format specific date components
let year = Libs.DateTimeFormat.format(date, "YYYY");
let month = Libs.DateTimeFormat.format(date, "MMMM");
let day = Libs.DateTimeFormat.format(date, "dddd");

// Relative time
let relativeTime = Libs.DateTimeFormat.relative(timestamp);  // "2 hours ago"

// Timezone conversion
let localTime = Libs.DateTimeFormat.convert(date, "America/New_York");

// Date calculations
let tomorrow = Libs.DateTimeFormat.addDays(date, 1);
let nextWeek = Libs.DateTimeFormat.addWeeks(date, 1);
```

You can also create your own custom libraries for specialized functionality.

## Webhook System

The webhook is the endpoint that receives updates from Telegram. It serves as the entry point for all interactions with your bot.

### How Webhooks Work

1. When a user interacts with your bot, Telegram sends an update to your webhook URL
2. The webhook endpoint receives the HTTPS request containing the update data
3. The update is processed by the TBL update handler
4. Any responses are sent back to Telegram via the Bot API

### Webhook Configuration

The webhook endpoint is automatically created and managed by the TBL platform. You don't need to configure this manually unless you're hosting your own TBL instance.

For custom hosting, you'll need to:
1. Set up an HTTPS endpoint
2. Register it with Telegram using:
   ```javascript
   Api.setWebhook({
     url: "https://your-domain.com/your-bot-path",
     certificate: "YOUR_PUBLIC_CERTIFICATE",  // Optional
     max_connections: 40,  // Optional
     allowed_updates: ["message", "callback_query"]  // Optional
   });
   ```

3. Implement the update handling logic at your endpoint

## Module Interaction

Here's how the different components interact in a comprehensive workflow:

1. **Update Reception**: Webhook receives update from Telegram
   ```
   Telegram API → Webhook → Update Handler
   ```

2. **Command Execution**: Update handler processes and routes to commands
   ```
   Update Handler → Command (@, main command, @@)
   ```

3. **Data Operations**: Commands interact with properties for data persistence
   ```
   Command → Bot.setProp/User.setProp/Chat.setProp → Database
   ```

4. **API Operations**: Commands use the API object to send responses
   ```
   Command → Api.sendMessage/sendPhoto → Telegram API → User
   ```

5. **Library Usage**: Commands leverage libraries for common functionality
   ```
   Command → Libs.Random/TgUtil/DateTimeFormat → Command logic
   ```

6. **Command Chaining**: Commands can execute other commands
   ```
   Command → Bot.runCommand → Another Command
   ```

7. **Error Handling**: Errors are caught and processed
   ```
   Command (error) → Error Handler → ! Command
   ```

This comprehensive interaction model ensures efficient processing and response to user interactions.

## File Structure (for Custom Hosting)

If you're hosting your own TBL instance, here's the detailed file structure and explanation:

```
my-bot/
├── app.js                  # Main application entry point
│   # Initializes the bot, sets up webhooks, and handles incoming updates
│   
├── Bot.js                  # Bot object implementation
│   # Implements all Bot methods (sendMessage, runCommand, etc.)
│   # Manages bot properties and state
│   
├── Api.js                  # Api object implementation
│   # Provides interface to Telegram Bot API
│   # Handles API requests and responses
│   
├── User.js                 # User object implementation
│   # Manages user data, properties, and user-specific methods
│   
├── Chat.js                 # Chat object implementation
│   # Handles chat data, properties, and chat-specific methods
│   
├── Message.js              # Message handling
│   # Parses and processes message data
│   # Provides message manipulation methods
│   
├── CommandHandler.js       # Command execution
│   # Routes updates to appropriate commands
│   # Manages command execution flow
│   
├── Session.js              # Session management
│   # Tracks multi-step command state
│   # Manages awaiting reply status
│   
├── Properties.js           # Property management
│   # Provides interface to the database
│   # Handles property storage and retrieval
│   
├── Libs/                   # Libraries
│   ├── libs.js             # Library loader
│   │   # Dynamically loads libraries as needed
│   │   
│   └── libs/               # Individual libraries
│       ├── random.js       # Random library
│       │   # Random number generation
│       │   # Array randomization
│       │   # Random strings and UUIDs
│       │   
│       ├── TgUtil.js       # Telegram utilities
│       │   # Text formatting
│       │   # Entity parsing
│       │   # Mention creation
│       │   
│       └── dateTimeFormat.js  # Date formatting
│           # Date manipulation
│           # Timezone handling
│           # Relative time formatting
│           
├── database/               # Database connection and models
│   ├── connection.js       # Database connection setup
│   ├── models/             # Data models
│   │   ├── bot.js          # Bot property model
│   │   ├── user.js         # User property model
│   │   ├── chat.js         # Chat property model
│   │   └── command.js      # Command storage model
│   └── index.js            # Database interface
│   
├── config.js               # Configuration settings
│   # API keys
│   # Database connection
│   # Webhook settings
│   
└── package.json            # Dependencies and scripts
```

This file structure provides a modular, maintainable architecture for TBL bot hosting.

## Best Practices

### 1. Command Organization

Group related commands logically for better maintainability:

- **Main Menu Commands**: Commands that appear in the main menu (/start, /help, /settings)
- **Functional Groups**: Commands related to specific features (e.g., /weather, /forecast, /location for a weather bot)
- **Administrative Commands**: Commands for bot administration (/admin, /broadcast, /stats)
- **User Commands**: Commands for user account management (/profile, /preferences)
- **System Commands**: Special system commands (@, @@, *, !)

### 2. Library Usage

Use libraries for common functionality to keep your code DRY (Don't Repeat Yourself):

```javascript
// Bad practice: repeating code
function command1() {
  let randomNum = Math.floor(Math.random() * 10) + 1;
  // ... command code
}

function command2() {
  let randomNum = Math.floor(Math.random() * 10) + 1;
  // ... command code
}

// Good practice: using libraries
function command1() {
  let randomNum = Libs.Random.randomInt(1, 10);
  // ... command code
}

function command2() {
  let randomNum = Libs.Random.randomInt(1, 10);
  // ... command code
}
```

### 3. Property Management

Store configuration in properties for easy updates without code changes:

```javascript
// Bad practice: hardcoding values
function welcomeMessage() {
  Bot.sendMessage("Welcome to my bot! The current version is 1.2.3");
}

// Good practice: using properties
function setupBot() {
  Bot.setProp("welcome_message", "Welcome to my bot!");
  Bot.setProp("version", "1.2.3");
}

function welcomeMessage() {
  let msg = Bot.getProp("welcome_message");
  let version = Bot.getProp("version");
  Bot.sendMessage(msg + " The current version is " + version);
}
```

### 4. Master Command Usage

Use the master command (@) for setup, authentication, and preprocessing:

```javascript
// Command: @
// Code:
// Track all command usage
let command = params;
let stats = Bot.getProp("command_stats") || {};
stats[command] = (stats[command] || 0) + 1;
Bot.setProp("command_stats", stats);

// Check for banned users
let banned = Bot.getProp("banned_users") || [];
if(banned.includes(user.id)){
  Bot.sendMessage("You are banned from using this bot.");
  return Bot.runCommand("/banned");
}

// Log activity
console.log("User " + user.id + " ran command: " + command);
```

### 5. Error Handling

Use the error command (!) for consistent error handling:

```javascript
// Command: !
// Code:
Bot.sendMessage("⚠️ An error occurred: " + params);

// Log the error
console.error("Error for user " + user.id + ": " + params);

// Track errors
let errors = Bot.getProp("errors") || [];
errors.push({
  user: user.id,
  error: params,
  command: Bot.getCurrentCommand(),
  time: Date.now()
});
Bot.setProp("errors", errors);
```

### 6. Command Documentation

Document your commands thoroughly for easier maintenance:

```javascript
// Command: /weather
// Description: Shows weather for a location
// Parameters: city name or "current" for current location
// Usage examples: 
//   /weather New York
//   /weather current
// Code:
let location = params || User.getProp("default_location");
if(!location){
  Bot.sendMessage("Please specify a location or set your default location with /setlocation");
  return;
}

// Get weather data...
// Display results...
```

### 7. Multi-Step Command Flow

Use need_reply for clear multi-step interactions:

```javascript
// Command: /register
// need_reply: true
// Code:
if(!params){
  // First step
  User.setProp("registration_step", "name");
  Bot.sendMessage("What's your name?");
  return;
}

let step = User.getProp("registration_step");
if(step=="name"){
  // Save name and ask for email
  User.setProp("name", params);
  User.setProp("registration_step", "email");
  Bot.sendMessage("Thanks! Now what's your email?");
  Bot.runCommand("/register");
  return;
}

if(step=="email"){
  // Save email and complete registration
  User.setProp("email", params);
  User.delProp("registration_step");
  Bot.sendMessage("Registration complete!");
  Bot.runCommand("/welcome");
}
```

## Next Steps

Now that you have a comprehensive understanding of TBL bot structure, you're ready to:

1. [Create your first bot](getting-started.md) with step-by-step guidance
2. Learn more about [commands](commands.md) and their capabilities
3. Explore the [Bot object](bot-object.md) and its powerful methods
4. Master the [API object](api-object.md) for direct Telegram API access
5. Understand the [User object](user-object.md) for user data management
6. Leverage built-in [libraries](libraries.md) for common functionality
7. Browse [tutorials](tutorial-echo-bot.md) for practical examples

## API Reference

The API reference is organized by functionality to help you quickly find what you need:

```
/docs/api-reference/
  ├── messages/
  │   ├── send-message.md
  │   ├── edit-message.md
  │   ├── delete-message.md
  │   ├── pin-message.md
  │   ├── forward-message.md
  │   └── ...
  ├── media/
  │   ├── send-photo.md
  │   ├── send-video.md
  │   ├── send-document.md
  │   ├── send-audio.md
  │   └── ...
  ├── chats/
  │   ├── get-chat.md
  │   ├── get-chat-member.md
  │   ├── ban-chat-member.md
  │   ├── unban-chat-member.md
  │   └── ...
  ├── users/
  │   ├── get-user-profile-photos.md
  │   ├── get-user-info.md
  │   └── ...
  ├── inline/
  │   ├── answer-inline-query.md
  │   ├── inline-query-results.md
  │   └── ...
  ├── callbacks/
  │   ├── answer-callback-query.md
  │   ├── callback-data.md
  │   └── ...
  └── ...
```

Each method is documented with full parameters, return values, and examples.

## Types Reference

Complete reference for all Telegram object types:

```
/docs/types/
  ├── message.md
  ├── user.md
  ├── chat.md
  ├── update.md
  ├── inline-query.md
  ├── callback-query.md
  ├── message-entity.md
  ├── input-media.md
  ├── inline-keyboard-markup.md
  ├── reply-keyboard-markup.md
  └── ...
```

Each type includes all properties, methods, and usage examples. 