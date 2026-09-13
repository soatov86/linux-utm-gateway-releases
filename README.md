# Linux UTM Gateway

**An integrated UTM firewall appliance for Debian — firewall, IPS, content
filtering, antivirus, VPN and HA in one panel.**
Open-source-style alternative to **pfSense**, **OPNsense**, **Kerio Control**,
**Untangle/Arista NG Firewall**, **Zentyal**, **Endian** and **ClearOS**.

|  |  |
|---|---|
| 📀 **ISO Download** | **https://t.me/Linux_UTM** |
| ✉️ **Contact** | **soatov86@gmail.com** |
| 🖥 Platform | Debian 12 (bookworm), amd64 — offline installable ISO |
| 🌐 Panel languages | Русский · English · Oʻzbekcha |

🇬🇧 [English](#english) · 🇷🇺 [Русский](#русский) · 🇺🇿 [Oʻzbekcha](#ozbekcha)

> ## ⚠️ Status: early — please read before you deploy
>
> **EN.** Linux UTM Gateway is young. Development started in **August 2026**.
> It runs today on the author's own equipment — a two-node HA pair and a third
> gateway serving real clients: a live IPsec tunnel, a measured DHCP failover,
> real Windows workstations — and **274 automated tests** run before every
> release.
>
> What it has **not** had is production use by anybody else. There is no
> third-party deployment, no installation measured in months rather than weeks,
> and no independent security audit. Defects are still being found regularly by
> *running* the product rather than by reading it — several in the past week
> alone.
>
> So: try it in a lab or on a segment you can afford to lose, keep
> configuration backups (the panel makes them), and do not yet put it in front
> of a network you cannot take down. If you do run it, tell me what broke —
> **soatov86@gmail.com**. That is worth more to this project than a star.
>
> **RU.** Продукт молодой. Разработка начата в **августе 2026**. Сегодня он
> работает на оборудовании автора — пара узлов в режиме отказоустойчивости и
> третий шлюз обслуживают реальных клиентов: живой IPsec-туннель, измеренное
> переключение DHCP, реальные рабочие станции Windows; перед каждым релизом
> прогоняются **274 автотеста**.
>
> Чего у него **нет** — эксплуатации у кого-то ещё: ни одного стороннего
> внедрения, ни одной установки, живущей месяцами, ни независимого аудита
> безопасности. Дефекты по-прежнему находятся регулярно — тем, что продукт
> *запускают*, а не читают; только за последнюю неделю их было несколько.
>
> Поэтому: пробуйте в лаборатории или на сегменте, который не жалко, делайте
> резервные копии конфигурации (панель умеет), и пока не ставьте его перед
> сетью, которую нельзя положить. Если всё же поставите — напишите, что
> сломалось: **soatov86@gmail.com**. Это ценнее звезды на GitHub.
>
> **UZ.** Mahsulot yosh. Ishlanma **2026-yil avgustida** boshlangan. Bugun u
> muallifning o'z jihozida ishlaydi — ikki tugunli HA juftligi va uchinchi
> shlyuz haqiqiy mijozlarga xizmat qiladi: tirik IPsec tunnel, o'lchangan DHCP
> failover, haqiqiy Windows ish stantsiyalari; har bir relizdan oldin **274 ta
> avtotest** yuritiladi.
>
> Unda **yo'q** narsa — boshqa birov tomonidan ekspluatatsiya qilinishi: hech
> qanday tashqi joriy etish, haftalar emas oylar bilan o'lchanadigan
> o'rnatish, va mustaqil xavfsizlik auditi yo'q. Nuqsonlar hamon muntazam
> topilmoqda — mahsulotni o'qish bilan emas, **ishlatish** bilan; faqat
> o'tgan haftaning o'zida bir nechtasi.
>
> Shuning uchun: laboratoriyada yoki yo'qotishga achinmaydigan segmentda
> sinab ko'ring, konfiguratsiya zaxirasini saqlang (panel qiladi), va hozircha
> uni o'chirib bo'lmaydigan tarmoq oldiga qo'ymang. Agar qo'ysangiz — nima
> buzilganini yozing: **soatov86@gmail.com**. Bu loyiha uchun yulduzchadan
> qimmatroq.

This repository is the **update channel**: an installed appliance reads
`latest.json` here and downloads the signed package when it is newer than what
it runs. Only released packages live here; the source code is elsewhere.

## Hozirgi reliz — Текущий релиз — Current release

| | |
|---|---|
| Versiya | **1.1.57** |
| Fayl | `utm-update-1.1.57.utmupd` |
| Chiqarilgan | 2026-09-13 |
| SHA256 | `c63739489e7fe42cb675e246f1a3db6aff672062b89357bf31078596491bfb39` |

> This table is written by the release script. It is deliberately the **only**
> copy in this file — three translated copies would drift apart the way two
> answers to one question always do.

---

<a name="english"></a>
## English

**Linux UTM Gateway** is a complete UTM (Unified Threat Management) gateway
built on Debian: nftables, Suricata, Squid, ClamAV, dnsmasq/Kea, nginx,
strongSwan, OpenVPN and WireGuard, all driven from one web panel and one
**Apply** button. It installs from an ISO with **no internet access at all** —
every package is already in the image.

📀 **Get the ISO:** https://t.me/Linux_UTM  ·  ✉️ **Contact:** soatov86@gmail.com

### What it does

| Area | Capabilities |
|---|---|
| **Firewall & NAT** | nftables stateful rules, ordering, schedules, IP/URL groups, port forward, 1:1 NAT, per-rule SNAT, GeoIP blocking, MAC filter, anti-spoofing, full IPv4 **and IPv6** rule families |
| **Routing & WAN** | Static/DHCP/PPPoE WAN, VLAN 802.1Q, LACP bonding, Multi-WAN failover and load balancing, policy-based routing, **OSPFv2** and **BGP** (FRR) |
| **High Availability** | VRRP virtual addresses (keepalived), one-way **configuration cluster**, session-state sync (**conntrackd**) so open connections survive a failover, and **DHCP lease failover** via Kea's HA hook — measured on a live pair: the client kept its address |
| **IPS / IDS** | Suricata integrated, **inline blocking through NFQUEUE** (a matching rule really drops the packet), Emerging Threats rules, scheduled signature updates, ignore lists |
| **Content filtering** | UT1 category database, DNS blacklists, SafeSearch enforcement, application and P2P/torrent filtering, per-user and per-group policies |
| **HTTPS inspection** | Three modes: DNS-only, **filtering by TLS server name (SNI) — the default, nothing is decrypted and nothing is installed on clients**, or full decryption with your own CA (URL paths, keywords, antivirus inside HTTPS) |
| **Antivirus** | ClamAV + c-icap for HTTP/HTTPS, **SMTP/POP3 mail proxy**, FTP over the explicit proxy, signature updates, end-user notification |
| **Proxy & web** | Squid explicit and transparent, WPAD/PAC, AD SSO, caching, parent proxy; **nginx reverse proxy** with SSL offload, load balancing and **active health checks** |
| **VPN** | OpenVPN server with per-user profiles and ACLs, **WireGuard** server, peers and site-to-site tunnels, **IPsec** site-to-site (IKEv1/IKEv2) and IKEv2 road-warrior with EAP-MSCHAPv2 |
| **DHCP & DNS** | dnsmasq **or Kea** (selectable engine), multiple scopes, reservations, custom options, lease monitoring and history, DHCPv6, Router Advertisement, **DHCPv6-PD**; DNS cache/forwarder, **DNSSEC**, **DNS-over-TLS** upstream |
| **Users & auth** | Local users and groups, **Active Directory**, LDAP, RADIUS, Kerberos/NTLM SSO, captive portal, admin RBAC, read-only admins, **per-user TOTP 2FA** |
| **QoS** | Per-IP, per-user, per-group and per-service limits, guaranteed bandwidth, priority queues, schedules, **fq_codel and CAKE** for bufferbloat, traffic quotas with block-or-throttle |
| **Monitoring** | Live and historical graphs, active connections and hosts, reports by user / MAC / site / protocol / day, CSV export, scheduled e-mail reports, SNMP v2c and v3, syslog, alerts |
| **Operations** | Local CA, Let's Encrypt, configuration backup with retention, **signed updates with automatic rollback** if the panel stops answering, offline ISO install |

### Why people move to it

- Everything in **one panel** — no package hunting, no separate IDS, proxy,
  antivirus and reporting products to glue together.
- **Real HTTPS content filtering by default**, without installing a certificate
  on every client.
- **User-centric accounting**: traffic follows the person through AD, MAC and
  DHCP changes, not just the IP address.
- **Offline installation and offline updates** — for networks that cannot reach
  the internet.
- Panel in **Russian, English and Uzbek**.

A full, evidence-based comparison against pfSense ships **on the appliance**
(`docs/linux-utm-gateway-vs-pfsense.md`) — the source repository is not public,
so the gaps it names are reproduced below in
**[What it does not do](#limits)**. Read that section before the table above.

---

<a name="русский"></a>
## Русский

**Linux UTM Gateway** — интегрированный UTM-шлюз на Debian: nftables,
Suricata, Squid, ClamAV, dnsmasq/Kea, nginx, strongSwan, OpenVPN и WireGuard —
всё управляется из одной веб-панели одной кнопкой **«Применить»**.
Устанавливается с ISO **полностью без интернета**: все пакеты уже в образе.

📀 **Скачать ISO:** https://t.me/Linux_UTM  ·  ✉️ **Контакт:** soatov86@gmail.com

### Возможности

| Направление | Что есть |
|---|---|
| **Межсетевой экран и NAT** | Правила nftables с учётом состояния, порядок правил, расписания, группы IP/URL, проброс портов, NAT 1:1, SNAT в правиле, блокировка по GeoIP, MAC-фильтр, антиспуфинг, полноценные семейства правил IPv4 **и IPv6** |
| **Маршрутизация и WAN** | WAN: статика/DHCP/PPPoE, VLAN 802.1Q, агрегация каналов, Multi-WAN с резервированием и балансировкой, policy-based routing, **OSPFv2** и **BGP** (FRR) |
| **Отказоустойчивость** | Виртуальные адреса VRRP (keepalived), односторонний **кластер конфигурации**, синхронизация состояния сессий (**conntrackd**) — открытые соединения переживают переключение, и **синхронизация аренд DHCP** через HA-хук Kea: на живой паре клиент сохранил свой адрес |
| **IPS / IDS** | Suricata интегрирована, **инлайн-блокировка через NFQUEUE** (сработавшее правило действительно отбрасывает пакет), правила Emerging Threats, обновление сигнатур по расписанию, списки исключений |
| **Контент-фильтр** | База категорий UT1, DNS-чёрные списки, принудительный SafeSearch, фильтрация приложений и P2P/торрентов, политики на пользователя и группу |
| **Инспекция HTTPS** | Три режима: только DNS, **фильтрация по имени сервера в TLS (SNI) — по умолчанию, трафик не расшифровывается и клиентам ничего не устанавливается**, либо полная расшифровка со своим CA (пути URL, ключевые слова, антивирус внутри HTTPS) |
| **Антивирус** | ClamAV + c-icap для HTTP/HTTPS, **почтовый прокси SMTP/POP3**, FTP через явный прокси, обновление баз, уведомление пользователя |
| **Прокси и веб** | Squid в явном и прозрачном режиме, WPAD/PAC, SSO с AD, кэш, вышестоящий прокси; **обратный прокси на nginx** с SSL offload, балансировкой и **активными проверками доступности** |
| **VPN** | Сервер OpenVPN с профилями и ACL по пользователям, **WireGuard** (сервер, пиры, site-to-site), **IPsec** site-to-site (IKEv1/IKEv2) и IKEv2 для мобильных клиентов с EAP-MSCHAPv2 |
| **DHCP и DNS** | dnsmasq **или Kea** (выбор движка), несколько областей, резервирования, произвольные опции, мониторинг и история аренд, DHCPv6, Router Advertisement, **DHCPv6-PD**; DNS-кэш и форвардер, **DNSSEC**, **DNS-over-TLS** |
| **Пользователи** | Локальные пользователи и группы, **Active Directory**, LDAP, RADIUS, Kerberos/NTLM SSO, captive portal, RBAC администраторов, админ «только чтение», **TOTP-2FA на пользователя** |
| **QoS** | Лимиты на IP, пользователя, группу и сервис, гарантированная полоса, приоритетные очереди, расписания, **fq_codel и CAKE** против bufferbloat, квоты трафика с блокировкой или снижением скорости |
| **Мониторинг** | Живые и исторические графики, активные соединения и узлы, отчёты по пользователю / MAC / сайту / протоколу / дням, экспорт CSV, отчёты на почту по расписанию, SNMP v2c и v3, syslog, оповещения |
| **Эксплуатация** | Локальный CA, Let's Encrypt, резервные копии с ретенцией, **подписанные обновления с автоматическим откатом**, если панель перестала отвечать, установка с ISO без интернета |

### Почему переходят

- Всё в **одной панели** — не нужно собирать связку из отдельных IDS, прокси,
  антивируса и системы отчётов.
- **Фильтрация HTTPS работает по умолчанию** — без установки сертификата на
  каждый компьютер.
- **Учёт по пользователю**, а не по IP: трафик следует за человеком через AD,
  MAC и смену адреса DHCP.
- **Офлайн-установка и офлайн-обновления** — для сетей без выхода в интернет.
- Панель на **русском, английском и узбекском**.

Полное сравнение с pfSense поставляется **на самом устройстве**
(`docs/linux-utm-gateway-vs-pfsense.md`); репозиторий с исходным кодом закрыт,
поэтому названные там пробелы вынесены ниже — **[Чего продукт не
умеет](#limits)**. Прочитайте этот раздел прежде, чем таблицу выше.

---

<a name="ozbekcha"></a>
## Oʻzbekcha

**Linux UTM Gateway** — Debian asosidagi yaxlit UTM shlyuz: nftables,
Suricata, Squid, ClamAV, dnsmasq/Kea, nginx, strongSwan, OpenVPN va
WireGuard — hammasi bitta veb-panel va bitta **«Применить»** tugmasi bilan
boshqariladi. ISO'dan **internetsiz** o'rnatiladi: barcha paketlar obraz
ichida.

📀 **ISO yuklab olish:** https://t.me/Linux_UTM  ·  ✉️ **Aloqa:** soatov86@gmail.com

### Imkoniyatlar

| Yo'nalish | Nimalar bor |
|---|---|
| **Firewall va NAT** | Holatni hisobga oluvchi nftables qoidalari, tartib, jadval bo'yicha ishlash, IP/URL guruhlari, port forward, 1:1 NAT, qoidada SNAT, GeoIP bo'yicha bloklash, MAC filtr, anti-spoofing, to'liq IPv4 **va IPv6** qoida oilalari |
| **Marshrutlash va WAN** | Statik/DHCP/PPPoE WAN, VLAN 802.1Q, kanallarni birlashtirish, Multi-WAN zaxira va balanslash, policy-based routing, **OSPFv2** va **BGP** (FRR) |
| **Otkazoustoychivost (HA)** | VRRP virtual manzillari (keepalived), bir tomonlama **konfiguratsiya klasteri**, sessiya holati sinxronizatsiyasi (**conntrackd**) — ochiq ulanishlar failoverdan omon chiqadi, va Kea HA hook'i bilan **DHCP ijaralari sinxronizatsiyasi**: jonli juftlikda mijoz o'z manzilini saqlab qoldi |
| **IPS / IDS** | Suricata integratsiyalangan, **NFQUEUE orqali inline bloklash** (mos kelgan qoida paketni haqiqatan tushiradi), Emerging Threats qoidalari, jadval bo'yicha signatura yangilash, istisno ro'yxatlari |
| **Kontent filtri** | UT1 kategoriyalar bazasi, DNS qora ro'yxatlari, SafeSearch majburlash, ilovalar va P2P/torrent filtri, foydalanuvchi va guruh bo'yicha siyosatlar |
| **HTTPS tekshiruvi** | Uch rejim: faqat DNS, **TLS'dagi server nomi (SNI) bo'yicha filtr — standart, trafik ochilmaydi va mijozlarga hech narsa o'rnatilmaydi**, yoki o'z CA'ngiz bilan to'liq deshifratsiya (URL yo'llari, kalit so'zlar, HTTPS ichidagi antivirus) |
| **Antivirus** | HTTP/HTTPS uchun ClamAV + c-icap, **SMTP/POP3 pochta proksisi**, aniq proksi orqali FTP, bazalarni yangilash, foydalanuvchini ogohlantirish |
| **Proksi va veb** | Squid aniq va shaffof rejimda, WPAD/PAC, AD SSO, kesh, yuqori proksi; **nginx teskari proksisi** — SSL offload, balanslash va **aktiv health check** |
| **VPN** | Foydalanuvchi profillari va ACL bilan OpenVPN serveri, **WireGuard** (server, peer'lar, site-to-site), **IPsec** site-to-site (IKEv1/IKEv2) va EAP-MSCHAPv2 bilan IKEv2 mobil klientlari |
| **DHCP va DNS** | dnsmasq **yoki Kea** (dvigatel tanlanadi), bir nechta soha, rezervatsiyalar, ixtiyoriy opsiyalar, ijaralar monitoringi va tarixi, DHCPv6, Router Advertisement, **DHCPv6-PD**; DNS kesh va forwarder, **DNSSEC**, **DNS-over-TLS** |
| **Foydalanuvchilar** | Lokal foydalanuvchi va guruhlar, **Active Directory**, LDAP, RADIUS, Kerberos/NTLM SSO, captive portal, administrator RBAC, «faqat o'qish» admini, **har bir foydalanuvchiga TOTP 2FA** |
| **QoS** | IP, foydalanuvchi, guruh va xizmat bo'yicha limitlar, kafolatlangan tezlik, ustuvor navbatlar, jadval, bufferbloat'ga qarshi **fq_codel va CAKE**, bloklash yoki tezlikni pasaytirish bilan trafik kvotalari |
| **Monitoring** | Jonli va tarixiy grafiklar, aktiv ulanishlar va uzellar, foydalanuvchi / MAC / sayt / protokol / kunlar bo'yicha hisobotlar, CSV eksport, jadval bo'yicha e-mail hisobotlar, SNMP v2c va v3, syslog, ogohlantirishlar |
| **Ekspluatatsiya** | Lokal CA, Let's Encrypt, retention bilan zaxira nusxalar, panel javob bermay qolsa **avtomatik qaytariladigan imzolangan yangilanishlar**, internetsiz ISO o'rnatish |

### Nega o'tishadi

- Hammasi **bitta panelda** — alohida IDS, proksi, antivirus va hisobot
  mahsulotlarini bir-biriga ulash shart emas.
- **HTTPS filtri standart holatda ishlaydi** — har bir kompyuterga sertifikat
  o'rnatmasdan.
- **Foydalanuvchi bo'yicha hisob**: trafik AD, MAC va DHCP manzili
  o'zgarganda ham odamga ergashadi, IP ga emas.
- **Oflayn o'rnatish va oflayn yangilanish** — internetga chiqmaydigan
  tarmoqlar uchun.
- Panel **rus, ingliz va o'zbek** tillarida.

pfSense bilan to'liq taqqoslash **qurilmaning o'zida** keladi
(`docs/linux-utm-gateway-vs-pfsense.md`); manba kod repozitoriysi yopiq,
shuning uchun u yerdagi bo'shliqlar quyida — **[Nimalarni
qilmaydi](#limits)**. Yuqoridagi jadvaldan oldin o'sha bo'limni o'qing.

---

<a name="limits"></a>
## What it does not do · Чего продукт не умеет · Nimalarni qilmaydi

Every product has gaps. These are this one's, taken from the comparison
document that ships on the appliance — not a shortened version of it.

### 🇬🇧 English

| Gap | Detail |
|---|---|
| **Routing policy** | OSPFv2 and BGP work, but there are **no route-maps, prefix-lists or community filters**. If your routing decisions are made by filtering, use pfSense/FRR directly |
| **Clusters larger than two** | VRRP + conntrackd + the config cluster are designed for **a pair**. Three or more nodes are not supported; pfSense CARP is |
| **Multi-WAN depth** | Failover and load balancing work, but gateway groups and their diagnostics are far less mature than pfSense's |
| **Advanced IPv6** | Basic firewall, RA, DHCPv6 and DHCPv6-PD are there. Wide DHCPv6 option sets, downstream delegation pools, tunnel broker/6RD and complex Track Interface handling are not |
| **IPsec for old clients** | Road-warrior is **IKEv2 only**. No IKEv1, so no Cisco Unity split-include. Windows, macOS, iOS and Android built-in clients are unaffected |
| **L2TP server** | Not implemented |
| **SMS authentication** | Not implemented |
| **FTP antivirus** | Only through the **explicit** proxy — the client must be configured to use it. FTP's data channel is a second connection and cannot be caught transparently |
| **Full HTTPS decryption** | Requires the gateway CA installed on **every** client, and closes HTTP/3 (QUIC) while inspection is on, or browsers route around it. SNI filtering — the default — has neither cost |
| **DHCP failover** | Only on the **Kea** engine. On dnsmasq the standby cannot serve DHCP at all; the gap is narrowed by copying the lease file every five minutes, which is not the same thing |
| **Kea logging** | Kea does not put the client name on the lease line (the name is in the leases table) and does not log a returned lease at INFO |
| **Platform** | Debian 12 (bookworm), Python 3.11, amd64 — nothing else. Updates carry bytecode bound to that interpreter and refuse to install elsewhere |
| **Ecosystem** | No package or plugin system, and no third-party community. What ships is what there is |
| **Source code** | Not public. Only this release channel is |

### 🇷🇺 Русский

| Пробел | Подробности |
|---|---|
| **Политика маршрутизации** | OSPFv2 и BGP работают, но **route-map, prefix-list и community-фильтров нет**. Если маршрутные решения принимаются фильтрацией — берите pfSense/FRR |
| **Кластер больше двух узлов** | VRRP + conntrackd + кластер конфигурации рассчитаны на **пару**. Три и более узлов не поддерживаются, у pfSense CARP — да |
| **Глубина Multi-WAN** | Резервирование и балансировка есть, но gateway groups и их диагностика заметно менее зрелые, чем в pfSense |
| **Продвинутый IPv6** | Базовый firewall, RA, DHCPv6 и DHCPv6-PD есть. Широких наборов опций DHCPv6, пулов delegation вниз по сети, tunnel broker/6RD и сложного Track Interface — нет |
| **IPsec для старых клиентов** | Мобильный доступ **только IKEv2**. IKEv1 нет, значит нет и Cisco Unity split-include. Встроенных клиентов Windows, macOS, iOS и Android это не касается |
| **Сервер L2TP** | Не реализован |
| **SMS-аутентификация** | Не реализована |
| **Антивирус для FTP** | Только через **явный** прокси — клиента нужно на него настроить. Канал данных FTP идёт вторым соединением и прозрачно не перехватывается |
| **Полная расшифровка HTTPS** | Требует установки CA шлюза на **каждый** клиент и закрывает HTTP/3 (QUIC) на время инспекции, иначе браузеры её обходят. У фильтрации по SNI — режима по умолчанию — этих издержек нет |
| **Переключение DHCP** | Только на движке **Kea**. На dnsmasq резервный узел вообще не может раздавать адреса; копирование файла аренд раз в пять минут — не замена |
| **Журнал Kea** | Kea не пишет имя клиента в строке выдачи аренды (имя есть в таблице аренд) и не пишет возврат аренды на уровне INFO |
| **Платформа** | Debian 12 (bookworm), Python 3.11, amd64 — и ничего больше. Обновления несут байт-код, привязанный к этому интерпретатору, и на другом не устанавливаются |
| **Экосистема** | Нет системы пакетов и плагинов, нет стороннего сообщества. Есть ровно то, что поставляется |
| **Исходный код** | Закрыт. Открыт только этот канал обновлений |

### 🇺🇿 Oʻzbekcha

| Bo'shliq | Tafsilot |
|---|---|
| **Marshrutlash siyosati** | OSPFv2 va BGP ishlaydi, lekin **route-map, prefix-list va community filtrlari yo'q**. Marshrut qarorlari filtrlash bilan qabul qilinsa — pfSense/FRR ni oling |
| **Ikkitadan katta klaster** | VRRP + conntrackd + konfiguratsiya klasteri **juftlik** uchun mo'ljallangan. Uch va undan ortiq tugun qo'llab-quvvatlanmaydi, pfSense CARP da esa bor |
| **Multi-WAN chuqurligi** | Zaxiralash va balanslash bor, lekin gateway groups va uning diagnostikasi pfSense'nikidan ancha sodda |
| **Kengaytirilgan IPv6** | Asosiy firewall, RA, DHCPv6 va DHCPv6-PD bor. Keng DHCPv6 opsiyalari, quyi tarmoqqa delegation poollari, tunnel broker/6RD va murakkab Track Interface yo'q |
| **Eski klientlar uchun IPsec** | Mobil kirish **faqat IKEv2**. IKEv1 yo'q, demak Cisco Unity split-include ham yo'q. Windows, macOS, iOS va Android'ning o'rnatilgan klientlariga taalluqli emas |
| **L2TP serveri** | Amalga oshirilmagan |
| **SMS autentifikatsiya** | Amalga oshirilmagan |
| **FTP uchun antivirus** | Faqat **aniq** proksi orqali — mijozni unga sozlash kerak. FTP ma'lumot kanali ikkinchi ulanishda ketadi va shaffof ushlanmaydi |
| **To'liq HTTPS deshifratsiyasi** | **Har bir** mijozga shlyuz CA sini o'rnatishni talab qiladi va tekshiruv davomida HTTP/3 (QUIC) ni yopadi, aks holda brauzerlar uni chetlab o'tadi. SNI bo'yicha filtrda — standart rejimda — bu xarajatlar yo'q |
| **DHCP failover** | Faqat **Kea** dvigatelida. dnsmasq'da zaxira tugun umuman manzil bera olmaydi; ijara faylini besh daqiqada bir ko'chirish uning o'rnini bosmaydi |
| **Kea jurnali** | Kea ijara berish qatorida mijoz nomini yozmaydi (nom ijaralar jadvalida bor) va qaytarilgan ijarani INFO darajasida yozmaydi |
| **Platforma** | Debian 12 (bookworm), Python 3.11, amd64 — boshqasi yo'q. Yangilanishlar shu interpretatorga bog'langan bayt-kod olib yuradi va boshqasiga o'rnatilmaydi |
| **Ekotizim** | Paket yoki plagin tizimi yo'q, tashqi hamjamiyat yo'q. Nima yetkazilsa — o'shanigina bor |
| **Manba kod** | Yopiq. Faqat shu yangilanish kanali ochiq |

---

## Keywords / Ключевые слова / Kalit so'zlar

For anyone searching for a **pfSense alternative**, an **OPNsense
alternative**, a **Kerio Control alternative** or a **Kerio Control
replacement**, an **Untangle / Arista NG Firewall alternative**, a **Zentyal**,
**Endian**, **ClearOS**, **IPFire**, **Sophos UTM/XG** or **Fortigate**
alternative — or simply for a **Linux UTM firewall appliance**:

`UTM` · `unified threat management` · `next-generation firewall` · `NGFW` ·
`Linux firewall appliance` · `Debian firewall distro` · `firewall ISO` ·
`open source firewall` · `pfSense alternative` · `OPNsense alternative` ·
`Kerio Control alternative` · `Kerio Control zamena` · `Untangle alternative` ·
`Zentyal alternative` · `Endian alternative` · `ClearOS alternative` ·
`IPFire alternative` · `Sophos UTM alternative` · `nftables` · `Suricata IPS` ·
`intrusion prevention` · `NFQUEUE inline IPS` · `Squid proxy` ·
`transparent proxy` · `WPAD PAC` · `ClamAV antivirus gateway` · `c-icap` ·
`content filter` · `URL filtering` · `UT1 categories` · `SafeSearch` ·
`HTTPS inspection` · `SNI filtering` · `SSL bump` · `DNS blacklist` ·
`DNSSEC` · `DNS over TLS` · `OpenVPN` · `WireGuard` · `IPsec IKEv2` ·
`strongSwan` · `road warrior VPN` · `site-to-site VPN` · `captive portal` ·
`Active Directory` · `LDAP` · `RADIUS` · `Kerberos SSO` · `TOTP 2FA` ·
`bandwidth management` · `traffic shaping` · `QoS` · `CAKE` · `fq_codel` ·
`traffic accounting` · `user quota` · `high availability` · `VRRP` ·
`keepalived` · `conntrackd` · `Kea DHCP HA` · `DHCP failover` ·
`multi-WAN failover` · `policy-based routing` · `OSPF` · `BGP` · `FRR` ·
`reverse proxy` · `nginx` · `SSL offloading` · `load balancing` ·
`GeoIP blocking` · `offline install` · `air-gapped` · `signed updates`

**Русский:** межсетевой экран · шлюз безопасности · UTM-шлюз · аналог Kerio
Control · замена Kerio Control · аналог pfSense · контент-фильтр ·
фильтрация HTTPS · контроль трафика · учёт трафика по пользователям ·
прокси-сервер · антивирус на шлюзе · система обнаружения вторжений ·
отказоустойчивость · кластер · VPN-сервер · родительский контроль ·
шейпер трафика · офлайн-установка

**Oʻzbekcha:** xavfsizlik shlyuzi · tarmoq ekrani · kontent filtri ·
trafik hisobi · proksi-server · antivirus shlyuz · hujumlarni aniqlash ·
otkazoustoychivost · VPN server · internetsiz o'rnatish

---

## Qurilma buni qanday ishlatadi

Odatda hech narsa qilish kerak emas. Panel kanalni davriy tekshiradi,
**Дополнительные параметры → Обновление ПО** varag'ida yangi versiyani
ko'rsatadi va «Устанавливать обновления автоматически» yoqilgan bo'lsa o'zi
o'rnatadi.

Yangilanish tartibi ataylab shunday:

1. paket yuklab olinadi va **tekshiriladi** — hech narsa ochilmasdan oldin;
2. yangi daraxt `/opt/linux-utm-gateway.new` ga ochiladi;
3. eskisi `/opt/linux-utm-gateway.prev` ga o'tadi, yangisi o'rniga qo'yiladi;
4. systemd unitlari va CLI skriptlari qayta o'rnatiladi, panel qayta ishga tushadi;
5. panel belgilangan vaqt ichida javob bermasa — **hammasi o'z-o'zidan qaytariladi**.

Beshinchi qadam muhim: boshqarib bo'lmaydigan shlyuz o'tgan haftaning kodida
ishlayotgan shlyuzdan yomonroq.

Kanalda faqat eng yangi imzolangan paket saqlanadi. Joriy ko‘rsatkich va
digestning mashina o‘qiydigan manbasi — `latest.json`.

---

## Paket nima?

`.utmupd` — gzip'langan tar, ichida roppa-rosa uchta a'zo:

| A'zo | Nima |
|---|---|
| `manifest.json` | versiya, sana, commit, yuk hajmi va uning `sha256` i, `python_tag` |
| `manifest.sig` | manifest ustidan Ed25519 imzosi |
| `payload.tar.gz` | `/opt/linux-utm-gateway` ustiga ochiladigan daraxt |

Tekshirish tartibi va uning sababi:

1. **Avval imzo** — manifest bizniki ekani isbotlanadi;
2. **keyin digest** — yuk imzolangan manifest aytgan narsa ekani isbotlanadi;
3. va faqat shundan keyin biror narsa diskka ochiladi.

Teskari tartibda tekshirilganda imzolanmagan ma'lumot allaqachon fayl tizimida
bo'lardi. Ochilishdan oldin hajmlar ham cheklanadi (manifest 64 KB, imzo 4 KB,
yuk 200 MB), ya'ni yovuz arxiv imzo tekshirilgunga qadar xotirani to'ldira
olmaydi.

Ochiq kalit mahsulot bilan birga keladi:
`/opt/linux-utm-gateway/keys/release-ed25519.pub`. Maxfiy kalit hech qachon
repozitoriyda bo'lmagan.

### Moslik

Paket **bayt-kod** olib yuradi, manba emas. Bayt-kod aynan bitta Python
minor versiyasiga bog'langan, shuning uchun manifestda `python_tag`
(`cpython-311`) bor. Boshqa interpretatorli qurilma paketni **o'rnatmaydi** —
yuklab bo'lmaydigan daraxtni o'rnatgandan ko'ra rad etgani yaxshi.

Ya'ni: Debian 12 (bookworm), Python 3.11.

---

## Qo'lda o'rnatish

Kanal ishlamasa yoki qurilmada internet bo'lmasa.

Paketni oling:

```bash
curl -fLO https://raw.githubusercontent.com/soatov86/linux-utm-gateway-releases/main/utm-update-1.1.57.utmupd
```

`latest.json` dagi digest bilan solishtiring:

```bash
sha256sum utm-update-1.1.57.utmupd
```

Qurilmaga ko'chiring va o'rnatishga qo'ying:

```bash
sudo mkdir -p /var/lib/utm/updates && sudo install -m600 -o root -g root utm-update-1.1.57.utmupd /var/lib/utm/updates/pending.utmupd
```

O'rnatuvchini `/opt` dan emas, `/var/lib/utm` dan yurgizing — o'rnatish
jarayonida `/opt` nomi o'zgartiriladi va skript ostidan yer siljib ketmasligi
kerak. Panel ham aynan shunday qiladi:

```bash
sudo cp /opt/linux-utm-gateway/scripts/utm-update-apply /var/lib/utm/updates/ && sudo python3 /var/lib/utm/updates/utm-update-apply
```

O'rnatuvchi imzoni va digestni yana o'zi tekshiradi — yuqoridagi `sha256sum`
sizning nusxangiz to'liq yuklanganini bildiradi, o'rnatuvchining ishonchi esa
imzoga tayanadi.

## Orqaga qaytarish

Oldingi daraxt `/opt/linux-utm-gateway.prev` da saqlanadi:

```bash
sudo python3 /var/lib/utm/updates/utm-update-apply --rollback
```

Yangilanishdan keyin panel javob bermasa, bu avtomatik bajariladi — buyruq
esa keyinroq, masalan xatti-harakat o'zgargani ma'lum bo'lganda kerak bo'ladi.

---

© Linux UTM Gateway · ISO: https://t.me/Linux_UTM · soatov86@gmail.com
