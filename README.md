# ANC New Job Lucky Reward — Prototype

ฟีเจอร์กระตุ้นการแจ้งงานใหม่สำหรับ ANC Portal: พนักงาน/ตัวแทน/ผู้ใช้ที่ได้รับอนุมัติ แจ้งงานใหม่ที่ผ่านเงื่อนไขแล้วได้สิทธิ์หมุนวงล้อลุ้นรางวัลเงินสด 20–1,000 บาท ภายใต้งบ 5,000 บาท/วัน

## สรุปโปรเจค

**เป้าหมายธุรกิจ:** ปัจจุบันมีงานเข้า ~200 งาน/วัน เป็นงานใหม่ ~20% (40 งาน/วัน) ต้องการเพิ่มเป็น ≥30% (60 งาน/วัน) โดยใช้งบรางวัล 5,000 บาท/วัน (เฉลี่ย 83.33 บาท/งานเป้าหมาย หรือ 250 บาท/งานส่วนเพิ่ม)

**กติกาหลัก:** 1 งานใหม่ที่ผ่านการตรวจสอบ = 1 สิทธิ์หมุนวงล้อ (หมุนแล้วได้เสมอ ขั้นต่ำ 20 บาท) รางวัลใหญ่ 1,000 บาท จำกัดวันละ 1 รางวัล เมื่องบเงินสดของวันหมด ระบบสลับเป็น Bonus Point Mode อัตโนมัติ (ผู้ใช้เห็นเฉพาะข้อความเชิงบวก ไม่มีการโชว์งบคงเหลือบนหน้าผู้ใช้เด็ดขาด) รางวัลเข้า Reward Wallet สถานะ รอตรวจสอบ → อนุมัติ → จ่ายแล้ว

**สิ่งที่อยู่ใน prototype นี้ (ไฟล์เดียว ไม่มี backend):**
หน้าผู้ใช้ `/portal/new-job-reward` — Hero + มาสคอต SKY, การ์ดรางวัลใหญ่, วงล้อ ANC Lucky Reward Wheel (สลับเป็นวงล้อคะแนนใน Bonus Point Mode), Leaderboard 3 แท็บ, Reward Wallet, กติกา, T&C/PDPA, รองรับมือถือ + sticky CTA และหน้าแอดมิน `/admin/new-job-reward` — การ์ดสรุปแคมเปญ/งบ/KPI, Reward Pool, ตารางตรวจงาน (กดอนุมัติ/ปฏิเสธได้), Transaction Log, Fraud Monitoring, Settings, Reports, ปุ่ม Pause/Emergency Stop

**ปุ่ม "สถานะเดโม่" (มุมขวาบน):** ใช้สลับ state เพื่อรีวิว UX — โหมดคะแนนสะสม (งบหมดภายใน), รางวัลใหญ่ออกแล้ว, จำนวนสิทธิ์ 0/3, empty states

## โครงสร้างไฟล์

```
anc-new-job-lucky-reward/
├── index.html                        ← prototype หลัก (เปิดไฟล์นี้)
├── README.md                         ← ไฟล์นี้
├── docs/
│   ├── anc-lucky-wheel-design.md     ← เอกสารออกแบบ: สัดส่วนรางวัล งบ ซิมูเลชัน ข้อกังวล (กฎหมาย/ภาษี/fraud)
│   └── anc-lucky-wheel-prompt.md     ← พรอมพ์สำหรับสั่งพัฒนาระบบจริง (backend logic, DB, API, test)
└── legacy/
    └── anc-lucky-wheel-demo.html     ← เดโม่เวอร์ชันแรก (เก็บไว้อ้างอิง)
```

## วิธีรันบน local

**แบบที่ 1 — เปิดตรง ๆ (ง่ายสุด):** ดับเบิลคลิก `index.html` เปิดด้วย Chrome/Edge/Safari ได้เลย ทุกฟีเจอร์ทำงานครบเพราะเป็นไฟล์เดียวจบ ไม่เรียก API ภายนอก (ต้องแตก zip ออกมาก่อน อย่าเปิดจากในไฟล์ zip หรือจากพรีวิวในแอปแชท เพราะสคริปต์จะโดนบล็อกทำให้วงล้อไม่หมุน)

**แบบที่ 2 — รันผ่าน local server (เหมือนสภาพจริงกว่า):**

```bash
cd anc-new-job-lucky-reward

# ถ้ามี Python
python -m http.server 8000
# แล้วเปิด http://localhost:8000

# หรือถ้ามี Node.js
npx serve .
# แล้วเปิด URL ที่ขึ้นใน terminal (ปกติ http://localhost:3000)
```

หมายเหตุ: ฟอนต์ Prompt โหลดจาก Google Fonts ถ้าออฟไลน์หน้าเว็บยังใช้ได้แต่ฟอนต์จะเป็นฟอนต์ระบบแทน

## วิธี deploy ขึ้น Vercel

**แบบที่ 1 — ผ่าน Vercel CLI (แนะนำ เร็วสุด):** ต้องมี Node.js ในเครื่อง

```bash
npm i -g vercel

cd anc-new-job-lucky-reward
vercel login        # ใส่อีเมล แล้วไปกดยืนยันลิงก์ในกล่องเมล
vercel              # ตอบคำถาม: กด Enter รับค่า default ทั้งหมด (ไม่ต้องตั้ง build command — เป็น static site)
vercel --prod       # ขึ้น production ได้ URL จริง เช่น https://anc-new-job-lucky-reward.vercel.app
```

Vercel จะเห็น `index.html` ที่ root แล้ว serve เป็น static site อัตโนมัติ ไม่ต้องตั้งค่าอะไรเพิ่ม อัปเดตครั้งถัดไปแค่แก้ไฟล์แล้วรัน `vercel --prod` ซ้ำ

**แบบที่ 2 — ผ่าน GitHub + Vercel Dashboard (เหมาะถ้าจะทำต่อเป็นทีม):** push โฟลเดอร์นี้ขึ้น GitHub repo → เข้า [vercel.com](https://vercel.com) → Add New → Project → Import repo นั้น → Framework Preset เลือก **Other** → Deploy จากนั้นทุกครั้งที่ push โค้ดใหม่ Vercel จะ deploy ให้อัตโนมัติ

**ข้อควรระวังเรื่องการเผยแพร่:** prototype นี้เป็นข้อมูลจำลองทั้งหมดจึงเผยแพร่ได้ แต่ถ้าพัฒนาต่อเป็นระบบจริง อย่า deploy ตัวที่ต่อข้อมูลจริงขึ้น public URL — ควรอยู่หลังระบบล็อกอิน/intranet ของ ANC

## สิ่งสำคัญก่อนพัฒนาเป็นระบบจริง

1. **ตรรกะรางวัลทั้งหมดใน prototype นี้จำลองไว้ฝั่ง browser เพื่อการเดโม่เท่านั้น** ระบบจริงต้องสุ่มรางวัล คุมงบ และจำกัดแจ็คพอตฝั่ง server 100% (ใช้ CSPRNG, atomic update กัน race condition, idempotent endpoint) — frontend มีหน้าที่แค่เล่นแอนิเมชันวงล้อให้หยุดตรงผลที่ API ตอบกลับ
2. สเปคฉบับเต็มสำหรับทีมพัฒนา (DB schema, API endpoints, anti-fraud, budget guard, test cases) อยู่ใน `docs/anc-lucky-wheel-prompt.md`
3. ข้อกังวลที่ต้องเคลียร์ก่อนเปิดใช้ (ใบอนุญาตชิงโชคกรณีเปิดให้บุคคลภายนอก, ภาษีหัก ณ ที่จ่าย, PDPA, การป้องกันการโกง, ระบบแจ้งเตือน LINE, นโยบาย clawback) อยู่ใน `docs/anc-lucky-wheel-design.md`
