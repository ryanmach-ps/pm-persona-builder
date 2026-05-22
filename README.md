# PetScreening Claude Persona Builder

A single-page HTML tool that helps PetScreening product managers generate a personalized context prompt to paste into Claude (or the internal **Ask PetScreening** project). It bakes in shared company context automatically and lets each PM layer in their own role, focus, and working-style preferences.

---

## What it does

1. **Collects personal context** — name, role, pod, PM experience, product area ownership, current OKRs, active initiatives, key metric, open challenges, stakeholders, and cross-pod dependencies.
2. **Generates a ready-to-paste prompt** — combines a hard-coded shared PetScreening context block with the PM's personal details into a single formatted prompt.
3. **Copies to clipboard** — one click copies the full output so it can be pasted directly into Claude or saved as a Project instruction.

---

## File structure

```
persona-builder.html   # Single self-contained HTML file — no build step, no dependencies
```

All CSS, JavaScript, and content live in one file. The only external dependencies are two Google Fonts loaded from CDN (`DM Sans`, `DM Mono`).

---

## How to use

### Option A — Open locally
```bash
open persona-builder.html
```
No server required. Works from the filesystem in any modern browser.

### Option B — Host internally
Drop the file anywhere that can serve static HTML — an S3 bucket, a Confluence page via iframe, an internal nginx server, Vercel, etc.
Currently hosted at https://ryanmach-ps.github.io/pm-persona-builder/
---

## Using the generated prompt

Once you click **Generate my Claude prompt** and copy the output, you have two options:

| Where | How |
|---|---|
| **One-off chat** | Open a new Claude chat → paste the prompt at the top of your first message |
| **Persistent (recommended)** | Go to your **Ask PetScreening** project → **Project Settings → Instructions** → paste the prompt there so it applies to every conversation automatically |

Update Section 2 (Current Focus) each quarter when your OKRs roll over.

---

## Shared context block

The tool hard-codes a `SHARED_CONTEXT` block in the JavaScript that covers:

- Company overview and customer segments (SPs, Consumers, AA applicants)
- All four pods: **LTR**, **STR**, **FidoAlerts**, **ESA Registration**
- Tool stack: Jira, Confluence, Amplitude, Figma, FlagSmith, Datadog, HubSpot, Slack, Snowflake
- Team ways of working: OKR-driven quarters, agile sprints, data-informed decisions, cross-pod dependencies

**PMs do not need to fill this in themselves** — it is included in every generated prompt automatically.

> ⚠️ If company context changes (new tools, new pods, updated ways of working), edit the `SHARED_CONTEXT` constant at the top of the `<script>` block in the HTML file.

---

## Customising the tool

### Adding a new pod to the dropdown
In the `<select id="pod">` element, add a new `<option>`:
```html
<option>New Pod Name</option>
```

### Adding a new style chip
In the **Response style** chip group, add a pair:
```html
<input type="checkbox" class="chip-option" id="s-yourkey" name="style" value="The instruction Claude will receive.">
<label class="chip-label" for="s-yourkey">Display label</label>
```
The `value` attribute is what gets written verbatim into the generated prompt.

### Updating the shared context block
Find `const SHARED_CONTEXT = \`...\`` in the `<script>` tag and edit the markdown text inside the template literal. Changes apply immediately on next page load.

### Hosting in the PetScreening Confluence
Confluence supports embedding HTML via the **HTML macro** (if enabled by your admin). Paste the full file contents into the macro. Note that Google Fonts CDN calls may be blocked depending on your network policy — if so, swap the fonts for a system stack:
```css
font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
```

---

## Browser support

Works in all evergreen browsers (Chrome, Firefox, Safari, Edge). Uses `navigator.clipboard.writeText` for the copy button — this requires either HTTPS or `localhost`; it will silently fail on plain HTTP.

---

## Design tokens (CSS variables)

| Variable | Value | Usage |
|---|---|---|
| `--ps-blue` | `#1A5FA8` | Primary brand blue |
| `--ps-blue-dark` | `#134a85` | Hover state |
| `--ps-blue-light` | `#EBF3FC` | Info notice background |
| `--accent` | `#F0A500` | Reserved for future use |
| `--radius` | `6px` | Standard border radius |
| `--radius-lg` | `10px` | Card / section radius |

---

## Maintainers

Built for the PetScreening Product Team. To request changes, open a Jira ticket in the relevant pod or drop a message in the product Slack channel.
