“Press escape and then type :q! To cancel commit” this is not really a git command, it’s a vim command. Vim is a text editor, like Atom, but for the terminal. It uses key sequences to do things, so unlike most modern GUI editors, you have to switch between “command mode” and “insert mode”. In command mode, keys you press have different functions, and don’t enter text into the document. Pressing escape takes you out of insert mode and into command mode. :q! is the command to quit without saving.
On many systems, git will use vim as its default editor, but I recommend changing that.
Which is what the next command in the document does :slightly_smiling_face:
What git is _actually_ doing, is when you tell it to commit, it opens up a copy of the “commit template” in your configured text editor. Then it waits for you to save and close it.
The commit template is a file where you can write your commit message. Any lines starting with `#` are comments, and they aren’t included in the final commit message. The default commit template adds helpful comments, telling you what to do and which files are included in the pending commit. You can change the template, but the default is pretty good for most people.
Once you have written the commit message, saved and closed the file, git will read the file and look for lines that aren’t comments. If there are any, it uses them as the commit message and stores the commit. If there are _no_ lines that aren’t comments, git assumes you changed your mind about committing and cancels the commit. That’s why `<esc>:q!` cancelled the commit in vim: the command quits vim _without_ saving, so git only saw the comments in the template.

How do you share a branch so you can both work on it?
Let’s say that Sophie started the branch, and Jenny wants to contribute to it.
Sophie would commit her work in progress to the branch,
then `git push origin <branch-name>` (or `git push origin HEAD` as a shortcut).
That would copy those changes up to https://github.com/ifPairElseUnknown/ifme
GitHub
ifPairElseUnknown/ifme
ifme - Open source app to share mental health experiences with loved ones
Then, Jenny would fetch those changes:

```git fetch origin
```
This makes a local copy of all of the changes in `origin` (which is the alias for https://github.com/ifPairElseUnknown/ifme you previously created with `git remote add` or `git clone`)
Alternatively, instead of `fetch` you can `pull`. When you run `git pull origin master` it is a shortcut for `git fetch origin`, then `git merge origin/master`.

(That’s with the default settings, you can also reconfigure it to be a shortcut for `git fetch origin` then `git rebase origin/master`, which is what my `.gitconfig` file does here: https://github.com/TimMoore/dotfiles/blob/master/.gitconfig#L25-L26)
GitHub
TimMoore/dotfiles
dotfiles - Bash, Git, SSH and Vim configuration files
Then, Jenny would check out the branch `git checkout <branch-name>`. This creates a “local” branch that “tracks” the “remote” branch, and sets your working copy to that branch. 
I’ll define all of that:

- “local branch” a branch that lives only on your computer
- “remote branch” a branch in another git repository, in this case the one on GitHub at https://github.com/ifPairElseUnknown/ifme
- “tracking” this links up a local branch with a remote branch so that git understands they’re connected. It can use this information to let you run `git push` and `git pull` without having to enter the remote name and remote branch name every time, and `git status` will tell you if your local branch is “ahead” or “behind” the remote branch.
- “ahead” means you have commits on your local branch that you have not pushed to the remote branch yet
- “behind” means there are commits on the remote branch that you have not merged to your local branch yet

It is possible to be both “ahead” and “behind” simultaneously! This could happen if Jenny has made some commits locally after fetching the branch, and in the meantime, Sophie has also made some new commits to the branch and pushed them to GitHub. In this situation, Jenny will need to either “merge” or “rebase” to combine the changes before she can push them to GitHub.
Here’s the abbreviated version:

1. Sophie commits changes using `git add` and `git commit` as normal.
2. Sophie runs `git push --set-upstream origin <branch-name>` to sync those changes to GitHub and to tell git to track the remote branch.
3. Jenny runs `git fetch origin` to sync Sophie’s changes _from_ GitHub.
4. Jenny runs `git checkout <branch-name>` to switch to that branch.
5. Jenny makes changes and commits them with `git add` and `git commit`.
6. Jenny runs `git push` (no need to add `origin <branch-name>` this time, because git knows already).
7. Sophie runs `git pull` (again, no need to tell it where to pull from, because `--set-upstream` in step #2 stores that information)
8. You can both continue to take turns committing, running `git push` to sync your own changes to GitHub, and running `git pull` to sync each other’s changes _from_ GitHub.

If you both make changes without taking turns pushing and pulling, then you might get an error when you try to push. This just means you need to pull again. Git will merge your changes with the changes from GitHub. If you both change the same thing, you’ll get a “conflict” and will need to manually resolve it. The first time that happens, I’d suggest getting a coach to help you through it.

The resource I recommend most is the “Pro Git” book by Scott Chacon, which you can read online for free https://git-scm.com/book/en/v2
GitHub’s help is also pretty good https://help.github.com. There’s a lot there and you don’t need to read it all, but keep it in mind for when you have questions.
help.github.com
GitHub Help
Help documentation for GitHub.com, GitHub Enterprise, GitHub Pages, and GitHub for Mac and Windows… 
This cheatsheet is good for common commands https://services.github.com/on-demand/downloads/github-git-cheat-sheet/
And this for when you need to fix a mistake https://services.github.com/on-demand/git-trouble/
http://learngitbranching.js.org will help you understand branches
learngitbranching.js.org
Learn Git Branching
A interactive Git visualization tool to educate and challenge! (71kB)
There’s more stuff linked from https://help.github.com/articles/git-and-github-learning-resources/
help.github.com
Git and GitHub learning resources - User Documentation
There are a lot of helpful Git and GitHub resources on the web. This is a short list of our favorites! Using Git Familiarize yourself with Git by visiting the official Git project site
Also these guides :slightly_smiling_face: https://guides.github.com
That should be enough to keep you busy for a while :wink:
`git fetch upstream`
A fetch just tells git to check what new commits/branches exist but don’t apply them to your local branches.

`git checkout upstream/master -b feature/update_to_ruby_2_3_4`
Check out a copy of the projects version of the master branch into a new branch called feature/update_to_ruby_2_3_4
`git cherry-pick bba7338`
Unlike merge, which copies everything that is different between 2 branches, a cherry-pick allows you to just grab only 1 commit and move it into your branch. bba7338 was the ID (SHA) for your Ruby upgrade commit. You only need to provide the first 7 characters, not the whole, massive string.
`git push origin feature/update_to_ruby_2_3_4`
Push the code to GitHub as a new branch
`git rebase --interactive` if you want to pick which commits to include and which to drop (and which to squash together or edit)
When I did my first PR, an extra commit was accidently pushed. To resolve this:
`git log`check all commits on branch
`git rebase -i HEAD~2` 
`git push -f origin updateruby`
PR is failing because it is out of sync with Julia’s version of master.
This is something you will encounter a lot because the upstream project will keep changing while you are working on your tasks.

There are 2 ways to deal with this:
• merge upstream master into your branch (this can even be done on the PR itself)
• rebase your branch against the upstream master

In the first case, all of the changes that have occurred on upstream are bundled up into 1 new commit on your branch.
In the second case, history is rewritten and all of the commits you don’t yet have are inserted into your branch almost as if they had always been there. You will have heard a bit about this in that video from the Ruby Meetup.

My preference in the second option—even though it sometimes gives you merge conflicts that need to be resolved, you get a nice, clean history.

My recommendation would be to add the upstream as a new remote and then rebase your branch off upstream master.
```git remote add upstream git@github.com:julianguyen/ifme.git
git rebase upstream master



`git add...`
`git commit`
`gpsu` (git branch set upstream my branch origin/master)


`git checkout -b update_contributor_blub` Create new branch

`git branch --set-upstream-to=origin/master`


git checkout some_branch_name
git fetch upstream
git rebase upstream/master
git push --force-with-lease

git mv app/assets/images/contributors/sophie_mcdonald.JPG app/assets/images/contributors/sophie_mcdonald.jpg
git commit --amend --no-edit
git push --force-with-lease

running test suite on firefox 
Happy to discuss in detail when I’m there but, if you still have Firefox 46 installed, it is as simple as:
```bundle exec rspec```

```git remote -v
git remote add upstream git@github.com:julianguyen/ifme.git
git fetch upstream
git merge upstream/master```

```git pull
   git branch -a # checkout branches
   
```
`git diff <filename> <filename>` will show the difference between 2 files, before merging.

`git branch -D update_contributor_blurbs` Capital D is forceful, lower case d is safer version.

RUBY
https://github.com/bbatsov/ruby-style-guide

 single '' 
 double "" 
my_string = 'my_var' # my_var


symbols: :symbols
mutable can be changed
imutable can't be changed
.freeze can't change, saves memory (only one copy of string)

MY_THING = ['a','b'].freeze
arrays, hashes.
numbers are already imuntable, can;t be changed.
HASH
{
:somthing =>:somth+ing
something: :something
}

if/else/unless
return 'bacon' unless 'vegan'

user id= 
if  profile.blank?
current_user.id
else
profile
end

`git status | fgrep 'modified' | cut -d " " -f 4 | xargs bundle exec rubocop` rubocop will check modified files.
`-a` is automatic


JAVASCRIPT
'use strict'; its focus isn't about cleaner code, but does what you expect it to do. helps to not create an acidental global variable.Must be the first line


