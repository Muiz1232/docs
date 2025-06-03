
# 📦 Commands in Detail – The Building Blocks of Your Bot's Brain  

Welcome to the core of **Tele Bot Lang (TBL)**—where every command you write becomes a neuron in your bot’s *adorably artificial* brain. Commands are what make your bot think, respond, and occasionally pretend it has a personality. Let’s break it down.  

---

## 🧱 Anatomy of a Command  

Each TBL command is like a tiny robot: it has a **name**, a **script**, an optional **answer**, and sometimes, a few **buttons** to make it interactive.  

### **Command Name** *(Required)*  
The trigger that makes your bot think, *"Ah, my time to shine!"*  
- **Slash commands:** `/start`, `/help`  
- **Friendly triggers:** `hi`, `menu`, `order_pizza`  
- **System triggers:** `@`, `!`, `@@`, `*` (more on these later)  

### **Answer Field** *(Optional)*  
Your bot’s *verbal response*. Supports **Markdown** for styling:  
- Use for quick replies.  
- Works alone or alongside code.  
- Example:  
  ```  
  *Welcome!* Here’s what I can do:  
  - 🛍️ Shop  
  - ℹ️ Info  
  - 🆘 Help  
  ```  

### **Code Block** *(Optional)*  
The **brain juice**—where logic lives. Write **JavaScript** like a pro:  
- `async/await` for APIs, databases, or existential crises.  
- `if/else`, loops, `try/catch` for handling life’s unpredictability.  
- Example:  
  ```javascript  
  const user = await getUser(ctx.user.id);  
  if (user.vip) {  
    reply(`🎉 Welcome back, ${user.name}!`);  
  } else {  
    reply(`Hi ${user.name}! Want to upgrade?`);  
  }  
  ```  

### **Keyboard Layout** *(Optional)*  
Turn your bot into a clickable app:  
- **Same row:** `"Yes, No"` → *Yes* and *No* side by side.  
- **New row:** `"Yes\nNo"` → *Yes* above *No*.  
- **Dynamic keyboards:** Generate buttons in code.  

### **Aliases** *(Optional)*  
Give your command multiple personalities (no therapy needed):  
- Example: `aliases: hi, hey, hello, greetings`  
- Covers typos (`hlep`), slang (`sup`), and multilingual hellos (`hola`).  

### **Need Reply Flag** *(Optional)*  
Set to `true` to make the bot **wait for the user’s next message**. Great for:  
- Multi-step forms.  
- Quizzes.  
- Deep, meaningful conversations (or pizza orders).  

---

## 🧠 **Special Command Types**  

TBL isn’t just about user commands—it’s got **system-level magic** too:  

### **`@` – The Initialization Wizard** *(Runs at Bot Startup)*  
Think of this as your bot’s **morning coffee routine**. It runs *once* when the bot starts, **synchronously**, and sets up everything else.  

#### **What You Can Do in `@`:**  
- **Define global variables** (shared across all commands).  
- **Create helper functions** (like formatting currency).  
- **Load configs** (admin IDs, API keys).  
- **Sync-only**—no `await` here!  

#### Example:  
```javascript  
// Global bot config  
let botConfig = {  
  adminIds: [111, 222],  
  currency: "💰"  
};  

// Shared function  
function formatMoney(amount) {  
  return `${botConfig.currency} ${amount.toFixed(2)}`;  
}  
```  
*Note:* Other commands can **access** `botConfig` and `formatMoney()` later!  

---

### **`!` – The "Oops" Handler** *(Error Handling)*  
When things go wrong (and they will), this is your bot’s **panic button**.  
- Automatically triggered on errors.  
- Logs issues, apologizes, or blames the user (kidding… mostly).  
- Example:  
  ```javascript  
  reply(`Oops! Something broke. We’re on it.`);  
  logError(`User ${ctx.user.id} caused: ${ctx.error}`);  
  ```  

### **`@@` – The Janitor** *(Post-Command Cleanup)*  
Runs **after every command**, no matter what. Use for:  
- Logging.  
- Analytics.  
- Closing sessions.  
- Example:  
  ```javascript  
  log(`Command ${ctx.command} finished for ${ctx.user.id}`);  
  ```  

### **`*` – The Wildcard Fallback**  
When no other command matches, `*` steps in like, *"I gotchu."*  
- Perfect for **NLP-based replies**.  
- Example:  
  ```javascript  
  reply(`I didn’t understand that. Try /help?`);  
  ```  

### **`/channel_update` – The Channel Ninja**  
Handles **channel posts automatically**. Use for:  
- Reposting.  
- Stats tracking.  
- Auto-moderation.  

### **`/inline_query` – The Cool Kid**  
Processes **inline searches** (when users type `@yourbot query`).  
- Returns real-time results.  
- Supports caching.  
- Falls back to `*` if undefined.  

---

## 🎓 **Best Practices for Command Design**  
1. **Clear Names:** `buy_ticket`, not `cmd_69`.  
2. **Modularize:** Split big tasks (`faq_login`, `faq_payment`).  
3. **Be Consistent:** Pick a style (emoji or no emoji) and stick to it.  
4. **Handle Errors:** Use `!` to avoid silent failures.  
5. **Test Async Code:** Bugs love `await`. Hunt them early.  
