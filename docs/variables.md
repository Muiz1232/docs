# **Variables in TBL – The Secret Sauce of Your Bot’s Brain**  

Welcome to the **hidden variables** that power your bot! These are the *magic ingredients* that make your commands dynamic, responsive, and occasionally sassy.  

---

## **🚀 Built-in Variables – Ready to Use, No Setup Needed**  

### **`bot` – Your Bot’s Identity Card** *(Object)*  
Contains all the juicy details about your bot.  
```javascript  
msg.reply(`Hi! I'm ${bot.first_name}. My username: @${bot.username}`);  
```  
📌 **Key Fields:**  
- `id` (Bot’s Telegram ID)  
- `first_name`, `last_name`  
- `username`  
- `is_premium` (Yes, even bots can flex.)  
- `can_join_groups`, `can_read_messages` (Privacy settings.)  

🔗 *Full docs:* [objects/bot.md](#) *(Pretend this is a real link.)*  

---

### **`update` – The Raw Telegram Update** *(Object)*  
The **unfiltered, unedited** data straight from Telegram’s servers.  
⚠️ **Warning:** Mess with this only if you *really* know what you’re doing.  

📌 **Example Structure:**  
```json  
{
  "update_id": 123456789,
  "message": {
    "message_id": 42,
    "from": { "id": 123, "first_name": "John" },
    "chat": { "id": 123, "type": "private" },
    "text": "/start"
  }
}
```  

---

### **`update_type` – What Just Happened?** *(String)*  
Tells you **what kind of update** triggered your command.  
```javascript  
if (update_type === "callback_query") {  
  msg.reply("You clicked a button!");  
}  
```  
📌 **Common Types:**  
- `"message"` (Regular message)  
- `"edited_message"`  
- `"callback_query"` (Button clicks)  
- `"inline_query"`  
- `"poll_answer"`  

---

### **`request` – The Useful Part of `update`** *(Object)*  
A **cleaned-up version** of `update`, focusing only on the relevant part.  

📌 **How It Works:**  
| If `update_type` is... | Then `request` = `update.[type]` |  
|------------------------|----------------------------------|  
| `"message"`            | `update.message`                |  
| `"callback_query"`     | `update.callback_query`         |  
| `"inline_query"`       | `update.inline_query`           |  

Example:  
```javascript  
// For a callback query, get the data  
if (update_type === "callback_query") {  
  const buttonData = request.data;  
  msg.reply(`You chose: ${buttonData}`);  
}  
```  

---

### **`message` – The Text (If It Exists)** *(String | Null)*  
- If the update is a text message → `request.text`  
- Otherwise → `null`  

```javascript  
if (message) {  
  msg.reply(`You said: "${message}"`);  
}  
```  

---

### **`user` – Who’s Talking?** *(Object)*  
The **current user** (the one who triggered the command).  

📌 **Key Fields:**  
- `id` (Telegram ID)  
- `first_name`, `last_name`  
- `username`  
- `language_code` (e.g., `"en"`)  
- `is_premium` (💎)  
- `is_bot` (Watch out for impostors!)  

🔗 *Full docs:* [objects/user.md](#)  

---

### **`chat` – Where the Magic Happens** *(Object)*  
Details about the **current chat** (group, private, channel, etc.).  

📌 **Key Fields:**  
- `id`  
- `type` (`"private"`, `"group"`, `"supergroup"`, `"channel"`)  
- `title` (Groups/channels only)  
- `username` (If public)  

🔗 *Full docs:* [objects/chat.md](#)  

---

### **`params` – Extra Text After Commands** *(String)*  
- User sends `/start 123` → `params = "123"`  
- User sends `hello world` → `params = "world"` (if command is `"hello"`)  

```javascript  
// /price BTC  
if (params) {  
  const crypto = params.trim(); // "BTC"  
  msg.reply(`Fetching price of ${crypto}...`);  
}  
```  

---

### **`filename` – Current Command Name** *(String)*  
The **name of the command** being executed.  
```javascript  
msg.reply(`You triggered: ${filename}`);  
```  

---

## **🎮 Special Variables for Advanced Play**  

### **Dynamic Button Data (`callback_data`)**  
When a user clicks a button, the `request` object contains:  
- `request.data` (The custom payload you set)  
- `request.message` (The original message)  

Example:  
```javascript  
// Create a button  
keyboard: "Yes?callback=yes, No?callback=no"  

// Handle the click  
if (request.data === "yes") {  
  msg.reply("You agreed!");  
}  
```  

---

## **🔧 How to Use These Variables Like a Pro**  

### **1. Personalize Replies**  
```javascript  
msg.reply(`Welcome, ${user.first_name}!`);  
```  

### **2. Check User Permissions**  
```javascript  
if (chat.type === "group" && !user.is_admin) {  
  msg.reply("Admins only, sorry!");  
}  
```  

### **3. Handle Inline Queries**  
```javascript  
if (update_type === "inline_query") {  
  const query = request.query; // What the user typed after @yourbot  
  msg.answerInlineQuery([...]); // Return results  
}  
```  

### **4. Debug Like a Detective**  
```javascript  
console.log({ update_type, user, chat }); // Log everything!  
```  

---

## **💡 Pro Tips**  
✅ **Always check `update_type`** before assuming `message` exists.  
✅ **Use `params` for flexible commands** (e.g., `/ban USER_ID`).  
✅ **`user` and `chat` are your best friends** for context-aware replies.  

---

## **🎉 Ready to Build?**  
Now that you know the **variables**, go make your bot **smarter**, **funnier**, or at least **less confused**.  

Next up: **Functions and APIs**—because what’s a bot without *superpowers*? 🦸  
