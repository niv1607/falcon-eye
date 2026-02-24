# Decision Intelligence – Master Spec v1.1
תאריך: 2026-02-24

## מה חדש בגרסה 1.1
- נוספה לוגיקת סריקה מלאה (Fast Filter) + טריגרים לכניסה לפייפליין
- נוספו תנאי אות (Signals) לכל מנוע (Momentum / Mean Reversion / Event / Core)
- נוספו סליידרים עם ברירות מחדל וטווחים ב-Risk Lab (גישה C)

---

## 1) לוגיקת סריקה (Scan Pipeline)
**Universe:** 400 נכסים  
**תדירות:** סריקה יומית; תוך-יומית אופציונלית רק ל-Candidate Pool; סריקה ידנית לפי דרישה.

### Fast Filter – מה נסרק
- שינוי יומי/5 ימים (%)
- Gap (%)
- VolRatio (מחזור/ממוצע 20)
- ATR% (14)
- Vol20 (סטיית תקן 20)
- RSI(14)

### כניסה לפייפליין (ANY-OF)
- |Δ1D| ≥ 3% (טווח 2–6)
- |Δ5D| ≥ 8% (טווח 5–15)
- |Gap| ≥ 5% (טווח 3–10)
- VolRatio ≥ 1.8 (טווח 1.3–3)
- ATR% ≥ 2% (טווח 1–4)
- RSI ≤ 30 או ≥ 70 (טווח 20–35 / 65–80)

**פלט צפוי:** 30–60 נכסים ל-Candidate Pool ביום.

---

## 2) אותות מנועים (Signal Definitions)

### Momentum – Breakout
ברירת מחדל:
- Close > HighestClose(20) (טווח 15–55)
- VolRatio ≥ 1.5 (טווח 1.2–2.5)
- ATR% ≥ 1.5 (טווח 1–3)
- אופציונלי: Close > EMA20

Anti-noise:
- |Δ1D| < 10% (טווח 6–15)

Tier A requires confirmation:
- סגירה נוספת מעל רמת הפריצה או Retest מוצלח.

### Mean Reversion – Dislocation
Dislocation (ANY-OF):
- ZScore20 ≤ -2 (טווח -1.5 עד -3)
- RSI ≤ 25 (טווח 20–35)
- GapDown ≥ 7% (טווח 4–12)
- 3 אדומים + ירידה מצטברת ≥ 8% (טווח 5–15)

Tier A requires confirmation (ANY-OF):
- bullish engulfing / pin bar / higher low / close>EMA5 / close>EMA10

Early Exception:
- CS≥85 AND Eq≥8.5 AND Hs≥8 AND no_hard_stop → entry≤1/3

### Event
Data-supported (ANY-OF):
- earnings_surprise ≥ 5% (טווח 2–15)
- guidance_change ≥ 5% (טווח 2–20)
- fundamental_change ≥ 5% (טווח 2–25)

Tier A requires:
- Verification Gate מדיד
- מינימום מקורות: 5 (טווח 5–10)

### Core
Watchlist (ANY-OF):
- quality_screen_pass / valuation_gap / fundamental_trend_positive

Tier A requires:
- CS≥80 (טווח 75–90)
- בתוך תקרות חשיפה
- Hard Stop מוגדר לתזה

---

## 3) Risk Lab (Sliders)
כל סף במסמך מוגדר כברירת מחדל + טווח, וכל שינוי נשמר עם version_id.

