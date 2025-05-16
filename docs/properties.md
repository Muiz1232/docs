# Properties System

The Properties system in TBL provides persistent data storage for your bot. It allows you to store and retrieve data across different commands and user sessions.

## Types of Properties

TBL offers three different scopes for storing properties:

### 1. Bot Properties

Bot properties are global to your entire bot and accessible from any command.

```javascript
// Store a global setting
Bot.setProp("welcome_message", "Welcome to my bot!");
Bot.setProp("maintenance_mode", false);
Bot.setProp("version", "1.2.3");

// Retrieve global settings
let welcome = Bot.getProp("welcome_message");
let inMaintenance = Bot.getProp("maintenance_mode");

// Remove a property
Bot.delProp("temporary_setting");

// Get all bot properties
let allSettings = Bot.getAllProp();
```

Use bot properties for:
- Global configuration settings
- Counters (total users, messages, etc.)
- Shared data across all users
- Bot state tracking

### 2. User Properties

User properties are specific to individual users and perfect for storing user data.

```javascript
// Store user preferences
User.setProp("language", "en");
User.setProp("notifications", true);
User.setProp("points", 100);

// Retrieve user settings with default values
let userLang = User.getProp("language", "en");
let points = User.getProp("points", 0);

// Delete user data
User.delProp("temporary_data");

// Get all properties for current user
let userData = User.getAllProp();
```

Use user properties for:
- User preferences
- User progress tracking
- Multi-step command state
- User authentication data
- Game scores or points

### 3. Chat Properties

Chat properties are specific to chat groups and can be used across users in the same chat.

```javascript
// Store chat settings
Chat.setProp("rules", "Be respectful to everyone.");
Chat.setProp("welcome_new_members", true);
Chat.setProp("spam_protection", "strict");

// Access chat settings
let rules = Chat.getProp("rules", "No rules set.");
let welcomeNewMembers = Chat.getProp("welcome_new_members", true);

// Delete chat setting
Chat.delProp("old_setting");

// Get all chat properties
let chatSettings = Chat.getAllProp();
```

Use chat properties for:
- Chat-specific settings
- Group rules or configuration
- Welcome messages
- Moderation settings
- Group statistics

## Property Methods

All property types support the same set of methods:

| Method | Description | Example |
|--------|-------------|---------|
| `setProp(name, value)` | Store a property value | `Bot.setProp("count", 42)` |
| `getProp(name, defaultValue)` | Get a property with optional default | `User.getProp("points", 0)` |
| `delProp(name)` | Delete a property | `Chat.delProp("temp")` |
| `getAllProp()` | Get all properties as object | `let all = Bot.getAllProp()` |

## Working with Complex Data

You can store complex data structures as properties:

```javascript
// Store an array
User.setProp("history", ["action1", "action2", "action3"]);

// Store an object
Bot.setProp("config", { 
  theme: "dark",
  features: ["polls", "games"],
  limits: {
    messages: 100,
    files: 10
  }
});

// Update specific object properties
let config = Bot.getProp("config", {});
config.theme = "light";
Bot.setProp("config", config);

// Add to an array
let history = User.getProp("history", []);
history.push("new_action");
User.setProp("history", history);
```

## Best Practices

1. **Use Appropriate Scope**: Choose the right property type (Bot, User, Chat) based on data scope.

2. **Provide Default Values**: Always use default values when getting properties to handle cases where the property doesn't exist yet.
   ```javascript
   // Good practice
   let points = User.getProp("points", 0);
   
   // Potential error if property doesn't exist
   let badExample = User.getProp("points") + 10; // Could be NaN if points is undefined
   ```

3. **Use Descriptive Names**: Choose clear property names that indicate their purpose.
   ```javascript
   // Good
   User.setProp("last_login_date", Date.now());
   
   // Bad
   User.setProp("lld", Date.now());
   ```

4. **Namespace Properties**: For complex bots, use namespaces in property names to prevent conflicts.
   ```javascript
   User.setProp("game:level", 5);
   User.setProp("settings:notifications", true);
   ```

5. **Clean Up Temporary Properties**: Delete properties that are no longer needed to save storage space.
   ```javascript
   // Start workflow
   User.setProp("registration:step", 1);
   
   // End workflow
   User.delProp("registration:step");
   ```

6. **Keep Property Values JSON-Serializable**: All property values should be able to be serialized to JSON.

## Examples

### User Preferences System

```javascript
// Command: /settings
Bot.sendKeyboard("Language,Notifications\nTheme,Back", "Choose a setting to change:");

// Command: Language
Bot.sendKeyboard("English,Spanish\nFrench,German\nBack", "Select your language:");

// Command: English
User.setProp("language", "en");
Bot.sendMessage("Language set to English!");

// Using the setting
let language = User.getProp("language", "en");
if(language === "en") {
  Bot.sendMessage("Hello!");
} else if(language === "es") {
  Bot.sendMessage("¡Hola!");
}
```

### Points System

```javascript
// Award points to user
function awardPoints(amount) {
  let currentPoints = User.getProp("points", 0);
  let newPoints = currentPoints + amount;
  User.setProp("points", newPoints);
  Bot.sendMessage("You earned " + amount + " points! Total: " + newPoints);
}

// Command: /daily
// Give daily points
let lastDaily = User.getProp("last_daily", 0);
let now = Date.now();
let oneDayMs = 24 * 60 * 60 * 1000;

if(now - lastDaily >= oneDayMs) {
  User.setProp("last_daily", now);
  awardPoints(100);
} else {
  let timeLeft = oneDayMs - (now - lastDaily);
  let hoursLeft = Math.floor(timeLeft / (60 * 60 * 1000));
  Bot.sendMessage("You already claimed your daily reward. Try again in " + hoursLeft + " hours.");
}
```

### Chat Configuration

```javascript
// Command: /setwelcome
// Set welcome message for new members
let welcomeMsg = params;
if(!welcomeMsg) {
  Bot.sendMessage("Please provide a welcome message after the command.");
  return;
}

Chat.setProp("welcome_message", welcomeMsg);
Bot.sendMessage("Welcome message set!");

// In @@ command, handle new chat members
if(msg.new_chat_members) {
  let welcomeMsg = Chat.getProp("welcome_message", "Welcome to the group!");
  
  for(let newMember of msg.new_chat_members) {
    let name = newMember.first_name;
    Bot.sendMessage(welcomeMsg.replace("{name}", name));
  }
}
```

## Limitations

1. Property values must be JSON-serializable (no functions, circular references, etc.)
2. There may be size limits on property values depending on the implementation
3. Properties are not real-time synced across different instances of your bot

## Related Topics

- [Bot Object](bot-object.md)
- [User Object](user-object.md)
- [Session Management](session-management.md)
- [Commands System](commands.md) 