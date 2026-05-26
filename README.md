# GitHub SSH Setup on Linux Server (One-Time Setup)

This guide helps you configure SSH authentication for GitHub on a Linux server/VPS so Git stops asking for a username and password every time.

---

## Step 1: Check if SSH Key Already Exists

Run:

```bash
ls ~/.ssh
```

If you see:

```text
id_ed25519
id_ed25519.pub
```

Then SSH key already exists → skip to **Step 4**.

---

## Step 2: Generate SSH Key

Run:

```bash
ssh-keygen -t ed25519 -C "your_email@gmail.com"
```

Example:

```bash
ssh-keygen -t ed25519 -C "ankurkumar7753@gmail.com"
```

It will ask:

### Save Key Location

```text
Enter file in which to save the key (/root/.ssh/id_ed25519):
```

Just press:

```text
Enter
```

### Passphrase

```text
Enter passphrase (empty for no passphrase):
```

Press:

```text
Enter
```

Again:

```text
Enter same passphrase again:
```

Press:

```text
Enter
```

Expected Output:

```text
Your identification has been saved in /root/.ssh/id_ed25519
Your public key has been saved in /root/.ssh/id_ed25519.pub
```

---

## Step 3: Start SSH Agent

Run:

```bash
eval "$(ssh-agent -s)"
```

Expected output:

```text
Agent pid 12345
```

---

## Step 4: Add SSH Key to Agent

Run:

```bash
ssh-add ~/.ssh/id_ed25519
```

Expected output:

```text
Identity added: /root/.ssh/id_ed25519
```

---

## Step 5: Copy Public SSH Key

Run:

```bash
cat ~/.ssh/id_ed25519.pub
```

You will get something like:

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA.... your_email@gmail.com
```

Copy the **entire line**.

---

## Step 6: Add SSH Key to GitHub

Open GitHub SSH Settings:

https://github.com/settings/keys

Steps:

1. Click **New SSH Key**
2. Title → Add server name

Example:

```text
cp3-server
```

3. Paste the copied SSH key into the **Key** field
4. Click **Add SSH Key**

---

## Step 7: Test SSH Connection

Run:

```bash
ssh -T git@github.com
```

First time it may ask:

```text
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Type:

```text
yes
```

Expected success message:

```text
Hi username! You've successfully authenticated
```

---

## Step 8: Clone Repository Using SSH

❌ Wrong (HTTPS):

```bash
git clone https://github.com/user/repo.git
```

✅ Correct (SSH):

```bash
git clone git@github.com:user/repo.git
```

Example:

```bash
git clone git@github.com:ankur/repo-name.git
```

---

## Step 9: Convert Existing Repository from HTTPS to SSH

Check current remote:

```bash
git remote -v
```

If output looks like:

```text
https://github.com/username/repo.git
```

Change it to SSH:

```bash
git remote set-url origin git@github.com:USERNAME/REPO.git
```

Example:

```bash
git remote set-url origin git@github.com:ankur/kenome-fe-v2.git
```

Verify:

```bash
git remote -v
```

You should now see:

```text
git@github.com:username/repo.git
```

---

## Step 10: Done

Now these commands will work without asking for username/password:

```bash
git clone
git pull
git push
```

---

## Common Errors

### Error: `Permission denied (publickey)`

Fix:

```bash
ssh-add ~/.ssh/id_ed25519
```

Then test again:

```bash
ssh -T git@github.com
```

---

### Error: `No such file or directory`

Check if SSH key exists:

```bash
ls ~/.ssh
```

If missing, generate again:

```bash
ssh-keygen -t ed25519 -C "your_email@gmail.com"
```

---

## Useful Commands

Check SSH files:

```bash
ls ~/.ssh
```

View SSH public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Check remote URL:

```bash
git remote -v
```

Change remote URL:

```bash
git remote set-url origin git@github.com:USERNAME/REPO.git
```
