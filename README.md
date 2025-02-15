import logging
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes

# 🔧 Logging setup
logging.basicConfig(format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
                    level=logging.INFO)
logger = logging.getLogger(__name__)

# 🤖 Bot API Token
TOKEN = '7934042014:AAHKYssagK435kGGdcdfuerEaHnr-sicaSI'

# 📢 Your Telegram Channel link
CHANNEL_LINK = "https://t.me/BreakingNowUpdates"

# ✅ Function to check if the user is in the Telegram channel
async def is_user_in_channel(update: Update, context: ContextTypes.DEFAULT_TYPE) -> bool:
    user_id = update.message.from_user.id
    try:
        member = await context.bot.get_chat_member('@BreakingNowUpdates', user_id)
        if member.status in ['member', 'administrator', 'creator']:
            return True
    except Exception as e:
        logger.error(f"🚨 Error checking channel membership: {e}")
    return False

# 📰 Breaking News Command
async def news(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    if not await is_user_in_channel(update, context):
        await update.message.reply_text(
            f"⚠️ कृपया पहले हमारे चैनल से जुड़ें: {CHANNEL_LINK} 📢")
        return

    news_text = """
📰 नमस्कार! आज, 15 फरवरी 2025, की प्रमुख ब्रेकिंग समाचार इस प्रकार हैं:

1️⃣ सीबीएसई बोर्ड की परीक्षाएं शुरू 🎓  
केंद्रीय माध्यमिक शिक्षा बोर्ड की 10वीं और 12वीं की वार्षिक परीक्षाएं आज से शुरू हो गई हैं।  

2️⃣ संभल हिंसा के आरोपियों की सुनवाई ⚖️  
उत्तर प्रदेश के संभल जिले में हुई हिंसा के 15 आरोपियों की जमानत याचिका पर आज सुनवाई होगी।  

3️⃣ अमृतसर में विशेष विमान की लैंडिंग ✈️  
अमेरिका से निर्वासित 119 भारतीयों को लेकर एक विशेष विमान अमृतसर में लैंड करेगा।  

4️⃣ पुलवामा आतंकी हमले की 6वीं बरसी 🕯️  
आज पुलवामा में हुए आतंकी हमले की 6वीं बरसी है, जिसमें 40 से अधिक भारतीय जवान शहीद हुए थे।  

5️⃣ वैलेंटाइन डे मनाया जा रहा है ❤️  
आज वैलेंटाइन डे है, जिसे प्रेमी-प्रेमिकाएं अपने प्यार का इज़हार करने के लिए मना रहे हैं।  

6️⃣ लोकसभा में नया इनकम टैक्स बिल पेश 🏛️  
वित्त मंत्री निर्मला सीतारमण आज लोकसभा में नया इनकम टैक्स बिल पेश करेंगी।  

7️⃣ दिल्ली में नई सरकार के गठन की प्रक्रिया तेज 🏆  
दिल्ली विधानसभा चुनावों में भाजपा की प्रचंड जीत के बाद, नई सरकार के गठन की प्रक्रिया तेज हो गई है।  

8️⃣ किसान नेताओं और सरकार के बीच बैठक 🌾  
किसान नेताओं और केंद्र सरकार के बीच आज MSP पर कानूनी गारंटी देने पर चर्चा होगी।  

9️⃣ पंजाब में VHP नेता हत्याकांड में NIA का एक्शन 🚔  
पंजाब में VHP नेता की हत्या के मामले में NIA ने दो आतंकियों के खिलाफ चार्जशीट दायर की है।  

🔟 अंतरिक्ष यात्री सुनीता विलियम्स की वापसी 🚀  
अंतरिक्ष में आठ महीने बिताने के बाद, नासा की अंतरिक्ष यात्री सुनीता विलियम्स जल्द ही धरती पर लौटेंगी।  

📌 ताज़ा अपडेट के लिए /news टाइप करें 🔥
"""
    await update.message.reply_text(news_text)

# 🚀 Start Command
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    if not await is_user_in_channel(update, context):
        await update.message.reply_text(
            f"🚀 स्वागत है! 🙏 इस बॉट का उपयोग करने के लिए पहले हमारे चैनल से जुड़ें: {CHANNEL_LINK}")
        return

    user_name = update.message.from_user.first_name
    welcome_message = f"""
🎉 नमस्ते {user_name}! 🤗  
आपका BREAKING NEWS BOT में स्वागत है! 🚀  

🔥 यहाँ आपको मिलेंगी तेज और ताज़ा ख़बरें,  
🗳️ रोचक पोल्स,  
📢 और बहुत कुछ!  

📍 अभी शुरू करें  
👉 /news - आज की ताज़ा खबरें 📰  
👉 /poll - आज का बड़ा सवाल ❓  

⚡ आपकी अपडेटेड दुनिया सिर्फ एक क्लिक दूर है! 🌎  
"""
    await update.message.reply_text(welcome_message)

# 🗳️ Poll Command - आज का बड़ा सवाल
async def poll(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    if not await is_user_in_channel(update, context):
        await update.message.reply_text(
            f"⚠️ पहले हमारे चैनल से जुड़ें: {CHANNEL_LINK} 📢")
        return

    question = "🤔 क्या सरकार को न्यूनतम समर्थन मूल्य (MSP) पर कानूनी गारंटी देनी चाहिए?"  
    options = ["✅ हां, यह ज़रूरी है", "❌ नहीं, इसकी जरूरत नहीं", "🤷 मुझे जानकारी नहीं", "🏛️ सरकार को खुद तय करना चाहिए"]

    await context.bot.send_poll(
        chat_id=update.message.chat_id,
        question=question,
        options=options,
        is_anonymous=False
    )

# 🔥 Main function to set up the bot
def main() -> None:
    app = ApplicationBuilder().token(TOKEN).build()

    # Register command handlers
    app.add_handler(CommandHandler("start", start))
    app.add_handler(CommandHandler("poll", poll))  # आज का बड़ा सवाल Poll
    app.add_handler(CommandHandler("news", news))

    # Run the bot
    app.run_polling()

if __name__ == '__main__':
    main()
