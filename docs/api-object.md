# API Object

The `Api` object provides direct access to the Telegram Bot API, allowing you to use all the features available in the official Telegram Bot API.

## Overview

The `Api` object is a powerful interface that:
- Provides access to all Telegram Bot API methods
- Handles error management automatically
- Maintains the bot state between calls
- Manages chat ID automatically for most methods

## Available Methods

The `Api` object includes all methods from the [Telegram Bot API](https://core.telegram.org/bots/api#available-methods). Here are some of the most commonly used methods:

### Message Methods

#### Api.sendMessage(params)

Sends a text message.

**Parameters:**
- `params` (object): Message parameters
  - `text` (string): Text of the message
  - `parse_mode` (string, optional): "Markdown" or "HTML"
  - `entities` (array, optional): List of special entities in the message
  - `disable_web_page_preview` (boolean, optional): Disables link previews
  - `disable_notification` (boolean, optional): Sends the message silently
  - `protect_content` (boolean, optional): Protects content from forwarding/saving
  - `reply_to_message_id` (number, optional): Message to reply to
  - `allow_sending_without_reply` (boolean, optional): If true, message will be sent even if replied-to message is not found
  - `reply_markup` (object, optional): Additional interface options

**Example:**
```javascript
Api.sendMessage({
  text: "Hello world!",
  parse_mode: "Markdown",
  reply_markup: {
    inline_keyboard: [
      [{ text: "Button 1", callback_data: "btn1" }],
      [{ text: "Button 2", callback_data: "btn2" }]
    ]
  }
});
```

#### Api.sendPhoto(params)

Sends a photo.

**Parameters:**
- `params` (object): Photo parameters
  - `photo` (string): Photo to send (file_id, URL, or file path)
  - `caption` (string, optional): Caption for the photo
  - `parse_mode` (string, optional): "Markdown" or "HTML" for caption formatting
  - `caption_entities` (array, optional): Special entities in the caption
  - Other parameters similar to sendMessage

**Example:**
```javascript
Api.sendPhoto({
  photo: "https://example.com/image.jpg",
  caption: "A beautiful landscape",
  parse_mode: "Markdown"
});
```

#### Api.sendDocument(params)

Sends a file as a document.

**Parameters:**
- `params` (object): Document parameters
  - `document` (string): Document to send (file_id, URL, or file path)
  - `thumb` (string, optional): Thumbnail of the file
  - `caption` (string, optional): Document caption
  - `parse_mode` (string, optional): "Markdown" or "HTML" for caption formatting
  - Other parameters similar to sendMessage

**Example:**
```javascript
Api.sendDocument({
  document: "https://example.com/document.pdf",
  caption: "Important document",
  parse_mode: "Markdown"
});
```

#### Api.sendAudio(params)

Sends an audio file.

**Parameters:**
- `params` (object): Audio parameters
  - `audio` (string): Audio file to send (file_id, URL, or file path)
  - `caption` (string, optional): Audio caption
  - `parse_mode` (string, optional): "Markdown" or "HTML" for caption formatting
  - `duration` (number, optional): Duration in seconds
  - `performer` (string, optional): Performer name
  - `title` (string, optional): Track name
  - `thumb` (string, optional): Thumbnail of the file
  - Other parameters similar to sendMessage

**Example:**
```javascript
Api.sendAudio({
  audio: "https://example.com/audio.mp3",
  caption: "My favorite song",
  performer: "Artist Name",
  title: "Song Title",
  parse_mode: "Markdown"
});
```

#### Api.sendVideo(params)

Sends a video file.

**Parameters:**
- `params` (object): Video parameters
  - `video` (string): Video to send (file_id, URL, or file path)
  - `duration` (number, optional): Duration in seconds
  - `width` (number, optional): Video width
  - `height` (number, optional): Video height
  - `thumb` (string, optional): Thumbnail of the file
  - `caption` (string, optional): Video caption
  - `parse_mode` (string, optional): "Markdown" or "HTML" for caption formatting
  - Other parameters similar to sendMessage

**Example:**
```javascript
Api.sendVideo({
  video: "https://example.com/video.mp4",
  caption: "Check out this video!",
  parse_mode: "Markdown"
});
```

### Chat Management Methods

#### Api.getChat(params)

Gets information about a chat.

**Parameters:**
- `params` (object): Parameters object
  - `chat_id` (string/number): Unique identifier for the target chat

**Example:**
```javascript
let chatInfo = Api.getChat({
  chat_id: chat.id
});
Bot.inspect(chatInfo);
```

#### Api.getChatMember(params)

Gets information about a member of a chat.

**Parameters:**
- `params` (object): Parameters object
  - `chat_id` (string/number): Unique identifier for the target chat
  - `user_id` (number): Unique identifier of the target user

**Example:**
```javascript
let memberInfo = Api.getChatMember({
  chat_id: chat.id,
  user_id: user.id
});
let isAdmin = memberInfo.status === "administrator" || memberInfo.status === "creator";
```

#### Api.banChatMember(params)

Bans a user from a chat.

**Parameters:**
- `params` (object): Parameters object
  - `chat_id` (string/number): Unique identifier for the target chat
  - `user_id` (number): Unique identifier of the target user
  - `until_date` (number, optional): Date when the user will be unbanned (Unix time)
  - `revoke_messages` (boolean, optional): Pass True to delete all messages from the user

**Example:**
```javascript
// Ban a user for 24 hours
let untilDate = Math.floor(Date.now() / 1000) + 86400;
Api.banChatMember({
  chat_id: chat.id,
  user_id: userId,
  until_date: untilDate
});
```

#### Api.unbanChatMember(params)

Unbans a previously banned user from a chat.

**Parameters:**
- `params` (object): Parameters object
  - `chat_id` (string/number): Unique identifier for the target chat
  - `user_id` (number): Unique identifier of the target user
  - `only_if_banned` (boolean, optional): Do nothing if the user is not banned

**Example:**
```javascript
Api.unbanChatMember({
  chat_id: chat.id,
  user_id: userId,
  only_if_banned: true
});
```

### Message Editing Methods

#### Api.editMessageText(params)

Edits the text of a message.

**Parameters:**
- `params` (object): Parameters object
  - `chat_id` (string/number): Unique identifier for the target chat
  - `message_id` (number): Identifier of the message to edit
  - `inline_message_id` (string, optional): Identifier of the inline message
  - `text` (string): New text of the message
  - `parse_mode` (string, optional): "Markdown" or "HTML"
  - `entities` (array, optional): List of special entities
  - `disable_web_page_preview` (boolean, optional): Disables link previews
  - `reply_markup` (object, optional): Inline keyboard markup

**Example:**
```javascript
Api.editMessageText({
  message_id: msgId,
  text: "Updated information",
  parse_mode: "Markdown"
});
```

#### Api.editMessageCaption(params)

Edits the caption of a media message.

**Parameters:**
- `params` (object): Parameters object
  - `chat_id` (string/number): Unique identifier for the target chat
  - `message_id` (number): Identifier of the message to edit
  - `inline_message_id` (string, optional): Identifier of the inline message
  - `caption` (string): New caption of the message
  - `parse_mode` (string, optional): "Markdown" or "HTML"
  - `caption_entities` (array, optional): List of special entities
  - `reply_markup` (object, optional): Inline keyboard markup

**Example:**
```javascript
Api.editMessageCaption({
  message_id: msgId,
  caption: "Updated caption",
  parse_mode: "Markdown"
});
```

#### Api.editMessageReplyMarkup(params)

Edits only the reply markup of a message.

**Parameters:**
- `params` (object): Parameters object
  - `chat_id` (string/number): Unique identifier for the target chat
  - `message_id` (number): Identifier of the message to edit
  - `inline_message_id` (string, optional): Identifier of the inline message
  - `reply_markup` (object): Inline keyboard markup

**Example:**
```javascript
Api.editMessageReplyMarkup({
  message_id: msgId,
  reply_markup: {
    inline_keyboard: [
      [{ text: "Updated Button", callback_data: "new_btn" }]
    ]
  }
});
```

### Inline Mode Methods

#### Api.answerInlineQuery(params)

Sends answers to an inline query.

**Parameters:**
- `params` (object): Parameters object
  - `inline_query_id` (string): Unique identifier for the answered query
  - `results` (array): Array of InlineQueryResult objects
  - `cache_time` (number, optional): Maximum cache time in seconds
  - `is_personal` (boolean, optional): Pass True if results may be different for different users
  - `next_offset` (string, optional): Pass the offset for results pagination
  - `switch_pm_text` (string, optional): Text shown in the switch_pm button
  - `switch_pm_parameter` (string, optional): Parameter for the deep-linking in the bot when the button is pressed

**Example:**
```javascript
Api.answerInlineQuery({
  inline_query_id: update.inline_query.id,
  results: [
    {
      type: "article",
      id: "1",
      title: "Result Title",
      input_message_content: {
        message_text: "Result content text"
      },
      description: "Result description"
    }
  ],
  cache_time: 300
});
```

### Callback Query Methods

#### Api.answerCallbackQuery(params)

Sends an answer to a callback query.

**Parameters:**
- `params` (object): Parameters object
  - `callback_query_id` (string): Unique identifier for the query to be answered
  - `text` (string, optional): Text of the notification (0-200 characters)
  - `show_alert` (boolean, optional): If true, shows an alert instead of a notification
  - `url` (string, optional): URL that will be opened by the user's client
  - `cache_time` (number, optional): Cache time in seconds

**Example:**
```javascript
Api.answerCallbackQuery({
  callback_query_id: update.callback_query.id,
  text: "Button clicked!",
  show_alert: false
});
```

## Working with Telegram API

### Custom Keyboards

#### Reply Keyboards

```javascript
Api.sendMessage({
  text: "Please select an option:",
  reply_markup: {
    keyboard: [
      [{ text: "Option 1" }, { text: "Option 2" }],
      [{ text: "Option 3" }]
    ],
    resize_keyboard: true,
    one_time_keyboard: true
  }
});
```

#### Inline Keyboards

```javascript
Api.sendMessage({
  text: "Please select an option:",
  reply_markup: {
    inline_keyboard: [
      [
        { text: "Option 1", callback_data: "option1" },
        { text: "Option 2", callback_data: "option2" }
      ],
      [
        { text: "Visit Website", url: "https://example.com" }
      ]
    ]
  }
});
```

#### Remove Keyboard

```javascript
Api.sendMessage({
  text: "Keyboard removed",
  reply_markup: {
    remove_keyboard: true
  }
});
```

### Markdown and HTML Formatting

#### Markdown Example

```javascript
Api.sendMessage({
  text: "*Bold text* _Italic text_ `Code` [Link](https://example.com)",
  parse_mode: "Markdown"
});
```

#### HTML Example

```javascript
Api.sendMessage({
  text: "<b>Bold text</b> <i>Italic text</i> <code>Code</code> <a href='https://example.com'>Link</a>",
  parse_mode: "HTML"
});
```

### Media Groups

Send multiple photos or videos as an album:

```javascript
Api.sendMediaGroup({
  media: [
    {
      type: "photo",
      media: "https://example.com/image1.jpg",
      caption: "First image"
    },
    {
      type: "photo",
      media: "https://example.com/image2.jpg"
    }
  ]
});
```

### Location and Venues

#### Sending Location

```javascript
Api.sendLocation({
  latitude: 37.7749,
  longitude: -122.4194
});
```

#### Sending Venue

```javascript
Api.sendVenue({
  latitude: 37.7749,
  longitude: -122.4194,
  title: "San Francisco",
  address: "California, USA"
});
```

### Contact Sharing

```javascript
Api.sendContact({
  phone_number: "+1234567890",
  first_name: "John",
  last_name: "Doe"
});
```

## Error Handling

The `Api` object automatically handles errors, but you can also implement custom error handling:

```javascript
try {
  Api.sendMessage({
    text: "Message with error handling"
  });
} catch (error) {
  Bot.sendMessage("Error occurred: " + error.message);
}
```

## Best Practices

### Chat ID Management

In most cases, the `Api` object automatically uses the correct chat ID from the current update. However, you can explicitly set it when needed:

```javascript
// Send to a different chat
Api.sendMessage({
  chat_id: otherChatId,
  text: "Message to another chat"
});
```

### Handling Rate Limits

Telegram imposes rate limits. Consider adding delays between multiple API calls:

```javascript
// Send multiple messages with delay
for (let i = 0; i < messages.length; i++) {
  setTimeout(() => {
    Api.sendMessage({
      text: messages[i]
    });
  }, i * 1000); // 1 second delay between messages
}
```

### Secure Data Handling

Never expose sensitive data in messages:

```javascript
// DON'T DO THIS
Api.sendMessage({
  text: "API key: " + apiKey
});

// BETTER:
Api.sendMessage({
  text: "Configuration updated successfully"
});
```

### Performance Optimization

Use the `on_run` parameter for chaining commands when needed:

```javascript
Api.sendMessage({
  text: "Processing complete!",
  on_run: "nextCommand"
});
``` 