# TBL (Tele Bot Lang) Documentation

Welcome to the official documentation for TBL (Tele Bot Lang), a custom programming language designed specifically for Telegram bot developers.

## Introduction

TBL is a powerful JavaScript-based language that simplifies Telegram bot development with built-in features, API access, and efficient performance. This documentation will guide you through using TBL to create sophisticated Telegram bots with ease.

## Table of Contents

- [Getting Started](#getting-started)
  - [Creating Your First Bot](#creating-your-first-bot)
  - [Bot Structure](#bot-structure)
- [Commands](#commands)
  - [Command Structure](#command-structure)
  - [Command Properties](#command-properties)
  - [Special Commands](#special-commands)
- [Core Language](#core-language)
  - [Variables](#variables)
  - [Functions](#functions)
  - [Control Flow](#control-flow)
- [Bot Object](#bot-object)
  - [Properties](#bot-properties)
  - [Methods](#bot-methods)
- [API Object](#api-object)
  - [Available Methods](#api-methods)
  - [Working with Telegram API](#working-with-telegram-api)
- [User Object](#user-object)
  - [User Properties](#user-properties) 
  - [User Methods](#user-methods)
- [Message Object](#message-object)
  - [Message Properties](#message-properties)
  - [Message Methods](#message-methods)
- [Libraries](#libraries)
  - [Random](#random-library)
  - [TgUtil](#tgutil-library)
  - [DateTimeFormat](#datetimeformat-library)
- [Advanced Features](#advanced-features)
  - [Session Management](#session-management)
  - [Property Storage](#property-storage)
  - [Inline Mode](#inline-mode)
  - [Callback Queries](#callback-queries)
- [Examples](#examples)
  - [Simple Echo Bot](#simple-echo-bot)
  - [Weather Bot](#weather-bot)
  - [Quiz Bot](#quiz-bot)
- [Best Practices](#best-practices)
  - [Code Organization](#code-organization)
  - [Error Handling](#error-handling)
  - [Security](#security)
- [FAQ](#faq)

## Getting Started

### Creating Your First Bot

1. Register a new bot with [@BotFather](https://t.me/BotFather) on Telegram
2. Obtain your bot's API token
3. Create a new bot on our platform and enter the token
4. Create your first command

### Bot Structure

A TBL bot consists of:
- Commands (handling user interactions)
- Properties (storing data)
- Libraries (reusable components)
- Webhook (receiving updates from Telegram)

## Commands

### Command Structure

Commands are the core of your bot. Each command can be triggered by a user message and contains:
- Name
- Answer (optional static text response)
- Code (TBL code to execute)
- Keyboard (optional keyboard layout)
- Aliases (alternative ways to trigger the command)

### Command Properties

Each command can have these properties:
- `name`: The command identifier (e.g., "/start")
- `answer`: Static text response
- `keyboard`: Button layout for reply keyboard
- `aliases`: Comma-separated list of alternative triggers
- `code`: TBL code to execute when command is triggered
- `need_reply`: Whether to await a reply from the user
- `allow_only_group`: Whether the command only works in group chats

### Special Commands

- `*`: Default command (executed when no other command matches)
- `!`: Error handling command (executed when errors occur)
- `@`: Master command (executed before any other command)
- `@@`: Finally command (executed after any other command)
- `/inline_query`: Handles inline queries
- `/channel_update`: Handles channel events

## Core Language

### Variables

TBL supports standard JavaScript variable declarations:

```javascript
let count = 0;         // Mutable variable
const PI = 3.14159;    // Immutable constant
var legacy = true;     // Function-scoped variable (not recommended)
```

### Functions

Functions work just like in JavaScript:

```javascript
function greet(name) {
  return `Hello, ${name}!`;
}

// Arrow functions
const multiply = (a, b) => a * b;
```

### Control Flow

Standard JavaScript control structures:

```javascript
if (condition) {
  // do something
} else {
  // do something else
}

for (let i = 0; i < 10; i++) {
  // loop code
}

while (condition) {
  // loop until condition is false
}
```

## Bot Object

The `Bot` object provides methods for bot operations.

### Bot Properties

- `bot.id`: Bot's unique ID
- `bot.token`: Bot's API token
- `bot.name`: Bot's username
- `bot.status`: Current status (working/stopped)

### Bot Methods

- `Bot.sendMessage(text, options)`: Send a text message
- `Bot.sendPhoto(photo, options)`: Send a photo
- `Bot.sendDocument(document, options)`: Send a document
- `Bot.sendKeyboard(text, keyboard)`: Send message with keyboard
- `Bot.runCommand(command, options)`: Execute another command
- `Bot.setProp(name, value)`: Store bot property
- `Bot.getProp(name)`: Retrieve bot property
- `Bot.delProp(name)`: Delete bot property
- `Bot.getAllProp()`: Get all bot properties

## API Object

The `Api` object provides direct access to Telegram's Bot API.

### API Methods

All Telegram Bot API methods are available through the `Api` object:

- `Api.sendMessage(params)`
- `Api.sendPhoto(params)`
- `Api.getChat(params)`
- `Api.getChatMember(params)`
- ...and [all other Telegram Bot API methods](https://core.telegram.org/bots/api#available-methods)

### Working with Telegram API

Example of sending a formatted message:

```javascript
Api.sendMessage({
  text: "*Hello* _world_!",
  parse_mode: "Markdown",
  reply_markup: {
    inline_keyboard: [
      [{ text: "Button", callback_data: "button_clicked" }]
    ]
  }
});
```

## User Object

The `User` object provides access to user data and methods.

### User Properties

- `user.id`: User's Telegram ID
- `user.first_name`: User's first name
- `user.last_name`: User's last name (if available)
- `user.username`: User's username (if available)
- `user.language_code`: User's language code

### User Methods

- `User.getProp(name)`: Get user property
- `User.setProp(name, value)`: Set user property
- `User.delProp(name)`: Delete user property
- `User.getAllProp()`: Get all user properties

## Message Object

The `msg` object provides access to the message being processed.

### Message Properties

- `msg.message_id`: Unique message ID
- `msg.text`: Message text
- `msg.from`: User who sent the message
- `msg.chat`: Chat where message was sent
- `msg.date`: Message date (Unix timestamp)
- `msg.reply_to_message`: Original message (if this is a reply)

### Message Methods

- `msg.reply(text, options)`: Reply to the current message
- `msg.edit(text, options)`: Edit the current message
- `msg.delete()`: Delete the current message
- `msg.pin(options)`: Pin the current message
- `msg.forward(chat_id)`: Forward the message to another chat

## Libraries

TBL includes several built-in libraries for common tasks.

### Random Library

Provides functions for generating random values:

```javascript
Libs.Random.randomInt(1, 10);      // Random integer 1-10
Libs.Random.randomFloat(0, 1);     // Random float 0-1
Libs.Random.randomChoice(array);   // Random item from array
Libs.Random.randomString(10);      // Random string (10 chars)
Libs.Random.randomColor();         // Random color in hex
```

### TgUtil Library

Utilities for Telegram-specific operations:

```javascript
Libs.TgUtil.formatNumber(1000);     // Formats: 1,000
Libs.TgUtil.escapeMarkdown("text"); // Escapes markdown
Libs.TgUtil.extractUsername("@user"); // Returns "user"
```

### DateTimeFormat Library

Date and time formatting:

```javascript
Libs.DateTimeFormat.format(new Date(), "dd/MM/yyyy"); // Format date
Libs.DateTimeFormat.fromNow(timestamp);               // Relative time
Libs.DateTimeFormat.duration(milliseconds);           // Format duration
```

## Advanced Features

### Session Management

TBL provides session management for multi-step commands:

```javascript
// Command with need_reply: true
Bot.sendMessage("What's your name?");
User.setProp("awaiting", "name");

// Command that handles the reply
let name = message;
User.setProp("name", name);
Bot.sendMessage(`Hello, ${name}!`);
```

### Property Storage

Store data persistently with properties:

```javascript
// Store data
Bot.setProp("counter", 42);

// Retrieve data
let counter = Bot.getProp("counter");

// Store JSON
Bot.setProp("user_data", { name: "John", age: 25 });
```

### Inline Mode

Handle inline queries with the `/inline_query` command:

```javascript
// Command: /inline_query
let query = update.inline_query.query;
Api.answerInlineQuery({
  inline_query_id: update.inline_query.id,
  results: [
    {
      type: "article",
      id: "1",
      title: "Search result",
      description: `Results for: ${query}`,
      input_message_content: {
        message_text: `You searched for: ${query}`
      }
    }
  ]
});
```

### Callback Queries

Handle inline keyboard button clicks:

```javascript
// Check if update contains callback query
if(update.callback_query){
  let data = update.callback_query.data;
  
  if(data == "button1"){
    Api.answerCallbackQuery({
      callback_query_id: update.callback_query.id,
      text: "Button 1 clicked!"
    });
  }
}
```

## Examples

### Simple Echo Bot

```javascript
// Command: /echo
Bot.sendMessage("Please send me text to echo");

// Command: *
// This will echo any message received
Bot.sendMessage(message);
```

### Weather Bot

```javascript
// Command: /weather
Bot.sendMessage("Please enter a city name");

// Handle the reply
let city = message;
HTTP.get({
  url: `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=YOUR_API_KEY&units=metric`,
  success: function(response){
    let weather = response.content;
    
    if(weather.cod == 200){
      let temp = weather.main.temp;
      let desc = weather.weather[0].description;
      
      Bot.sendMessage(`Weather in ${city}:\n${desc}, ${temp}°C`);
    }else{
      Bot.sendMessage("City not found");
    }
  }
});
```

### Quiz Bot

```javascript
// Command: /quiz
let questions = Bot.getProp("questions") || [
  {question: "What is 2+2?", answer: "4"},
  {question: "What is the capital of France?", answer: "Paris"}
];

let userScore = User.getProp("score") || 0;
let currentQuestion = User.getProp("currentQuestion") || 0;

Bot.sendMessage(`Question: ${questions[currentQuestion].question}`);
User.setProp("waitingAnswer", true);
User.setProp("currentQuestion", currentQuestion);

// Command: *
// Handle quiz answers
if(User.getProp("waitingAnswer")){
  let currentQuestion = User.getProp("currentQuestion");
  let userScore = User.getProp("score") || 0;
  let questions = Bot.getProp("questions");
  
  if(message.toLowerCase() == questions[currentQuestion].answer.toLowerCase()){
    userScore++;
    User.setProp("score", userScore);
    Bot.sendMessage("Correct! 🎉");
  }else{
    Bot.sendMessage(`Wrong! The correct answer is: ${questions[currentQuestion].answer}`);
  }
  
  currentQuestion++;
  
  if(currentQuestion < questions.length){
    Bot.sendMessage(`Next question: ${questions[currentQuestion].question}`);
    User.setProp("currentQuestion", currentQuestion);
  }else{
    Bot.sendMessage(`Quiz completed! Your score: ${userScore}/${questions.length}`);
    User.setProp("waitingAnswer", false);
  }
}
```

## Best Practices

### Code Organization

- Organize commands logically
- Use modularity for complex bots
- Keep command code focused on single responsibilities
- Use libraries for common functionality

### Error Handling

- Create an error command (!) to handle exceptions
- Validate user input before processing
- Use try/catch blocks for error-prone operations

```javascript
// Command: !
Bot.sendMessage("⚠️ Something went wrong. Please try again later.");
```

### Security

- Never hardcode sensitive information (API keys, passwords)
- Use Bot properties to store configuration securely
- Validate user input to prevent injection attacks
- Implement rate limiting for commands when needed

## FAQ

### How do I add command aliases?

Set the `aliases` property of your command to a comma-separated list.

### How do I store data persistently?

Use `Bot.setProp()`, `User.setProp()`, or `Chat.setProp()` to store data.

### How do I handle multiple steps in a conversation?

Set `need_reply: true` in your command and use the session to track the conversation state.

### Can I use external APIs?

Yes, use the `HTTP` object for API requests:

```javascript
HTTP.get({
  url: "https://api.example.com/data",
  success: function(response){
    let data = response.content;
    Bot.sendMessage("Data received: " + JSON.stringify(data));
  }
});
```

### How do I create a keyboard?

```javascript
Bot.sendKeyboard("Button 1,Button 2\nButton 3", "Please select an option:");
```

Or with the API:

```javascript
Api.sendMessage({
  text: "Please select an option:",
  reply_markup: {
    keyboard: [
      [{ text: "Button 1" }, { text: "Button 2" }],
      [{ text: "Button 3" }]
    ],
    resize_keyboard: true
  }
});
``` 