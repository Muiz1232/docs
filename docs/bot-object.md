# Bot Object

The `Bot` object is one of the core components of TBL, providing methods for managing bot operations, sending messages, and handling data storage.

## Properties

The `bot` object provides access to the bot's basic information:

- `bot.id`: The bot's unique numeric ID
- `bot.token`: The bot's API token from BotFather
- `bot.name`: The bot's username
- `bot.status`: Current status (working/stopped)
- `bot.created_at`: Timestamp when the bot was created
- `bot.updated_at`: Timestamp of the last update
- `bot.owner`: Owner of the bot
- `bot.platform`: Information about the platform:
  - `bot.platform.name`: Platform name
  - `bot.platform.version`: Platform version
  - `bot.platform.description`: Platform description
  - `bot.platform.website`: Platform website

## Message Methods

### Bot.sendMessage(text, options)

Sends a text message to the current chat.

**Parameters:**
- `text` (string): The text content of the message
- `options` (object, optional): Additional options for the message
  - `parse_mode` (string): "Markdown" or "HTML" (defaults to "Markdown")
  - `reply_markup` (object): Keyboard options
  - `reply_to_message_id` (number): ID of the message to reply to
  - `disable_web_page_preview` (boolean): Whether to disable link previews

**Example:**
```javascript
Bot.sendMessage("Hello, *world*!");

// With options
Bot.sendMessage("Welcome to my bot!", {
  parse_mode: "Markdown",
  disable_web_page_preview: true
});
```

### Bot.sendKeyboard(text, keyboardString)

Sends a message with a custom keyboard.

**Parameters:**
- `text` (string): The text content of the message
- `keyboardString` (string): Keyboard layout as a string
  - Buttons in the same row are separated by commas
  - Rows are separated by newlines

**Example:**
```javascript
Bot.sendKeyboard(
  "Please select an option:",
  "Button 1,Button 2\nButton 3"
);
```

This creates a keyboard with:
- First row: "Button 1", "Button 2"
- Second row: "Button 3"

### Bot.sendPhoto(photo, options)

Sends a photo message.

**Parameters:**
- `photo` (string): URL or file_id of the photo
- `options` (object, optional): Additional options
  - `caption` (string): Caption for the photo
  - `parse_mode` (string): "Markdown" or "HTML" for caption formatting
  - `reply_markup` (object): Keyboard options

**Example:**
```javascript
Bot.sendPhoto("https://example.com/image.jpg", {
  caption: "Beautiful landscape!"
});
```

### Bot.sendDocument(document, options)

Sends a document (file).

**Parameters:**
- `document` (string): URL or file_id of the document
- `options` (object, optional): Additional options
  - `caption` (string): Caption for the document
  - `parse_mode` (string): "Markdown" or "HTML" for caption formatting
  - `reply_markup` (object): Keyboard options

**Example:**
```javascript
Bot.sendDocument("https://example.com/document.pdf", {
  caption: "Here's the PDF you requested."
});
```

### Bot.sendAudio(audio, options)

Sends an audio file.

**Parameters:**
- `audio` (string): URL or file_id of the audio
- `options` (object, optional): Additional options
  - `caption` (string): Caption for the audio
  - `parse_mode` (string): "Markdown" or "HTML" for caption formatting
  - `duration` (number): Duration of the audio in seconds
  - `performer` (string): Performer name
  - `title` (string): Audio title

**Example:**
```javascript
Bot.sendAudio("https://example.com/song.mp3", {
  caption: "Enjoy this song!",
  performer: "Artist Name",
  title: "Song Title"
});
```

### Bot.sendVideo(video, options)

Sends a video file.

**Parameters:**
- `video` (string): URL or file_id of the video
- `options` (object, optional): Additional options
  - `caption` (string): Caption for the video
  - `parse_mode` (string): "Markdown" or "HTML" for caption formatting
  - `duration` (number): Duration of the video in seconds
  - `width` (number): Video width
  - `height` (number): Video height

**Example:**
```javascript
Bot.sendVideo("https://example.com/video.mp4", {
  caption: "Check out this video!"
});
```

### Bot.sendVoice(voice, options)

Sends a voice message.

**Parameters:**
- `voice` (string): URL or file_id of the voice message
- `options` (object, optional): Additional options
  - `caption` (string): Caption for the voice message
  - `parse_mode` (string): "Markdown" or "HTML" for caption formatting
  - `duration` (number): Duration of the voice message in seconds

**Example:**
```javascript
Bot.sendVoice("https://example.com/voice.ogg", {
  caption: "Voice message"
});
```

### Bot.inspect(object)

Sends a message with a detailed inspection of an object.

**Parameters:**
- `object` (any): The object to inspect

**Example:**
```javascript
Bot.inspect(user);  // Displays detailed information about the user object
```

## Command Execution

### Bot.runCommand(command, options)

Executes another command from within the current command.

**Parameters:**
- `command` (string): The command to execute (without the leading slash)
- `options` (object, optional): Additional options to pass to the command

**Example:**
```javascript
// Execute the "welcome" command
Bot.runCommand("welcome");

// Execute with parameters
Bot.runCommand("calculate", { x: 10, y: 20 });
```

## Property Storage

### Bot.setProp(name, value, type)

Stores a property in the bot's persistent storage.

**Parameters:**
- `name` (string): Property name
- `value` (any): Property value
- `type` (string, optional): Property type ("string", "number", "json", etc.)

**Example:**
```javascript
// Store a simple value
Bot.setProp("welcomeMessage", "Hello, welcome to my bot!");

// Store a number
Bot.setProp("visitorCount", 42);

// Store a JSON object
Bot.setProp("config", { apiKey: "1234", enabled: true });
```

**Alternative Syntax:**
```javascript
Bot.setProp({ name: "welcomeMessage", value: "Hello!", type: "string" });
```

### Bot.getProp(name)

Retrieves a property from the bot's storage.

**Parameters:**
- `name` (string): The name of the property to retrieve

**Returns:**
- The value of the property, or null if not found

**Example:**
```javascript
let message = Bot.getProp("welcomeMessage");
let count = Bot.getProp("visitorCount");
let config = Bot.getProp("config");

Bot.sendMessage(`Welcome message: ${message}`);
Bot.sendMessage(`Visitor count: ${count}`);
Bot.sendMessage(`API Key: ${config.apiKey}`);
```

### Bot.delProp(name)

Deletes a property from the bot's storage.

**Parameters:**
- `name` (string): The name of the property to delete

**Example:**
```javascript
Bot.delProp("temporaryData");
```

### Bot.getAllProp()

Retrieves all properties stored for the bot.

**Returns:**
- An object containing all bot properties

**Example:**
```javascript
let allProps = Bot.getAllProp();
Bot.inspect(allProps);  // Display all properties
```

### Bot.delAllProp()

Deletes all properties stored for the bot.

**Example:**
```javascript
Bot.delAllProp();  // Clear all bot properties
```

## Shorthand Aliases

These are alternative names for the property functions:

- `Bot.set(name, value, type)`: Alias for `Bot.setProp()`
- `Bot.get(name)`: Alias for `Bot.getProp()`
- `Bot.getAll()`: Alias for `Bot.getAllProp()`
- `Bot.del(name)`: Alias for `Bot.delProp()`
- `Bot.delAll()`: Alias for `Bot.delAllProp()`

## Best Practices

### Efficient Property Storage

- Store related data as a single JSON object rather than multiple separate properties
- Use meaningful property names for better code readability
- Don't store sensitive information like API keys directly; consider encryption

### Command Organization

- Use `Bot.runCommand()` to create modular bot workflows
- Keep command functions focused on single responsibilities
- Pass parameters when running commands to share data between commands

### Error Handling

Always check that properties exist before using them:

```javascript
let config = Bot.getProp("config");
if (!config) {
  // Handle the case when config doesn't exist
  config = { defaultSetting: true };
  Bot.setProp("config", config);
}
``` 