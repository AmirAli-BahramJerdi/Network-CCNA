# 🔐 سناریوی اول: تفاوت بین Login و Login Local در کابل کنسول

در این سناریو قصد داریم تفاوت بین دو روش احراز هویت برای دسترسی به روتر یا سوئیچ از طریق کابل کنسول را بررسی کنیم: `login` و `login local`.

---

## 🧪 هدف سناریو

آشنایی با:

- تنظیم رمز عبور برای دسترسی از طریق پورت کنسول
- استفاده از دستور `login` (تکیه بر رمز مستقیم)
- استفاده از `login local` (تکیه بر نام کاربری و رمز)
- افزودن بنر خوش‌آمدگویی هنگام لاگین

---

## 🧩 بخش اول: استفاده از login

در این روش، فقط یک رمز عبور برای ورود از طریق کنسول تعیین می‌شود و نیازی به نام کاربری نیست.

```bash
Switch> enable
Switch# configure terminal
Switch(config)# line console 0
Switch(config-line)# password P@assWord!
Switch(config-line)# login
Switch(config-line)# exit
Switch(config)# exit
```

✅ در این حالت، کاربر هنگام اتصال از طریق کنسول فقط رمز عبور `P@assWord!` را وارد می‌کند.

---

## 🧩 بخش دوم: استفاده از login local

در این روش باید ابتدا یک نام کاربری با رمز عبور تعریف شود. سپس پورت کنسول به حالت احراز هویت محلی (local) تنظیم می‌شود.

```bash
Switch> enable
Switch# configure terminal
Switch(config)# username User1 privilege 1 secret Password
Switch(config)# line console 0
Switch(config-line)# no login
Switch(config-line)# login local
Switch(config-line)# exit
Switch(config)# exit
```

✅ در این حالت، کاربر باید نام کاربری `User1` و رمز `Password` را وارد کند.

📌 نکته: دستور `no login` برای حذف هرگونه حالت احراز هویت قبلی استفاده می‌شود.

---

## 🎨 اضافه کردن بنر خوش‌آمدگویی

برای نمایش پیام سفارشی هنگام ورود کاربر:

```bash
Switch> enable
Switch# configure terminal
Switch(config)# banner motd %Enter your Username and Password%
Switch(config)# line console 0
Switch(config-line)# motd-banner
Switch(config-line)# exit
Switch(config)# exit
```

✳️ نتیجه: قبل از ورود به سیستم، پیام زیر نمایش داده می‌شود:
```
Enter your Username and Password
```

---

## 📝 جمع‌بندی تفاوت‌ها

| ویژگی                     | login               | login local                   |
|--------------------------|---------------------|-------------------------------|
| نوع احراز هویت           | فقط رمز عبور        | نام کاربری + رمز عبور         |
| نیاز به تعریف یوزرنیم   | ❌                  | ✅                            |
| امنیت بیشتر              | ⬇️ پایین‌تر        | ⬆️ بالاتر                     |

---

با اجرای این تنظیمات، می‌توانید امنیت دسترسی به دستگاه‌های سیسکو از طریق پورت کنسول را کنترل کرده و شخصی‌سازی بهتری داشته باشید.
