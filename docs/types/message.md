# Message Object

The Message object represents a message sent in a chat.

## Structure

```javascript
{
  message_id: 123,
  from: User,
  date: 1623456789,
  chat: Chat,
  text: "Hello, world!",
  // ... other fields depending on message type
}
```

## Properties

| Property | Type | Description |
|----------|------|-------------|
| message_id | Integer | Unique message identifier inside this chat |
| from | [User](user.md) | Sender of the message; empty for messages sent to channels. For backward compatibility, the field contains a fake sender user in non-channel chats, if the message was sent on behalf of a chat |
| sender_chat | [Chat](chat.md) | Sender of the message, sent on behalf of a chat (for channels or anonymous admins) |
| date | Integer | Date the message was sent in Unix time |
| chat | [Chat](chat.md) | Conversation the message belongs to |
| forward_from | [User](user.md) | *Optional*. For forwarded messages, sender of the original message |
| forward_from_chat | [Chat](chat.md) | *Optional*. For messages forwarded from channels or from anonymous admins, information about the original sender chat |
| forward_from_message_id | Integer | *Optional*. For messages forwarded from channels, identifier of the original message in the channel |
| forward_signature | String | *Optional*. For forwarded messages that were originally sent in channels or by an anonymous chat administrator, signature of the message sender if present |
| forward_sender_name | String | *Optional*. Sender's name for messages forwarded from users who disallow adding a link to their account in forwarded messages |
| forward_date | Integer | *Optional*. For forwarded messages, date the original message was sent in Unix time |
| reply_to_message | [Message](message.md) | *Optional*. For replies, the original message. Note that the Message object in this field will not contain further reply_to_message fields even if it itself is a reply |
| via_bot | [User](user.md) | *Optional*. Bot through which the message was sent |
| edit_date | Integer | *Optional*. Date the message was last edited in Unix time |
| media_group_id | String | *Optional*. The unique identifier of a media message group this message belongs to |
| author_signature | String | *Optional*. Signature of the post author for messages in channels, or the custom title of an anonymous group administrator |
| text | String | *Optional*. For text messages, the actual UTF-8 text of the message, 0-4096 characters |
| entities | Array of [MessageEntity](message-entity.md) | *Optional*. For text messages, special entities like usernames, URLs, bot commands, etc. that appear in the text |
| animation | [Animation](animation.md) | *Optional*. Message is an animation, information about the animation |
| audio | [Audio](audio.md) | *Optional*. Message is an audio file, information about the file |
| document | [Document](document.md) | *Optional*. Message is a general file, information about the file |
| photo | Array of [PhotoSize](photo-size.md) | *Optional*. Message is a photo, available sizes of the photo |
| sticker | [Sticker](sticker.md) | *Optional*. Message is a sticker, information about the sticker |
| video | [Video](video.md) | *Optional*. Message is a video, information about the video |
| video_note | [VideoNote](video-note.md) | *Optional*. Message is a video note, information about the video message |
| voice | [Voice](voice.md) | *Optional*. Message is a voice message, information about the file |
| caption | String | *Optional*. Caption for the animation, audio, document, photo, video or voice, 0-1024 characters |
| caption_entities | Array of [MessageEntity](message-entity.md) | *Optional*. For messages with a caption, special entities like usernames, URLs, bot commands, etc. that appear in the caption |
| contact | [Contact](contact.md) | *Optional*. Message is a shared contact, information about the contact |
| dice | [Dice](dice.md) | *Optional*. Message is a dice with random value |
| game | [Game](game.md) | *Optional*. Message is a game, information about the game |
| poll | [Poll](poll.md) | *Optional*. Message is a native poll, information about the poll |
| venue | [Venue](venue.md) | *Optional*. Message is a venue, information about the venue |
| location | [Location](location.md) | *Optional*. Message is a shared location, information about the location |
| new_chat_members | Array of [User](user.md) | *Optional*. New members that were added to the group or supergroup and information about them (the bot itself may be one of these members) |
| left_chat_member | [User](user.md) | *Optional*. A member was removed from the group, information about them (this member may be the bot itself) |
| new_chat_title | String | *Optional*. A chat title was changed to this value |
| new_chat_photo | Array of [PhotoSize](photo-size.md) | *Optional*. A chat photo was change to this value |
| delete_chat_photo | Boolean | *Optional*. Service message: the chat photo was deleted |
| group_chat_created | Boolean | *Optional*. Service message: the group has been created |
| supergroup_chat_created | Boolean | *Optional*. Service message: the supergroup has been created |
| channel_chat_created | Boolean | *Optional*. Service message: the channel has been created |
| message_auto_delete_timer_changed | [MessageAutoDeleteTimerChanged](message-auto-delete-timer-changed.md) | *Optional*. Service message: auto-delete timer settings changed in the chat |
| migrate_to_chat_id | Integer | *Optional*. The group has been migrated to a supergroup with the specified identifier |
| migrate_from_chat_id | Integer | *Optional*. The supergroup has been migrated from a group with the specified identifier |
| pinned_message | [Message](message.md) | *Optional*. Specified message was pinned |
| invoice | [Invoice](invoice.md) | *Optional*. Message is an invoice for a payment, information about the invoice |
| successful_payment | [SuccessfulPayment](successful-payment.md) | *Optional*. Message is a service message about a successful payment |
| connected_website | String | *Optional*. The domain name of the website on which the user has logged in |
| passport_data | [PassportData](passport-data.md) | *Optional*. Telegram Passport data |
| proximity_alert_triggered | [ProximityAlertTriggered](proximity-alert-triggered.md) | *Optional*. Service message. A user in the chat triggered another user's proximity alert while sharing Live Location |
| video_chat_scheduled | [VideoChatScheduled](video-chat-scheduled.md) | *Optional*. Service message: video chat scheduled |
| video_chat_started | [VideoChatStarted](video-chat-started.md) | *Optional*. Service message: video chat started |
| video_chat_ended | [VideoChatEnded](video-chat-ended.md) | *Optional*. Service message: video chat ended |
| video_chat_participants_invited | [VideoChatParticipantsInvited](video-chat-participants-invited.md) | *Optional*. Service message: new participants invited to a video chat |
| reply_markup | [InlineKeyboardMarkup](inline-keyboard-markup.md) | *Optional*. Inline keyboard attached to the message |

## Access in TBL

In TBL, the current message is usually available through the `msg` object:

```javascript
// Access basic message properties
let messageId = msg.message_id;
let messageText = msg.text;
let senderId = msg.from.id;
let chatId = msg.chat.id;

// Check message type
if(msg.photo) {
  // Handle photo message
  let photoId = msg.photo[0].file_id;
}

if(msg.voice) {
  // Handle voice message
  let voiceDuration = msg.voice.duration;
}

// Access reply to message
if(msg.reply_to_message) {
  let repliedText = msg.reply_to_message.text;
}

// Check for entities
if(msg.entities) {
  for(let entity of msg.entities) {
    if(entity.type === "url") {
      // Extract URL from the message
      let url = msg.text.substring(entity.offset, entity.offset + entity.length);
    }
  }
}
```

## Examples

### Text Message

```javascript
{
  message_id: 123,
  from: {
    id: 12345678,
    is_bot: false,
    first_name: "John",
    last_name: "Doe",
    username: "johndoe",
    language_code: "en"
  },
  chat: {
    id: 12345678,
    first_name: "John",
    last_name: "Doe",
    username: "johndoe",
    type: "private"
  },
  date: 1623456789,
  text: "Hello, world!",
  entities: [
    {
      offset: 0,
      length: 5,
      type: "bold"
    }
  ]
}
```

### Photo Message

```javascript
{
  message_id: 124,
  from: {
    id: 12345678,
    is_bot: false,
    first_name: "John",
    last_name: "Doe",
    username: "johndoe",
    language_code: "en"
  },
  chat: {
    id: 12345678,
    first_name: "John",
    last_name: "Doe",
    username: "johndoe",
    type: "private"
  },
  date: 1623456790,
  photo: [
    {
      file_id: "AgACAgIAAxkBAAECu7ZhF8hzCy-vZf9X...",
      file_unique_id: "AQADWlC6y74AAwI",
      file_size: 1672,
      width: 90,
      height: 51
    },
    {
      file_id: "AgACAgIAAxkBAAECu7ZhF8hzCy-vZf9X...",
      file_unique_id: "AQADWlC6y74AAwM",
      file_size: 17343,
      width: 320,
      height: 180
    },
    {
      file_id: "AgACAgIAAxkBAAECu7ZhF8hzCy-vZf9X...",
      file_unique_id: "AQADWlC6y74AAwQ",
      file_size: 66792,
      width: 800,
      height: 450
    }
  ],
  caption: "A beautiful sunset",
  caption_entities: []
}
```

### Reply Message

```javascript
{
  message_id: 125,
  from: {
    id: 12345678,
    is_bot: false,
    first_name: "John",
    last_name: "Doe",
    username: "johndoe",
    language_code: "en"
  },
  chat: {
    id: 12345678,
    first_name: "John",
    last_name: "Doe",
    username: "johndoe",
    type: "private"
  },
  date: 1623456791,
  text: "This is a reply",
  reply_to_message: {
    message_id: 123,
    from: {
      id: 12345678,
      is_bot: false,
      first_name: "John",
      last_name: "Doe",
      username: "johndoe",
      language_code: "en"
    },
    chat: {
      id: 12345678,
      first_name: "John",
      last_name: "Doe",
      username: "johndoe",
      type: "private"
    },
    date: 1623456789,
    text: "Hello, world!"
  }
}
```

## Common Operations

### Checking Message Type

```javascript
// Check if message contains text
if(msg.text) {
  // Process text message
}

// Check if message contains photo
if(msg.photo) {
  // Process photo message
}

// Check if message contains document
if(msg.document) {
  // Process document message
}

// Check if message is a group chat creation
if(msg.group_chat_created) {
  // Handle new group creation
}
```

### Working with Entities

```javascript
// Get all URLs from message
function getUrls(msg) {
  let urls = [];
  if(msg.entities) {
    for(let entity of msg.entities) {
      if(entity.type === "url") {
        urls.push(msg.text.substring(entity.offset, entity.offset + entity.length));
      }
    }
  }
  return urls;
}

// Get all mentioned users
function getMentions(msg) {
  let mentions = [];
  if(msg.entities) {
    for(let entity of msg.entities) {
      if(entity.type === "mention") {
        mentions.push(msg.text.substring(entity.offset, entity.offset + entity.length));
      }
    }
  }
  return mentions;
}
```

### Forwarding Messages

```javascript
// Forward a message to another chat
Api.forwardMessage({
  chat_id: targetChatId,
  from_chat_id: msg.chat.id,
  message_id: msg.message_id
});
```

## See Also

- [User Object](user.md)
- [Chat Object](chat.md)
- [MessageEntity Object](message-entity.md)
- [Api.sendMessage](../api-reference/messages/send-message.md)
- [Api.forwardMessage](../api-reference/messages/forward-message.md)
- [Api.copyMessage](../api-reference/messages/copy-message.md) 