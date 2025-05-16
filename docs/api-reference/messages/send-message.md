# Api.sendMessage

Sends a text message to a chat.

## Signature

```javascript
Api.sendMessage(params)
```

## Description

This method sends a text message to a specified chat. The message can include formatting (Markdown or HTML), inline keyboards, reply markup, and other customization options. It's the most basic and frequently used method for sending content to users.

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| chat_id | Integer or String | Yes | Unique identifier for the target chat or username of the target channel (in the format @channelusername) |
| text | String | Yes | Text of the message (1-4096 characters) |
| parse_mode | String | No | Mode for parsing entities. Either "Markdown" or "HTML" |
| entities | Array of MessageEntity | No | List of special entities that appear in the message text. Can be used instead of parse_mode |
| disable_web_page_preview | Boolean | No | Disables link previews for links in this message |
| disable_notification | Boolean | No | Sends the message silently. Users will receive a notification with no sound |
| protect_content | Boolean | No | Protects the content of the sent message from forwarding and saving |
| reply_to_message_id | Integer | No | If the message is a reply, ID of the original message |
| allow_sending_without_reply | Boolean | No | Pass True if the message should be sent even if the specified replied-to message is not found |
| reply_markup | Object | No | Additional interface options. A JSON-serialized object for an inline keyboard, custom reply keyboard, instructions to remove reply keyboard or to force a reply from the user |

## Returns

On success, the sent [Message](../../types/message.md) object is returned.

## Example

### Basic Usage

```javascript
// Simple text message
Api.sendMessage({
  chat_id: chat.id,
  text: "Hello world!"
});
```

### Formatted Text

```javascript
// Using Markdown formatting
Api.sendMessage({
  chat_id: chat.id,
  text: "*Bold text* and _italic text_",
  parse_mode: "Markdown"
});

// Using HTML formatting
Api.sendMessage({
  chat_id: chat.id,
  text: "<b>Bold text</b> and <i>italic text</i>",
  parse_mode: "HTML"
});
```

### With Inline Keyboard

```javascript
Api.sendMessage({
  chat_id: chat.id,
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

### With Reply Keyboard

```javascript
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

### Silent Message

```javascript
Api.sendMessage({
  chat_id: chat.id,
  text: "This message won't trigger a notification sound",
  disable_notification: true
});
```

### As Reply to Another Message

```javascript
Api.sendMessage({
  chat_id: chat.id,
  text: "This is a reply",
  reply_to_message_id: msg.message_id
});
```

## Error Handling

The method can throw errors in these cases:
- Chat not found
- Bot was blocked by the user
- Text is empty or exceeds 4096 characters
- Markdown/HTML parsing errors if parse_mode is specified
- Invalid reply_to_message_id

```javascript
try {
  Api.sendMessage({
    chat_id: chat.id,
    text: "Hello world!"
  });
} catch (error) {
  // Handle error
  console.log("Error sending message: " + error.message);
}
```

## Notes

- Maximum message length is 4096 characters
- When using HTML parsing, certain tags must be escaped:
  - `<` should be replaced with `&lt;`
  - `>` should be replaced with `&gt;`
  - `&` should be replaced with `&amp;`
- Reply markup can be:
  - InlineKeyboardMarkup
  - ReplyKeyboardMarkup
  - ReplyKeyboardRemove
  - ForceReply
- Mobile clients will display a notification for incoming messages by default. disable_notification can be used to send a silent message.

## See Also

- [Message Object](../../types/message.md)
- [Edit Message Text](edit-message-text.md)
- [Delete Message](delete-message.md)
- [Formatting Options](../../guides/formatting.md)
- [Keyboards Guide](../../guides/keyboards.md) 