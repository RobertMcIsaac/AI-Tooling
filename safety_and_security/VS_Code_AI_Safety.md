# VS Code AI Safety  
**Guidance for Protecting Company Code from AI Tool Access**

---

## 1. Purpose

The purpose of this document is to reduce the risk of company source code, configuration, and intellectual property being accessed by third-party AI code assistants running inside local development environments (e.g. VS Code).

Examples of AI tools this applies to include (but are not limited to):

- GitHub Copilot  
- ChatGPT / OpenAI IDE extensions  
- Any AI-powered code assistant or autocomplete extension  

Although our primary codebase is hosted on **Bitbucket**, most AI IDE tools operate by reading files from the **local VS Code environment**, not directly from GitHub or Bitbucket.  
This means that if an AI extension is installed locally, it may technically be able to access company code regardless of where the repo is hosted.

This document defines recommendations for **setup and controls** for using VS Code safely on work machines.

---

## 2. Recommendation Summary

To mitigate risk, developers can:

1. Use a dedicated **VS Code Work Profile** with AI tools removed.  
2. Ensure AI extensions (e.g. Copilot, ChatGPT) are **not installed or active** in that profile.  
3. Prevent VS Code Settings Sync from reinstalling AI extensions.  
4. Add enforced safeguards to `settings.json`.  
5. Apply repo-level protections on sensitive codebases.  
---

## 3. Recommended Procedure

### 3.1 Create a Dedicated VS Code Work Profile

Developers can create and use a dedicated work profile.

**Steps:**

1. Open VS Code.  
2. Click ⚙️ (bottom-left corner).  
3. Select **Profiles → Create Profile…**  
4. Name it something like:
```Work (No AI)```
5. When prompted, **disable copying** of:
   - Extensions  
   - Settings  
   - Snippets  

This ensures a clean separation from any personal or AI-enabled setup.

---

### 3.2 Disable Extension Sync

This prevents GitHub Copilot or other AI tools from being silently reinstalled through Settings Sync.

Inside the **Work profile**:

1. Go to **⚙️ → Settings Sync…**  
2. Choose one of the following:
- Disable Settings Sync entirely  
- OR keep Sync enabled but **disable extension syncing**

---

### 3.3 Remove and Block AI Extensions

In the **Work profile**:

1. Open the Extensions panel (`Ctrl + Shift + X`).  
2. Search for and **uninstall** any of the following if present:
- `GitHub Copilot`  
- `GitHub Copilot Chat`  
- `OpenAI` / `ChatGPT`  
- Any other AI coding assistant  

---

### 3.4 Add Hard Security Controls in `settings.json`

Open:
```bash
Ctrl + Shift + P → Preferences: Open Settings (JSON)
```

Add or merge the following configuration:

```json
{
  // Disable Copilot functionality entirely
  "github.copilot.enable": {
    "*": false
  },

  // Explicitly ignore AI extensions during Settings Sync
  "settingsSync.ignoredExtensions": [
    "GitHub.copilot",
    "GitHub.copilot-chat",
    "openai.chatgpt"
  ],

  // Prevent VS Code from suggesting AI extensions
  "extensions.ignoreRecommendations": true,

  // Reduce experimental or remote features
  "workbench.enableExperiments": false
}
```

### Why this matters
These settings ensure that:
 - Even if Copilot exists on a personal machine, it will **not sync to the work profile**.
 - Even if someone tries to install it, it will remain **disabled**.
 - Even if VS Code tries to suggest it, it will **not be promoted or recommended**.

---


## 4. Repository-Level Safeguards (Recommended for Sensitive Projects)

For sensitive or high-risk repositories, an additional repository-level guard is recommended.

Create or update:
`<repo-root>/.vscode/settings.json`
With:
```json
{
  "github.copilot.enable": {
    "*": false
  }
}
```

This ensures Copilot is disabled even if a developer opens the repo on a personal machine.

This is a defence-in-depth measure, especially useful for sensitive codebases.

⸻

## 5. GitHub Usage on Work Machines

If team members use their personal GitHub accounts on work devices:

They should:

✅ Only use GitHub within the Work (No AI) VS Code profile
❌ Never install AI extensions in that profile

Logging into GitHub alone should not expose code to AI.
The risk comes ***specifically from AI extensions***.

⸻

## 6. Why This Matters (Risk Overview)

Without these protections, AI tools may:
 - Transmit proprietary logic patterns externally
 - Analyse internal system architecture
 - Expose sensitive algorithms or IP
 - Create potential licensing, confidentiality or compliance issues

This policy focuses specifically on preventing local AI code access within VS Code, which is one of the most realistic risk vectors.

⸻

# Summary Checklist

✅ Use a Work VS Code profile\
✅ Disable or restrict Settings Sync\
✅ Uninstall all AI code assistants\
✅ Add settings.json security configuration\
✅ Apply repo-level safeguards where needed\
✅ Never install AI tools in the work profile

