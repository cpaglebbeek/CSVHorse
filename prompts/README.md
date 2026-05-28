# prompts/ — CSVHorse sessie-MD's

Per sessie één markdown-bestand met:
- Frontmatter (`date`, `repo`, `status: open|pending|done`, `resume: "<trigger-zin>"`)
- De originele prompts (vraag van gebruiker)
- Genomen acties en gemaakte keuzes
- Eventuele cliffhanger / open punten

**Naamconventie:** `YYYY-MM-DD_<korte_slug>.md`.

**Template:** zie `Meta_Master/templates/PROMPT_SESSION_TEMPLATE.md`.

**Regenereren resume-register:** na elke nieuwe sessie-MD met `status: open|pending`:
```bash
python3 /Users/christian/Documents/Gemini_Projects/Meta_Master/tools/update_resume.py
```
