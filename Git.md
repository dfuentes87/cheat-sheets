# Git

## Contributing to a GitHub/GitLab project

How to create a pull request that causes no pain to the maintainer.

### Setting up your local development environment

1. Fork the repository
2. Clone it on your local system
3. Add the upstream repository as a remote:

   ```bash
   git remote add upstream https://github.com/<upstream-owner>/<upstream-repo>.git
   ```

4. Verify the new remote:

   ```bash
   git remote -v
   ```

### Working locally

In most cases, the master branch is the "production environment" of the upstream project. **Never** work on the master branch directly, instead create a new branch. Use short, descriptive names matching issue or ticket IDs, e.g.:

```bash
git checkout -b bugfix/gh-45
```

1. Every time you start working on the code, it's important to be sure you have the latest from upstream:

    ```bash
    git fetch upstream
    git pull upstream/master
    ```

2. Rebase your local branch onto the master branch:

    ```bash
    git rebase master
    ```

3. Resolve any conflicts and commit
4. Do your work, commit
5. Push to your own fork

    ```bash
    git push
    ```

### Sending the Pull Request

1. Push your branch to your fork on GitHub:

   ```bash
   git push origin <your-branch-name>
   ```

2. Open the upstream repository in your browser and click **"New pull request"**.
3. Select your branch from the drop-down as the compare branch, and ensure the upstream repository’s main branch is the base.
4. Review the changes shown in the diff pane to confirm they are correct.
5. Fill out the PR form:
   - Title: concise summary of your change.
   - Description: context, purpose, and any testing performed.
6. Submit the pull request.
7. Monitor for feedback:
   - Respond to review comments.
   - Push updates to your branch; the Pull Request will auto-update.
8. The repo owner will merge the Pull Request once reviews are complete and checks pass.

## Resources

- [GitHub - About pull request merges](https://help.github.com/articles/about-pull-request-merges/)
