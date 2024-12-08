# access_to_github_command
# Git Account Authorization on MacBook

This guide explains how to authorize a Git account on your MacBook, including setup for GitHub and SSH authentication.

---

## 1. **Set Up Git**

### Check if Git is Installed
Open your terminal and type:
```bash
git --version
```
If Git is not installed, install it using:
```bash
brew install git
```

---

## 2. **Configure Git with Your Account**

### Add Your Name and Email
Set your global username and email address:
```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

### Verify Configuration
Check your settings:
```bash
git config --list
```

---

## 3. **Authenticate with GitHub**

### **Option 1: Use HTTPS with Personal Access Token (Recommended)**

#### Generate a Token
1. Go to [GitHub Personal Access Tokens](https://github.com/settings/tokens).
2. Click **Generate new token**, set permissions, and copy the token.

#### Use the Token
When cloning or pushing, Git will prompt you for a password. Enter the token instead of your GitHub password.

Alternatively, cache the token:
```bash
git config --global credential.helper store
```

---

### **Option 2: SSH Authentication**

#### Generate SSH Key
```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```
Press Enter to accept defaults. This generates keys in `~/.ssh/`.

#### Add SSH Key to Agent
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

#### Add SSH Key to GitHub
1. Copy the SSH key:
   ```bash
   pbcopy < ~/.ssh/id_ed25519.pub
   ```
2. Go to [GitHub SSH Keys](https://github.com/settings/keys), click **New SSH Key**, and paste the key.

#### Test SSH Connection
```bash
ssh -T git@github.com
```

---

## 4. **Authenticate with Other Git Providers**
For other platforms like GitLab or Bitbucket, the process is similar. Replace GitHub-specific URLs with the provider's URLs in the commands above.

---

## 5. **Optional: Use Multiple Accounts**

If you use multiple Git accounts, configure SSH keys for each:

### Create Separate Keys for Each Account
Repeat the SSH key generation process for each account, specifying unique filenames.

### Edit the `~/.ssh/config` File
Configure which key to use for each host:
```bash
Host github-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal

Host github-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_work
```

### Use the Appropriate Host When Cloning
```bash
git clone git@github-personal:username/repo.git
