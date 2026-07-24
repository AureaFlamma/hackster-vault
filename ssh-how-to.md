# SSH Setup with GitHub

## 1. Check for existing SSH keys
```bash
ls -al ~/.ssh
```

## 2. Generate a new SSH key
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
(Fallback for older systems: `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`)

## 3. Start the SSH agent and add your key
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```
macOS keychain option:
```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

## 4. Copy your public key
```bash
# macOS
pbcopy < ~/.ssh/id_ed25519.pub

# Linux
cat ~/.ssh/id_ed25519.pub

# Windows (Git Bash)
clip < ~/.ssh/id_ed25519.pub
```

## 5. Add the key to GitHub
1. GitHub → Settings → SSH and GPG keys
2. New SSH key
3. Add title
4. Paste public key
5. Add SSH key

## 6. Test the connection
```bash
ssh -T git@github.com
```

## 7. Use SSH URLs for repos
```bash
git clone git@github.com:username/repo.git
```
Switch existing remote from HTTPS to SSH:
```bash
git remote set-url origin git@github.com:username/repo.git
```
