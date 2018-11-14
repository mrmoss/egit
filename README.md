# BEGIN RANT

This is a WIP encryption wrapper around git...not sure if I'll actually use it for anything...might just give up on it...

Been doing some thinking about encryption and git lately.

I'm not saying that Microsoft will do bad things with Github...but I don't sleep as well at night anymore knowing they have access to my files...don't worry Microsoft...it's me...not you...I never slept well knowing Github had access to my files...

My first thought was: "I'll just run my own git server on a VPS!"

But then I was like: "It's hosted in Romania...what could go wrong..."

I don't have a secure facility to put my sensitive files on...I'd love to...but I don't...

@GitDevelopers - You people are awesome...it'd be cool if the server side encrypted your files with your private key/auth password on the server side, and didn't store that key ANYWHERE (well, hashes are whatever are fine). If I knew anything about the git server architecture, I might do this myself...if I wasn't in school...and didn't have a job...

Anyways, I've been working on this TERRIBLE python script for ~20 hours now (I had 4 hours of sleep somewhere between those hours...it's not included in that time...)...needs heavy refactoring...

Known Issues:
- Filenames are encrypted - so if the filename is already long, it can get longer, which may raise a "filename is too long" error. I personally think this is a "why are your filenames 100ish characters?!" issue...
- This essentially keeps two copies of a repo on a machine...one encrypted...one decrypted...
- No auto-init process...
- First round of encryption/decryption is slow...don't think this can be fixed. After the first round, modification states are kept in a local flat-file database to speed things up.
- Doesn't detect unused encrypted files (you have to remove the file in both decrypted and encrypted folders).
- Doesn't keep file attributes (as this is technically giving away some information about your files).

Dependencies:
```
pycrypto
```

Here's how you initialize a git repo with it (assuming egit is in your path...):
```
cd /tmp/
mkdir new_git
cd new_git
git init
mkdir decrypted
echo 'we attack at dawn' > decrypted/secret.txt
echo -e '.egit\ndecrypted' > .gitignore
egit encrypt
git add .gitignore encrypted
git commit -m 'secret message uploaded'
git push
```

If you want to push:
```
echo 'we attack at noon' > decrypted/secret.txt
egit encrypt
#Your files are now encrypted into /tmp/new_git/encrypted
git add encrypted/secret.txt
git commit -m "changes to secret message"
git push
```

If you want to pull:
```
git pull
egit decrypt
#Your files are now decrypted into /tmp/new_git/decrypted
```

If you want to see which files have been changed in the decrypted dir:
```
egit status
#This will show new files, removed files, modified files, etc...
```

Again, this will probably not go anywhere. The current state of what happens with personal data makes me have nightmares. I'd like to fix it, but it'll probably end with a couple large businesses having their noses into everything...lol...like I have anything they want...

Rant over - Cheers.

# END RANT
