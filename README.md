import telebot

# 1️⃣ BotFather dan olgan token
TOKEN = "AAHTOEevkH4wK3500yFG96crovJLsI11AKE"
bot = telebot.TeleBot(TOKEN)

# 2️⃣ Foydalanuvchi balanslari uchun temporary dictionary
user_balances = {}

# 3️⃣ /start buyrug‘i
@bot.message_handler(commands=['start'])
def start(message):
    user_id = message.from_user.id
    if user_id not in user_balances:
        user_balances[user_id] = 0
    bot.reply_to(message, f"Salom! Sizning balansingiz: {user_balances[user_id]} TON 💎\n"
                          "Deposit qilish uchun /deposit, yechib olish uchun /cashout yozing.")

# 4️⃣ /deposit buyrug‘i (minimal)
@bot.message_handler(commands=['deposit'])
def deposit(message):
    user_id = message.from_user.id
    # Hozircha faqat virtual deposit
    user_balances[user_id] += 10  # har safar 10 TON qo‘shiladi
    bot.reply_to(message, f"✅ Sizning balansingizga 10 TON qo‘shildi! Yangi balans: {user_balances[user_id]} TON")

# 5️⃣ /cashout buyrug‘i
@bot.message_handler(commands=['cashout'])
def cashout(message):
    user_id = message.from_user.id
    if user_balances.get(user_id, 0) > 0:
        amount = user_balances[user_id]
        user_balances[user_id] = 0
        bot.reply_to(message, f"💰 {amount} TON yechib olindi. Balansingiz: 0 TON")
    else:
        bot.reply_to(message, "❌ Balansingiz bo‘sh.")

# 6️⃣ Default javob
@bot.message_handler(func=lambda message: True)
def echo(message):
    bot.reply_to(message, "❗ Iltimos buyruqni ishlating: /deposit yoki /cashout")

# 7️⃣ Botni ishga tushirish
bot.polling()
