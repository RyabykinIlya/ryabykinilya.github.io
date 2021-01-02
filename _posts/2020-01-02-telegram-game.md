---
layout: default
title: "How to create telegram game"
date: 2020-01-02 21:35:00 -0000
categories: telegram bot
---

## Small example of telegram game creation 
That is very simple, but not documented very well.
This guide targeted on advanced programmers with good understanding what is going on :)
If you have some questions, please, contact with me through [https://t.me/greshish](https://t.me/greshish).

### First. Tools
I use python as primary programming language, therefore this example based on it.

### Second. Libraries
I used python-telegram-bot library, because it's first and may be only one library that described some methods about games for telegram.
Get it [here](https://github.com/python-telegram-bot/python-telegram-bot).

### Third. BotFather
To create game you should create simple bot first.  
Sequence of commands I used:
```python
/newbot
/setinline
/newgame
```

### Fourth. Code
Through couple of hours of reverse-engineering I found that it's very simple to get it done for mvp.
#### Create start command which will response game object to the user
Please, pay attention to the game_short_name="fsg",  
fsg - game unique identifier which I used during /newgame creation  
```python
from telegram.ext import Updater
from telegram.ext import CommandHandler
TOKEN = 'token here'

updater = Updater(token=TOKEN, use_context=True)
dispatcher = updater.dispatcher

def start(update, context):
    context.bot.send_game(chat_id=update.effective_chat.id, game_short_name="fsg")

start_handler = CommandHandler('start', start)

dispatcher.add_handler(start_handler)
updater.start_polling()
```
just test it and all witll be clear.

#### Add CallbackQueryHandler for getting game request
That piece of code get request when user presses "Play 'GameName''" button
then answers with url of the game, which telegram will open in browser for the user.
```python
from telegram import CallbackQuery
from telegram.ext import CallbackQueryHandler

def button(update, context):
    user = update.effective_user
    query: CallbackQuery = update.callback_query
    query.answer(url='http://your-game-page.com/?user-id=%s' % user['id'])

dispatcher.add_handler(CallbackQueryHandler(button))
```

That's it. All that you need is write in-browser game and use it in your bot.   
Thanks for reading. Good luck!