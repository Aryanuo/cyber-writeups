# My-Git

**Platform:** picoCTF / CyLab 2026
**Category:** General Skills
**Difficulty:** Easy
**Author:** Darkraicg492
**Challenge Link:** [My-Git](https://learn.cylabacademy.org/library/766?page=1)

---

## Challenge Description

> I have built my own Git server with my own rules!

After launching the instance, the challenge provides a Git repository along with the credentials required to access it.

Example:

```text
You can clone the challenge repo using the command below.

git clone ssh://git@foggy-cliff.picoctf.net:64661/git/challenge.git

Here's the password: <password>

Check the README to get your flag!
```

### Hint

> How do you specify your Git username and email?

---

# Objective

* Clone the provided Git repository.
* Understand the server's requirements.
* Modify the Git author information.
* Push a commit to retrieve the flag.

---

# Solution

## Step 1 - Clone the Repository

Clone the repository using the command provided by the challenge:

```bash
git clone ssh://git@foggy-cliff.picoctf.net:<PORT>/git/challenge.git
```

Enter the supplied password when prompted.

---

## Step 2 - Read the README

Navigate into the repository and inspect the README file.

```bash
cd challenge
cat README.md
```

Output:

```text
# MyGit

### If you want the flag, make sure to push the flag!

Only flag.txt pushed by root:root@picoctf will be updated with the flag.

GOOD LUCK!
```

The important clue is:

```text
root:root@picoctf
```

This indicates that the server validates the **Git commit author information**.

---

## Step 3 - Change the Git Author

Configure Git to use the required username and email address:

```bash
git config user.name "root"
git config user.email "root@picoctf"
```

---

## Step 4 - Create and Commit `flag.txt`

Create a file named `flag.txt`:

```bash
echo "The flag" > flag.txt
```

Stage the file:

```bash
git add flag.txt
```

Create a commit:

```bash
git commit -m "Requesting flag"
```

---

## Step 5 - Push the Commit

Push the commit to the remote repository:

```bash
git push origin master
```

After authenticating with the provided password, the server responds:

```text
remote: Author matched and flag.txt found in commit...
remote: Congratulations! You have successfully impersonated the root user
remote: Here's your flag: picoCTF{REDACTED}
```

---

# Flag

```text
picoCTF{Try It}
```

---

# Why the Attack Works

Git stores the author's **name** and **email** as metadata inside every commit.

Normally, these fields identify who created the commit. However, they are not cryptographically verified by default and can be changed locally using `git config`.

The challenge server only checks whether the commit was authored by:

```text
root
root@picoctf
```

Since the server trusts this metadata without verifying the author's identity, changing the Git configuration allows the user to impersonate the expected author and satisfy the challenge requirements.

---

# Key Takeaways

* Git commits contain author metadata (name and email).
* `git config` can be used to change the commit author.
* Git author information is not proof of identity unless additional verification mechanisms (such as GPG signing) are used.
* Applications should never rely solely on Git author metadata for authentication or authorization.

---

# Tools Used

* Git
* SSH
* Linux Terminal
