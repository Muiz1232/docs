# User Object

The `User` object provides access to information about the current user and methods for storing user-specific data. This is essential for building personalized user experiences and tracking user information.

## User Properties

The `user` object contains information about the current user:

- `user.id`: Unique identifier for the user (Telegram user ID)
- `user.first_name`: User's first name
- `user.last_name`: User's last name (if provided)
- `user.username`: User's username without @ (if available)
- `user.language_code`: User's language code (e.g., "en", "ru", "es")
- `user.is_bot`: Boolean indicating if the user is a bot
- `user.is_premium`: Boolean indicating if the user has Telegram Premium

Example of accessing user properties:

```javascript
Bot.sendMessage(`Hello, ${user.first_name}!`);

if (user.username) {
  Bot.sendMessage(`Your username is: @${user.username}`);
} else {
  Bot.sendMessage("You don't have a username set.");
}
```

## User Methods

### Property Management

#### User.setProp(name, value, type)

Stores a user-specific property.

**Parameters:**
- `name` (string): Property name
- `value` (any): Property value
- `type` (string, optional): Property type ("string", "number", "json", etc.)

**Example:**
```javascript
// Store a simple value
User.setProp("level", 5);

// Store a complex object
User.setProp("profile", {
  age: 25,
  location: "New York",
  interests: ["coding", "music"]
});

// With specific type
User.setProp("joinDate", new Date().toISOString(), "string");
```

**Alternative Syntax:**
```javascript
User.setProp({ name: "level", value: 5, type: "number" });
```

#### User.getProp(name)

Retrieves a user property.

**Parameters:**
- `name` (string): The name of the property to retrieve

**Returns:**
- The value of the property, or null if not found

**Example:**
```javascript
let level = User.getProp("level");
Bot.sendMessage(`Your current level is: ${level}`);

let profile = User.getProp("profile");
if (profile) {
  Bot.sendMessage(
    `User Profile:\n` +
    `Age: ${profile.age}\n` +
    `Location: ${profile.location}\n` +
    `Interests: ${profile.interests.join(", ")}`
  );
}
```

#### User.delProp(name)

Deletes a user property.

**Parameters:**
- `name` (string): The name of the property to delete

**Example:**
```javascript
User.delProp("temporary_data");
Bot.sendMessage("Temporary data has been cleared.");
```

#### User.getAllProp()

Retrieves all properties stored for the user.

**Returns:**
- An object containing all user properties

**Example:**
```javascript
let allProps = User.getAllProp();
Bot.inspect(allProps);  // Display all user properties

// Check if user has any properties
if (Object.keys(allProps).length === 0) {
  Bot.sendMessage("You don't have any stored properties yet.");
}
```

#### User.delAllProp()

Deletes all properties stored for the user.

**Example:**
```javascript
User.delAllProp();  // Clear all user data
Bot.sendMessage("All your data has been reset.");
```

### Shorthand Aliases

These are alternative names for the property functions:

- `User.set(name, value, type)`: Alias for `User.setProp()`
- `User.get(name)`: Alias for `User.getProp()`
- `User.getAll()`: Alias for `User.getAllProp()`
- `User.del(name)`: Alias for `User.delProp()`
- `User.delAll()`: Alias for `User.delAllProp()`

## Working with Multiple Users

### Accessing Other Users

You can access data for users other than the current one by specifying the user ID:

```javascript
// Get property for a specific user
let otherUserScore = User.getProp("score", otherUserId);

// Set property for a specific user
User.setProp("notification", true, null, otherUserId);
```

### User Interactions

Here's an example of tracking user interactions:

```javascript
// Command: /start
// Track first interaction time
let isNewUser = !User.getProp("first_visit");
if (isNewUser) {
  User.setProp("first_visit", new Date().toISOString());
  User.setProp("visit_count", 1);
  Bot.sendMessage("Welcome to the bot for the first time!");
} else {
  let visitCount = User.getProp("visit_count") || 0;
  User.setProp("visit_count", visitCount + 1);
  User.setProp("last_visit", new Date().toISOString());
  Bot.sendMessage(`Welcome back! This is visit #${visitCount + 1}`);
}
```

## User Data Privacy and Management

### Data Privacy Best Practices

When working with user data, follow these best practices:

1. Only store what you need
2. Provide a way for users to view their stored data
3. Allow users to delete their data
4. Secure sensitive information
5. Be transparent about data usage

Example command for data transparency:

```javascript
// Command: /mydata
let userData = User.getAllProp();
let formattedData = JSON.stringify(userData, null, 2);
Bot.sendMessage(
  "Here's the data we store about you:\n\n" +
  "`" + formattedData + "`\n\n" +
  "Use /cleardata to delete all your data.",
  { parse_mode: "Markdown" }
);

// Command: /cleardata
User.delAllProp();
Bot.sendMessage("All your data has been deleted.");
```

### User Data Use Cases

#### User Preferences

```javascript
// Command: /setlanguage
Bot.sendKeyboard("English,Spanish\nFrench,German", 
  "Please select your preferred language:");

// Handle selection
let language = message;
User.setProp("language", language);
Bot.sendMessage(`Language set to: ${language}`);

// Later, in other commands:
let userLang = User.getProp("language") || "English";
let greeting = {
  "English": "Hello",
  "Spanish": "Hola",
  "French": "Bonjour",
  "German": "Hallo"
}[userLang];

Bot.sendMessage(`${greeting}, ${user.first_name}!`);
```

#### User Progress Tracking

```javascript
// Command: /addpoints
let points = parseInt(params) || 10;
let currentPoints = User.getProp("points") || 0;
let newTotal = currentPoints + points;

User.setProp("points", newTotal);
Bot.sendMessage(`Added ${points} points! New total: ${newTotal}`);

// Check for level up
let currentLevel = User.getProp("level") || 1;
let pointsForNextLevel = currentLevel * 100;

if (newTotal >= pointsForNextLevel) {
  let newLevel = currentLevel + 1;
  User.setProp("level", newLevel);
  Bot.sendMessage(`🎉 Congratulations! You've reached level ${newLevel}!`);
}
```

## Technical Details

### Data Storage

User properties are stored in a database and are:
- Persistent (saved between bot restarts)
- Linked to the specific user ID
- Available across all bot commands
- Fast to retrieve and update

### Performance Considerations

- Properties are loaded on demand for each user
- Frequently accessed properties can be stored in variables for performance
- Consider caching frequently used properties in the command code

## Common Patterns

### Initialization Pattern

A common pattern is to initialize user properties if they don't exist:

```javascript
// Initialize user if needed
if (!User.getProp("initialized")) {
  User.setProp("initialized", true);
  User.setProp("points", 0);
  User.setProp("level", 1);
  User.setProp("inventory", []);
  Bot.sendMessage("Your account has been initialized!");
}
```

### Default Values Pattern

Always provide default values when reading properties:

```javascript
let score = User.getProp("score") || 0;
let settings = User.getProp("settings") || { notifications: true, theme: "light" };
let inventory = User.getProp("inventory") || [];
```

### User State Machine

User properties can be used to implement state machines for complex interactions:

```javascript
// Command: /order
User.setProp("state", "awaiting_product");
Bot.sendMessage("What product would you like to order?");

// Command: * (default)
let state = User.getProp("state");

if (state === "awaiting_product") {
  User.setProp("product", message);
  User.setProp("state", "awaiting_quantity");
  Bot.sendMessage("How many would you like?");
  return;
}

if (state === "awaiting_quantity") {
  let quantity = parseInt(message);
  if (isNaN(quantity) || quantity <= 0) {
    Bot.sendMessage("Please enter a valid number.");
    return;
  }
  
  let product = User.getProp("product");
  User.setProp("quantity", quantity);
  User.setProp("state", "awaiting_confirmation");
  
  Bot.sendKeyboard("Confirm,Cancel", 
    `Order Summary:\nProduct: ${product}\nQuantity: ${quantity}\n\nConfirm this order?`);
  return;
}

if (state === "awaiting_confirmation") {
  if (message === "Confirm") {
    let product = User.getProp("product");
    let quantity = User.getProp("quantity");
    
    // Process order here...
    
    Bot.sendMessage(`Order confirmed! ${quantity} × ${product}`);
  } else {
    Bot.sendMessage("Order canceled.");
  }
  
  // Clear state
  User.delProp("state");
  User.delProp("product");
  User.delProp("quantity");
}
``` 