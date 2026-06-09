# מדריך הסגנון של הצוות — שפה גרפית Binary Code (רטרו-פיקסל)

> מדריך זה מגדיר את **השפה הוויזואלית** של תוצרי ה-HTML שיעל מפיקה ב-`Output/`.
> כל קובץ `<original-name>.html` חייב להיראות ולהתנהג לפי הכללים שכאן.
> האסתטיקה: מחשבי שולחן רטרו משנות ה-80/90, מסכים פיקסליים, מסגרות שחורות עבות,
> צורות גיאומטריות צבעוניות מפוזרות, פריסת סלייידים במסך מלא.
> אתר השראה: https://starlit-pavlova-8aa0bc.netlify.app/

---

## פלטת צבעים (חובה — בדיוק הערכים האלו)

```css
--bc-black:        #000000;   /* רקע ראשי, מסגרות, טקסט */
--bc-ink:          #1A1A1A;   /* מסגרות "כמעט שחור", טקסט כהה */
--bc-pink:         #E84477;   /* מבטא חם — עיגולים, נקודות, "1" */
--bc-teal:         #3BBFB8;   /* מסך מחשב, רקע סלייד "ON/OFF" */
--bc-yellow:       #F5C800;   /* מקלדת, כותרות, רצועות */
--bc-yellow-soft:  #F5E070;   /* מקשי מקלדת */
--bc-cream:        #F5F0D0;   /* רקע מסך / קופסאות תוכן */
--bc-gray:         #C8C8C8;   /* כפתור START */
--bc-gray-shadow:  #888888;   /* צל כפתור START */
```

## טיפוגרפיה

טען מ-Google Fonts בתוך ה-`<head>`:
```html
<link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=Libre+Baskerville:wght@400;700&display=swap" rel="stylesheet">
```

- **כותרות / לייבלים / כפתורים / טקסט פיקסלי**: `'Press Start 2P', monospace`
- **גוף הטקסט / פסקאות / משפטים ארוכים**: `'Libre Baskerville', serif`
- גדלים תמיד רספונסיביים עם `clamp()`:
  - כותרת ראשית: `clamp(16px, 3.5vw, 40px)`
  - כותרת משנית: `clamp(14px, 2.8vw, 30px)`
  - גוף: `clamp(15px, 1.8vw, 23px)`
  - לייבלים פיקסליים: `clamp(11px, 1.6vw, 18px)`, `letter-spacing: 2px`
- `line-height: 1.5–1.8` — גובה שורות נדיב (קריטי לקריאות בפונט הפיקסלי).

## מסגרות וקצוות

- מסגרות **שחורות עבות**: `border: 3px–5px solid #1A1A1A` (או `#000`).
- `border-radius`: מסכים/קופסאות `8px`, מוניטור/קופסאות גדולות `16px`, מקשי מקלדת `2px`, עיגולים `50%`.
- **אין shadows רכים.** צללים, אם בכלל — קשיחים 8-bit: `box-shadow: 4px 4px 0 #888;`

## פריסה (Layout)

- כל "סלייד"/סקשן: `min-height: 100vh`, flex/grid במרכז.
- `padding` רספונסיבי: `clamp(40px, 6vw, 80px)`; `gap` נדיב: `28px–60px`.
- `overflow-x: hidden` ב-`body` (בגלל אלמנטים דקורטיביים שיוצאים מהמסך).
- **כיוון**: `<html lang="he" dir="rtl">` — חובה. רספונסיבי מובייל/טאבלט/דסקטופ.

## אלמנטים דקורטיביים (חלק מהזהות — חובה)

צורות גיאומטריות בקצוות הסלייד, יוצאות חלקית מהמסך (`position: absolute`, ערכים שליליים, גודל `clamp(120px, 15vw, 260px)`):
- חצי-עיגול ורוד (`#E84477`) בפינה עליונה.
- קשת/ספירלה צהובה (SVG) בפינה.
- קשת טורקיז בפינה תחתונה.

## רכיבים מאפיינים (תבניות מוכנות)

### מוניטור רטרו עם מסך (לכותרת המאמר)
```css
.monitor { background: #3BBFB8; border: 4px solid #1a1a1a; border-radius: 16px; padding: 14px 14px 0; }
.screen { background: #F5F0D0; border: 3px solid #1a1a1a; border-radius: 8px; padding: clamp(24px,4vw,44px) 20px; text-align: center; }
.screen-title { font-family: 'Press Start 2P', monospace; color: #000; line-height: 1.8; }
.monitor-neck { width: 60px; height: 26px; background: #3BBFB8; border: 4px solid #1a1a1a; border-top: none; margin: 0 auto; }
```

### קופסת תוכן (לגוף המאמר)
```css
.content-box { background: #F5F0D0; border: 3px solid #2a2a2a; border-radius: 4px;
  padding: clamp(30px,5vw,60px) clamp(24px,5vw,70px); max-width: 900px; }
```

### עיגול-מבטא עם מספר פיקסלי (לכותרות סקשן / מספור)
```css
.one-circle { width: clamp(130px,18vw,210px); aspect-ratio: 1; background: #E84477;
  border-radius: 50%; border: 5px solid #000; display: grid; place-items: center; }
.pixel-num { font-family: 'Press Start 2P', monospace; font-size: clamp(38px,7vw,80px); color: #000; }
```

### רצועת צבע עליונה
```css
.yellow-band { position: absolute; top: 0; left: 0; right: 0; height: 38%; background: #F5C800; z-index: 0; }
```

(תבניות נוספות — מקלדת פיקסלית, כפתור START רטרו — בסקיל `binary-code-retro-style`.)

## מתכון ל-HTML טיפוסי של מאמר

1. רקע מהפלטה (שחור / טורקיז / קרם / צהוב).
2. כותרת המאמר ב-`Press Start 2P` בתוך מסך מוניטור, בצבע מבטא (צהוב על שחור / שחור על טורקיז).
3. גוף המאמר ב-`Libre Baskerville` בתוך קופסת קרם (`.content-box`), `line-height` נדיב.
4. כותרות-משנה כעיגולי-מבטא ממוספרים או על רקע צהוב.
5. 2–3 צורות דקורטיביות שיוצאות מקצוות העמוד.

## ✅ לעשות / ❌ לא לעשות

✅ צבעים שטוחים, מסגרות עבות, פיקסלים חדים, צורות גיאומטריות, ניגוד גבוה (שחור על צהוב, ורוד על שחור), פריסה מלאה במסך.

❌ אין gradients רכים, אין shadows מטושטשים (blur), אין skeuomorphism מודרני.
❌ אין סאנס-סריף מודרני (Inter, Helvetica) — רק Press Start 2P + Libre Baskerville.
❌ אין `border-radius` עגלגל בסגנון iOS — קצוות חדים או מעוגלים מתונים בלבד.

---

## טון הכתיבה (טקסט)

> _(להשלמה — מדריך זה מכסה כרגע את השפה הגרפית של תוצרי ה-HTML.
> כללי הניסוח, הטון והקול של הטקסט המשוכתב יתווספו בנפרד.)_
