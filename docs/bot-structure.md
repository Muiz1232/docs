
# 📦 Commands in Detail - The Building Blocks of Your Bot

In TBL (Tele Bot Lang), **commands** are the fundamental units that define your bot's intelligence and behavior. They control every aspect of interaction - from simple responses to complex conversational flows. Think of commands as the neurons in your bot's artificial brain that determine how it processes information and responds to users.

## 🧱 Anatomy of a Command

Every command in TBL consists of several powerful components that work together:

### Command Name (Required)
The unique identifier that triggers the command. This can be:
- Standard commands like `/start`, `/help`
- Natural language triggers like `hi`, `menu`
- Special system commands (explained in detail later)

### Answer Field (Optional)
The static text response your bot sends immediately when the command executes. This field supports:
- Markdown formatting for rich text
- Multi-line responses
- Simple text-only replies when no dynamic processing is needed

### Code Block (Optional)
The brain of your command where dynamic processing happens:
- Full TBL scripting capabilities
- API calls, data processing, conditional logic
- Can work alongside or instead of static answers
- Supports both synchronous and asynchronous operations

### Keyboard Layout (Optional)
Creates interactive button interfaces:
- Inline keyboards for instant responses
- Custom layouts with row/column control
- Dynamic keyboard generation through code
- Persistent or one-time-use keyboards

### Aliases System (Optional)
Allows multiple triggering phrases for one command:
- Natural language variations (hi, hello, hey)
- Common misspellings or abbreviations
- Multi-language support through alternate triggers

### Need Reply Flag (Optional)
When enabled (`need_reply: true`):
- Bot enters conversational mode after command
- Waits for user's next message
- Creates stateful interactions
- Perfect for questionnaires or multi-step processes

## 🎹 Designing Interactive Keyboards

Keyboards transform your bot from passive responder to interactive interface. The keyboard syntax provides precise layout control:

### Basic Syntax Rules
- **Comma (`,`)** separates buttons on the same row  
  `"Yes,No,Maybe"` creates three side-by-side buttons
- **Newline (`\n`)** starts a new button row  
  `"First row\nSecond row"` creates two vertical buttons
- **Combination** allows complex layouts  
  `"Buy,Info\nCancel,Help"` creates a 2x2 grid

### Advanced Keyboard Features
- Buttons can trigger commands or URLs
- Dynamic keyboards can be generated in code
- Keyboard persistence settings (one-time or sticky)
- Inline keyboards for seamless message integration

Example of a well-structured keyboard:
```js
"Shop, Support\nView Cart, Track Order\n↩ Main Menu"
```

## 🧠 Special Command Types - Beyond Basic Triggers

TBL features several special command types that handle specific scenarios or provide system-level functionality:

### `@` - The Initialization Command
The setup maestro that runs before any other command:
- **Execution Timing**: Immediate bot launch
- **Use Cases**:
  - Global variable initialization
  - Shared function definitions
  - Bot configuration settings
- **Special Characteristics**:
  - Always synchronous
  - No user triggering
  - Creates shared resources

Example initialization:
```js
let botConfig = {
  adminIds: [12345, 67890],
  currency: "USD"
}

function formatPrice(amount) {
  return `${botConfig.currency} ${amount.toFixed(2)}`
}
```

### `!` - The Error Handler
The safety net for your bot:
- **Automatic Triggering** on errors or crashes
- **Fallback Behavior** when normal commands fail
- **Error Context** available in code:
  - Error message
  - Stack trace
  - User context

Typical uses:
- Error logging
- User-friendly failure messages
- Recovery procedures

### `@@` - The Cleanup Command
The post-processor that runs after every interaction:
- **Execution Guarantee**: Runs regardless of success/failure
- **Common Uses**:
  - Conversation analytics
  - Resource cleanup
  - Session logging
  - Follow-up reminders

### `*` - The Universal Fallback
The catch-all when no other command matches:
- **Trigger Condition**: No other command matches
- **Flexible Uses**:
  - Dynamic content handling
  - Natural language processing
  - Context-aware responses
  - Progressive disclosure menus

### `/channel_update` - Channel Specialist
Handles channel-specific events:
- **Automatic Triggering** on channel updates
- **Specialized For**:
  - Channel post analytics
  - Broadcast content handling
  - Subscriber management

### `/inline_query` - Inline Request Handler
Processes inline queries (@botname queries):
- **Unique Features**:
  - Handles real-time search requests
  - Returns dynamic result sets
  - Supports cached results
- **Fallback**: To `*` command if not defined

## 🔐 The Code Field - Where Magic Happens

The code block transforms simple commands into powerful interactions:

### Core Capabilities
- **Full JavaScript Syntax**:
  - Variables, functions, conditionals
  - Loops and iterations
  - Error handling
- **Async/Await Support**:
  - Network requests
  - Database operations
  - File processing
- **Telegram API Access**:
  - Message manipulation
  - User management
  - Media handling

### Code Field Advantages
- **State Management**: Track conversation context
- **Dynamic Responses**: Generate personalized replies
- **Multi-step Flows**: Guide users through processes
- **External Integration**: Connect to databases/APIs

Example showing multiple capabilities:
```js
// Fetch data from external API
const userData = await getUserProfile(ctx.user.id)

// Conditional response
if (userData.premium) {
  reply(`Welcome back VIP member! 🎩`)
  showPremiumMenu()
} else {
  reply(`Hello ${userData.name}! Upgrade to premium?`)
  showUpgradeButton()
}

// Log interaction
logAnalytics('command_executed', ctx)
```

## 🗣️ Aliases - Multiple Personalities

Aliases multiply your command's accessibility:

### Key Benefits
- **Natural Conversation**: `hi`, `hello`, `hey` all work
- **Language Support**: `hola`, `bonjour`, `namaste`
- **Typo Forgiveness**: `helo`, `hlep`, `halp`
- **Command Variations**: `/start`, `begin`, `menu`

Implementation Example:
```
aliases: hii, hey, hello, greetings, sup
```

## 📝 Answer Field - Quick Static Responses

When you need simple replies without code:

### Formatting Features
- **Markdown Support**:
  - *Italics*, **bold**, `code`
  - [Links](https://example.com)
  - Headers, lists, quotes
- **Multi-line Responses**:
  ```
  Line one
  Line two
  ```
- **Emoji Integration**: 🚀 👍 🎉

### Smart Combinations
- Use with code for hybrid responses
- Can be conditionally overridden
- Supports basic variable interpolation

Example with markdown:
```
*Welcome* to **Bot World**!

Choose an option:
- 🛍️ Shop
- ℹ️ Info
- 🆘 Help
```

## � Execution Models - Sync vs Async

TBL intelligently handles execution timing:

### Synchronous Execution
- Immediate processing
- Simple operations
- Used in `@` command
- Blocking but predictable

### Asynchronous Execution
- Non-blocking operations
- API/database calls
- Parallel processing
- Most commands support await

Example showing async/await:
```js
// Async data fetching
const data = await fetch('https://api.example.com/data')

// Process response
if (data.status === 'success') {
  reply(`Data received: ${data.value}`)
} else {
  reply(`Error: ${data.message}`)
}
```

## 🎁 Command Design Best Practices

### Structure Recommendations
1. **Clear Naming**: Use intuitive command names
2. **Modular Design**: Split complex flows into sub-commands
3. **Consistent Responses**: Maintain uniform messaging style
4. **Error Handling**: Plan for failure cases

