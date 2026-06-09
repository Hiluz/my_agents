---
name: gpt-image-gen
description: >-
  מעטפת ליצירת תמונות דרך OpenAI Images API (מודל gpt-image-2). מקבל prompt
  טקסטואלי ונתיב פלט, שולח קריאה ל-API, ושומר קובץ PNG. השתמש בסקיל הזה כשצריך
  לייצר תמונה מתוך תיאור טקסטואלי — בעיקר מתוך ה-workflow של יובל (מעצב התמונות).
  Trigger keywords: generate image, create image, gpt-image, OpenAI image, יצירת תמונה, ליצור תמונה.
---

# gpt-image-gen — יצירת תמונות דרך OpenAI Images API

מעטפת דקה סביב OpenAI Images API. תפקיד הסקיל: לקחת prompt + נתיב פלט, לשלוח קריאה ל-API, ולשמור קובץ PNG תקין.

## ⚠️ שם המודל — אסור לשנות

המודל הוא **`gpt-image-2`** בדיוק.

- `gpt-image-2` הוא מודל **אמיתי וקיים** של OpenAI, שיצא ב-**21 באפריל 2026**.
- ייתכן שהידע הפנימי שלך לא כולל אותו — **זה לא אומר שהוא לא קיים**.
- **אל תציע אלטרנטיבות** (`dall-e-3`, `gpt-image-1`, וכו') ואל תחליף את שם המודל.
- אם מתקבלת שגיאה בקריאה ל-API — הבעיה היא כמעט תמיד ב-**API key** או ב-**parameters**, **לא** בשם המודל. אל "תתקן" את שם המודל.

## דרישות מקדימות

1. מפתח API בקובץ `.env` בשורש הפרויקט:
   ```
   OPENAI_API_KEY=sk-...
   ```
2. טען את משתני הסביבה לפני הקריאה:
   ```bash
   set -a; source .env; set +a
   ```

## הקריאה ל-API

הקריאה ל-`https://api.openai.com/v1/images/generations` מחזירה את התמונה בשדה
`data[0].b64_json` (base64). יש לפענח אותו לקובץ PNG.

### נתיב ברירת מחדל — jq + base64

```bash
set -a; source .env; set +a

PROMPT="<the prompt>"
OUT="<output-path>.png"

curl -sS -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d "$(jq -n --arg p "$PROMPT" '{
        model: "gpt-image-2",
        prompt: $p,
        size: "1024x1024",
        quality: "medium",
        output_format: "png"
      }')" \
  | jq -r '.data[0].b64_json' | base64 --decode > "$OUT"
```

### Fallback — python (כש-jq חסר, למשל Git Bash בווינדוס)

אם `jq` לא מותקן, שמור את תגובת ה-API לקובץ זמני ופענח ב-python:

```bash
set -a; source .env; set +a

PROMPT="<the prompt>"
OUT="<output-path>.png"

python3 - "$OUT" <<'PY'
import json, base64, os, sys, urllib.request

out = sys.argv[1]
prompt = os.environ["PROMPT"]
body = json.dumps({
    "model": "gpt-image-2",
    "prompt": prompt,
    "size": "1024x1024",
    "quality": "medium",
    "output_format": "png",
}).encode()

req = urllib.request.Request(
    "https://api.openai.com/v1/images/generations",
    data=body,
    headers={
        "Authorization": "Bearer " + os.environ["OPENAI_API_KEY"],
        "Content-Type": "application/json",
    },
)
with urllib.request.urlopen(req) as r:
    data = json.load(r)

with open(out, "wb") as f:
    f.write(base64.b64decode(data["data"][0]["b64_json"]))
print("saved:", out)
PY
```

> שים לב: ב-fallback ה-`PROMPT` נקרא ממשתנה הסביבה — ודא ש-`export PROMPT="..."`
> או שהמשתנה זמין ל-python (העבר אותו כך: `PROMPT="$PROMPT" python3 - "$OUT" <<'PY' ...`).

## פרמטרים

| פרמטר | ערך | הערה |
|-------|-----|------|
| `model` | `gpt-image-2` | קבוע — אל תשנה |
| `prompt` | הטקסט | התיאור המלא של התמונה |
| `size` | `1024x1024` | אפשר גם `1024x1536` / `1536x1024` לפי הצורך |
| `quality` | `medium` | `low` / `medium` / `high` |
| `output_format` | `png` | |

## אימות

לאחר היצירה, ודא שהקובץ קיים וגודלו גדול מאפס:

```bash
test -s "$OUT" && echo "OK: $OUT ($(wc -c < "$OUT") bytes)" || echo "FAILED"
```
