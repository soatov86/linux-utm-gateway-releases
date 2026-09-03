# Linux UTM Gateway — reliz kanali

Bu repozitoriy shlyuzning yangilanish kanalidir. O'rnatilgan qurilma shu
yerdagi `latest.json` faylini o'qiydi va o'zidagidan yangi versiya bo'lsa,
imzolangan paketni shu yerdan yuklab oladi.

Bu yerda faqat chiqarilgan paketlar turadi. Manba kod boshqa joyda.

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
curl -fLO https://raw.githubusercontent.com/soatov86/linux-utm-gateway-releases/main/utm-update-1.x.x.utmupd
```

`latest.json` dagi digest bilan solishtiring:

```bash
sha256sum utm-update-1.0.95.utmupd
```

Qurilmaga ko'chiring va o'rnatishga qo'ying:

```bash
sudo mkdir -p /var/lib/utm/updates && sudo install -m600 -o root -g root utm-update-1.x.x.utmupd /var/lib/utm/updates/pending.utmupd
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
