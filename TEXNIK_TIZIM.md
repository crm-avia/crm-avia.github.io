# Pilot Akademiyasi — texnik tizim hujjati

## 1. Maqsad

## 2. Fayl tuzilmasi (har bir bo'lim alohida HTML)
| Fayl | Vazifasi |
|---|---|
| `index.html` | Bosh sahifa: hero, bo'limlar, statistika |
| `darslar.html` | Darajalar bo'yicha ketma-ket darslar + dars oxirida test |
| `testlar.html` | Foydalanuvchi progressi va natijalari |
| `samolyotlar.html` | Samolyot / vertolyot / harbiy qiruvchilar (Vikipediya) |
| `kokpit.html` | Kokpit qurilmalari va vazifalari |
| `qisqartmalar.html` | Qisqartmalar: to'liq shakli, ma'nosi, qo'llanilishi |
| `lugat.html` | Aviatsiya atamalari |
| `radio.html` | Radio aloqa va frazeologiya |
| `admin.html` | Admin kirishi / chiqishi |
| `style.css` | Yagona dizayn tizimi |
| `app.js` | Umumiy mantiq: ma'lumotlar ombori, CRUD, auth, test |

## 3. Dizayn tizimi
- Ranglar: `#050b1c` (fon), `#0a1630` (panel), `#16284d` (chiziq), `#2f7cff` (asosiy ko'k), `#4aa3ff` (ochiq ko'k), `#e8eefc` (matn).
- Navigatsiya yuqorida, kartalar tarmoq (grid) ko'rinishida, responsiv (mobil uchun moslashadi).

## 4. Ma'lumotlar modeli (hozir: `localStorage`, kalit `pa_<nom>`)
- `abbr`: `{id, t (qisqartma), f (to'liq shakli), m (ma'nosi), u (qachon qo'llanadi)}`
- `lugat`: `{id, t, m}`
- `radio`: `{id, t, m}`
- `kokpit`: `{id, t, m, c (Umumiy/Samolyot/Vertolyot)}`
- `aircraft`: `{id, t (Vikipediya sahifa nomi), c (toifa)}`
- `lessons`: `{id, lv (daraja), no (tartib), t, body, qs}`; `qs` har qatorida: `Savol|A|B|C|D|to'g'ri raqam(1-4)`
- `res`: `{lessonId: {s (ball), n (savollar soni), d (sana)}}`
- `admin`: `true/false`

## 5. Asosiy mexanizmlar
**Darslar ketma-ketligi.** Darslar `daraja → tartib raqami` bo'yicha saralanadi. Keyingi dars faqat oldingi test kamida 60% bilan topshirilganda ochiladi (admin uchun hammasi ochiq).

**Test va progress.** Dars ichida test ishlanadi, natija `res` ga yoziladi. `testlar.html` umumiy progress, o'rtacha natija va har bir dars natijasini ko'rsatadi.

**Samolyotlar (Vikipediya).** Admin faqat sahifa nomini kiritadi. Brauzer `https://uz.wikipedia.org/api/rest_v1/page/summary/<nom>` ga so'rov yuboradi, topilmasa `en.wikipedia.org` ishlatiladi. Rasm, qisqa tavsif va havola ko'rsatiladi.


**Admin.** `admin.html` da parol kiritiladi (hozirgi demo parol: `admin123`, `app.js` ichida `ADMIN_PASS`). Admin rejimida har bir bo'limda "Qo'shish / Tahrirlash / O'chirish" tugmalari chiqadi (CRUD).

## 6. Muhim cheklov va keyingi bosqich (haqiqiy production uchun)
Hozirgi versiya serversiz ishlaydi, shuning uchun:
- Ma'lumotlar faqat shu brauzerda saqlanadi (boshqa qurilmada ko'rinmaydi).
- Admin paroli kodda turadi — haqiqiy himoya emas.
- Foydalanuvchi hisoblari yo'q, natijalar brauzerga bog'liq.

Production uchun tavsiya:
| Qatlam | Texnologiya |
|---|---|
| Backend | Node.js (Express) yoki Python (FastAPI) |
| Baza | PostgreSQL (jadvallar: users, abbr, lugat, radio, cockpit, aircraft, lessons, questions, results) |
| Auth | JWT + parol hash (bcrypt), rollar: `user`, `admin` |
| API | `GET/POST/PUT/DELETE /api/<bo'lim>`, admin yo'llari rol bilan himoyalanadi |
| Vikipediya | Server tomonida keshlash (ma'lumot 24 soat saqlanadi) |
| Hosting | Nginx + HTTPS |

## 7. Ishga tushirish

## 8. Yangilanishlar (v2)
- **Til tizimi (uz / ru / en):** birinchi kirishda til tanlanadi, keyin yuqoridagi menyudan o'zgartiriladi. Matnlar `{uz, ru, en}` obyekti sifatida saqlanadi; admin formasida har til uchun alohida maydon bor.
- **Samolyotlar:** Vikipediya ma'lumoti tanlangan tilda olinadi (topilmasa inglizcha).
- **Rasmlar:** bosh sahifa bo'limlari va kokpit qurilmalari uchun Vikipediya rasmi avtomatik olinadi; admin kokpitda o'z rasmini URL yoki fayl orqali qo'yishi mumkin (fayl 500 px gacha kichraytirilib saqlanadi).
- **Darslar:** har til uchun alohida YouTube havolasi (`yt.uz / yt.ru / yt.en`).
- **Olib tashlandi:** Ob-havo bo'limi. **Qo'shildi:** FAA, EASA, ICAO, IATA va boshqa qisqartmalar; kengaytirilgan radio aloqa; admin uchun "Chiqish".

