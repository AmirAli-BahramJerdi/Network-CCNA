# 🔐 سناریوی چهارم: امنیت پورت‌ها (Port Security)

در این سناریو، قصد داریم با استفاده از قابلیت امنیت پورت (Port Security) در سوئیچ‌های سیسکو، دسترسی به پورت‌های فیزیکی را کنترل و محدود کنیم.

---

## 🧪 هدف سناریو

- جلوگیری از اتصال دستگاه‌های غیرمجاز به پورت‌ها
- محدودسازی تعداد مجاز دستگاه‌های متصل به هر پورت
- انتخاب رفتار مناسب در صورت تخلف
- ذخیره‌سازی آدرس‌های MAC مجاز

---

## ⚙️ مراحل پیکربندی امنیت پورت

### 1️⃣ ورود به مد پیکربندی

```bash
Switch> enable
Switch# configure terminal
```

---

### 2️⃣ پیکربندی پورت فیزیکی (مثلاً FastEthernet 0/1)

```bash
Switch(config)# interface fastEthernet 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
```

- پورت را در حالت **Access** قرار می‌دهیم تا فقط یک VLAN مشخص را بپذیرد.  
- فعال‌سازی **Port Security** برای کنترل دسترسی.

---

### 3️⃣ محدودسازی تعداد دستگاه‌های مجاز

```bash
Switch(config-if)# switchport port-security maximum 2
```

- حداکثر **۲ دستگاه** بتوانند از طریق این پورت متصل شوند.

---

### 4️⃣ تعیین رفتار در صورت تخلف

```bash
Switch(config-if)# switchport port-security violation protect
# یا
Switch(config-if)# switchport port-security violation restrict
```

| رفتار     | توضیح |
|-----------|--------|
| **protect** | بسته شدن فریم‌های اضافی بدون هشدار یا لاگ |
| **restrict** | بلاک فریم‌ها و ثبت لاگ در کنسول |

---

### 5️⃣ ثبت آدرس MAC

```bash
Switch(config-if)# switchport port-security mac-address sticky
```

- با استفاده از دستور بالا، آدرس‌های MAC دستگاه‌هایی که به پورت متصل می‌شوند **به‌صورت خودکار ذخیره** می‌شوند.  
- اگر بخواهید MAC مشخصی را دستی وارد کنید:

```bash
Switch(config-if)# switchport port-security mac-address 0011.2233.4455
```

---

### 🚪 خروج از Interface

```bash
Switch(config-if)# exit
```

---

## 📋 بررسی وضعیت Port Security

برای مشاهده وضعیت امنیتی پورت‌ها از دستور زیر استفاده می‌کنیم:

```bash
Switch# show port-security
```

یا برای پورت خاص:

```bash
Switch# show port-security interface fastEthernet 0/1
```

---

## ⚠️ نکات مهم

| نکته | توضیح |
|------|-------|
| حالت پیش‌فرض Violation | shutdown است (یعنی پورت غیرفعال می‌شود) |
| sticky | آدرس‌های MAC به صورت دائمی در جدول ذخیره می‌شوند |
| max | محدودیت تعداد دستگاه روی پورت |

---

با این پیکربندی، امنیت پورت‌های سوئیچ افزایش یافته و از اتصال دستگاه‌های غیرمجاز جلوگیری خواهد شد.
