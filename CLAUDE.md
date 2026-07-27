# Nutrition Meal Prep System

Personal Mediterranean-style bowl meal prep system for Diego.

## Constraints
- 2 meals per day (14 bowls/week)
- Max 10 prep containers per week
- Simple cooking only — basics + rice cooker, no advanced techniques
- Mediterranean diet focus (olive oil, whole grains, legumes, veggies, lean proteins, fish)
- Beginner-friendly: minimal knife work, forgiving recipes

## System Design
- **Modular bowls**: each bowl is assembled from 3-4 containers
- **Container categories**: Base, Protein, Veggie, Flavor Booster
- **Prep day**: one batch-cook session per week (~1-2 hours)
- Plans live in `plans/` as weekly markdown files
- Container recipes live in `containers/` organized by category
- Complete meal recipes (burritos, crockpot meals) live in `containers/meals/`
- `recipes/index.md` — master recipe index with per-serving macros (protein, carbs, fat) for all tracked recipes; update when adding new recipes
- `preferences.md` in repo root — ordering/prep preferences (dislikes, pre-cut options). Always check this when building shopping lists.
- Run `/validate-shopping-list` before ordering to check leftovers with the user
