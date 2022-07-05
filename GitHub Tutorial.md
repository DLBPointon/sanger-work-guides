# GitHub

In terminal enter:
```bash
git config --global user.name "YOUR USERNAME"

git config --global user.email "YOUR ASSOCIATED EMAIL"

git config -l
```

Do not use -system, that will wipe everyones account.

---

It also might be worth running this, this set your preffered editor for github.
```bash
git config --global core.editor vim
```

This is helpful if you don't know any vim or emacs, otherwise the only fix is restarting your terminal.

---

Now head here: https://github.com/settings/tokens

The page should be called "Personal access tokens"

Click: Generate New Token

Give it a name like "farm5"
	- This will be be personal code for farm5 after all

Expiration you can select "No Expiration" for simplicity.

For the scopes, select all of them. No point in limiting your self. This will generate a token that you will only ever see once so copy and paste it somehwere safe.

---

Go back to your console and enter:
`git config --global credential.helper cache`
This should cache your access code when you next enter it.

Try git push again, this time it should ask for a password. ENTER THE PERSONAL ACCESS CODE and then you should see it update.

---

## Notes

Everytime someone changes something upstream (e.g. they push their commits) you will need to git pull. Best idea is to use branches but then thats a whole other thing.

So start of day working on a project, always `git pull` even if they tell you haven't done anything. 

You can't push/pull too often.