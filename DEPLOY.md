# Deploying to GitHub Pages

This folder is the finished site. Do **not** edit `comp2119/index.html` or `phys1650/index.html`; they are the finished apps. Publish the folder as it is.

Recommended repository name: `hku-study-guides` (any name works). Replace `NAME` below if you choose another.

## With the GitHub CLI (`gh`, already logged in)

Run these from the repository root (the folder that contains this file).

```bash
# 1. Commit
git init -b main
git add -A
git commit -m "Add COMP2119 and PHYS1650 study guides"

# 2. Create the GitHub repo and push (use --private if you prefer; see the note below)
gh repo create NAME --public --source=. --remote=origin --push

# 3. Turn on GitHub Pages: branch main, folder / (root)
OWNER=$(gh api user --jq .login)
gh api --method POST "repos/$OWNER/NAME/pages" -f "source[branch]=main" -f "source[path]=/" \
  || gh api --method PUT "repos/$OWNER/NAME/pages" -f "source[branch]=main" -f "source[path]=/"

# 4. Wait for the first build (usually 1 to 2 minutes), then check it
for i in $(seq 1 30); do
  s=$(gh api "repos/$OWNER/NAME/pages/builds/latest" --jq .status 2>/dev/null); echo "build: ${s:-pending}"
  [ "$s" = "built" ] && break
  [ "$s" = "errored" ] && { echo "Pages build failed"; break; }
  sleep 10
done
echo "https://$OWNER.github.io/NAME/"
curl -sI "https://$OWNER.github.io/NAME/" | head -1                # expect HTTP/2 200
curl -sI "https://$OWNER.github.io/NAME/comp2119/" | head -1       # expect HTTP/2 200
curl -sI "https://$OWNER.github.io/NAME/phys1650/" | head -1       # expect HTTP/2 200
```

Then tell the user the three links:

- Front page: `https://OWNER.github.io/NAME/`
- COMP2119: `https://OWNER.github.io/NAME/comp2119/`
- PHYS1650: `https://OWNER.github.io/NAME/phys1650/`

## Without the CLI

1. Create an empty repository on github.com and push this folder to its `main` branch.
2. Repository **Settings → Pages → Build and deployment**: Source **Deploy from a branch**, Branch **main**, Folder **/ (root)**, then **Save**.
3. Open the URL shown at the top of that page after the build finishes.

## Notes

- Pages for a **private** repository needs a paid GitHub plan, and the published site is still public to anyone with the link. On a free account use a public repository.
- The apps have no server code and make no API calls. The only external request is the Google Fonts stylesheet.
- To update later, replace the file for one app, commit and push; Pages rebuilds automatically.
