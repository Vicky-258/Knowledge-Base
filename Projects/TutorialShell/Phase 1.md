# 🛠️ Phase 1: Creator MVP (Local Only)

> Focus: Build a local-only prototype that lets users edit code, track changes over time, and export timelines for playback or future use.

---

## 📦 Features

### ✅ 1. Setup Monaco Editor
- Integrate Monaco Editor into the app
- Basic syntax highlighting
- Dark mode-friendly theme
- Language: JavaScript / TypeScript (for now)

### ✅ 2. Record Edits with Timestamps
- Hook into editor’s `onChange`
- Store changes with timestamp
- Format: `{ timestamp: number, code: string }`

### ✅ 3. Store as Timeline
- Maintain an in-memory array of snapshots
- Use a state manager like Zustand or Context API
- Debounce changes to avoid excessive recordings

### ✅ 4. Create Controls
- `Start Recording` → begins tracking changes
- `Stop Recording` → pauses tracking
- `Export Timeline` → downloads JSON file

### ✅ 5. Export as JSON
- Export the timeline array into a `.json` file
- Include metadata like `createdAt`, `duration`, `language`, etc.

---

## 🧪 Optional (Phase 1.5)
- Add simple playback mode (read-only, step-through timeline)
- Add code formatting toggle (Prettier)

---

## 📁 Folder Suggestion
 ```yaml
 src/  
├── components/  
│ ├── Editor.tsx  
│ ├── Controls.tsx  
│ └── TimelineExport.tsx  
├── stores/  
│ └── timeline.ts  
├── utils/  
│ └── debounce.ts  
├── App.tsx  
└── main.tsx
```


---

> This MVP is purely local — no backend, no auth, no network. Just buttery fast offline-first prototyping for creators.
