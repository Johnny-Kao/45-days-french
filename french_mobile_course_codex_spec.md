# 30-Day Mobile French Speaking Course｜Codex Build Spec

## 0. Project Summary

Build a mobile-first static web app for a 30-day French speaking course.

The course is designed for a busy adult beginner learning French from zero. The learning model is not textbook-first or grammar-first. It is task-based, audio-first, and mobile-first.

The learner should be able to open a webpage on a phone, choose a lesson, see a simple scene image, listen to French audio, repeat sentence by sentence, practice patterns, and complete a short output task.

The MVP should use **free macOS built-in Text-to-Speech** via the `say` command to generate audio files locally.

No backend is required for the MVP.

---

## 1. Product Goal

Create a lightweight mobile learning site that supports:

1. Daily task-based French lessons.
2. Image-assisted learning.
3. Audio-first learning.
4. Sentence-level replay.
5. Normal-speed audio.
6. Slow-speed audio.
7. Shadowing audio with pauses.
8. Local progress tracking.
9. Static deployment to GitHub Pages / Netlify / Cloudflare Pages / Vercel.

The target learner uses the course on a phone while commuting, walking, or taking short breaks.

---

## 2. Learning Design Principle

The course should not behave like a traditional grammar textbook.

The learning unit is:

> One day = one real-life task.

Examples:

- Day 1: Greet someone politely.
- Day 2: Introduce yourself.
- Day 3: Say where you are from and what languages you speak.
- Day 4: Talk about your work.
- Day 5: Express what you want.
- Day 6: Order coffee.
- Day 7: Say you do not understand.

Each lesson should include:

1. A real-life situation.
2. A very short dialogue.
3. Core sentence patterns.
4. Minimal pronunciation notes.
5. One speaking output task.

Avoid large vocabulary tables, long grammar explanations, and dense text.

---

## 3. MVP Scope

Build Day 1–3 only.

The app must be extendable to 30 days, but the MVP content only needs to include three lessons.

### MVP lessons

```text
Day 1｜Bonjour
打招呼、道謝與道歉

Day 2｜Je m’appelle...
自我介紹

Day 3｜Je viens de...
國籍、語言與居住地
```

Each lesson should include:

1. One cover image.
2. One short core dialogue.
3. Normal dialogue audio.
4. Slow dialogue audio.
5. Sentence-by-sentence audio.
6. Shadowing audio with pauses.
7. Pattern practice.
8. Pronunciation notes.
9. Output task.

---

## 4. Recommended File Structure

```text
french-mobile-course/
  index.html
  styles.css
  app.js
  README.md
  .env.example

  data/
    lessons.json

  assets/
    images/
      day01_cover.jpg
      day02_cover.jpg
      day03_cover.jpg

    audio/
      day01/
        sentence_01.mp3
        sentence_02.mp3
        sentence_03.mp3
        dialogue_normal.mp3
        dialogue_slow.mp3
        shadowing.mp3

      day02/
        ...

      day03/
        ...

  scripts/
    generate_audio.py
    requirements.txt
```

---

## 5. Frontend Requirements

### 5.1 General

Use plain HTML, CSS, and JavaScript.

No React, no Vue, no backend, no build step.

The app should load lesson data from:

```text
data/lessons.json
```

### 5.2 Mobile-first layout

The site must be optimized for phone screens.

Use:

```css
max-width: 480px;
margin: 0 auto;
```

Design style:

- Vertical layout.
- Large French text.
- Smaller Chinese explanation.
- Clear audio buttons.
- Card-based layout.
- High readability.
- Minimal visual noise.
- Warm off-white background.
- Rounded white cards.

### 5.3 Home screen

The home screen should show all lessons from `lessons.json`.

Each lesson card should display:

- Day number.
- French title.
- Chinese subtitle.
- Task summary.
- Completion status.
- Start button.

Example card:

```text
Day 1
Bonjour
打招呼、道謝與道歉
我能用法語完成最基本的禮貌互動。
[Start]
```

Completion status should be stored in `localStorage`.

### 5.4 Lesson screen

Clicking a lesson opens a lesson detail page.

Using hash routing is acceptable:

```text
#/lesson/day01
```

Lesson page sections:

1. Lesson Header
2. Listen First
3. Core Dialogue
4. Pattern Practice
5. Pronunciation Notes
6. Output Task
7. Mark as Done
8. Back to Lessons

---

## 6. Lesson Page Details

### 6.1 Lesson Header

Display:

- Day number.
- Title.
- Subtitle.
- Task.
- Cover image.

Example:

```text
Day 1
Bonjour
打招呼、道謝與道歉

Today's task:
我能用法語完成最基本的禮貌互動。
```

### 6.2 Listen First

Display three audio buttons:

```text
▶ Normal
▶ Slow
▶ Shadowing
```

Requirements:

- Clicking a button plays the corresponding audio.
- Only one audio file may play at a time.
- When another audio starts, stop the previous audio.
- Button state should show whether it is currently playing.
- Must work on mobile Safari and mobile Chrome.

### 6.3 Core Dialogue

Each dialogue sentence should appear as a card.

Each sentence card should display:

- Speaker label.
- French sentence.
- Chinese translation.
- Play button for sentence-level audio.

Example:

```text
A
Bonjour.
您好。
[▶ Play]
```

### 6.4 Pattern Practice

Display reusable sentence patterns.

Example:

```text
Bonjour, ____.
您好，____。

Examples:
- Bonjour, monsieur. / 您好，先生。
- Bonjour, madame. / 您好，女士。
```

### 6.5 Pronunciation Notes

Only show short and practical notes.

Avoid long linguistic explanations.

Example:

```text
Bonjour
bon 是鼻音，不要讀成 bonn。jour 的 j 接近英文 measure 裡的 s。
```

### 6.6 Output Task

Show a short speaking task.

Example:

```text
不用看稿，模擬你走進店裡：
1. 打招呼
2. 說不好意思
3. 道謝
4. 回應不客氣
```

No recording feature is required in MVP.

### 6.7 Mark as Done

A button at the bottom:

```text
Mark as Done
```

When clicked:

```js
localStorage.setItem("lesson_day01_done", "true")
```

The home screen should show the lesson as completed.

---

## 7. Audio Requirements

The MVP should generate audio locally using macOS built-in TTS.

Use:

```bash
say
```

The audio generation flow is:

```text
lesson text
→ macOS say command
→ AIFF file
→ ffmpeg conversion
→ MP3 file
```

### 7.1 Why macOS TTS

Use macOS TTS first because:

1. Free.
2. Local.
3. No API key required.
4. Works with Python.
5. Good enough for MVP.
6. Can later be replaced with OpenAI / Azure / ElevenLabs.

### 7.2 Required audio files per lesson

Each lesson should generate:

```text
assets/audio/dayXX/sentence_01.mp3
assets/audio/dayXX/sentence_02.mp3
assets/audio/dayXX/sentence_03.mp3
...

assets/audio/dayXX/dialogue_normal.mp3
assets/audio/dayXX/dialogue_slow.mp3
assets/audio/dayXX/shadowing.mp3
```

### 7.3 Audio speed

Use macOS `say -r` to control speech rate.

Default settings:

```text
Normal rate: 170
Slow rate: 120
Shadowing rate: 140
```

These values should be configurable through `.env`.

---

## 8. Python Audio Script Requirements

Create:

```text
scripts/generate_audio.py
```

The script must:

1. Read `data/lessons.json`.
2. Generate sentence audio for each dialogue sentence.
3. Generate `dialogue_normal.mp3`.
4. Generate `dialogue_slow.mp3`.
5. Generate `shadowing.mp3`.
6. Use macOS `say` command by default.
7. Convert AIFF to MP3 using `ffmpeg`.
8. Use `pydub` to concatenate shadowing audio.
9. Add 3 seconds of silence between shadowing sentences.
10. Skip existing files by default.
11. Regenerate files when `--force` is passed.
12. Support generating a single lesson.
13. Support generating all lessons.

### 8.1 CLI usage

```bash
python scripts/generate_audio.py --lesson day01
python scripts/generate_audio.py --lesson day01 --force
python scripts/generate_audio.py --all
python scripts/generate_audio.py --all --force
```

### 8.2 Environment variables

Create `.env.example`:

```env
TTS_PROVIDER=macos

MACOS_TTS_VOICE=Thomas
MACOS_TTS_RATE_NORMAL=170
MACOS_TTS_RATE_SLOW=120
MACOS_TTS_RATE_SHADOWING=140
MACOS_TTS_SHADOWING_PAUSE_MS=3000
```

### 8.3 List available macOS voices

README should instruct the user to run:

```bash
say -v '?'
```

Then choose an available French voice.

Possible French voices may include:

```text
Thomas
Amélie
Audrey
Aurelie
```

Exact voice availability depends on macOS version and installed voices.

### 8.4 Install high-quality French voices

README should mention:

```text
System Settings → Accessibility → Spoken Content → System Voice → Manage Voices
```

Download a French voice if not already installed.

### 8.5 macOS say command example

```bash
say -v Thomas -r 170 -o sentence_01.aiff "Bonjour."
```

Then convert:

```bash
ffmpeg -y -i sentence_01.aiff sentence_01.mp3
```

---

## 9. Python Script Implementation Notes

### 9.1 Generate one sentence

Use a helper function:

```python
def generate_tts_mac(text, output_mp3, voice="Thomas", rate=170, force=False):
    ...
```

Expected behavior:

1. Create parent directory if missing.
2. Skip if output exists and `force=False`.
3. Generate temporary `.aiff`.
4. Convert to `.mp3`.
5. Delete temporary `.aiff`.

### 9.2 Use subprocess

Use Python `subprocess.run`.

Example logic:

```python
subprocess.run([
    "say",
    "-v", voice,
    "-r", str(rate),
    "-o", str(temp_aiff),
    text
], check=True)

subprocess.run([
    "ffmpeg",
    "-y",
    "-i", str(temp_aiff),
    str(output_mp3)
], check=True)
```

### 9.3 Shadowing audio

Use `pydub`.

Example logic:

```python
from pydub import AudioSegment

combined = AudioSegment.empty()
pause = AudioSegment.silent(duration=pause_ms)

for sentence_file in sentence_files:
    audio = AudioSegment.from_mp3(sentence_file)
    combined += audio + pause

combined.export(output_file, format="mp3")
```

### 9.4 Dialogue audio

For `dialogue_normal.mp3` and `dialogue_slow.mp3`, generate from combined dialogue text.

Example:

```text
Bonjour.
Bonjour, monsieur.
Excusez-moi.
Oui ?
Merci beaucoup.
Je vous en prie.
```

Do not include speaker labels in the TTS text for MVP. Speaker labels may cause unnatural reading.

---

## 10. Data Model

Use `data/lessons.json`.

Example full structure:

```json
[
  {
    "id": "day01",
    "day": 1,
    "title": "Bonjour",
    "subtitle": "打招呼、道謝與道歉",
    "task": "我能用法語完成最基本的禮貌互動。",
    "coverImage": "assets/images/day01_cover.jpg",
    "audio": {
      "dialogueNormal": "assets/audio/day01/dialogue_normal.mp3",
      "dialogueSlow": "assets/audio/day01/dialogue_slow.mp3",
      "shadowing": "assets/audio/day01/shadowing.mp3"
    },
    "dialogue": [
      {
        "speaker": "A",
        "fr": "Bonjour.",
        "zh": "您好。",
        "audio": "assets/audio/day01/sentence_01.mp3"
      }
    ],
    "patterns": [
      {
        "pattern": "Bonjour, ____.",
        "meaning": "您好，____。",
        "examples": [
          {
            "fr": "Bonjour, monsieur.",
            "zh": "您好，先生。"
          }
        ]
      }
    ],
    "pronunciationNotes": [
      {
        "item": "Bonjour",
        "note": "bon 是鼻音，不要讀成 bonn。jour 的 j 接近英文 measure 裡的 s。"
      }
    ],
    "outputTask": {
      "title": "今日輸出",
      "instruction": "不用看稿，模擬你走進店裡：打招呼、說不好意思、道謝，最後說不客氣。"
    }
  }
]
```

---

## 11. Sample Lesson Data

Create `data/lessons.json` with the following MVP data.

```json
[
  {
    "id": "day01",
    "day": 1,
    "title": "Bonjour",
    "subtitle": "打招呼、道謝與道歉",
    "task": "我能用法語完成最基本的禮貌互動。",
    "coverImage": "assets/images/day01_cover.jpg",
    "audio": {
      "dialogueNormal": "assets/audio/day01/dialogue_normal.mp3",
      "dialogueSlow": "assets/audio/day01/dialogue_slow.mp3",
      "shadowing": "assets/audio/day01/shadowing.mp3"
    },
    "dialogue": [
      {
        "speaker": "A",
        "fr": "Bonjour.",
        "zh": "您好。",
        "audio": "assets/audio/day01/sentence_01.mp3"
      },
      {
        "speaker": "B",
        "fr": "Bonjour, monsieur.",
        "zh": "您好，先生。",
        "audio": "assets/audio/day01/sentence_02.mp3"
      },
      {
        "speaker": "A",
        "fr": "Excusez-moi.",
        "zh": "不好意思。",
        "audio": "assets/audio/day01/sentence_03.mp3"
      },
      {
        "speaker": "B",
        "fr": "Oui ?",
        "zh": "是的？",
        "audio": "assets/audio/day01/sentence_04.mp3"
      },
      {
        "speaker": "A",
        "fr": "Merci beaucoup.",
        "zh": "非常謝謝。",
        "audio": "assets/audio/day01/sentence_05.mp3"
      },
      {
        "speaker": "B",
        "fr": "Je vous en prie.",
        "zh": "不客氣。",
        "audio": "assets/audio/day01/sentence_06.mp3"
      }
    ],
    "patterns": [
      {
        "pattern": "Bonjour, ____.",
        "meaning": "您好，____。",
        "examples": [
          {
            "fr": "Bonjour, monsieur.",
            "zh": "您好，先生。"
          },
          {
            "fr": "Bonjour, madame.",
            "zh": "您好，女士。"
          }
        ]
      },
      {
        "pattern": "Merci ____.",
        "meaning": "謝謝 / 非常謝謝。",
        "examples": [
          {
            "fr": "Merci.",
            "zh": "謝謝。"
          },
          {
            "fr": "Merci beaucoup.",
            "zh": "非常謝謝。"
          }
        ]
      }
    ],
    "pronunciationNotes": [
      {
        "item": "Bonjour",
        "note": "bon 是鼻音，不要讀成 bonn。jour 的 j 接近英文 measure 裡的 s。"
      },
      {
        "item": "Merci",
        "note": "r 是法語小舌音，初期不用追求完美，但不要讀成英文 r。"
      }
    ],
    "outputTask": {
      "title": "今日輸出",
      "instruction": "不用看稿，模擬你走進店裡：打招呼、說不好意思、道謝，最後說不客氣。"
    }
  },
  {
    "id": "day02",
    "day": 2,
    "title": "Je m’appelle...",
    "subtitle": "自我介紹",
    "task": "我能用法語說出自己的名字，並進行最簡單的初次見面寒暄。",
    "coverImage": "assets/images/day02_cover.jpg",
    "audio": {
      "dialogueNormal": "assets/audio/day02/dialogue_normal.mp3",
      "dialogueSlow": "assets/audio/day02/dialogue_slow.mp3",
      "shadowing": "assets/audio/day02/shadowing.mp3"
    },
    "dialogue": [
      {
        "speaker": "A",
        "fr": "Bonjour, je m’appelle Johnny.",
        "zh": "您好，我叫 Johnny。",
        "audio": "assets/audio/day02/sentence_01.mp3"
      },
      {
        "speaker": "B",
        "fr": "Enchanté.",
        "zh": "幸會。",
        "audio": "assets/audio/day02/sentence_02.mp3"
      },
      {
        "speaker": "A",
        "fr": "Enchanté aussi.",
        "zh": "我也很高興認識您。",
        "audio": "assets/audio/day02/sentence_03.mp3"
      },
      {
        "speaker": "B",
        "fr": "Vous vous appelez comment ?",
        "zh": "您叫什麼名字？",
        "audio": "assets/audio/day02/sentence_04.mp3"
      },
      {
        "speaker": "A",
        "fr": "Je m’appelle Johnny.",
        "zh": "我叫 Johnny。",
        "audio": "assets/audio/day02/sentence_05.mp3"
      }
    ],
    "patterns": [
      {
        "pattern": "Je m’appelle ____.",
        "meaning": "我叫 ____。",
        "examples": [
          {
            "fr": "Je m’appelle Johnny.",
            "zh": "我叫 Johnny。"
          },
          {
            "fr": "Je m’appelle Marie.",
            "zh": "我叫 Marie。"
          }
        ]
      },
      {
        "pattern": "Vous vous appelez comment ?",
        "meaning": "您叫什麼名字？",
        "examples": [
          {
            "fr": "Vous vous appelez comment ?",
            "zh": "您叫什麼名字？"
          }
        ]
      }
    ],
    "pronunciationNotes": [
      {
        "item": "Je m’appelle",
        "note": "Je 的聲音很短，不要讀成英文 jay。m’appelle 中 app 的 p 較明顯。"
      },
      {
        "item": "Enchanté",
        "note": "en 是鼻音，末尾 é 是清楚的 /e/ 音。"
      }
    ],
    "outputTask": {
      "title": "今日輸出",
      "instruction": "不用看稿，說出：您好，我叫你的名字，很高興認識您。"
    }
  },
  {
    "id": "day03",
    "day": 3,
    "title": "Je viens de...",
    "subtitle": "國籍、語言與居住地",
    "task": "我能用法語說明自己來自哪裡、住在哪裡、會說什麼語言。",
    "coverImage": "assets/images/day03_cover.jpg",
    "audio": {
      "dialogueNormal": "assets/audio/day03/dialogue_normal.mp3",
      "dialogueSlow": "assets/audio/day03/dialogue_slow.mp3",
      "shadowing": "assets/audio/day03/shadowing.mp3"
    },
    "dialogue": [
      {
        "speaker": "A",
        "fr": "Vous venez d’où ?",
        "zh": "您來自哪裡？",
        "audio": "assets/audio/day03/sentence_01.mp3"
      },
      {
        "speaker": "B",
        "fr": "Je viens de Taïwan.",
        "zh": "我來自台灣。",
        "audio": "assets/audio/day03/sentence_02.mp3"
      },
      {
        "speaker": "A",
        "fr": "Vous habitez où ?",
        "zh": "您住在哪裡？",
        "audio": "assets/audio/day03/sentence_03.mp3"
      },
      {
        "speaker": "B",
        "fr": "J’habite à Tokyo.",
        "zh": "我住在東京。",
        "audio": "assets/audio/day03/sentence_04.mp3"
      },
      {
        "speaker": "A",
        "fr": "Vous parlez français ?",
        "zh": "您說法語嗎？",
        "audio": "assets/audio/day03/sentence_05.mp3"
      },
      {
        "speaker": "B",
        "fr": "Je parle un peu français.",
        "zh": "我會說一點法語。",
        "audio": "assets/audio/day03/sentence_06.mp3"
      }
    ],
    "patterns": [
      {
        "pattern": "Je viens de ____.",
        "meaning": "我來自 ____。",
        "examples": [
          {
            "fr": "Je viens de Taïwan.",
            "zh": "我來自台灣。"
          },
          {
            "fr": "Je viens du Japon.",
            "zh": "我來自日本。"
          }
        ]
      },
      {
        "pattern": "J’habite à ____.",
        "meaning": "我住在 ____。",
        "examples": [
          {
            "fr": "J’habite à Tokyo.",
            "zh": "我住在東京。"
          },
          {
            "fr": "J’habite à Paris.",
            "zh": "我住在巴黎。"
          }
        ]
      },
      {
        "pattern": "Je parle ____.",
        "meaning": "我會說 ____。",
        "examples": [
          {
            "fr": "Je parle chinois.",
            "zh": "我會說中文。"
          },
          {
            "fr": "Je parle anglais.",
            "zh": "我會說英文。"
          },
          {
            "fr": "Je parle un peu français.",
            "zh": "我會說一點法語。"
          }
        ]
      }
    ],
    "pronunciationNotes": [
      {
        "item": "d’où",
        "note": "où 是 /u/，接近中文的「烏」，不要讀成英文 ou。"
      },
      {
        "item": "J’habite",
        "note": "h 不發音，所以 J’habite 連在一起讀。"
      },
      {
        "item": "français",
        "note": "ç 發 /s/ 音，最後的 s 不發音。"
      }
    ],
    "outputTask": {
      "title": "今日輸出",
      "instruction": "不用看稿，說出：我來自哪裡、我住在哪裡、我會說哪些語言。"
    }
  }
]
```

---

## 12. Image Requirements

For MVP, use one image per lesson.

### Image specs

```text
Size: 1080 x 1350
Ratio: 4:5
Format: JPG
Style: realistic, clean, calm, everyday-life
No embedded text
```

### Placeholder files

Create placeholder image files or use simple neutral placeholders.

Expected paths:

```text
assets/images/day01_cover.jpg
assets/images/day02_cover.jpg
assets/images/day03_cover.jpg
```

### Image prompts

#### Day 1 image prompt

```text
A clean realistic vertical image for a beginner French lesson about polite greetings. A calm small Parisian shop entrance or café counter, warm daylight, no people close-up, no visible brand logos, simple everyday atmosphere, mobile learning card style, 4:5 aspect ratio, no text.
```

#### Day 2 image prompt

```text
A clean realistic vertical image for a beginner French lesson about self-introduction. Two adults meeting politely in a quiet café or professional lounge, soft daylight, natural body language, no close-up faces, no brand logos, calm and simple, mobile learning card style, 4:5 aspect ratio, no text.
```

#### Day 3 image prompt

```text
A clean realistic vertical image for a beginner French lesson about country, language, and where someone lives. A small table with a map, passport-like travel notebook, pen, and coffee cup, calm daylight, no readable text, no brand logos, mobile learning card style, 4:5 aspect ratio.
```

---

## 13. CSS Requirements

Use a simple mobile design.

Suggested CSS direction:

```css
:root {
  --bg: #f7f3ed;
  --card: #ffffff;
  --text: #1f1f1f;
  --muted: #666666;
  --border: #e8e1d8;
  --accent: #1f1f1f;
}

body {
  margin: 0;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  background: var(--bg);
  color: var(--text);
}

.app {
  max-width: 480px;
  margin: 0 auto;
  padding: 16px;
}

.card {
  background: var(--card);
  border-radius: 18px;
  padding: 18px;
  margin-bottom: 16px;
  box-shadow: 0 6px 20px rgba(0,0,0,0.06);
}

.cover {
  width: 100%;
  border-radius: 20px;
  object-fit: cover;
  margin-bottom: 16px;
}

.fr {
  font-size: 24px;
  font-weight: 650;
  line-height: 1.35;
}

.zh {
  font-size: 15px;
  color: var(--muted);
  margin-top: 6px;
}

.section-title {
  font-size: 18px;
  font-weight: 700;
  margin: 24px 0 12px;
}

button {
  border: none;
  border-radius: 999px;
  padding: 12px 16px;
  font-size: 16px;
  background: var(--accent);
  color: white;
  cursor: pointer;
}

button.secondary {
  background: #eeeeee;
  color: #111111;
}

.audio-row {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}

.lesson-card {
  cursor: pointer;
}

.done-badge {
  display: inline-block;
  padding: 4px 8px;
  border-radius: 999px;
  background: #e7f3e7;
  color: #1f6b35;
  font-size: 12px;
}
```

---

## 14. JavaScript Requirements

Create `app.js`.

### 14.1 Load lesson data

```js
fetch("data/lessons.json")
  .then(res => res.json())
  .then(data => {
    lessons = data;
    render();
  });
```

### 14.2 Routing

Use hash routing.

Examples:

```text
#/lessons
#/lesson/day01
```

Default route should show home screen.

### 14.3 Audio manager

Implement a simple global audio manager.

Requirements:

1. Store current audio object.
2. Stop current audio before playing a new one.
3. Reset button state when audio ends.

Pseudo:

```js
let currentAudio = null;
let currentButton = null;

function playAudio(src, button) {
  if (currentAudio) {
    currentAudio.pause();
    currentAudio.currentTime = 0;
  }

  if (currentButton) {
    currentButton.textContent = currentButton.dataset.label;
  }

  const audio = new Audio(src);
  currentAudio = audio;
  currentButton = button;

  button.textContent = "Playing...";

  audio.onended = () => {
    button.textContent = button.dataset.label;
    currentAudio = null;
    currentButton = null;
  };

  audio.play();
}
```

### 14.4 Progress tracking

Use:

```js
function isDone(id) {
  return localStorage.getItem(`lesson_${id}_done`) === "true";
}

function markDone(id) {
  localStorage.setItem(`lesson_${id}_done`, "true");
}
```

---

## 15. Requirements File

Create:

```text
scripts/requirements.txt
```

Content:

```txt
python-dotenv
pydub
```

No OpenAI SDK is needed for MVP.

---

## 16. README Requirements

Create `README.md`.

It should include:

### 16.1 Setup

```bash
cd french-mobile-course
python3 -m venv .venv
source .venv/bin/activate
pip install -r scripts/requirements.txt
brew install ffmpeg
```

### 16.2 Check macOS French voices

```bash
say -v '?'
```

If no good French voice is installed:

```text
System Settings → Accessibility → Spoken Content → System Voice → Manage Voices
```

Download a French voice.

### 16.3 Configure environment

```bash
cp .env.example .env
```

Edit:

```env
MACOS_TTS_VOICE=Thomas
```

### 16.4 Generate audio

```bash
python scripts/generate_audio.py --lesson day01
python scripts/generate_audio.py --all
```

### 16.5 Open locally

Because `fetch("data/lessons.json")` may not work from `file://`, start a local server:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

### 16.6 Deploy

This is a static app.

Can deploy to:

- GitHub Pages
- Netlify
- Cloudflare Pages
- Vercel

---

## 17. Acceptance Criteria

The MVP is complete when:

1. `index.html` opens a mobile-first course home page.
2. The home page lists Day 1–3 lessons.
3. Clicking a lesson opens the lesson detail page.
4. The lesson page shows cover image, task, dialogue, patterns, pronunciation notes, and output task.
5. Audio buttons exist for normal, slow, shadowing, and sentence-level playback.
6. Only one audio plays at once.
7. Progress is saved to `localStorage`.
8. `scripts/generate_audio.py --lesson day01` generates MP3 files for Day 1.
9. `scripts/generate_audio.py --all` generates MP3 files for all lessons in `lessons.json`.
10. Existing MP3 files are skipped unless `--force` is passed.
11. The README explains setup clearly.
12. The app can run locally using `python3 -m http.server 8000`.

---

## 18. Future Extensions

Do not implement these in MVP unless easy.

Possible future features:

1. Add recording and playback.
2. Add self-rating.
3. Add spaced repetition.
4. Add Anki export.
5. Add OpenAI / Azure / ElevenLabs TTS provider.
6. Add multiple voices for A/B dialogue.
7. Add auto-generated images.
8. Add offline PWA mode.
9. Add search.
10. Add 30 full lessons.
11. Add B1/B2 workplace French track.

---

## 19. Important Constraints

Do not overbuild.

For MVP:

- No backend.
- No database.
- No login.
- No cloud sync.
- No payment.
- No user accounts.
- No AI scoring.
- No grammar-heavy pages.
- No PDF export required.
- No React required.

The purpose is to create a working learning prototype fast.

Core priority:

```text
mobile webpage
+ lesson cards
+ image
+ correct audio files
+ sentence-level replay
+ shadowing
+ local progress
```

---

## 20. Codex Task Prompt

Copy this prompt into Codex:

```text
Build a mobile-first static web app for a task-based 30-day French speaking course.

Use plain HTML, CSS, and JavaScript. No backend. No build step.

The app should load lesson data from data/lessons.json and show a home page listing the lessons. Each lesson has a mobile-friendly detail page with:
- cover image
- day number
- title
- Chinese subtitle
- task
- normal dialogue audio
- slow dialogue audio
- shadowing audio
- sentence-level dialogue cards with play buttons
- pattern practice
- pronunciation notes
- output task
- mark as done button
- back to lessons button

Use HTML5 Audio. Only one audio file may play at once.

Store lesson completion in localStorage.

Create MVP data for Day 1–3 using data/lessons.json.

Create a Python script at scripts/generate_audio.py that uses macOS built-in `say` command to generate French audio files locally:
- sentence_01.mp3, sentence_02.mp3, etc.
- dialogue_normal.mp3
- dialogue_slow.mp3
- shadowing.mp3

The script should:
- read data/lessons.json
- use .env settings
- use macOS `say` to generate temporary AIFF files
- use ffmpeg to convert AIFF to MP3
- use pydub to concatenate sentence files with silence for shadowing audio
- support --lesson day01
- support --all
- support --force
- skip existing files unless --force is passed

Create:
- index.html
- styles.css
- app.js
- data/lessons.json
- scripts/generate_audio.py
- scripts/requirements.txt
- .env.example
- README.md

Keep the code simple, readable, and easy to extend.
```

---

## 21. Recommended Execution Order

Implement in this order:

1. Create file structure.
2. Create `data/lessons.json` with Day 1–3.
3. Build `index.html`, `styles.css`, `app.js`.
4. Add placeholder images.
5. Build static lesson navigation.
6. Add audio playback manager.
7. Add localStorage completion.
8. Create `.env.example`.
9. Create `scripts/requirements.txt`.
10. Create `scripts/generate_audio.py`.
11. Test Day 1 audio generation.
12. Test all audio generation.
13. Run local server.
14. Test on desktop browser.
15. Test on iPhone browser.
16. Deploy static app.

---

## 22. Testing Checklist

### Local web test

```bash
python3 -m http.server 8000
```

Open:

```text
http://localhost:8000
```

Check:

- Home page loads.
- Day 1–3 appear.
- Lesson page opens.
- Back button works.
- Mark as Done works.
- Refresh keeps completion status.

### Audio generation test

```bash
python scripts/generate_audio.py --lesson day01 --force
```

Check generated files:

```text
assets/audio/day01/sentence_01.mp3
assets/audio/day01/dialogue_normal.mp3
assets/audio/day01/dialogue_slow.mp3
assets/audio/day01/shadowing.mp3
```

### Audio playback test

Check:

- Normal audio plays.
- Slow audio plays.
- Shadowing audio plays.
- Sentence audio plays.
- Starting a new audio stops the previous one.

### Mobile test

Open from phone.

Check:

- Text size readable.
- Buttons large enough.
- Audio can play after tap.
- Layout does not overflow.
- Cover image fits screen.
- Card spacing is comfortable.

---

## 23. Notes on French Audio Quality

macOS TTS is acceptable for MVP but not perfect.

The main limitation:

- Voice may sound slightly mechanical.
- Dialogue may not feel like natural human conversation.
- French rhythm may be less natural than cloud TTS.

This is acceptable for the first version.

If the learning flow works, later replace or supplement with:

- Azure TTS French voices.
- OpenAI TTS.
- ElevenLabs.
- Native speaker recordings.

Do not optimize audio quality before validating the learning format.

---

## 24. Final MVP Definition

The MVP is successful if the user can:

1. Open the course on a phone.
2. Pick Day 1.
3. See the scene image.
4. Listen to normal dialogue.
5. Listen to slow dialogue.
6. Tap each sentence and repeat.
7. Listen to shadowing audio.
8. Review patterns.
9. Complete a short speaking task.
10. Mark the lesson done.

That is the minimum useful product.
