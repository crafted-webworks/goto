# Goto — link router

Two files, kept in the same folder:

- `index.html`: the page itself (design and logic). You rarely need to touch this.
- `data.js`: your projects list (`const routes = { ... }`). **This is the only file you edit to add or change projects.**

It redirects short links to your projects, or shows a directory of all projects.

## How links work

| Link | What happens |
|---|---|
| `.../goto/` | Shows the project directory |
| `.../goto/?to=projects` | Shows the "Resolving your link" screen, then redirects after 1.5s |
| `.../goto/?to=projects&force=true` | Redirects instantly, nothing is shown |
| `.../goto/?to=unknown` | Shows "No Data Found" with a Go Home button (opens the directory without `?to=`) |

- `projects` is the **key** of a route (see below). Keys are not case-sensitive.
- `force=true` also accepts `1` or `yes`.
- If a route has no `url`, even `force=true` shows the "This link isn't ready" screen.

## Add a new project

Open `data.js` and add one block inside `const routes = { ... }`. Don't forget the comma after the previous block.

```js
mynewproject: {
  name: "My New Project",
  url: "https://example.com",
  status: "live"
},
```

Then your link is `https://crafted-webworks.github.io/goto/?to=mynewproject`.

### Fields

| Field | Required | Meaning |
|---|---|---|
| `name` | yes | Title shown on the card |
| `url` | yes | Where the link redirects to. Leave empty or remove it for "not ready" |
| `status` | yes | `live`, `demo`, `sample`, `upcoming` or `expired` |
| `type` | no | Small badge, e.g. `"Business Page"` |
| `desc` | no | Short description |
| `about` | no | Longer description (shown instead of `desc` in the Info panel) |
| `tools` | no | List, e.g. `["HTML", "CSS"]` |
| `tags` | no | List, e.g. `["client"]`. Tags become filter chips automatically |
| `logo` | no | Image URL (use `https://`). Falls back to a number if it fails to load |
| `accentColor` | no | Hex colour for a glow around the logo, e.g. `"#2563eb"` |
| `show` | no | `false` hides it from the directory. The link still works |
| `direct` | no | `true` makes a minimal card with just a Go button |
| `hold` | no | `true` shows only that project in the directory |
| `caseStudy` | no | Text for a "Read case study" section |

## Common edits

- **Hide a project from the list but keep its link working:** add `show: false`.
- **Turn off a project temporarily:** change `status` to `"expired"` and remove its `url`.
- **Change the routing screen time:** edit `REDIRECT_DELAY` (milliseconds) in `index.html`.
- **Add a new status:** add a line in `STATUS_CONFIG` in `index.html`, plus matching `.status-pill.yourstatus` CSS.

## Notes

- Redirecting depends only on `url`. A route with status `upcoming` but a `url` will still redirect, so remove the `url` if the site isn't live yet.
- Keys should be lowercase with no spaces, e.g. `my-project`.
- Upload `index.html` and `data.js` together. If `data.js` is missing, the page can't load your projects.
