# Libraries in TBL

TBL provides a powerful library system that allows you to access pre-built functionality. Libraries help you avoid reinventing the wheel and provide consistent solutions for common tasks.

## Using Libraries

Libraries are accessed through the `Libs` object, which dynamically loads library modules as they are requested:

```javascript
// General syntax
Libs.LibraryName.methodName(parameters);

// Example
let randomNumber = Libs.Random.randomInt(1, 10);
```

## Available Libraries

TBL includes several built-in libraries for common tasks:

### Random Library

The Random library provides functions for generating various types of random values.

#### Basic Random Functions

##### randomInt(min, max, inclusive = true)

Generates a random integer between min and max.

**Parameters:**
- `min` (number): Minimum possible value
- `max` (number): Maximum possible value
- `inclusive` (boolean, optional): Whether to include the maximum value (defaults to true)

**Returns:**
- A random integer

**Example:**
```javascript
let dice = Libs.Random.randomInt(1, 6);
Bot.sendMessage("You rolled: " + dice);
```

##### randomFloat(min, max, precision = Infinity)

Generates a random floating-point number between min and max.

**Parameters:**
- `min` (number): Minimum possible value
- `max` (number): Maximum possible value
- `precision` (number, optional): Number of decimal places (defaults to Infinity)

**Returns:**
- A random float

**Example:**
```javascript
let probability = Libs.Random.randomFloat(0, 1, 2);
Bot.sendMessage("Probability: " + probability);
```

##### randomBoolean(probability = 0.5)

Generates a random boolean value.

**Parameters:**
- `probability` (number, optional): Probability of getting true (0-1, defaults to 0.5)

**Returns:**
- A random boolean

**Example:**
```javascript
let coinFlip = Libs.Random.randomBoolean();
Bot.sendMessage(coinFlip ? "Heads!" : "Tails!");
```

#### Collection Functions

##### randomChoice(arr, count = 1, unique = false)

Selects random items from an array.

**Parameters:**
- `arr` (array): Source array to pick from
- `count` (number, optional): Number of items to pick (defaults to 1)
- `unique` (boolean, optional): Whether to avoid duplicates (defaults to false)

**Returns:**
- A single item (if count = 1) or array of items

**Example:**
```javascript
let names = ["Alice", "Bob", "Charlie", "Diana"];
let winner = Libs.Random.randomChoice(names);
Bot.sendMessage("The winner is: " + winner);

// Multiple winners
let winners = Libs.Random.randomChoice(names, 2, true);
Bot.sendMessage("Winners: " + winners.join(", "));
```

##### randomShuffle(arr, inPlace = false)

Randomly shuffles the items in an array.

**Parameters:**
- `arr` (array): Array to shuffle
- `inPlace` (boolean, optional): Whether to modify the original array (defaults to false)

**Returns:**
- Shuffled array

**Example:**
```javascript
let deck = ["A♠", "K♠", "Q♠", "J♠", "10♠"];
let shuffled = Libs.Random.randomShuffle(deck);
Bot.sendMessage("Shuffled deck: " + shuffled.join(", "));
```

#### String and Text Functions

##### randomString(length = 10, options = {})

Generates a random string.

**Parameters:**
- `length` (number, optional): Length of the string (defaults to 10)
- `options` (object, optional): Configuration options
  - `charset` (string, optional): Charset to use ("alphanumeric", "alpha", "numeric", "symbols")
  - `custom` (string, optional): Custom character set to use

**Returns:**
- A random string

**Example:**
```javascript
// Alphanumeric string (default)
let token = Libs.Random.randomString(16);

// Only digits
let pin = Libs.Random.randomString(4, { charset: "numeric" });

// Custom charset
let hex = Libs.Random.randomString(8, { custom: "0123456789ABCDEF" });
```

##### randomPassword(length = 12, options = {})

Generates a random secure password.

**Parameters:**
- `length` (number, optional): Length of the password (defaults to 12)
- `options` (object, optional): Configuration options
  - `upper` (number, optional): Minimum uppercase letters (defaults to 2)
  - `lower` (number, optional): Minimum lowercase letters (defaults to 2)
  - `numbers` (number, optional): Minimum numbers (defaults to 2)
  - `special` (number, optional): Minimum special characters (defaults to 2)

**Returns:**
- A random password string

**Example:**
```javascript
let password = Libs.Random.randomPassword(16, {
  upper: 3,
  lower: 5,
  numbers: 4,
  special: 4
});
Bot.sendMessage("Generated password: " + password);
```

##### randomEmail(domains = ['gmail.com', 'yahoo.com', 'hotmail.com'])

Generates a random email address.

**Parameters:**
- `domains` (array, optional): Array of domains to use

**Returns:**
- A random email address

**Example:**
```javascript
let email = Libs.Random.randomEmail();
Bot.sendMessage("Random email: " + email);
```

#### Other Random Generators

##### randomColor(type = 'hex', alpha = false)

Generates a random color.

**Parameters:**
- `type` (string, optional): Color format ("hex", "rgb", "hsl", defaults to "hex")
- `alpha` (boolean, optional): Whether to include alpha channel (defaults to false)

**Returns:**
- A random color string in the specified format

**Example:**
```javascript
let hexColor = Libs.Random.randomColor();
let rgbaColor = Libs.Random.randomColor("rgb", true);
let hslColor = Libs.Random.randomColor("hsl");

Bot.sendMessage("Random colors:\n" +
  "Hex: " + hexColor + "\n" +
  "RGBA: " + rgbaColor + "\n" +
  "HSL: " + hslColor);
```

##### randomPhone(format = 'us')

Generates a random phone number.

**Parameters:**
- `format` (string, optional): Country format ("us", "uk", "fr", defaults to "us")

**Returns:**
- A random phone number string

**Example:**
```javascript
let phone = Libs.Random.randomPhone();
Bot.sendMessage("Random phone: " + phone);
```

##### randomGeoPoint(latRange = [-90, 90], lonRange = [-180, 180], precision = 6)

Generates a random geographic location.

**Parameters:**
- `latRange` (array, optional): Range for latitude [min, max]
- `lonRange` (array, optional): Range for longitude [min, max]
- `precision` (number, optional): Decimal precision

**Returns:**
- An object with `latitude` and `longitude`

**Example:**
```javascript
let location = Libs.Random.randomGeoPoint();
Bot.sendMessage(`Random location: ${location.latitude}, ${location.longitude}`);

// Send as a Telegram location
Api.sendLocation({
  latitude: location.latitude,
  longitude: location.longitude
});
```

### TgUtil Library

The TgUtil library provides utilities specifically for Telegram bot development.

#### Text Formatting

##### escapeMarkdown(text)

Escapes special characters in Markdown.

**Parameters:**
- `text` (string): Text to escape

**Returns:**
- Escaped text safe for Markdown formatting

**Example:**
```javascript
let userInput = "Hello! This is *bold* and [a link](https://example.com)";
let safe = Libs.TgUtil.escapeMarkdown(userInput);
Bot.sendMessage(safe);
```

##### formatNumber(number, options = {})

Formats a number with thousands separators.

**Parameters:**
- `number` (number): Number to format
- `options` (object, optional): Formatting options
  - `locale` (string, optional): Locale to use (defaults to "en-US")
  - `style` (string, optional): Formatting style ("decimal", "currency", etc)
  - `currency` (string, optional): Currency code for currency formatting

**Returns:**
- Formatted number string

**Example:**
```javascript
let price = Libs.TgUtil.formatNumber(1234567.89);
let euro = Libs.TgUtil.formatNumber(1234.56, {
  style: "currency",
  currency: "EUR"
});

Bot.sendMessage("Price: " + price);
Bot.sendMessage("Euro: " + euro);
```

#### User Management

##### extractUsername(text)

Extracts a username from text.

**Parameters:**
- `text` (string): Text that might contain a username

**Returns:**
- Username without the @ symbol, or null if none found

**Example:**
```javascript
let text = "Please contact @support_bot for help";
let username = Libs.TgUtil.extractUsername(text);
Bot.sendMessage("Extracted username: " + username);
```

##### isAdmin(userId, chatId)

Checks if a user is an admin in a chat.

**Parameters:**
- `userId` (number): User ID to check
- `chatId` (number/string, optional): Chat ID to check (defaults to current chat)

**Returns:**
- Promise resolving to a boolean

**Example:**
```javascript
let isAdmin = await Libs.TgUtil.isAdmin(user.id);
if (isAdmin) {
  Bot.sendMessage("You are an admin in this chat.");
} else {
  Bot.sendMessage("You are not an admin in this chat.");
}
```

#### Message Handling

##### parseCommand(text)

Parses a command and its parameters from text.

**Parameters:**
- `text` (string): Text to parse

**Returns:**
- Object with `command` and `params` properties

**Example:**
```javascript
let parsed = Libs.TgUtil.parseCommand("/add 5 10");
Bot.sendMessage(`Command: ${parsed.command}, Params: ${parsed.params}`);
```

##### buildKeyboard(buttons, options = {})

Builds a Telegram keyboard from a list of buttons.

**Parameters:**
- `buttons` (string/array): Buttons to display
- `options` (object, optional): Keyboard options
  - `resize` (boolean): Whether to resize the keyboard
  - `oneTime` (boolean): Whether to hide after used
  - `selective` (boolean): Show to specific users only

**Returns:**
- Keyboard markup object

**Example:**
```javascript
// From string
let keyboard = Libs.TgUtil.buildKeyboard("Button 1,Button 2\nButton 3");

// From array
let keyboard2 = Libs.TgUtil.buildKeyboard([
  ["Button 1", "Button 2"],
  ["Button 3"]
]);

Bot.sendMessage("Choose an option:", { reply_markup: keyboard });
```

### DateTimeFormat Library

The DateTimeFormat library provides date and time formatting and manipulation.

#### Formatting

##### format(date, pattern = "yyyy-MM-dd HH:mm:ss")

Formats a date according to a pattern.

**Parameters:**
- `date` (Date/number): Date object or timestamp
- `pattern` (string, optional): Format pattern

**Returns:**
- Formatted date string

**Format patterns:**
- `yyyy`: 4-digit year
- `MM`: 2-digit month
- `dd`: 2-digit day
- `HH`: 2-digit hour (24-hour)
- `hh`: 2-digit hour (12-hour)
- `mm`: 2-digit minute
- `ss`: 2-digit second
- `a`: AM/PM marker

**Example:**
```javascript
let now = new Date();
let formatted = Libs.DateTimeFormat.format(now, "dd/MM/yyyy HH:mm");
Bot.sendMessage("Current time: " + formatted);
```

##### fromNow(date)

Returns a relative time description.

**Parameters:**
- `date` (Date/number): Date object or timestamp

**Returns:**
- Human-readable string (e.g., "5 minutes ago", "in 2 days")

**Example:**
```javascript
let pastDate = new Date(Date.now() - 3600000); // 1 hour ago
let futureDate = new Date(Date.now() + 86400000); // 1 day in future

Bot.sendMessage("Past: " + Libs.DateTimeFormat.fromNow(pastDate));
Bot.sendMessage("Future: " + Libs.DateTimeFormat.fromNow(futureDate));
```

#### Time Manipulation

##### addDays(date, days)

Adds days to a date.

**Parameters:**
- `date` (Date): Original date
- `days` (number): Number of days to add (can be negative)

**Returns:**
- New Date object

**Example:**
```javascript
let now = new Date();
let tomorrow = Libs.DateTimeFormat.addDays(now, 1);
let formatted = Libs.DateTimeFormat.format(tomorrow, "dd/MM/yyyy");
Bot.sendMessage("Tomorrow: " + formatted);
```

##### addHours(date, hours)

Adds hours to a date.

**Parameters:**
- `date` (Date): Original date
- `hours` (number): Number of hours to add (can be negative)

**Returns:**
- New Date object

**Example:**
```javascript
let now = new Date();
let later = Libs.DateTimeFormat.addHours(now, 3);
Bot.sendMessage("3 hours from now: " + Libs.DateTimeFormat.format(later));
```

#### Duration

##### duration(milliseconds, options = {})

Formats a duration in milliseconds to a human-readable string.

**Parameters:**
- `milliseconds` (number): Duration in milliseconds
- `options` (object, optional): Formatting options
  - `units` (array): Units to include (e.g., ["d", "h", "m", "s"])
  - `short` (boolean): Whether to use short format
  - `delimiter` (string): Delimiter between units

**Returns:**
- Formatted duration string

**Example:**
```javascript
// Format 3 hours, 45 minutes and 30 seconds
let ms = (3 * 60 * 60 + 45 * 60 + 30) * 1000;

let full = Libs.DateTimeFormat.duration(ms);
let short = Libs.DateTimeFormat.duration(ms, { short: true });
let custom = Libs.DateTimeFormat.duration(ms, {
  units: ["h", "m"],
  delimiter: " and "
});

Bot.sendMessage("Duration: " + full);
Bot.sendMessage("Short: " + short);
Bot.sendMessage("Custom: " + custom);
```

## Creating Your Own Libraries

You can create custom libraries for your bots. Libraries should be placed in the `Libs/libs/` directory with a `.js` file extension.

### Example Custom Library

Here's a simple example of a custom library:

```javascript
// File: Libs/libs/myLib.js

// Export functions and objects
module.exports = {
  // Simple utility function
  greet: function(name) {
    return `Hello, ${name}!`;
  },
  
  // Complex function
  calculateDiscount: function(price, percentage) {
    const discount = price * (percentage / 100);
    return {
      original: price,
      discount: discount,
      final: price - discount,
      percentage: percentage
    };
  }
};
```

### Using the Custom Library

Once created, you can use your custom library the same way as built-in libraries:

```javascript
// Get a greeting
let greeting = Libs.MyLib.greet(user.first_name);
Bot.sendMessage(greeting);

// Calculate a discount
let result = Libs.MyLib.calculateDiscount(100, 20);
Bot.sendMessage(
  `Original price: $${result.original}\n` +
  `Discount: $${result.discount} (${result.percentage}%)\n` +
  `Final price: $${result.final}`
);
```

## Best Practices for Libraries

### When to Use Libraries

- Use libraries for code that will be reused across multiple commands
- Use libraries for complex functionality that would clutter command code
- Use libraries to organize related functions into logical groups

### Library Performance

- Libraries are loaded on demand, so there's no performance penalty for unused libraries
- Once loaded, a library stays in memory for quick access
- For frequently used functions, consider using the master command (@) to pre-load libraries

### Error Handling in Libraries

Always include error handling in your custom libraries:

```javascript
module.exports = {
  riskyOperation: function(input) {
    try {
      // Complex operation that might fail
      return processInput(input);
    } catch (error) {
      // Handle error and return fallback
      console.error("Error in riskyOperation:", error);
      return null;
    }
  }
};
``` 