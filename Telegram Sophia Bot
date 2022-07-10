from telegram import ReplyKeyboardMarkup, ReplyKeyboardRemove
from telegram.ext import (
    Application,
    CommandHandler,
    ConversationHandler,
    MessageHandler,
    filters,
)

GENDER, PHOTO, LOCATION, BIO = range(4)

async def start(update, context):  # Building Blocks for Senses
    await update.message.reply_text(
        "Hello! Dear\n"
        "can i talk with you.\n"
        "Are you a boy or a girl?",
        reply_markup=ReplyKeyboardMarkup([["Boy", "Girl", "Other"]])
    )  # Mouth (speck this)
    return GENDER

async def gender(update, context):
    await update.message.reply_text(
        "oh great! Please send me your photo,\n"
        "so I know what you look like.",
        reply_markup=ReplyKeyboardRemove(),
    )
    return PHOTO

async def photo(update, context):
    photo_file = await update.message.photo[-1].get_file()
    await photo_file.download("user_photo.jpg")
    await update.message.reply_text(
        "oh! you look very handsome!\n"
        "where are you from"
    )
    return LOCATION

async def skip_photo(update, context):
    await update.message.reply_text(
        "really!\n"
        "okay then\n"
        "i dont wanna talk anymore\n"
    )
    return ConversationHandler.END

async def location(update, context):
    await update.message.reply_text(
        "Maybe we will meet!\n"
        "now tell me something about you"
    )
    return BIO

async def skip_location(update, context):
    await update.message.reply_text(
        "okay! then Bye "
    )
    return ConversationHandler.END

async def bio(update, context):
    await update.message.reply_text("what! the ...")
    return ConversationHandler.END

async def cancel(update, context):
    await update.message.reply_text("Bye! I hope we can talk again some day.")
    return ConversationHandler.END

def main():
    application = Application.builder().token("5572846425:AAG9a0OBCgvoQhzIQoOoiGj7XK_0PxLLcQs").build()  # Brain
    
    conv_handler = ConversationHandler(
        entry_points=[CommandHandler("start", start)],
        states={
            GENDER: [MessageHandler(filters.Regex("^(Boy|Girl|Other)$"), gender)],
            PHOTO: [MessageHandler(filters.PHOTO, photo), CommandHandler("skip", skip_photo)],
            LOCATION: [
                    MessageHandler(filters.LOCATION, location),
                    CommandHandler("skip", skip_location),
            ],
            BIO: [MessageHandler(filters.TEXT & ~filters.COMMAND, bio)],
        },
        fallbacks=[CommandHandler("cancel", cancel)]
    )  # Advance Memory

    application.add_handler(conv_handler)
    application.run_polling()  # in awaken state

main()
