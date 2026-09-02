# Linux UTM Gateway — reliz kanali

Bu repozitoriy shlyuzning yangilanish kanalidir. O'rnatilgan qurilma shu
yerdagi `latest.json` faylini o'qiydi va o'zidagidan yangi versiya bo'lsa,
imzolangan paketni shu yerdan yuklab oladi.

Bu yerda faqat chiqarilgan paketlar turadi. Manba kod boshqa joyda.

---

## Hozirgi reliz

| | |
|---|---|
| Versiya | **1.0.90** |
| Fayl | `utm-update-1.0.90.utmupd` |
| Chiqarilgan | 2026-09-02 |
| Talab | Debian 12 (bookworm), Python 3.11 |

Kanal holati doim `latest.json` da:

```json
{
  "version": "1.0.90",
  "file":    "utm-update-1.0.90.utmupd",
  "sha256":  "…",
  "released": "2026-09-02",
  "notes":   "…"
}
```

`sha256` — paket faylining o'zi uchun. Paket ichidagi yuk uchun alohida
digest bor va u imzolangan manifestda yotadi (pastga qarang).

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
curl -fLO https://raw.githubusercontent.com/soatov86/linux-utm-gateway-releases/main/utm-update-1.0.90.utmupd
```

`latest.json` dagi digest bilan solishtiring:

```bash
sha256sum utm-update-1.0.90.utmupd
```

Qurilmaga ko'chiring va o'rnatishga qo'ying:

```bash
sudo mkdir -p /var/lib/utm/updates && sudo install -m600 -o root -g root utm-update-1.0.90.utmupd /var/lib/utm/updates/pending.utmupd
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

## Relizlar

### 1.0.90 — 2026-09-02

**Hisobotlar**

- «За неделю» va «За месяц» o'rniga **«За 7 дней»** va **«За 30 дней»**: ustunlar
  endi hisobot ichidagi tugmalar bilan bir xil — surilib boruvchi oyna sanaydi.
  Ilgari dushanba kuni «За неделю» bir kunni bildirardi va «За месяц» dan katta
  chiqib qolardi. Kvotalar hamon hisob oyi bo'yicha o'lchanadi: har oyning
  1-sanasida qaytadan boshlanadigan limitni hech qachon qaytadan boshlanmaydigan
  oyna bilan o'lchab bo'lmaydi.
- **Ustundagi raqamning o'zi hisobotni ochadi** — har biri o'z davri bilan.
  «Отчёт» tugmasi olib tashlandi: u faqat bitta oraliqni bildira olardi.
- «Всего» endi yozuvlar haqiqatan boshlangan kundan hisoblaydi, `1970-01-01` dan
  emas.
- Sarlavha qatorida hisobot qaysi kunlarni qamrab olgani ko'rinadi; chop
  etilganda ham qoladi.
- «Оценка активности», «Доля», «Время» va «Заблокировано» olib tashlandi: ular
  hajmning o'lchovi emas, uning ustidagi hisob edi. Bloklangan so'rovlar
  «Деятельность» varag'ida, ular bilan nima bo'lgani ko'rinadigan joyda qoladi.
- Sahifa oynaga sig'adi, faqat jadval scroll bo'ladi.

**Rostgo'ylik**

- **IPsec tunnelining holati tekshirilmagan bo'lsa `—` ko'rsatiladi.** Ilgari
  uch xil vaziyatda — sahifa hali so'ramaganda, yuklama ostida tekshiruv
  o'tkazib yuborilganda va `swanctl` timeout bo'lganda — «Не подключён» deb
  yozilardi. Administrator buni ko'rib ishlab turgan tunnelni qayta ishga
  tushirardi.
- **Statistika yig'ish bosqichlari o'z holatini yozib boradi.** Xato ketma-ket
  ikki marta takrorlansa panelda ogohlantirish chiqadi va jurnalga yoziladi.
  Ilgari xatolar butunlay jim yutilardi: hisobotlar o'smay qo'yardi, sabab esa
  hech qayerda ko'rinmasdi.
- **Hosts va Connections** ma'lumot olinmaganini yoki ro'yxat qisqartirilganini
  aytadi. Bo'sh jadval «hech kim ulanmagan» degan da'vo — u skanerlash umuman
  qilinmaganda noto'g'ri.
- Dashboard yangilanish uzilganda «ma'lumot yangilanmayapti» deb ogohlantiradi.

**Tozalash**

- «Barcha statistikani o'chirish» endi haqiqatan o'chiradi: grafikning uchala
  aniqlik darajasi va web-faoliyat jadvali ham. `VACUUM` tranzaksiya ichida
  chaqirilardi — SQLite uni o'sha yerda rad etadi, xato jim yutilardi va fayl
  kichraymasdi. Endi bo'shatilgan bayt soni ham qaytariladi.
- Suricata'ning aylantirilgan jurnallari (`eve.json.1`, `fast.log.2.gz`)
  o'chiriladi.

**Boshqa**

- Dialoglardagi o'n bitta `?` tugmasi qo'llanmaning tegishli bo'limini ochadi
  (ilgari hech narsa qilmasdi).
- Hujjatlardagi bir qator noto'g'ri ma'lumot to'g'rilandi — jumladan Help'da
  ko'rsatilgan «zavod parollari», ular obrazda umuman yo'q.

### 1.0.89 — 2026-08-31

- **Statistika hajmi to'g'ri hisoblanadi.** «Хранилище» bo'limi butun
  `utm.db` faylini statistika deb ko'rsatardi. Endi faqat statistik
  jadvallar va indekslar egallagan SQLite sahifalari sanaladi.
- **Suricata jurnallarini tozalash.** Ilgari faqat tirik fayllar
  bo'shatilardi, logrotate qoldirgan `eve.json.1`, `fast.log.2.gz` kabi
  nusxalar esa joyni band qilib turardi. Endi ular o'chiriladi, katalogdagi
  boshqa fayllarga tegilmaydi.
- Ishlab chiquvchi uchun: `scripts/utm-publish` — reliz bitta buyruqda.

### 1.0.88 — 2026-08-31

- **Hisobotlarda faollik vaqti.** Vaqt endi har bir ulanish bo'yicha emas,
  subyekt bo'yicha bir marta sanaladi. Brauzer bitta sahifa uchun o'nlab
  ulanish ochadi, ularni qo'shish bir daqiqani yuzta daqiqaga aylantirardi.
- **MAC bo'yicha bog'langan foydalanuvchilar.** MAC-ga bog'langan qatordagi
  DHCP manzili endi doimiy IP egaligi hisoblanmaydi — aks holda o'sha
  manzilning avvalgi egasi yig'gan trafik yangi foydalanuvchiga yozilardi.
  QoS bunday xostning joriy manzilini yadro qo'shnilar jadvalidan oladi.
- **Hisob-kitob 10 soniyada bir yig'iladi** (ilgari 60) va yuklama ostida
  to'xtamaydi — tezlik testi aynan quti band bo'lgan paytda o'tadi.

---

Muammo yoki savol bo'lsa — manba repozitoriysining Issues bo'limiga yozing.
