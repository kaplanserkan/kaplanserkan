from telegram import Bot, Update
from telegram.ext import Updater, MessageHandler, Filters, CallbackContext
import logging

TOKEN = '6650730607:AAHaR9Xt5fUPpQicVfYGrnFm5MSnJds_fBQ'
banned_phrases = ["ios 12", "ios 13", "ios 14", "ios14", "Ios14", "ios13", "Ios13", "ios12", "Ios12", "ios 14 beta", "ios14beta", "Ios14beta", "ios 13 beta", "ios13beta", "Ios13beta", "ios 12 beta", "ios12beta", "Ios12beta", "iphone 6", "Iphone6", "Iphone 6", "Iphone5s", "Iphone 5s", "iPhone 5s"]

TARGET_GROUP_LINK = "https://t.me/oldbetaprofiles"  # Yönlendirilecek grubun linki

def check_banned_phrases(update: Update, context: CallbackContext) -> None:
    message_text = update.message.text.lower()
    for phrase in banned_phrases:
        if phrase in message_text:
            update.message.delete()
            
            # Kullanıcıyı özel mesaj ile başka bir gruba yönlendirme
            context.bot.send_message(
                chat_id=update.message.from_user.id,
                text=f"For information on older versions, you can join our other group: {TARGET_GROUP_LINK}"
            )
            break

def main() -> None:
    updater = Updater(token=TOKEN, use_context=True)
    dp = updater.dispatcher

    dp.add_handler(MessageHandler(Filters.text & ~Filters.command, check_banned_phrases))
    
    updater.start_polling()
    updater.idle()

if __name__ == '__main__':
    main()

