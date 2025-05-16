# User Object

The User object represents a Telegram user or bot that your bot can interact with.

## Structure

```javascript
{
  id: 12345678,
  is_bot: false,
  first_name: "John",
  last_name: "Doe",
  username: "johndoe",
  language_code: "en",
  is_premium: true,
  added_to_attachment_menu: false,
  can_join_groups: true,
  can_read_all_group_messages: false,
  supports_inline_queries: false
}
```

## Properties

| Property | Type | Description |
|----------|------|-------------|
| id | Integer | Unique identifier for this user or bot |
| is_bot | Boolean | True if this user is a bot |
| first_name | String | User's or bot's first name |
| last_name | String | *Optional*. User's or bot's last name |
| username | String | *Optional*. User's or bot's username |
| language_code | String | *Optional*. IETF language tag of the user's language |
| is_premium | Boolean | *Optional*. True if this user is a Telegram Premium user |
| added_to_attachment_menu | Boolean | *Optional*. True if this user added the bot to the attachment menu |
| can_join_groups | Boolean | *Optional*. True if the bot can be invited to groups |
| can_read_all_group_messages | Boolean | *Optional*. True if privacy mode is disabled for the bot |
| supports_inline_queries | Boolean | *Optional*. True if the bot supports inline queries |

## Access in TBL

In TBL, the User object can be accessed in several ways:

### From the Current Update

```javascript
// Access the current user who sent the message/update
let userId = user.id;
let firstName = user.first_name;
let lastName = user.last_name || ""; // Last name is optional
let username = user.username || ""; // Username is optional
```

### From a Message

```javascript
// Access user from a message
let messageUser = msg.from;
let senderId = msg.from.id;
let senderName = msg.from.first_name;
```

### From other Context

```javascript
// Get user from a callback query
let queryUser = query.from;

// Get user from an inline query
let inlineQueryUser = inlineQuery.from;

// Get user from a chosen inline result
let chosenResultUser = chosenInlineResult.from;
```

## User Data in TBL

TBL provides a special `User` object for interacting with user data. This is different from the Telegram User object described above. The TBL User object provides methods for managing user-specific data:

```javascript
// Store user property
User.setProp("points", 100);

// Retrieve user property
let points = User.getProp("points", 0); // Default value of 0 if property doesn't exist

// Delete user property
User.delProp("temporary_data");

// Get all user properties
let allUserData = User.getAllProp();
```

## Examples

### Accessing User Information

```javascript
// Command: /userinfo
// Code:
let text = "User Information:\n";
text += "🆔 ID: " + user.id + "\n";
text += "👤 Name: " + user.first_name;

if(user.last_name) {
  text += " " + user.last_name;
}

if(user.username) {
  text += "\n🔤 Username: @" + user.username;
}

if(user.language_code) {
  text += "\n🌍 Language: " + user.language_code;
}

if(user.is_premium) {
  text += "\n⭐ Premium User: Yes";
}

Bot.sendMessage(text);
```

### Storing User Preferences

```javascript
// Command: /setlanguage
// Code:
let language = params.toLowerCase();
let supportedLanguages = ["en", "es", "fr", "de", "ru"];

if(!supportedLanguages.includes(language)) {
  Bot.sendMessage("Unsupported language. Please use one of: " + supportedLanguages.join(", "));
  return;
}

User.setProp("language", language);
Bot.sendMessage("Language set to: " + language);
```

### User Points System

```javascript
// Command: /addpoints
// Code:
let points = parseInt(params) || 10;
let currentPoints = User.getProp("points", 0);
let newPoints = currentPoints + points;

User.setProp("points", newPoints);
Bot.sendMessage("Added " + points + " points. You now have " + newPoints + " points!");

// Command: /mypoints
// Code:
let points = User.getProp("points", 0);
Bot.sendMessage("You have " + points + " points!");
```

## Creating User References

```javascript
// Creating a mention with the user's name
let mention = "[" + user.first_name + "](tg://user?id=" + user.id + ")";

Api.sendMessage({
  chat_id: chat.id,
  text: "Hello " + mention + "!",
  parse_mode: "Markdown"
});

// Using TgUtil library to create mentions
let mention = Libs.TgUtil.mention(user.id, user.first_name);

Api.sendMessage({
  chat_id: chat.id,
  text: "Hello " + mention + "!",
  parse_mode: "HTML"
});
```

## User Management

### Checking Admin Status

```javascript
// Check if a user is an admin in a group
async function isUserAdmin(chatId, userId) {
  try {
    let member = await Api.getChatMember({
      chat_id: chatId,
      user_id: userId
    });
    
    return ["creator", "administrator"].includes(member.status);
  } catch (error) {
    console.log("Error checking admin status: " + error.message);
    return false;
  }
}

// Usage
let isAdmin = await isUserAdmin(chat.id, user.id);
if(isAdmin) {
  Bot.sendMessage("You are an admin in this chat!");
} else {
  Bot.sendMessage("You are not an admin in this chat.");
}
```

### Banning Users

```javascript
// Ban a user from a group
function banUser(chatId, userId, reason) {
  try {
    Api.banChatMember({
      chat_id: chatId,
      user_id: userId
    });
    
    return true;
  } catch (error) {
    console.log("Error banning user: " + error.message);
    return false;
  }
}

// Usage
let targetUserId = 12345678;
let success = banUser(chat.id, targetUserId, "Spam");

if(success) {
  Bot.sendMessage("User banned successfully.");
} else {
  Bot.sendMessage("Failed to ban user.");
}
```

## Working with User Photos

```javascript
// Get user profile photos
async function getUserPhotos(userId) {
  try {
    let photos = await Api.getUserProfilePhotos({
      user_id: userId,
      limit: 10
    });
    
    return photos;
  } catch (error) {
    console.log("Error getting user photos: " + error.message);
    return null;
  }
}

// Usage
let photos = await getUserPhotos(user.id);
if(photos && photos.total_count > 0) {
  // Send the first photo
  Api.sendPhoto({
    chat_id: chat.id,
    photo: photos.photos[0][0].file_id,
    caption: "Your profile photo!"
  });
} else {
  Bot.sendMessage("You don't have any profile photos.");
}
```

## Best Practices

1. **Always Check Optional Properties**: Some properties like `last_name`, `username`, and `language_code` are optional. Always check if they exist before using them.

2. **Use Proper Defaults**: When getting user properties, provide default values to handle cases where the property doesn't exist yet.

3. **Respect Privacy**: Don't share user information between different users without permission.

4. **Handle Large User Data Efficiently**: If storing complex user data, consider using JSON for nested structures rather than multiple properties.

5. **Use User IDs for Identification**: Always use the `id` field for uniquely identifying users, never rely on username as it can change.

6. **Clear Temporary Data**: Delete temporary user properties when they're no longer needed to save storage space.

## Limitations

- The `language_code` field represents the user's client language, not necessarily their preferred language for your bot.
- Not all optional fields are always available, depending on user privacy settings and whether they've interacted with your bot directly.
- You cannot access a user's phone number or email through the User object unless they've explicitly shared it.

## See Also

- [Chat Object](chat.md)
- [Message Object](message.md)
- [ChatMember Object](chat-member.md)
- [User Object in TBL](../user-object.md)
- [Api.getChatMember](../api-reference/chats/get-chat-member.md)
- [Api.getUserProfilePhotos](../api-reference/users/get-user-profile-photos.md) 