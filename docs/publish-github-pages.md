# Publish To GitHub Pages

This portfolio is now structured to work as a simple GitHub Pages site.

## Files That Matter

At minimum, upload:

- `index.md`
- `_config.yml`
- `projects/`
- `docs/images/`
- any other docs you want to keep linked from project pages

Optional but recommended:

- `resume.pdf` at the repo root
- `README.md` as the repository landing page on GitHub

## Recommended Repository Name

If you want a user site, create:

- `<your-github-username>.github.io`

Then the site will live at:

- `https://<your-github-username>.github.io/`

If you want a project site, create any repo name, for example:

- `portfolio`

Then the site will live at:

- `https://<your-github-username>.github.io/portfolio/`

For the cleanest setup, I recommend the user-site approach if this is your main portfolio.

## Upload Steps In GitHub Web UI

### Option A: Create A New Repo Through The Browser

1. Go to GitHub and create a new repository.
2. Name it either:
   - `<your-github-username>.github.io`, or
   - `portfolio`
3. Upload the contents of this `cv_pr/` folder into the root of that repository.
4. Commit the upload.

### Option B: Drag And Drop

If the repo is empty:

1. Open the new repo in GitHub.
2. Click `uploading an existing file`.
3. Drag all contents of `cv_pr/` into the upload area.
4. Commit directly to `main`.

## Enable GitHub Pages

After the files are uploaded:

1. Open the repository on GitHub.
2. Go to `Settings`.
3. Open `Pages`.
4. Under `Build and deployment`, choose:
   - `Source: Deploy from a branch`
   - `Branch: main`
   - `Folder: /(root)`
5. Save.

GitHub will build the site and give you the public URL.

## Important Follow-Up Edits

Before publishing, replace these placeholders in `index.md`:

- `https://www.linkedin.com/in/your-linkedin`
- `your@email.com`

Also add:

- `resume.pdf` at the repo root if you want the resume link to work

## Suggested Final Structure

```text
/
├── _config.yml
├── index.md
├── resume.pdf
├── projects/
│   ├── ai-consul.md
│   ├── ai-dev-pipeline.md
│   └── mood-food.md
└── docs/
    ├── images/
    └── ...
```

## After Publishing

Use only one portfolio link in your CV:

- header example: `Portfolio: https://<your-site-url>`

Then pin the three strongest repos on your GitHub profile:

- AI Consul presentation repo or case study repo
- AI development pipeline repo
- portfolio site repo

## Good First Check After Deploy

Open these pages and make sure they load:

- `/`
- `/projects/ai-consul`
- `/projects/ai-dev-pipeline`
- `/projects/mood-food`

Also confirm:

- images display correctly
- external links work
- the Mood&Food prototype opens from its project page
