# Ingliz tili self-study Telegram bot

Bot lug‘at takrorlash jadvalini, kunlik vazifalarni va bajarilgan ishlar foizini kuzatadi. Node.js 20+ va Docker bilan ishlaydi; tashqi AI xizmati talab qilinmaydi.

## Hozirgi imkoniyatlar

- `word_list.json` va `words2.json` dagi 30 tadan unitni yuklaydi (`A` va `B`).
- Har bir qo‘shilgan unitdagi so‘zlarni 25 daqiqa, 10 soat, 24 soat, 4 kun, 14 kun va 30 kundan keyin takrorlatadi. Har bir so‘z uchun “Esimda” yoki “Qayta ko‘raman” tugmasi bor.
- Grammatika, listening, reading va Murphy bo‘yicha 30 ta gap topshirilganini qayd etadi.
- Har kuni Toshkent vaqti bilan 21:00 dan keyin kunlik hisobot yuboradi.
- Ma’lumotlar qayta ishga tushganda saqlanishi uchun `/data/state.json` faylidan foydalanadi.

**Cheklovlar:** AI tekshiruvi o‘chiq. Hozircha bot grammatik gaplarni tuzatmaydi; reading/listening javoblarini qabul qilib, topshirilgan deb sanaydi, lekin xatolarni javob kaliti bilan solishtirmaydi. Javob kalitlari va listening audio fayllari topilgach, ularni tekshirish qismini alohida ulash kerak. Listening papkasida hozircha PDF kitob bor; alohida audio fayllar ko‘rinmadi.

## Render’da Telegram botni sozlash

1. Telegram’da `@BotFather` orqali bot yarating va token oling. Tokenni chatga yubormang.
2. Loyiha fayllarini o‘zingizning GitHub repozitoriyingizga joylang. Telegram tokenini repozitoriyga qo‘shmang.
3. Render’da **New → Blueprint** ni tanlab, repozitoriyingizni ulang. `render.yaml` background worker va `/data` doimiy diskini sozlaydi.
4. Blueprint sozlash vaqtida `BOT_TOKEN` maydoniga BotFather tokenini Render’ning maxfiy environment variable sifatida kiriting.
5. Deploy tugagach Telegram’da botga `/start` yuboring.

Bot ishlashi doimiy bo‘lishi va ma’lumotlar saqlanishi uchun Render’da to‘lovli worker hamda persistent disk kerak bo‘ladi; narxini deploydan oldin Render panelida tekshiring. Toshkent vaqti uchun `TZ_OFFSET_MINUTES=300` sozlangan.

Render’ga joylashdan oldin Docker image’ni mahalliy tekshirish mumkin:

```powershell
docker compose build
```

Mahalliy ishga tushirish uchun `BOT_TOKEN` environment variable’ini berib, `docker compose up -d` buyrug‘ini bajaring. Mahalliy `data` papkasi bot holatini saqlaydi.

## Buyruqlar

- `/vocab A 1` — `word_list.json` faylidagi 1-unitni qo‘shish.
- `/vocab B 1` — `words2.json` faylidagi 1-unitni qo‘shish.
- `/review` — vaqti kelgan lug‘atlarni ko‘rish.
- `/tasks` — kunlik vazifalarni belgilash.
- `/grammar` — Murphy mavzusini yuborib, grammatikani bajarilgan deb belgilash.
- `/submit listening` yoki `/submit reading` — javoblarni yuborib, topshirilganini qayd etish.
- `/sentences` — 30 ta gapni, har birini alohida qatorda, yuborish.
- `/done grammar`, `/done listening`, `/done reading`, `/done sentences` — vazifani qo‘lda bajarilgan deb belgilash.
- `/progress` — bugungi natija.

## Mahalliy tekshiruv

```powershell
npm test
```
