# The Eating Guide

A mix-and-match eating system for lowering cholesterol and weight — served as a
static site with no build step. All of the content lives in four JSON files, so
editing the guide never means touching HTML.

## Publish it (one time, about two minutes)

1. Create a new **public** repository on GitHub (for example `meal-guide`).
2. Upload everything in this folder — on the new repo's page choose
   **uploading an existing file** and drag the contents in (keep the `data/`
   folder structure), or push with git:

   ```
   git remote add origin https://github.com/YOUR-USERNAME/meal-guide.git
   git push -u origin main
   ```

3. In the repo: **Settings → Pages → Source: Deploy from a branch →
   Branch: main, folder: / (root) → Save.**
4. A minute later the site is live at
   `https://YOUR-USERNAME.github.io/meal-guide/`.

Every edit you make to the JSON files (directly on github.com is fine — open
the file, click the pencil, commit) republishes the site automatically within
a minute or two.

## Editing the content

| File | What's in it |
|---|---|
| `data/meals.json` | Daily targets, cuisine filter list, and every meal card |
| `data/batch.json` | The Sunday batch-cook rotations and the batch rules |
| `data/swaps.json` | The instead-of / use-this swap table |
| `data/shopping.json` | The standing shopping list, by category |

### Adding a meal

Add an object to the `meals` array in `data/meals.json`:

```json
{
  "slot": "dinner",
  "title": "Grilled shrimp skewers",
  "cuisine": "med",
  "cuisineLabel": "Mediterranean",
  "kcal": 650,
  "protein": 44,
  "ldlFighter": true,
  "desc": "6 oz shrimp, cherry tomatoes, and zucchini on skewers, farro, lemon-olive oil drizzle."
}
```

`slot` is one of `breakfast`, `lunch`, `dinner`, `snack`. `cuisine` must match
an `id` in the `cuisines` list at the top of the file (or use any id and set
`cuisineLabel` to `"Any"` to show it under every filter). `ldlFighter` marks
the ★ badge for meals heavy in soluble fiber or omega-3s.

### Adding a recipe to a meal

Add an optional `recipe` object to any meal and the card gets a click-to-expand
recipe (the meal title and a "Recipe" link both toggle it):

```json
"recipe": {
  "serves": "Serves 1",
  "ingredients": ["6 oz shrimp", "1 tbsp olive oil"],
  "method": ["Sear the shrimp.", "Plate it."]
}
```

All three fields are optional — `serves` is a short line above the lists,
`ingredients` renders as a bulleted list, `method` as numbered steps. Omit the
`recipe` object entirely for assembly snacks where the description already says
it all (the card then shows no recipe link).

Meal sizing convention: breakfasts ~425 cal, lunches ~575, dinners ~675,
snacks ~175, so that one pick per slot plus two snacks lands the day at the
calorie target without counting.

### Adding a batch-cook rotation

Add an object to `rotations` in `data/batch.json` with a unique `id`, a
`name`, an `outputs` list (`item` + `detail`), a `steps` list (`time` +
`step`), and a `weekPlan` paragraph. The rotation switcher builds itself from
whatever is in the file.

### Changing the daily targets

Edit `targets` in `data/meals.json` — `calories` and `protein` drive the tally
bar, and `skeleton` drives the per-slot sizing shown in the headings.

## Notes

- Day-builder picks, batch-cook checkoffs, and shopping-list checks are saved
  in the browser (localStorage), per device. "Reset day" and the clear buttons
  wipe them.
- To preview locally, run `python3 -m http.server` in this folder and open
  `http://localhost:8000` — opening `index.html` straight from disk won't load
  the data files (browsers block local fetch).
- This is nutrition guidance, not medical advice. Anyone borrowing the plan
  should keep their own physician in the loop.
