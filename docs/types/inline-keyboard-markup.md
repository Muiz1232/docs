# InlineKeyboardMarkup Object

The InlineKeyboardMarkup object represents an inline keyboard that appears right below the message it belongs to.

## Structure

```javascript
{
  inline_keyboard: [
    // First row
    [
      {
        text: "Button 1",
        callback_data: "button1_pressed"
      },
      {
        text: "Button 2",
        callback_data: "button2_pressed"
      }
    ],
    // Second row
    [
      {
        text: "URL Button",
        url: "https://example.com"
      }
    ]
  ]
}
```

## Properties

| Property | Type | Description |
|----------|------|-------------|
| inline_keyboard | Array of Array of [InlineKeyboardButton](inline-keyboard-button.md) | Array of button rows, each represented by an array of InlineKeyboardButton objects |

## InlineKeyboardButton Types

An InlineKeyboardButton can be one of several types based on the field that is set:

| Type | Required Field | Description |
|------|---------------|-------------|
| Callback Button | callback_data | Sends a callback query when pressed |
| URL Button | url | Opens the specified URL when pressed |
| Login Button | login_url | Sends login info to the bot when pressed |
| Switch Inline Query | switch_inline_query | Prompts user to select a chat to send an inline query |
| Switch Inline Query Current Chat | switch_inline_query_current_chat | Inserts an inline query in the current chat |
| Callback Game | callback_game | Launches a game when pressed |
| Pay Button | pay | Initiates a payment when pressed |

## Usage in TBL

```javascript
// Creating an inline keyboard markup
let inlineKeyboard = {
  inline_keyboard: [
    // First row with two buttons
    [
      { text: "Button 1", callback_data: "button1" },
      { text: "Button 2", callback_data: "button2" }
    ],
    // Second row with one button
    [
      { text: "Visit Website", url: "https://example.com" }
    ]
  ]
};

// Sending a message with inline keyboard
Api.sendMessage({
  chat_id: chat.id,
  text: "Please select an option:",
  reply_markup: inlineKeyboard
});

// Editing a message to change the inline keyboard
Api.editMessageReplyMarkup({
  chat_id: chat.id,
  message_id: message_id,
  reply_markup: updatedInlineKeyboard
});
```

## Button Types Examples

### Callback Button

```javascript
{
  text: "Click me",
  callback_data: "button_clicked"
}
```

When pressed, Telegram sends a callback query to your bot with the specified `callback_data`. The maximum length for callback_data is 64 bytes.

### URL Button

```javascript
{
  text: "Open Website",
  url: "https://example.com"
}
```

Opens the URL when pressed. If you want to create a custom URL with parameters, you can use:

```javascript
{
  text: "View Profile",
  url: "https://example.com/profile?id=" + user.id
}
```

### Login Button

```javascript
{
  text: "Login with Telegram",
  login_url: {
    url: "https://example.com/auth",
    forward_text: "Log in to my service",  // Optional
    bot_username: "MyAuthBot",  // Optional
    request_write_access: true  // Optional
  }
}
```

### Switch Inline Query Button

```javascript
{
  text: "Search in other chats",
  switch_inline_query: "default query"
}
```

This will prompt the user to select a chat, then automatically enter inline mode with your bot in that chat with the specified query pre-filled.

### Switch Inline Query Current Chat Button

```javascript
{
  text: "Search here",
  switch_inline_query_current_chat: "default query"
}
```

This will enter inline mode in the current chat with the specified query pre-filled.

### Pay Button

```javascript
{
  text: "Buy now",
  pay: true
}
```

This button must be the first button in the first row and can only be used with invoice messages.

## Layout Patterns

### Simple Row

```javascript
{
  inline_keyboard: [
    [
      { text: "Button 1", callback_data: "button1" },
      { text: "Button 2", callback_data: "button2" },
      { text: "Button 3", callback_data: "button3" }
    ]
  ]
}
```

### Multiple Rows

```javascript
{
  inline_keyboard: [
    [{ text: "Row 1 Button", callback_data: "row1" }],
    [{ text: "Row 2 Button", callback_data: "row2" }],
    [{ text: "Row 3 Button", callback_data: "row3" }]
  ]
}
```

### Grid Layout

```javascript
{
  inline_keyboard: [
    [
      { text: "1", callback_data: "1" },
      { text: "2", callback_data: "2" },
      { text: "3", callback_data: "3" }
    ],
    [
      { text: "4", callback_data: "4" },
      { text: "5", callback_data: "5" },
      { text: "6", callback_data: "6" }
    ],
    [
      { text: "7", callback_data: "7" },
      { text: "8", callback_data: "8" },
      { text: "9", callback_data: "9" }
    ]
  ]
}
```

### Pagination Layout

```javascript
{
  inline_keyboard: [
    // Content buttons
    [
      { text: "Item 1", callback_data: "item_1" },
      { text: "Item 2", callback_data: "item_2" }
    ],
    // Navigation buttons
    [
      { text: "◀️", callback_data: "prev_page" },
      { text: "1/5", callback_data: "current_page" },
      { text: "▶️", callback_data: "next_page" }
    ]
  ]
}
```

## Handling Callback Queries

When a user clicks an inline button with callback_data, your bot receives a callback query that you can handle:

```javascript
// Command: /on_callback_query
if(query.data == "button1") {
  // Handle button 1 press
  Api.answerCallbackQuery({
    callback_query_id: query.id,
    text: "You pressed Button 1!"
  });
  
  // Update the message
  Api.editMessageText({
    chat_id: query.message.chat.id,
    message_id: query.message.message_id,
    text: "Button 1 was pressed!",
    reply_markup: newKeyboard
  });
}
```

## Best Practices

1. **Keep Button Text Short**: Button text should be concise and clearly indicate what the button does.

2. **Limit Number of Buttons**: Avoid having too many buttons in a single message. If you need many options, consider:
   - Pagination (Next/Previous buttons)
   - Multiple messages with different sets of buttons
   - Hierarchical menus (Category buttons that show more specific options)

3. **Ensure Responsive UI**: Always provide feedback when a button is pressed:
   - Use `Api.answerCallbackQuery()` to show a notification or alert
   - Update the message text or buttons to reflect the change

4. **Handle Outdated Buttons**: If your keyboard shows dynamic data (like counts, time-sensitive options), make sure to handle outdated button presses gracefully.

5. **Group Related Buttons**: Put related buttons in the same row and separate different functional groups with new rows.

6. **Mix Button Types**: Combine callback and URL buttons where appropriate to provide complete functionality.

## Limitations

- A message can have at most 100 buttons total
- Each row can have at most 8 buttons
- The sum of all callback_data cannot exceed 10KB
- Text on each button can be up to 200 characters

## See Also

- [InlineKeyboardButton Object](inline-keyboard-button.md)
- [Callback Query Object](callback-query.md)
- [Api.sendMessage](../api-reference/messages/send-message.md)
- [Api.editMessageReplyMarkup](../api-reference/messages/edit-message-reply-markup.md)
- [Api.answerCallbackQuery](../api-reference/callbacks/answer-callback-query.md)
- [Keyboards Guide](../guides/keyboards.md) 