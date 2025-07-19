# 🟨 سناریوی پنجم: پیکربندی VLAN (Virtual LAN)

در این سناریو با مفهوم VLAN، دلیل استفاده از آن، تفاوت آن با VLSM، و روش پیکربندی آن روی سوئیچ آشنا می‌شویم.

---

## ❓ چرا VLAN؟

در شبکه‌های بزرگ، همه‌ی سیستم‌ها به طور پیش‌فرض در یک broadcast domain قرار دارند. این یعنی:

- ترافیک broadcast به همه‌ی پورت‌ها ارسال می‌شود  
- امنیت پایین است  
- کنترل و مدیریت سخت‌تر می‌شود  

### ✅ راه‌حل: VLAN

VLAN به ما این امکان را می‌دهد که **در سطح لایه ۲ (لینک دیتا)** شبکه‌های مجازی مجزا بسازیم؛ به‌طوری‌که دستگاه‌ها در VLANهای مختلف حتی اگر به یک سوئیچ متصل باشند، **با هم ارتباط مستقیم نداشته باشند.**

---

## 🔍 تفاوت VLAN با VLSM

| ویژگی             | VLAN                                 | VLSM                          |
|------------------|--------------------------------------|-------------------------------|
| لایه کاری         | لایه ۲ (Data Link)                   | لایه ۳ (Network)              |
| هدف              | جداسازی منطقی دستگاه‌ها              | تقسیم دقیق آدرس‌های IP       |
| Broadcast Domain | چند Broadcast Domain ایجاد می‌کند    | روی Broadcast اثری ندارد     |
| استفاده          | روی سوئیچ                            | روی روتر یا در طراحی آدرس‌دهی |

---

## ⚙️ مراحل پیکربندی VLAN

### 🧾 ایجاد VLANها

```bash
Switch> enable
Switch# configure terminal

Switch(config)# vlan 10
Switch(config-vlan)# name sale
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name management
Switch(config-vlan)# exit
```

---

### 🖧 تخصیص پورت‌ها به VLAN

#### پورت‌های 1 تا 3 → VLAN 10 (فروش)

```bash
Switch(config)# interface range fastEthernet 0/1 - 3
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit
```

#### پورت‌های 4 تا 6 → VLAN 20 (مدیریت)

```bash
Switch(config)# interface range fastEthernet 0/4 - 6
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# exit
```

---

## 🧪 بررسی VLANها

برای مشاهده وضعیت VLANها و پورت‌های مربوطه:

```bash
Switch# show vlan brief
```

---

## 💡 نکات تکمیلی

- VLANها لایه‌ی دوم را مدیریت می‌کنند، اما برای ارتباط بین آن‌ها به روتر یا سوئیچ لایه ۳ نیاز دارید (Inter-VLAN Routing).
- با VLAN می‌توان شبکه را از نظر عملکرد، امنیت و مدیریت بهبود داد.
- بدون VLAN، تمام دستگاه‌های شبکه در یک broadcast domain باقی می‌مانند.

---

با استفاده از VLAN، می‌توان شبکه‌ای منطقی، ایمن و منعطف ساخت که براساس نیازهای سازمانی تقسیم‌بندی شده باشد.
