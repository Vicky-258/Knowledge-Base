# 🐚 Tutorial Shell: Game Plan (Creator MVP First)

> Escape tutorial hell. Don't just watch code — **live it**.

---

## 🎯 Vision

**Tutorial Shell** is an interactive platform where tutorials are not videos but replayable coding sessions.  
We're starting with the **Creator MVP** to enable coders to record and export tutorials first.

---

## 👤 Target Users

### 1. Creator (Primary MVP Focus)
- Wants to record tutorials quickly
- Doesn't want to deal with video editing
- Needs exportable, replayable formats

### 2. Learner (Phase 2+)
- Wants to code alongside tutorials
- Needs ability to pause, branch, and retry
- Learns better by doing, not watching

---

## 🧱 MVP Objective: Creator Mode

Let creators:
- Code inside Monaco Editor
- Record code changes + cursor selections
- Timestamp actions
- Export as JSON (tutorial file)
- Replay their session locally (dev-only for now)

---

## 🔁 Creator Flow (MVP)

1. Creator opens Tutorial Shell
2. Clicks **"Start Recording"**
3. Codes inside the Monaco Editor
4. Clicks **"Stop Recording"**
5. Clicks **"Export Tutorial"** → downloads `.json` file of session

---

## 🔨 Core Features for Creator MVP

| Feature | Description |
|--------|-------------|
| 🧠 Monaco Editor | Main coding interface |
| ⏺️ Record Actions | Insertions, deletions, selections |
| 🕰️ Timestamped Events | Accurate playback later |
| 🎛️ Start/Stop Controls | Simple UX for creators |
| 💾 JSON Export | Save tutorial locally |
| 🗂️ LocalStorage (Optional) | Temp store for sessions |

---

## 🧰 Tech Stack

| Layer | Tool |
|-------|------|
| Framework | React + Vite |
| Styling | Tailwind CSS |
| Editor | Monaco Editor |
| State Management | Zustand |
| Storage | Local JSON export |
| Playback (Preview) | setTimeout / RAF + diff replayer |
| Hosting | Vercel (frontend-only for MVP) |

---

## 📦 JSON Output Format (Sample)

```json
[
  {
    "timestamp": 0,
    "action": "insertText",
    "text": "function hello() {",
    "from": [0, 0],
    "to": [0, 0]
  },
  {
    "timestamp": 1200,
    "action": "insertText",
    "text": "console.log('Hi');",
    "from": [1, 2],
    "to": [1, 2]
  }
]

```
    