# Api.editMessageText

Edits the text and inline keyboard of a message that was previously sent by the bot.

## Signature

```javascript
Api.editMessageText(params)
```

## Description

This method allows you to edit the text content or reply markup of previously sent messages. This is particularly useful for updating information in messages based on user interactions, such as updating a message after a button is pressed.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| chat_id | Integer or String | Yes (if inline_message_id is not specified) | Unique identifier for the target chat or username of the target channel (in the format @channelusername) |
| message_id | Integer | Yes (if inline_message_id is not specified) | Identifier of the message to edit |
| inline_message_id | String | Yes (if chat_id and message_id are not specified) | Identifier of the inline message |
| text | String | Yes | New text of the message (1-4096 characters) |
| parse_mode | String | No | Mode for parsing entities. Either "Markdown" or "HTML" |
| entities | Array of MessageEntity | No | List of special entities that appear in the message text. Can be used instead of parse_mode |
| disable_web_page_preview | Boolean | No | Disables link previews for links in this message |
| reply_markup | Object | No | A JSON-serialized object for an inline keyboard |

## Returns

On success, if the edited message is not an inline message, the edited [Message](../../types/message.md) object is returned. Otherwise, True is returned.

## Examples

### Basic Usage

```javascript
// Edit a message text
Api.editMessageText({
  chat_id: chat.id,
  message_id: message_id,
  text: "Updated content"
});
```

### With Formatting

```javascript
// Edit a message with Markdown formatting
Api.editMessageText({
  chat_id: chat.id,
  message_id: message_id,
  text: "*Updated* _with_ `formatting`",
  parse_mode: "Markdown"
});

// Edit a message with HTML formatting
Api.editMessageText({
  chat_id: chat.id,
  message_id: message_id,
  text: "<b>Updated</b> <i>with</i> <code>formatting</code>",
  parse_mode: "HTML"
});
```

### Updating Inline Keyboard

```javascript
// Edit text and update inline keyboard
Api.editMessageText({
  chat_id: chat.id,
  message_id: message_id,
  text: "Please select an option:",
  reply_markup: {
    inline_keyboard: [
      [
        { text: "Option 1", callback_data: "option1" },
        { text: "Option 2", callback_data: "option2" }
      ],
      [{ text: "Visit Website", url: "https://example.com" }]
    ]
  }
});
```

### Editing Inline Messages

```javascript
// Edit an inline message
Api.editMessageText({
  inline_message_id: inline_message_id,
  text: "Updated content",
  reply_markup: newInlineKeyboard
});
```

### Progressive Updates

```javascript
// Update a loading indicator
Api.editMessageText({
  chat_id: chat.id,
  message_id: msg.message_id,
  text: "Loading: 0%"
});

// Wait some time
Bot.delay(1000);

// Update progress
Api.editMessageText({
  chat_id: chat.id,
  message_id: msg.message_id,
  text: "Loading: 50%"
});

// Wait more time
Bot.delay(1000);

// Final update
Api.editMessageText({
  chat_id: chat.id,
  message_id: msg.message_id,
  text: "Loading complete!"
});
```

## Common Use Cases

### Dynamic Content Updates

```javascript
// Command: /on_callback_query
// Callback query handler to update data
if(query.data.startsWith("show_data_")) {
  let dataId = query.data.split("_")[2];
  let data = fetchData(dataId);
  
  Api.editMessageText({
    chat_id: query.message.chat.id,
    message_id: query.message.message_id,
    text: "Data for ID " + dataId + ":\n\n" + data,
    parse_mode: "Markdown",
    reply_markup: getDataKeyboard(dataId)
  });
  
  Api.answerCallbackQuery({
    callback_query_id: query.id
  });
}
```

### Form Navigation

```javascript
// Form with multiple steps
function showFormStep(chatId, messageId, step) {
  let text = "Form - Step " + step + "\n\n";
  let keyboard = {
    inline_keyboard: []
  };
  
  switch(step) {
    case 1:
      text += "Please enter your name:";
      keyboard.inline_keyboard = [
        [{ text: "Next", callback_data: "form_step_2" }]
      ];
      break;
    case 2:
      text += "Please enter your email:";
      keyboard.inline_keyboard = [
        [
          { text: "Back", callback_data: "form_step_1" },
          { text: "Next", callback_data: "form_step_3" }
        ]
      ];
      break;
    case 3:
      text += "Please enter your phone number:";
      keyboard.inline_keyboard = [
        [
          { text: "Back", callback_data: "form_step_2" },
          { text: "Submit", callback_data: "form_submit" }
        ]
      ];
      break;
  }
  
  Api.editMessageText({
    chat_id: chatId,
    message_id: messageId,
    text: text,
    reply_markup: keyboard
  });
}

// Command: /on_callback_query
if(query.data.startsWith("form_step_")) {
  let step = parseInt(query.data.split("_")[2]);
  showFormStep(query.message.chat.id, query.message.message_id, step);
  
  Api.answerCallbackQuery({
    callback_query_id: query.id
  });
}
```

## Error Handling

The method can throw errors in these cases:
- Message to edit not found
- Message can't be edited (e.g., too old)
- Bot was blocked by the user
- Message is not a bot message
- Text is empty or exceeds 4096 characters
- Markdown/HTML parsing errors if parse_mode is specified

```javascript
try {
  Api.editMessageText({
    chat_id: chat.id,
    message_id: message_id,
    text: "Updated text"
  });
} catch (error) {
  // Handle the error
  console.log("Error editing message: " + error.message);
  
  // Try an alternative approach
  Api.sendMessage({
    chat_id: chat.id,
    text: "Could not update previous message. Here's the new information: Updated text"
  });
}
```

## Notes

- You can edit messages for 48 hours after they have been sent
- Inline keyboards can be attached to messages with pure text (no message entities)
- You cannot edit media messages using this method; use the appropriate methods like `editMessageMedia` instead
- If editing fails, consider sending a new message instead

## Limitations

- Cannot edit messages that are older than 48 hours
- Cannot edit messages in channels/groups where the bot is not an administrator
- Cannot edit messages sent by other users or bots
- Cannot add or remove buttons on messages with media content using this method

## See Also

- [Message Object](../../types/message.md)
- [InlineKeyboardMarkup](../../types/inline-keyboard-markup.md)
- [Api.sendMessage](send-message.md)
- [Api.editMessageReplyMarkup](edit-message-reply-markup.md)
- [Api.editMessageMedia](edit-message-media.md)
- [Api.deleteMessage](delete-message.md) 