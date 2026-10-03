# DEA Practice Quiz

A static quiz site for trainee Domestic Energy Assessors. It serves 20 random questions per quiz from a bank of 246, including 62 photo and diagram questions. Questions come from the RDSAP 10 course books, the City & Guilds unit outline (Units 371 to 374) and government EPC guidance. After each question it shows the correct answer with the reason and where to revise. At the end it shows the full results. The start screen lets learners focus a quiz on the site visit and safety, heating controls and meters, building fabric, or heating, hot water and ventilation.

There is no server code and no build step, so it runs on Cloudflare Pages as-is.

## Files

- `index.html` – the app (layout, styles and quiz logic)
- `questions.js` – the main question bank (course-book questions)
- `questions-site-visit.js` – the second set: site visit, heating controls, electricity meters, secondary heating and renewables
- `img/` – photos and diagrams used by the image questions
- `_headers` – Cloudflare Pages headers (security and image caching)

## Deploy to Cloudflare Pages

### Option 1: drag and drop (quickest)

1. In the Cloudflare dashboard go to **Workers & Pages**, then **Create**, then **Pages**, then **Upload assets**.
2. Name the project, for example `dea-quiz`.
3. Upload this folder (or the zip). `index.html` must be at the top level.
4. Select **Deploy**. Your site will be at `https://dea-quiz.pages.dev`.

To update later, open the project, select **Create deployment** and upload the changed files.

### Option 2: command line

```
npx wrangler pages deploy . --project-name dea-quiz
```

### Option 3: Git

Push this folder to a GitHub or GitLab repository, then connect it in Cloudflare Pages. Leave the build command empty and set the output directory to `/`.

## Restricting access (recommended)

The quiz uses images taken from the Skillsmax course slides. If you do not have permission to publish them openly, limit who can open the site with Cloudflare Access (Zero Trust): add a self-hosted application for your `*.pages.dev` address and allow only your students' email addresses.

## Adding questions

Open `questions.js` or `questions-site-visit.js` and copy an existing entry:

```
Q('Topic', 'Question text?', ['Right answer','Wrong 1','Wrong 2','Wrong 3'], 0,
  'Reason shown after answering.', SRC.B1(12), 'img/photo.jpg', 'Describe the image')
```

- The number after the options is the index of the correct option (0 is the first).
- Options are shuffled each time, so the correct answer moves around.
- Leave out the last two values for a question without an image.
- `SRC.B1(n)`, `SRC.B2(n)` and `SRC.B3(n)` give "Course book 1/2/3, page …". `SRC.CG(373)`, `SRC.GOV()`, `SRC.GP()`, `SRC.TP()` and `SRC.RB()` label the City & Guilds handbook, government guide, good practice, training provider and RdSAP 10 guidance sources.
- To add a new focus group, edit the `FOCI` list near the top of the script in `index.html`.

Scores are stored only in the learner's own browser (localStorage). Nothing is sent to a server.
