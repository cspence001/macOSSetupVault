Created: 09/13/2024
### 1. Fork the Repository
   - Navigate to the [euc-samples repository](https://github.com/euc-oss/euc-samples) on GitHub.
   - Click the "Fork" button in the upper right corner to create a copy of the repository in your own GitHub account.

### 2. Clone Your Fork Locally
   - Open your terminal and clone the repository to your local machine:
     ```bash
     git clone https://github.com/your-username/euc-samples.git
     cd euc-samples
     ```

### 3. Add Upstream Remote
   - Add the original repository as a remote to keep your fork updated:
     ```bash
     git remote add upstream https://github.com/euc-oss/euc-samples.git
     ```

### 4. Create a Topic Branch
   - Create and switch to a new branch where you’ll make your changes:
     ```bash
     git checkout -b my-new-feature main
     ```

### 5. Make Your Changes
   - Modify the code or documentation as needed. Ensure your changes align with the repository’s contribution guidelines.

### 6. Ensure Local Tests Pass
   - Run any existing tests to ensure your changes don’t break the functionality. Follow any testing instructions provided in the repository.

### 7. Update Documentation (if required)
   - If your changes affect documentation, update it accordingly.

### 8. Commit Your Changes
   - Add and commit your changes with a clear message:
     ```bash
     git add path/to/changed/files
     git commit -m "Describe the changes you made"
     ```

### 9. Push Changes to Your Fork
   - Push your branch to your fork on GitHub:
     ```bash
     git push origin my-new-feature
     ```

### 10. Create a Pull Request
   - Go to the [Pull Requests page](https://github.com/euc-oss/euc-samples/pulls) of the original repository.
   - Click the "New pull request" button.
   - Select your branch from your fork and compare it with the `main` branch of the upstream repository.
   - Provide a descriptive title and comment for your pull request, explaining the changes you’ve made.
   - If your pull request addresses an issue, include a reference to the issue in the description (e.g., `Closes #123`).

### 11. Stay In Sync with Upstream (if needed)
   - If your branch becomes outdated, update it by following these steps:
     ```bash
     git checkout my-new-feature
     git fetch -a
     git pull --rebase upstream main
     git push --force-with-lease origin my-new-feature
     ```

### 12. Update Your Pull Request (if needed)
   - If code review requests changes or you need to squash commits:
     - To amend the most recent commit:
       ```bash
       git add .
       git commit --amend
       git push --force-with-lease origin my-new-feature
       ```
     - To squash changes into an earlier commit:
       ```bash
       git add .
       git commit --fixup <commit-hash>
       git rebase -i --autosquash main
       git push --force-with-lease origin my-new-feature
       ```
   - Add a comment to your pull request indicating the changes are ready for review.

### 13. Formatting Commit Messages
   - Follow the conventions for writing commit messages. Include any related GitHub issue references in your commit message.

### 14. Reporting Bugs and Creating Issues
   - Follow the guidelines for reporting bugs or suggesting features. Ensure to include relevant details and check existing issues to avoid duplicates.

---
## DCO (Developer Certificate of Origin) check

To resolve the issue related to the DCO (Developer Certificate of Origin) check, you need to add a `Signed-off-by` line to your commit messages. This is a requirement for some projects to ensure that contributors agree to the terms of the DCO. 

### Steps to Fix DCO Issue

1. **Check Out Your Branch Locally:**
   Ensure you have a local copy of the branch where the issue occurred. You can check out your branch using the following command:
   ```bash
   git checkout add-CodeReq-appblock
   ```

2. **Rebase with Sign-off:**
   Use the `git rebase` command to add the `Signed-off-by` line to your commit messages. If you only need to sign off the latest commit, you can run:
   ```bash
   git rebase HEAD~1 --signoff
   ```
   If you have multiple commits that need the sign-off, adjust `HEAD~1` to the number of commits you need to include in the rebase. For example, `HEAD~3` for the last three commits.

3. **Force Push Your Changes:**
   After successfully rebasing, force push the updated branch to your remote repository. This will overwrite the existing branch with your signed commits:
   ```bash
   git push --force-with-lease origin add-CodeReq-appblock
   ```

### Detailed Explanation:

- **Rebasing with `--signoff`:**
  The `--signoff` flag automatically adds the `Signed-off-by` line to your commit messages. This line includes your name and email address and indicates that you agree to the DCO. This is required for the pull request to pass the DCO check.

- **Force Push:**
  Since rebase rewrites commit history, you need to force push the branch. The `--force-with-lease` flag ensures that your push will only succeed if nobody else has pushed changes to the branch in the meantime.

### Additional Tips:

- **Verify Your Commits:**
  Before rebasing, you can check your commit messages to see which ones are missing the `Signed-off-by` line. Use:
  ```bash
  git log
  ```
  Look for commits that don’t have the required line.

- **Automatic Sign-off for Future Commits:**
  To avoid this issue in the future, configure Git to automatically add the `Signed-off-by` line to your commits. Set this up globally or for the specific repository:
  ```bash
  git config --global commit.gpgSign true
  git config --global user.signingkey YOUR_KEY_ID
  ```
  This ensures that every commit includes the necessary sign-off information.

