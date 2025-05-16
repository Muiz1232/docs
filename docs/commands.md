# Commands in TBL

Commands are the foundation of your bot's interaction with users. Each command represents a specific function or action that your bot can perform.

## Command Structure

In TBL, a command consists of several components:

- **Name**: The identifier used to trigger the command (e.g., "/start")
- **Answer**: An optional static text response
- **Code**: TBL code to execute when the command is triggered
- **Keyboard**: An optional custom keyboard layout
- **Aliases**: Alternative ways to trigger the command
- **Need Reply**: Whether the command expects a follow-up response
- **Allow Only Group**: Whether the command can only be used in group chats

## Creating Commands

Commands can be created in several ways:

1. Through the web interface
2. Using CSV import
3. Through the API
4. Using a Git repository

### Basic Command Example

```
Command: /start
Answer: Welcome to my bot!
Keyboard: Help,About\nSettings
Code:
  Bot.sendMessage("Hello, " + user.first_name + "!");
  
  // Track user statistics
  let visits = User.getProp("visits") || 0;
  User.setProp("visits", visits + 1);
  User.setProp("last_visit", new Date().toISOString());
```

### Command Properties

#### Name

The command name is the identifier that users will type to trigger the command. By convention, commands in Telegram typically start with a forward slash ("/").

Examples:
- `/start`
- `/help`
- `/settings`

#### Answer

The answer property provides a static text response that will be sent automatically when the command is triggered. This text will be sent before any code is executed.

Example:
```
Answer: Welcome to my awesome bot! I can help you with many tasks.
```

The answer supports Markdown formatting:
```
Answer: *Welcome* to my _awesome_ bot! I can help you with many [tasks](https://example.com).
```

#### Code

The code property contains the TBL code that will be executed when the command is triggered. This is where you can add custom logic, API calls, database operations, and dynamic responses.

Example:
```javascript
// Greet the user
Bot.sendMessage("Hello, " + user.first_name + "!");

// Log this interaction
console.log("User " + user.id + " triggered the start command");

// Store user information
User.setProp("last_command", "/start");
User.setProp("last_interaction", new Date().toISOString());
```

#### Keyboard

The keyboard property lets you define a custom keyboard that will be shown to users when they trigger the command. This is useful for providing quick access to common functions.

Keyboard syntax:
- Buttons in the same row are separated by commas
- Rows are separated by newlines (\n)

Example:
```
Keyboard: Button 1,Button 2,Button 3
```

This creates a keyboard with three buttons in a single row.

Example with multiple rows:
```
Keyboard: Button 1,Button 2
Button 3,Button 4
Button 5
```

This creates a keyboard with:
- Row 1: Button 1, Button 2
- Row 2: Button 3, Button 4
- Row 3: Button 5

#### Aliases

Aliases allow you to define alternative ways to trigger a command. Multiple aliases are separated by commas.

Example:
```
Command: /start
Aliases: start,begin,hello
```

With this configuration, users can trigger the command by typing:
- `/start`
- `start`
- `begin`
- `hello`

Aliases are useful for making your bot more user-friendly by accepting variations of commands.

#### Need Reply

When set to `true`, this property indicates that the command expects a response from the user. The bot will enter a "waiting for reply" state after the command is triggered.

Example:
```
Command: /askname
Answer: What's your name?
Need Reply: true
```

When this command is triggered, the bot will ask "What's your name?" and wait for the user's response. The next message from the user will be processed by the same command or by a special handler for replies.

#### Allow Only Group

When set to `true`, this property restricts the command to be used only in group chats. If a user tries to use the command in a private chat, it will not be executed.

Example:
```
Command: /announce
Answer: Announcement sent to all members
Allow Only Group: true
```

## Special Commands

TBL supports several special command names that have specific behaviors:

### * (Default Command)

The `*` command acts as a catch-all for any message that doesn't match a specific command. It's useful for creating fallback responses or for bots that process natural language.

Example:
```
Command: *
Code:
  Bot.sendMessage("I didn't understand that. Try /help for available commands.");
```

### ! (Error Command)

The `!` command is executed when an error occurs during the execution of any command. It allows you to provide a graceful error handling experience.

Example:
```
Command: !
Code:
  Bot.sendMessage("Oops! Something went wrong. Please try again later.");
  console.error("Error: " + error);
```

### @ (Master Command)

The `@` command is executed before any other command. It's useful for performing common operations that should happen before every command, such as checking user authorization, loading common libraries, or setting up global variables.

Example:
```
Command: @
Code:
  // Load common libraries
  let utils = Libs.Random;
  
  // Check if user is banned
  let banned = Bot.getProp("banned_users") || [];
  if (banned.includes(user.id)) {
    Bot.sendMessage("You are banned from using this bot.");
    return;
  }
  
  // Log command usage
  console.log("User " + user.id + " triggered: " + message);
```

### @@ (Finally Command)

The `@@` command is executed after any other command. It's useful for cleanup operations, logging, or actions that should happen after every command.

Example:
```
Command: @@
Code:
  // Track command usage statistics
  let stats = Bot.getProp("command_stats") || {};
  let command = message.split(" ")[0];
  
  if (!stats[command]) {
    stats[command] = 0;
  }
  
  stats[command]++;
  Bot.setProp("command_stats", stats);
```

### /inline_query (Inline Mode)

The `/inline_query` command handles inline queries when a user types your bot's username in any chat followed by a query.

Example:
```
Command: /inline_query
Code:
  let query = update.inline_query.query;
  
  Api.answerInlineQuery({
    inline_query_id: update.inline_query.id,
    results: [
      {
        type: "article",
        id: "1",
        title: "Search result for: " + query,
        input_message_content: {
          message_text: "You searched for: " + query
        },
        description: "Click to send this result"
      }
    ]
  });
```

### /channel_update (Channel Events)

The `/channel_update` command handles events that occur in channels where your bot is an administrator.

Example:
```
Command: /channel_update
Code:
  if (update.channel_post) {
    // Handle new channel post
    let channelId = update.channel_post.chat.id;
    let messageId = update.channel_post.message_id;
    let text = update.channel_post.text;
    
    // Log all posts to the channel
    console.log("New post in channel " + channelId + ": " + text);
    
    // Maybe add reactions or pin important messages
    if (text.includes("#important")) {
      Api.pinChatMessage({
        chat_id: channelId,
        message_id: messageId
      });
    }
  }
```

## Command Organization

### Command Chaining

Commands can call other commands using `Bot.runCommand()`:

```javascript
// Command: /menu
Bot.sendMessage("Main Menu:");
Bot.sendKeyboard("Profile,Settings\nStats,Help", "Choose an option:");

// Command: profile
Bot.runCommand("/show_profile");

// Command: /show_profile
let profile = User.getProp("profile") || { name: "Unknown", age: 0 };
Bot.sendMessage(
  "Your Profile:\n" +
  "Name: " + profile.name + "\n" +
  "Age: " + profile.age
);
```

### Command Parameters

Commands can process parameters passed by the user:

```javascript
// Command: /add
// Usage: /add 5 10
let parts = params.split(" ");
let num1 = parseFloat(parts[0]) || 0;
let num2 = parseFloat(parts[1]) || 0;
let sum = num1 + num2;

Bot.sendMessage(`${num1} + ${num2} = ${sum}`);
```

The `params` variable contains the text that comes after the command.

## Multi-step Commands

You can create multi-step interactions using the "Need Reply" property:

```
Command: /register
Answer: What's your name?
Need Reply: true
Code:
  // Save the name
  User.setProp("name", message);
  User.setProp("registration_step", "age");
  
  // Ask for age
  Bot.sendMessage("How old are you?");

Command: *
Code:
  let step = User.getProp("registration_step");
  
  if (step === "age") {
    let age = parseInt(message);
    
    if (isNaN(age) || age < 0 || age > 120) {
      Bot.sendMessage("Please enter a valid age.");
      return;
    }
    
    User.setProp("age", age);
    User.setProp("registration_step", "location");
    Bot.sendMessage("Where are you from?");
    return;
  }
  
  if (step === "location") {
    User.setProp("location", message);
    User.setProp("registration_step", null);
    
    let name = User.getProp("name");
    let age = User.getProp("age");
    
    Bot.sendMessage(
      "Registration complete!\n\n" +
      "Name: " + name + "\n" +
      "Age: " + age + "\n" +
      "Location: " + message
    );
    return;
  }
  
  // Default handler for non-command messages
  Bot.sendMessage("I don't understand. Try /help for available commands.");
```

## Best Practices

### Command Naming

- Use clear, descriptive command names
- Stick to conventional naming for standard commands (e.g., `/start`, `/help`, `/settings`)
- Use a consistent naming scheme for related commands (e.g., `/user_info`, `/user_edit`, `/user_delete`)
- Keep command names short and easy to type

### Command Organization

- Group related commands together
- Use separate commands for distinct functions
- Break complex logic into multiple commands and use command chaining

### Error Handling

Always include error handling in your commands:

```javascript
try {
  // Risky operation
  let data = JSON.parse(message);
  processData(data);
} catch (error) {
  Bot.sendMessage("Sorry, there was an error processing your request.");
  console.error("Error in command:", error);
}
```

### Performance

- Keep commands efficient by minimizing API calls
- Use caching for frequently used data
- For long-running operations, consider using background processing

### Security

- Validate user input before processing
- Implement authorization checks for privileged commands
- Never expose sensitive information in command responses

Example of secure command:

```javascript
// Command: /admin
// Check if user is an admin
let adminList = Bot.getProp("admins") || [];

if (!adminList.includes(user.id)) {
  Bot.sendMessage("You don't have permission to use this command.");
  return;
}

// Continue with admin functionality
Bot.sendMessage("Welcome, admin! What would you like to do today?");
Bot.sendKeyboard("Stats,Ban User\nBroadcast,Settings", "Admin Panel:");
``` 