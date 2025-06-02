
# 📦 Commands in Detail

In TBL (Tele Bot Lang), **commands** are the brain cells of your bot.  
They control how it thinks, responds, interacts — basically, how it *vibes*.  

Each command is made of some optional and some powerful components. Here's the full lowdown:

---

## 🧱 Command Components

Every command can include:

- **Name**: The identifier or trigger — like `/start`, `menu`, or `help`.
- **Answer** *(optional)*: A static text reply that your bot sends when the command runs. Markdown formatting supported.
- **Code** *(optional)*: Write dynamic TBL code that does stuff — fetch data, call APIs, control logic.
- **Keyboard** *(optional)*: Show custom Telegram keyboards for quick replies.  
- **Aliases** *(optional)*: Extra command triggers like `hii, hey, hello`. One command, many faces.
- **need_reply** *(optional)*: If `true`, the bot will wait for a user's reply after executing this command.

---

## 🎹 Keyboard Structure

Want your bot to show clickable buttons? Easy.

Use this format in your `keyboard` field

- Use `,` to separate buttons *on the same row*  
- Use `\n` (newline) to start a *new row* of buttons

> So `"Help, Hello\nOk"` will render two buttons on the first row and one on the second.

---

## 🧠 Command Name & Special Types

Normally, your command will look like this:

/start /help /menu

But TBL supports **special command names** with special powers:

---

### `@` — **Before Everything**

- Runs **before** any normal command.
- Great for defining shared functions, variables, or setup code.
- Runs **synchronously only**.
- Example:  
```js
let myData = 555
```
Now myData is available in any other command. Like global scope... but cooler.


---

! — Error Handler

If your bot crashes or something throws an error... boom, this command catches it.

Acts as a universal fallback when stuff breaks.

Saves you from ugly silence or bot confusion.



---

@@ — After Everything

Runs after every command, regardless of which one it was.

Perfect for cleanup tasks, logging, analytics, or just a cheeky "P.S. bye!" message.



---

* — Wildcard Master

The ultimate catch-all.

If no other command matches, TBL runs this.

Great for building dynamic menus, fallback messages, or fuzzy search behavior.



---

/channel_update — Channel Update Handler

Automatically runs when Telegram sends a channel update.

If this doesn’t exist, it'll fallback to the * command.



---

/Inline_query — Inline Query Handler

Handles inline queries (@YourBot query) if defined.

Otherwise, falls back to the * command if available.



---

🔐 Code Field

This is where the action happens.

Write TBL code directly in the TBL Code Editor.
Supports await, functions, logic, and all the spicy stuff — even async operations!

> You can call APIs, manage memory, conditionally respond, send multiple messages, create keyboards dynamically, and more.




---

🗣️ Aliases

Why stick to one trigger when you can have them all?

Set multiple alternative names for a command like this:

hii, hey, hello

Now your user can say anything casual, and your bot will still know what’s up.
(We call this the “bot therapist” feature.)


---

📝 Answer Field

Use this for simple, static replies. Markdown formatting is supported, so go crazy with bolds, italics, and links.

*Welcome!*  
Tap one of the buttons below to begin 👇

> You can mix answer and code — send a static reply, and then run code. It's your party.




---

🧬 Async or Sync?

Most TBL code is asynchronous-ready.

Yes, you can use await inside your command code — TBL handles it gracefully.

The only exception: the @ command (the before-all one) is always synchronous.
Why? Because it runs instantly before everything — think of it like the bootloader.

But it’s special — you can define variables or helper functions here, and use them anywhere else.


---

🎁 Quick Summary

Define your bot’s behavior using simple, powerful command blocks.

Customize them with answers, code, buttons, and aliases.

Use special commands like @, !, and * for total control.

Almost everything supports await, so async logic just works.



---

Make bots like a wizard, not a programmer. 🧙‍♂️
Now go write some commands and let your bot speak your mind.

---
