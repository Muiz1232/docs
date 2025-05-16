# Creating Your First Bot

This guide will walk you through the process of creating your first TBL bot from scratch.

## Prerequisites

Before you begin, make sure you have:

1. A Telegram account
2. Access to the TBL platform
3. Basic understanding of JavaScript (helpful but not required)

## Step 1: Create a Telegram Bot with BotFather

1. Open Telegram and search for [@BotFather](https://t.me/BotFather)
2. Start a chat with BotFather and send the command `/newbot`
3. Follow the instructions to choose a name and username for your bot
4. When completed, you'll receive a token that looks like this: `123456789:ABCDefGhIJKlmNoPQRsTUVwxyZ`
5. **Important**: Keep this token secure and don't share it publicly!

## Step 2: Create Your Bot on the TBL Platform

1. Log in to the TBL platform
2. Click on "Create New Bot"
3. Enter your bot's name
4. Paste the token you received from BotFather
5. Click "Create"

## Step 3: Create Your First Command

Every bot needs a `/start` command, which is triggered when a user first interacts with your bot.

1. In your bot's dashboard, go to the "Commands" tab
2. Click "Add New Command"
3. For Command Name, enter: `/start`
4. For Answer, enter: `Welcome to my first bot!`
5. For Code, enter:
```javascript
Bot.sendMessage("Hello, " + user.first_name + "! I'm your new bot.");
```
6. Click "Save Command"

## Step 4: Add a Custom Keyboard

Let's make our bot more interactive by adding a keyboard:

1. Edit your `/start` command
2. In the Keyboard field, enter:
```
Help,About
Contact
```
3. Click "Save Command"

## Step 5: Create a Help Command

Now let's create a command that responds to the "Help" button:

1. Add a new command
2. For Command Name, enter: `help`
3. For Answer, enter: `Here's how to use this bot:`
4. For Code, enter:
```javascript
Bot.sendMessage(
  "Available commands:\n" +
  "/start - Start the bot\n" +
  "/help - Show this help message\n" +
  "/about - About this bot"
);
```
5. Click "Save Command"

## Step 6: Create an About Command

1. Add a new command
2. For Command Name, enter: `about`
3. For Answer, enter: `About this bot`
4. For Code, enter:
```javascript
Bot.sendMessage(
  "This is my first TBL bot.\n" +
  "Created on " + new Date().toDateString()
);
```
5. Click "Save Command"

## Step 7: Test Your Bot

1. Open Telegram and search for your bot using the username you created
2. Start a conversation with your bot
3. Send the `/start` command
4. You should see the welcome message and the keyboard
5. Click the "Help" and "About" buttons to test those commands

## Step 8: Add a Default Command

Let's handle messages that don't match any command:

1. Add a new command
2. For Command Name, enter: `*` (asterisk)
3. For Code, enter:
```javascript
Bot.sendMessage("I don't understand that command. Try /help for a list of commands.");
```
4. Click "Save Command"

## Next Steps

Congratulations! You've created your first Telegram bot using TBL. Here are some ideas for what to do next:

1. Add more commands
2. Explore the [`Bot` object](bot-object.md) to understand other available methods
3. Learn how to use the [`API` object](api-object.md) for advanced Telegram features
4. Try using [libraries](libraries.md) for additional functionality
5. Create a multi-step conversation with [session management](session-management.md)

## Example: Complete Bot Code

Here's a summary of all the commands we created:

### Start Command
```
Command: /start
Answer: Welcome to my first bot!
Keyboard: Help,About
Contact
Code:
  Bot.sendMessage("Hello, " + user.first_name + "! I'm your new bot.");
```

### Help Command
```
Command: help
Answer: Here's how to use this bot:
Code:
  Bot.sendMessage(
    "Available commands:\n" +
    "/start - Start the bot\n" +
    "/help - Show this help message\n" +
    "/about - About this bot"
  );
```

### About Command
```
Command: about
Answer: About this bot
Code:
  Bot.sendMessage(
    "This is my first TBL bot.\n" +
    "Created on " + new Date().toDateString()
  );
```

### Default Command
```
Command: *
Code:
  Bot.sendMessage("I don't understand that command. Try /help for a list of commands.");
``` 