# Releasing

1. `git checkout development`
1. Update files
  1. Update CONTRIBUTORS.md
    - this is annoying, so eventually we might get rid of this part of the process. This is the process that gives us CONTRIBUTORS.md and also updates package.json with the contributor info.
    - Use [`contributor`](https://www.npmjs.com/package/contributor) to automate this, though you might need to do some hand editing as well (email addresses, URLs, bonnici's name).
      - You need to merge development into master first, because contributors grabs info from the github api, and github only looks at the default/master branch.
    1. `git checkout master`
    1. `git reset --hard @~5`
      - There might be readme changes, etc; These should be in development, but they might be a different hash, so reset them.
    1. `git merge development`
    1. `git push --force`
    1. `git checkout development`
    1. `contributor`
    1. `mv contributors.md CONTRIBUTORS.md`
    1. `nano CONTRIBUTORS.md package.json` Fix bonnici's name in CONTRIBUTORS.md and the names/urls in package.json.
    1. `git add CONTRIBUTORS.md package.json`
  1. Update CHANGELOG.md
  1. Update chatBot.js version number to X.Y.Z (no -dev tag)
  1. `git add chatBot.js`
  1. `npm version X.Y.Z -m "vX.Y.Z"` (set the new version in package.json and commit chatBot.js as well)
1. `git checkout master`
1. `git merge development`
1. `git push`
1. `git checkout development`
1. Update versions for development
  1. `git checkout development`
  1. Update chatBot.js version number to X.Y.Z-dev
  1. `npm version X.Y.Z-dev -m "X.Y.Z Development"`
  1. `git push`
1. [Release on github.](https://github.com/Efreak/node-steam-chat-bot/releases/new). Include the changelog since last release, or if it's too long include only the important stuff.
1. Wait 24 hours.
1. If no bugs have shown up, release on npm
  - The easiest way to make sure the release is clean is to download the tarball from github (master) and release that:
  - `npm publish X.Y.Z.tar.gz --tag ReleaseName`