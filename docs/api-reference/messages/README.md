# Messages API Reference

This section covers all API methods related to sending, editing, and managing messages in TBL.

## Message Methods

| Method | Description |
|--------|-------------|
| [sendMessage](send-message.md) | Send a text message to a chat |
| [editMessageText](edit-message-text.md) | Edit the text of a message |
| [deleteMessage](delete-message.md) | Delete a message |
| [forwardMessage](forward-message.md) | Forward a message from one chat to another |
| [copyMessage](copy-message.md) | Copy a message content to another chat |
| [pinChatMessage](pin-chat-message.md) | Pin a message in a chat |
| [unpinChatMessage](unpin-chat-message.md) | Unpin a message in a chat |
| [unpinAllChatMessages](unpin-all-chat-messages.md) | Unpin all messages in a chat |

## Basic Usage

```javascript
// Send a simple text message
Api.sendMessage({
  chat_id: chat.id,
  text: "Hello world!"
});

// Edit a message
Api.editMessageText({
  chat_id: chat.id,
  message_id: msg.message_id,
  text: "Updated message text"
});

// Delete a message
Api.deleteMessage({
  chat_id: chat.id,
  message_id: msg.message_id
});
```

## Working with Formatting

```javascript
// Markdown formatting
Api.sendMessage({
  chat_id: chat.id,
  text: "*Bold text* and _italic text_",
  parse_mode: "Markdown"
});

// HTML formatting
Api.sendMessage({
  chat_id: chat.id,
  text: "<b>Bold text</b> and <i>italic text</i>",
  parse_mode: "HTML"
});
```

## Message with Reply Markup

```javascript
// Inline keyboard
Api.sendMessage({
  chat_id: chat.id,
  text: "Choose an option:",
  reply_markup: {
    inline_keyboard: [
      [
        { text: "Option 1", callback_data: "opt1" },
        { text: "Option 2", callback_data: "opt2" }
      ],
      [{ text: "Visit Website", url: "https://example.com" }]
    ]
  }
});

// Reply keyboard
Api.sendMessage({
  chat_id: chat.id,
  text: "Choose an option:",
  reply_markup: {
    keyboard: [
      ["Option 1", "Option 2"],
      ["Option 3", "Option 4"],
      ["Cancel"]
    ],
    resize_keyboard: true,
    one_time_keyboard: true
  }
});
```

## Related Topics

- [Message Object](../../types/message.md)
- [InlineKeyboardMarkup](../../types/inline-keyboard-markup.md)
- [ReplyKeyboardMarkup](../../types/reply-keyboard-markup.md)
- [Formatting Options](../../formatting.md) 