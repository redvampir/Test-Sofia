## 🧠 Codex Task: Auto Context Restoration in GPT Plugin (v2)

### 📌 Goal
Update the GPT plugin so that it can **automatically detect and restore lost context** more accurately and with safer conditions, including debug/testing mode triggers.

---

### 🔧 Function to Implement (Core logic)

```js
async function restoreContext(debug = false) {
  const index = await readFile("memory/index.json");
  const planPath = index?.plan;
  const profilePath = index?.profile;
  const currentLessonPath = index?.currentLesson;

  const plan = planPath ? await readFile(planPath) : null;
  const profile = profilePath ? await readFile(profilePath) : null;
  const lesson = currentLessonPath ? await readFile(currentLessonPath) : null;

  if (debug) {
    console.log("[DEBUG] Restored context:", { planPath, profilePath, currentLessonPath });
  }

  return { plan, profile, currentLesson: lesson };
}
```

---

### 🧪 Debug / Testing Mode Enhancements
- Add optional `debug = true` parameter to log restoration steps.
- Log warnings if any files are missing or empty.
- If in test mode, prompt user before restoring: "Context may be missing. Attempt to restore?"

---

### ⚡ Expanded Triggers for Auto-Restoration

#### ✅ Safe Triggers
1. GPT outputs: "What lesson?", "Who are you?", or other signs of amnesia.
2. `index.json`, `plan.md`, or `profile.md` are `null`, empty, or failed to load.
3. A new session (first few turns).
4. A user message such as: "Restore context", "Did you forget?", or "Load our plan."
5. Internal GPT session reaches 3000+ tokens since last memory read.
6. GPT finishes a large code or lesson and loses awareness of last files.

#### 🧪 Debugging Triggers (Only in Test Mode)
7. After memory read/write fails.
8. After user uploads a new index or plan file.
9. If user types `!debug-restore` or similar command.

---

### 📜 Instruction.md Suggestion
```
11. GPT must call `restoreContext()` when context is lost (as defined by triggers). If in testing mode, confirm with user.
```

---

### ✅ Acceptance Criteria
- Auto-restores context only when one or more safe triggers are active.
- Supports debug/test mode with console logging and optional prompts.
- Avoids unnecessary or repeated context fetches.
- Stable integration with `readFile()` and user-specific GitHub repo/token.
- Designed for expandability (e.g., use with dynamic memory models).