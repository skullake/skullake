import telebot
import random

bot = telebot.TeleBot('')
num = 0;
rand = 0

@bot.message_handler(content_types=['text'])
def Start(message):
    global  rand
    rand = random.randint(0, 100)
    bot.send_message(message.from_user.id, "Попробуй угадать число")
    bot.send_message(message.from_user.id, "Введи число от 1 до 100")
    bot.register_next_step_handler(message, Chislo)
def Chislo(message):
    global num
    global prevNum
    try:
        val = int(message.text)
    except ValueError:
        bot.send_message(message.from_user.id, "ты че? ")
        bot.register_next_step_handler(message, Chislo)
        return
    num = int(message.text)
    if num == rand:
        bot.send_message(message.from_user.id, "Угадал")
    elif num > rand:
        bot.send_message(message.from_user.id, "Высоко берешь")
        bot.register_next_step_handler(message, Chislo)
    else:
        bot.send_message(message.from_user.id, "Мало")
        bot.register_next_step_handler(message, Chislo)
bot.polling(none_stop=True, interval=0)
