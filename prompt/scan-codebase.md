You are a senior software engineer and technical writer.
Your job is to analyze this codebase and produce complete,
beginner-friendly documentation for a junior developer
who is new to this project.

The developer understands general programming concepts but
is NOT familiar with the specific patterns, conventions,
or business logic used here. Explain things clearly —
avoid assuming prior knowledge of this codebase.

---

## YOUR TASK

Analyze the entire codebase provided and generate
documentation covering ALL of the following sections,
in this exact order:

---

### 1. PROJECT OVERVIEW
- What does this project do? (in plain language)
- Who uses it and what problem does it solve?
- What is the overall tech stack?

---

### 2. ARCHITECTURE & BIG PICTURE
- How are the different parts of this system structured?
- How do they communicate with each other?
  (e.g. REST API, WebSocket, shared DB, message queue)
- Draw a simple ASCII or text-based architecture diagram
  showing the main components and their relationships.

---

### 3. HOW TO SETUP & RUN THE PROJECT
- List all prerequisites (languages, runtimes, tools,
  env variables)
- Step-by-step instructions to get the project running
  locally from scratch
- Common setup issues and how to fix them
  (if detectable from the code)

---

### 4. FOLDER & FILE STRUCTURE
- Explain the purpose of every top-level folder
- Highlight the most important files a new developer
  should know first and WHY they matter
- If there are multiple apps/services, cover each one

---

### 5. DATABASE & MODEL RELATIONSHIPS
- List all database models/entities
- For each model, describe:
  - What it represents (in business terms)
  - Its key fields
  - Its relationships to other models
    (one-to-many, many-to-many, etc.)
- Include a simple text-based ERD if possible

---

### 6. API CONTRACT & ENDPOINT LIST
- List every API endpoint (or the most critical ones
  if there are too many)
- For each endpoint include:
  - Method + URL (e.g. POST /api/v1/login)
  - What it does (in plain language)
  - Request payload (key fields)
  - Response structure (key fields)
  - Auth required? (yes/no)

---

### 7. KEY FEATURE FLOWS (END-TO-END)
- Identify the 3-5 most important features of this app
- For each feature, trace the complete flow:
  - What triggers it (user action / scheduled job / etc.)
  - Which files/functions are involved, in order
  - What data is read or written
  - What the final output/response is
- Write this as a numbered step-by-step narrative,
  not just a list of files

---

### 8. THINGS A NEW DEVELOPER SHOULD KNOW
- Non-obvious conventions or patterns used in this code
- Common pitfalls or things easy to break by mistake
- Any parts of the codebase that are messy, legacy,
  or need extra caution
- Recommended order of files to read when onboarding

---

## OUTPUT FORMAT RULES
- Use clear headings and subheadings
- Use bullet points and numbered lists where appropriate
- Use code blocks for file paths, function names,
  and code snippets
- For anything technical, add a plain-language
  explanation in parentheses right after
- Be thorough. If you're unsure about something,
  say so explicitly rather than guessing silently

---

Now analyze the codebase and generate the full
documentation following the structure above.