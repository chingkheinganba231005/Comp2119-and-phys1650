# Study Guides: COMP2119 + PHYS1650

Two interactive study guides in one static website, ready for GitHub Pages. There is no build step and no dependencies: it is plain HTML.

```
index.html            Front page with a card for each course
comp2119/index.html   COMP2119 "Data Structures from Scratch" (complete app; quiz + final-exam pages added Nov 2026)
phys1650/index.html   PHYS1650 "Nature of the Universe" (complete app; Test 2 prep upgraded Nov 2026)
.nojekyll             Tells GitHub Pages to serve the files as they are
```

Each app is a single self-contained page that keeps its own routing (`#ch3`, `#test2`, and so on), light/dark setting and quizzes. Everything uses relative links, so the site works under any repository name (`https://USER.github.io/REPO/`) and also at the root of a `USER.github.io` repository.

## Deploy (for Claude Code)

Follow [`DEPLOY.md`](DEPLOY.md). In short: create a GitHub repo from this folder, push `main`, and enable Pages from the `main` branch, folder `/ (root)`.

## Preview locally

```bash
python3 -m http.server 8000
# open http://localhost:8000/
```

## Notes

- The pages load their fonts from Google Fonts, so fonts need an internet connection (the layout still works without it, using fallback fonts).
- `noindex` is set on all three pages so search engines are asked not to list the site. Remove the `<meta name="robots" ...>` line in each `index.html` if you want it to be searchable.
- A GitHub Pages site is public to anyone who has the link, even when it is published from a private repository. The guides contain worked answers to assignments and tests, so share the link thoughtfully and follow your course's academic-integrity rules.
