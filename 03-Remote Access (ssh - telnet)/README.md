# 🌐 سناریوی سوم: دسترسی از راه دور (Remote Access - Telnet & SSH)

در این سناریو، نحوه‌ی پیکربندی دسترسی از راه دور به سوئیچ از طریق Telnet و SSH را بررسی می‌کنیم.

---

## 🧪 هدف سناریو

- راه‌اندازی دسترسی به دستگاه سیسکو از راه دور با Telnet و SSH  
- اعمال محدودیت برای پورت‌های VTY  
- تعریف کاربران با سطوح دسترسی مختلف  
- رمزنگاری پسوردها  
- تست اتصال از کامپیوتر کلاینت  

---

## 🔁 بخش اول: دسترسی با Telnet

### 🧾 پیکربندی کلی

```bash
Switch> enable
Switch# configure terminal
Switch(config)# username admin privilege 15 secret P@ssWord
Switch(config)# username guest privilege 0 secret guest
Switch(config)# enable secret EnPass
Switch(config)# service password-encryption
```

---

### 🎛 پیکربندی خطوط VTY برای Telnet

```bash
Switch(config)# line vty 0 15
Switch(config-line)# transport input none
Switch(config-line)# exit

Switch(config)# line vty 0 3
Switch(config-line)# transport input telnet
Switch(config-line)# no login
Switch(config-line)# login local
Switch(config-line)# exec-timeout 3 30
Switch(config-line)# exit
```

---

### 🌐 تنظیم آدرس IP روی VLAN1

```bash
Switch(config)# interface vlan 1
Switch(config-if)# ip address 192.168.10.100 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if)# exit
```

---

### 💻 تنظیمات کلاینت برای Telnet

- IP کلاینت: `192.168.10.10/24`  
- در ترمینال کامپیوتر وارد کنید:

```bash
telnet 192.168.10.100
```

---

## 🔐 بخش دوم: دسترسی با SSH

### 🧾 پیکربندی کلی

```bash
Switch> enable
Switch# configure terminal
Switch(config)# username admin privilege 15 secret P@ssWord
Switch(config)# username guest privilege 0 secret guest
Switch(config)# enable secret EnPass
Switch(config)# service password-encryption
```

---

### 🎛 پیکربندی خطوط VTY برای SSH

```bash
# (none)برای امنیت بیشتر ابتدا تمام لاین هارا میبندیم

Switch(config)# line vty 0 15
Switch(config-line)# transport input none
Switch(config-line)# exit

Switch(config)# line vty 0 3
Switch(config-line)# transport input ssh
Switch(config-line)# no login
Switch(config-line)# login local
Switch(config-line)# exec-timeout 3 30
Switch(config-line)# exit
```

---

### 🏷 تنظیم دامنه و تولید کلید RSA

```bash
Switch(config)# hostname sw
sw(config)# ip domain-name smaple.local
sw(config)# crypto key generate rsa
```

> 🔑 توجه: هنگام تولید کلید، سایز 1024 یا بالاتر را وارد کنید.

---

### 🌐 تنظیم IP برای VLAN1

```bash
sw(config)# interface vlan 1
sw(config-if)# ip address 192.168.10.100 255.255.255.0
sw(config-if)# no shutdown
sw(config-if)# exit
```

---

### 💻 تنظیمات کلاینت برای SSH

- IP کلاینت: `192.168.10.10/24`  
- دستور اتصال:

```bash
ssh -l admin 192.168.10.100
# یا
ssh admin@192.168.10.100
```

---

## 📌 مقایسه کلی Telnet و SSH

| ویژگی         | Telnet           | SSH                 |
|---------------|------------------|---------------------|
| رمزنگاری     | ❌ ندارد         | ✅ دارد             |
| امنیت        | ⬇ پایین‌تر       | ⬆ بالاتر           |
| پورت پیش‌فرض | 23               | 22                  |
| توصیه‌شده     | خیر              | بله                 |

---

با پیکربندی هر دو روش می‌توانید تفاوت‌های عملی امنیتی و کاربری آن‌ها را تجربه کرده و تصمیم بگیرید کدام روش مناسب‌تر است.
