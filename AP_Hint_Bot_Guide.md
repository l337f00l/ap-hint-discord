# AP Hint Bot — Setup Guide

This guide covers everything you need to know after the bot has been added to your Discord server — from setting up the hints channel to registering your slot name and using the bot's commands.

---

## Table of Contents

1. [Setting Up the Hints Channel](#1-setting-up-the-hints-channel)
2. [Creating a Webhook](#2-creating-a-webhook)
3. [Starting the Bot (Admins Only)](#3-starting-the-bot-admins-only)
4. [Registering Your Slot Name (Everyone)](#4-registering-your-slot-name-everyone)
5. [Command Reference](#5-command-reference)
6. [FAQ](#6-faq)

---

## 1. Setting Up the Hints Channel

The bot posts hint notifications into a dedicated channel. We recommend creating a channel called `#ap-hints` or similar.

### Recommended Channel Permissions

The bot needs the following permissions in the hints channel. To set these, go to the channel's **Edit Channel → Permissions** and make sure the bot's role has these enabled:

| Permission | Why It's Needed |
|---|---|
| View Channel | So the bot can see the channel |
| Send Messages | To post hint notifications |
| Create Public Threads | To create a thread for each hint |
| Create Private Threads | For private hint threads when both players are registered |
| Send Messages in Threads | To post the hint details inside the thread |
| Manage Threads | To organize and manage hint threads |
| Read Message History | Required for threads to work correctly |

### Who Should See the Channel

That's up to you and the host. Options:

- **Everyone in the server** — most open, players can search for their slot name
- **Role-gated** — only players in the multiworld can see it, reduces noise for everyone else

---

## 2. Creating a Webhook

A webhook is how the bot posts messages into your hints channel. You only need to do this once per channel.

**Step-by-step:**

1. Go to your hints channel in Discord
2. Click the **gear icon** to open Channel Settings
3. Click **Integrations** in the left sidebar
4. Click **Webhooks**
5. Click **New Webhook**
6. Give it a name (e.g. "AP Hint Bot") — this is just for your reference
7. Make sure the channel shown is your hints channel
8. Click **Copy Webhook URL**
9. Send this URL to the bot admin — they'll need it to start the bot

> ⚠️ **Keep the webhook URL private.** Anyone with it can post messages to your channel.

---

## 3. Starting the Bot (Admins Only)

Once the hints channel is set up and you have the webhook URL, the bot admin uses the `/setup` command to start monitoring the room.

The admin will need from you:
- The **room port number** (found in the archipelago.gg room URL)
- **Any one slot name** from the room (the bot finds the rest automatically)
- The **room password** if one is set
- The **webhook URL** from step 2

The admin types:
```
/setup name:bigmulti port:12345 slot:PlayerOne webhook:https://discord.com/api/webhooks/...
```

Once running, the bot will automatically discover all slots in the room and start listening for hints. You'll know it's working when the first hint gets posted as a thread in the hints channel.

---

## 4. Registering Your Slot Name (Everyone)

**What does registering do?**

When you register, you're telling the bot your Discord account and your slot name in the game are the same person. This unlocks two benefits:

- The bot will **ping you directly** when a hint involves you — either when someone hints an item that's in your world, or when you hint something and someone else has it
- If **both you and the other player are registered**, the hint thread will be **private** — only the two of you can see it, keeping the channel tidy

**If you don't register**, the bot still works — hints will just be posted as public threads with no pings. You can still search the channel for your slot name to find your hints.

### How to Register

Type this in any channel the bot can see:

```
/register room:bigmulti slots:YourSlotName
```

Replace `bigmulti` with the room name the admin is using, and `YourSlotName` with your exact slot name from the game — check the archipelago.gg room page if you're not sure what it is.

**Playing multiple slots?** List them all separated by spaces:

```
/register room:bigmulti slots:PlayerMMX PlayerOOT PlayerAlttP
```

The bot will confirm which slots were registered successfully. If it says a slot name wasn't found, double-check the spelling — it needs to match exactly.

### How to Unregister

If you need to remove your registration:

```
/unregister room:bigmulti
```

This removes all your slots. To remove just one:

```
/unregister room:bigmulti slots:PlayerMMX
```

---

## 5. Command Reference

### 🔒 Admin Commands
*Only available to designated admins.*

---

**`/setup`** — Start monitoring a multiworld room

| Option | Required | Description |
|---|---|---|
| `name` | ✅ | A short name for this room, e.g. `bigmulti` or `test` |
| `port` | ✅ | The room port from the archipelago.gg room URL |
| `slot` | ✅ | Any one valid slot name from the room |
| `webhook` | ✅ | The webhook URL from the hints channel |
| `password` | ❌ | The room password, if one is set |

Example:
```
/setup name:bigmulti port:49994 slot:PlayerOne webhook:https://discord.com/api/webhooks/... password:abc123
```

> Starting a new multiworld? Just run `/setup` again with the new details — the old room's hints and registrations won't carry over.

---

**`/status`** — Check the current state of all monitored rooms

Shows each room's server, number of connected slots, how many players have registered, hints posted so far, and how long it's been running.

---

**`/stop`** — Stop monitoring a room

```
/stop name:bigmulti
```

The bot stays online but stops listening to that room.

---

### 🌐 Public Commands
*Available to everyone in the server.*

---

**`/register`** — Link your Discord account to your AP slot name(s)

```
/register room:bigmulti slots:YourSlotName
```

Multiple slots:
```
/register room:bigmulti slots:SlotOne SlotTwo SlotThree
```

Only you can see the bot's response. Registration is saved automatically and persists for the whole multiworld.

---

**`/unregister`** — Remove your slot registration

```
/unregister room:bigmulti
```

Remove a specific slot:
```
/unregister room:bigmulti slots:YourSlotName
```

---

## 6. FAQ

**Do I have to register?**
No. The bot works without registration — hints are just posted as public threads with no pings. Registering improves your experience but isn't required.

**What's the difference between a public and private thread?**
A public thread is visible to everyone who can see the hints channel. A private thread is only visible to the players involved in that hint (and server admins). Private threads are created automatically when both the finder and the receiver have registered.

**I registered but I'm not getting pinged. What's wrong?**
Make sure the slot name you registered matches exactly what's shown on the archipelago.gg room page. It's case-insensitive but spelling must be exact. You can re-register at any time to correct it.

**Only one of us is registered — will we get a private thread?**
No. Private threads are only created when both players in a hint are registered. If only one player is registered, you'll get a public thread and the registered player will be pinged. They can then manually tag the unregistered player in the thread.

**Why didn't a hint get posted?**
A few reasons the bot intentionally skips hints:
- The item was already found
- It was an automatic hint from the game (not a manual `!hint` command)
- It's a trap item

**What happens between multiworlds?**
The admin runs `/setup` with the new room details. Hint history and registrations from the old room don't carry over — everyone starts fresh. Players will need to `/register` again for the new room.

**I have multiple slots across different games. Can I register them all?**
Yes — just list all your slot names in one `/register` command separated by spaces.

**The bot isn't showing hints. Is something wrong?**
Check with the admin that the bot is running (`/status`). If the room is showing as active, make sure players are using `!hint ItemName` in their AP client — the bot only posts threads for manual hints, not automatic game hints.
