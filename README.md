# DHCP Configuration and VLAN Address Assignment

## English Documentation

### Overview
This scenario demonstrates how to configure DHCP on a Cisco router to dynamically assign IPv4 addresses to clients across six VLANs in an enterprise network.

The router provides DHCP services and acts as the default gateway for each VLAN using IEEE 802.1Q subinterfaces. Two switches provide Layer 2 connectivity through access ports and trunk links.

### Objectives
- Configure DHCP pools for six IPv4 subnets.
- Assign default gateways to DHCP clients.
- Configure router-on-a-stick interfaces for VLANs 10, 20, and 30.
- Configure routed VLAN subinterfaces for VLANs 40, 50, and 60.
- Configure switch access ports and trunk links.
- Prepare the network for DHCP client testing.

### Network Addressing Plan

| VLAN | Department / Purpose | Network | Default Gateway |
|---|---|---|---|
| 10 | IT | `192.168.10.0/24` | `192.168.10.1` |
| 20 | R&D | `192.168.20.0/24` | `192.168.20.1` |
| 30 | HR | `192.168.30.0/24` | `192.168.30.1` |
| 40 | Management | `192.168.40.0/24` | `192.168.40.1` |
| 50 | Security | `192.168.50.0/24` | `192.168.50.1` |
| 60 | Planning | `192.168.60.0/24` | `192.168.60.1` |

All subnets use a `/24` subnet mask (`255.255.255.0`).

### DHCP Configuration

Six DHCP pools are configured on the router:

| DHCP Pool | Network | Default Router |
|---|---|---|
| `Vlan-IT` | `192.168.10.0/24` | `192.168.10.1` |
| `Vlan-R&D` | `192.168.20.0/24` | `192.168.20.1` |
| `Vlan-HR` | `192.168.30.0/24` | `192.168.30.1` |
| `Vlan-MGT` | `192.168.40.0/24` | `192.168.40.1` |
| `Vlan-SEC` | `192.168.50.0/24` | `192.168.50.1` |
| `Vlan-PLN` | `192.168.60.0/24` | `192.168.60.1` |

**Current configuration notes:**
- DHCP pools are configured for all six subnets.
- Default gateways are defined in each pool.
- DNS servers are not explicitly configured in these DHCP pools.
- Excluded address ranges have not been configured in the provided configuration.
- Successful DHCP lease assignment must be verified during final testing.

### Router Interface Design

The router uses 802.1Q subinterfaces to provide gateway connectivity for the VLANs.

**GigabitEthernet0/0**
- `Gi0/0.10` — VLAN 10 — `192.168.10.1/24`
- `Gi0/0.20` — VLAN 20 — `192.168.20.1/24`
- `Gi0/0.30` — VLAN 30 — `192.168.30.1/24`

**GigabitEthernet0/1**
- `Gi0/1.40` — VLAN 40 — `192.168.40.1/24`
- `Gi0/1.50` — VLAN 50 — `192.168.50.1/24`
- `Gi0/1.60` — VLAN 60 — `192.168.60.1/24`

### Switch Configuration

**Switch1**
- `Gi0/0` is configured as a trunk allowing VLANs 10, 20, and 30.
- Access ports are assigned to VLANs 10, 20, and 30.

**Switch2**
- `Gi0/1` is configured as a trunk allowing VLANs 40, 50, and 60.
- Access ports are assigned to VLANs 40, 50, and 60.
- `Gi0/0` is configured as an access port in VLAN 40.

### Important Topology Consideration

The provided configuration shows two separate trunk groups:

- Switch1 carries VLANs 10, 20, and 30.
- Switch2 carries VLANs 40, 50, and 60.

The complete physical topology and the links between the router and switches must match this design. The provided configurations alone do not confirm that all VLANs are end-to-end reachable.

### Verification Commands

Run these commands during the final validation phase.

**On the router:**
```text
show ip interface brief
show ip dhcp pool
show ip dhcp binding
show running-config | section dhcp
```

**On each switch:**
```text
show vlan brief
show interfaces trunk
show interfaces status
```

**On each DHCP client:**
- Request an IP address using DHCP.
- Verify the assigned IPv4 address and subnet mask.
- Verify the default gateway.
- Test connectivity to the local VLAN gateway.

---

## مستندات فارسی

### معرفی سناریو
این سناریو نحوه پیکربندی DHCP روی روتر سیسکو را برای اختصاص خودکار آدرس‌های IPv4 به کلاینت‌های شش VLAN در یک شبکه سازمانی نشان می‌دهد.

روتر وظیفه ارائه سرویس DHCP و فراهم‌کردن دروازه پیش‌فرض هر VLAN را بر عهده دارد. دو سوئیچ نیز از طریق پورت‌های Access و لینک‌های Trunk ارتباط لایه دوم را فراهم می‌کنند.

### اهداف
- ایجاد شش DHCP Pool برای شش زیرشبکه IPv4.
- تعیین Default Gateway برای کلاینت‌ها.
- پیکربندی Subinterfaceهای مبتنی بر 802.1Q برای VLANهای 10، 20 و 30.
- پیکربندی Subinterfaceهای VLANهای 40، 50 و 60.
- تنظیم پورت‌های Access و لینک‌های Trunk سوئیچ‌ها.
- آماده‌سازی شبکه برای آزمایش تخصیص خودکار IP.

### طرح آدرس‌دهی شبکه

| VLAN | واحد یا کاربرد | شبکه | دروازه پیش‌فرض |
|---|---|---|---|
| 10 | فناوری اطلاعات (IT) | `192.168.10.0/24` | `192.168.10.1` |
| 20 | تحقیق و توسعه (R&D) | `192.168.20.0/24` | `192.168.20.1` |
| 30 | منابع انسانی (HR) | `192.168.30.0/24` | `192.168.30.1` |
| 40 | مدیریت (Management) | `192.168.40.0/24` | `192.168.40.1` |
| 50 | امنیت (Security) | `192.168.50.0/24` | `192.168.50.1` |
| 60 | برنامه‌ریزی (Planning) | `192.168.60.0/24` | `192.168.60.1` |

Subnet Mask تمام زیرشبکه‌ها برابر با `255.255.255.0` یا `/24` است.

### پیکربندی DHCP

روی روتر شش DHCP Pool تعریف شده است:

| نام DHCP Pool | شبکه | دروازه پیش‌فرض |
|---|---|---|
| `Vlan-IT` | `192.168.10.0/24` | `192.168.10.1` |
| `Vlan-R&D` | `192.168.20.0/24` | `192.168.20.1` |
| `Vlan-HR` | `192.168.30.0/24` | `192.168.30.1` |
| `Vlan-MGT` | `192.168.40.0/24` | `192.168.40.1` |
| `Vlan-SEC` | `192.168.50.0/24` | `192.168.50.1` |
| `Vlan-PLN` | `192.168.60.0/24` | `192.168.60.1` |

**نکات پیکربندی فعلی:**
- برای هر شش زیرشبکه DHCP Pool تعریف شده است.
- دروازه پیش‌فرض در هر Pool مشخص شده است.
- DNS Server به‌صورت صریح در این DHCP Poolها تنظیم نشده است.
- محدوده‌های آدرس مستثناشده از DHCP در کانفیگ ارائه‌شده تعریف نشده‌اند.
- موفقیت تخصیص IP باید در مرحله تست نهایی بررسی شود.

### طراحی اینترفیس‌های روتر

روتر برای فراهم‌کردن ارتباط Gateway با VLANها از Subinterfaceهای مبتنی بر 802.1Q استفاده می‌کند.

**اینترفیس GigabitEthernet0/0**
- `Gi0/0.10` — VLAN 10 — `192.168.10.1/24`
- `Gi0/0.20` — VLAN 20 — `192.168.20.1/24`
- `Gi0/0.30` — VLAN 30 — `192.168.30.1/24`

**اینترفیس GigabitEthernet0/1**
- `Gi0/1.40` — VLAN 40 — `192.168.40.1/24`
- `Gi0/1.50` — VLAN 50 — `192.168.50.1/24`
- `Gi0/1.60` — VLAN 60 — `192.168.60.1/24`

### پیکربندی سوئیچ‌ها

**Switch1**
- پورت `Gi0/0` به‌صورت Trunk با VLANهای مجاز 10، 20 و 30 تنظیم شده است.
- پورت‌های Access به VLANهای 10، 20 و 30 اختصاص داده شده‌اند.

**Switch2**
- پورت `Gi0/1` به‌صورت Trunk با VLANهای مجاز 40، 50 و 60 تنظیم شده است.
- پورت‌های Access به VLANهای 40، 50 و 60 اختصاص داده شده‌اند.
- پورت `Gi0/0` به‌صورت Access در VLAN 40 قرار دارد.

### نکته مهم درباره توپولوژی

طبق کانفیگ ارائه‌شده، دو گروه Trunk جداگانه وجود دارد:

- Switch1، VLANهای 10، 20 و 30 را حمل می‌کند.
- Switch2، VLANهای 40، 50 و 60 را حمل می‌کند.

توپولوژی فیزیکی و لینک‌های بین روتر و سوئیچ‌ها باید با این طراحی مطابقت داشته باشند. صرفاً با مشاهده کانفیگ‌ها نمی‌توان تأیید کرد که تمام VLANها در کل مسیر شبکه به یکدیگر دسترسی دارند.

### دستورات بررسی

این دستورات را در مرحله تست نهایی اجرا کن.

**روی روتر:**
```text
show ip interface brief
show ip dhcp pool
show ip dhcp binding
show running-config | section dhcp
```

**روی هر سوئیچ:**
```text
show vlan brief
show interfaces trunk
show interfaces status
```

**روی هر کلاینت DHCP:**
- درخواست دریافت IP از DHCP را ارسال کن.
- IP و Subnet Mask تخصیص‌یافته را بررسی کن.
- Default Gateway را بررسی کن.
- ارتباط با Gateway همان VLAN را آزمایش کن.
