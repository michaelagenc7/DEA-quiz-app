# DEA Practice Quiz

A static quiz site for trainee Domestic Energy Assessors. It serves 20 random questions per quiz from a bank of 164, including 50 photo and diagram questions drawn from the RDSAP 10 course books. After each question it shows the correct answer with the reason and the course page to revise. At the end it shows the full results.

There is no server code and no build step, so it runs on Cloudflare Pages as-is.

## Files

- `index.html` – the app (layout, styles and quiz logic)
- `questions.js` – the question bank (edit this to add or change questions)
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

Open `questions.js` and copy an existing entry:

```
Q('Topic', 'Question text?', ['Right answer','Wrong 1','Wrong 2','Wrong 3'], 0,
  'Reason shown after answering.', SRC.B1(12), 'img/photo.jpg', 'Describe the image')
```

- The number after the options is the index of the correct option (0 is the first).
- Options are shuffled each time, so the correct answer moves around.
- Leave out the last two values for a question without an image.
- `SRC.B1(n)`, `SRC.B2(n)` and `SRC.B3(n)` give "Course book 1/2/3, page …".

Scores are stored only in the learner's own browser (localStorage). Nothing is sent to a server.
