# Telegram Types Reference

This section documents all the Telegram object types available in TBL. Understanding these types is essential for effectively working with the Telegram API.

## Core Types

| Type | Description |
|------|-------------|
| [User](user.md) | Represents a Telegram user or bot |
| [Chat](chat.md) | Represents a chat (private, group, supergroup, or channel) |
| [Message](message.md) | Represents a message |
| [Update](update.md) | Represents an incoming update |

## Message Content Types

| Type | Description |
|------|-------------|
| [MessageEntity](message-entity.md) | Represents a special entity in a message text (URLs, usernames, etc.) |
| [PhotoSize](photo-size.md) | Represents a photo with specific size |
| [Audio](audio.md) | Represents an audio file |
| [Document](document.md) | Represents a general file |
| [Video](video.md) | Represents a video file |
| [Animation](animation.md) | Represents an animation file (GIF) |
| [Voice](voice.md) | Represents a voice message |
| [VideoNote](video-note.md) | Represents a video note |
| [Contact](contact.md) | Represents a shared contact |
| [Location](location.md) | Represents a shared location |
| [Venue](venue.md) | Represents a venue |
| [Poll](poll.md) | Represents a poll |
| [Dice](dice.md) | Represents a dice with a random value |

## Keyboard Types

| Type | Description |
|------|-------------|
| [InlineKeyboardMarkup](inline-keyboard-markup.md) | Represents an inline keyboard |
| [InlineKeyboardButton](inline-keyboard-button.md) | Represents a button in an inline keyboard |
| [ReplyKeyboardMarkup](reply-keyboard-markup.md) | Represents a custom keyboard |
| [KeyboardButton](keyboard-button.md) | Represents a button in a custom keyboard |
| [ReplyKeyboardRemove](reply-keyboard-remove.md) | Requests clients to remove the custom keyboard |
| [ForceReply](force-reply.md) | Forces a reply from the user |

## Inline Types

| Type | Description |
|------|-------------|
| [InlineQuery](inline-query.md) | Represents an incoming inline query |
| [InlineQueryResult](inline-query-result.md) | Represents a result of an inline query |
| [ChosenInlineResult](chosen-inline-result.md) | Represents a result of an inline query that was chosen by the user |

## Callback Types

| Type | Description |
|------|-------------|
| [CallbackQuery](callback-query.md) | Represents an incoming callback query from a callback button |

## Chat Member Types

| Type | Description |
|------|-------------|
| [ChatMember](chat-member.md) | Contains information about a chat member |
| [ChatMemberOwner](chat-member-owner.md) | Represents a chat owner |
| [ChatMemberAdministrator](chat-member-administrator.md) | Represents a chat administrator |
| [ChatMemberMember](chat-member-member.md) | Represents a chat member |
| [ChatMemberRestricted](chat-member-restricted.md) | Represents a restricted chat member |
| [ChatMemberLeft](chat-member-left.md) | Represents a user that isn't in the chat |
| [ChatMemberBanned](chat-member-banned.md) | Represents a banned user |

## Usage in TBL

In TBL, most of these types are directly accessible through the update context. For example:

```javascript
// Accessing User object
let userId = user.id;
let firstName = user.first_name;

// Accessing Message object
let messageId = msg.message_id;
let messageText = msg.text;

// Using Chat object
let chatId = chat.id;
let chatType = chat.type;
```

## Type Conversion

When working with the Telegram API, you might need to convert between different formats:

```javascript
// Converting a JavaScript object to JSON
let jsonString = JSON.stringify(myObject);

// Parsing JSON response
let parsedObject = JSON.parse(jsonString);
```

## Related Topics

- [Bot Object](../bot-object.md)
- [API Object](../api-object.md)
- [User Object](../user-object.md)
- [API Reference](../api-reference/messages/README.md) 