# telegram-bot
Telegram bot for testing my code
# --- [ معالجة أزرار السرعة ] ---
@bot.on_callback_query()
async def callbacks(client, callback_query):
    user_id = callback_query.from_user.id
    data = callback_query.data
    
    if data == "set_speed":
        kb = InlineKeyboardMarkup([
            [InlineKeyboardButton("سريع جداً (0.1)", callback_data="sp_0.1")],
            [InlineKeyboardButton("متوسط (0.5)", callback_data="sp_0.5")],
            [InlineKeyboardButton("هادئ (1.0)", callback_data="sp_1.0")]
        ])
        await callback_query.edit_message_text("⚙️ اختر سرعة التسطير لحسابك:", reply_markup=kb)
    
    elif data.startswith("sp_"):
        speed_val = float(data.split("_")[1])
        if user_id in users_db:
            users_db[user_id]["speed"] = speed_val
            await callback_query.answer(f"🚀 تم ضبط السرعة على: {speed_val}")
        else:
            await callback_query.answer("❌ يجب تفعيل الاشتراك أولاً")

# تشغيل السورس
print("✅ السورس المتكامل يعمل الآن...")
bot.run()
