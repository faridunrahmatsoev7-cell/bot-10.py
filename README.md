import telebot, time, os, threading
from flask import Flask

TOKEN = "8830467337:AAFLegVrjduz937H3knGFK2JhEla9F6GRkQ"
bot = telebot.TeleBot(TOKEN)
app = Flask(__name__)

@app.route('/')
def home():
    return "Bot is alive!"

@bot.message_handler(commands=['start'])
def start(m):
    bot.reply_to(m, "Привет! Я бот Chinese Academy TJ. Напиши /help")

@bot.message_handler(func=lambda m: True)
def echo(m):
    bot.reply_to(m, "Получил: " + m.text)

def run_bot():
    while True:
        try:
            bot.polling(none_stop=True, timeout=60, long_polling_timeout=60)
        except Exception as e:
            print(e); time.sleep(5)

threading.Thread(target=run_bot, daemon=True).start()

if __name__ == "__main__":
    port = int(os.environ.get("PORT", 10000))
    app.run(host="0.0.0.0", port=port)
