## 🧠 Codex Task: Auto Context Restoration in GPT Plugin

### 📌 Goal
Implement a function that allows GPT to **automatically detect and restore lost context** by reading key memory files from the repo (such as `index.json`, `plan.md`, `profile.md`, etc.).

---

### 🔧 Function to Implement

```js
async function restoreContext() {
  // 1. Read index.json
  const index = await readFile("memory/index.json");

  // 2. From index, extract paths to plan, profile, last lesson
  const planPath = index["plan"];
  const profilePath = index["profile"];
  const currentLessonPath = index["currentLesson"];

  // 3. Attempt to read each file
  const plan = planPath ? await readFile(planPath) : null;
  const profile = profilePath ? await readFile(profilePath) : null;
  const lesson = currentLessonPath ? await readFile(currentLessonPath) : null;

  // 4. Return a context object
  return {
    plan,
    profile,
    currentLesson: lesson
  };
}
```

---

### 📂 Required Files

- `index.json` should contain:
```json
{
  "plan": "memory/plan.md",
  "profile": "memory/profile.md",
  "currentLesson": "memory/lessons/04_example.md"
}
```

- `readFile(path)` should be a helper that fetches from GitHub based on user's token and repo.

---

### 📜 Instruction.md Enhancement (Optional)

```md
11. If context is incomplete, GPT automatically calls `restoreContext()` to reload plan, profile, and lesson from memory.
```

---

### ✅ Acceptance Criteria
- GPT auto-restores context if critical memory is missing.
- Errors are handled (e.g., missing file).
- Works with any valid GitHub memory structure.