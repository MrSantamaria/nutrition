---
name: validate-shopping-list
description: Interactively review and finalize this week's shopping list. Checks for leftovers from last week, asks about pantry stock, and lets you add extras. Outputs a clean list ready to hand to Claude in Chrome for DoorDash ordering.
when_to_use: When starting a new meal prep week and need to finalize the shopping list before ordering groceries
argument-hint: [week date, e.g. 2026-04-20]
user-invocable: true
---

# Validate Shopping List

You are helping the user finalize their weekly grocery shopping list before they order via DoorDash.

## Process

### Step 1: Find the shopping lists

- Read the **current week's** shopping list from `plans/`. If an argument was provided, use it to find the right file (e.g., `plans/week-{{ARGUMENTS}}-shopping.md`). Otherwise, find the most recent shopping list file.
- Read the **previous week's** shopping list for context on what was bought last time.
- Read the current week's plan to understand what's being prepped.

### Step 2: Identify carryover items

Compare this week's list against last week's. Identify items that:
- Are **pantry staples** likely still in stock (oils, spices, dry goods)
- Were bought last week and may have **partial quantities left** (e.g., a bag of rice, jar of olives, block of feta)
- Are **new this week** and definitely need to be purchased

### Step 3: Interactive review

Walk through the shopping list **category by category** using `AskUserQuestion`. For each category:

1. Show the user what's on the list for that category
2. For items that might be leftover, ask if they still have them
3. Ask if they want to add anything to that category

Use clear, concise questions. Group related items together rather than asking one-by-one.

**Example flow:**

> **Pantry Staples** — Last week you bought: olive oil, oregano, cumin, paprika, garlic powder, salt, pepper.
> This week's list has the same items. Do you still have all of these?
> - A) Yes, skip all pantry staples
> - B) I need to restock some — let me tell you which

> **Canned Goods** — This week needs: 2 cans chickpeas, 1 can crushed tomatoes, 1 can black beans.
> You bought 2 cans chickpeas last week. Any left over?
> - A) No, need fresh cans
> - B) I have some chickpeas left — only need 1 can
> - C) I have other leftovers too — let me specify

After categories, ask one final open-ended question:

> **Anything else?** Need to add anything not on the list? (snacks, drinks, coffee, household items, etc.)

### Step 4: Read preferences

Before generating the final list, read `preferences.md` from the repo root. Apply all ordering preferences (dislikes, pre-cut options, mild heat defaults, Chrome extension rules) to the final output.

### Step 5: Generate the final list

After the interactive review, output a **clean, finalized shopping list** in markdown format with:

- Only the items the user actually needs to buy
- Any additions they requested
- The Chrome extension ordering rules block (from `preferences.md`) included at the top so Claude in Chrome sees them immediately
- Keep the format simple — just a flat bulleted list grouped by store section, easy to paste

Format the final output inside a markdown code block so the user can copy it directly.

The final output should follow this structure:

```
# Ordering Instructions
[Chrome extension rules from preferences.md — no screenshots, same product for duplicates, validate cart before checkout]

# Shopping List — [Week]
[Items grouped by store section]
```

## Important

- Be conversational and quick — don't make this feel like a chore
- If the user says "looks good" or "skip" at any point, move on
- Default to including an item if unsure — better to have extra than to run short
- The final list should be **self-contained** — someone (or Claude in Chrome) should be able to order from it without needing any other context
