Lab: Information Disclosure in Version Control History

## Description
This lab demonstrates how sensitive data, such as passwords, can be unintentionally exposed through version control history. An attacker can analyze Git metadata to retrieve previously committed secrets.

## Steps to Reproduce

1. **Access the `.git` directory:**
   - Open the lab and browse to `/.git/` to confirm version control data is exposed.

2. **Download the repository:**
   - Use the following command (Linux/macOS):
     ```bash
     wget -r https://YOUR-LAB-ID.web-security-academy.net/.git/
     ```

3. **Explore the Git repository:**
   - Use Git tools to explore commit history:
     ```bash
     git log
     ```
   - Look for a commit like:  
     `Remove admin password from config`

4. **Check the diff of that commit:**
   - Run:
     ```bash
     git show <commit-id>
     ```
   - You will find a file (e.g., `admin.conf`) where a hard-coded password was replaced with an environment variable, but the original password is still visible in the diff.

5. **Login and perform admin actions:**
   - Use the recovered password to log in as `administrator`.
   - Delete the user `carlos` via the admin interface.

## Key Commands
```bash
wget -r https://<your-lab-id>.web-security-academy.net/.git/
git log
git show <commit-id>
```

## Mitigation
- Do not commit secrets or passwords to version control.
- Always use `.gitignore` to exclude sensitive config files.
- Never expose `.git/` or any internal directories to the public web.
- Use tools like `truffleHog` or `git-secrets` to scan for exposed secrets in Git history.

## Tags
Git, Information Disclosure, Version Control, Secrets in History, Source Code Exposure
