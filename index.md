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

`git branch -D update_contributor_blurbs` Capital D is forceful, lower case d is safer version.

`git status | fgrep 'modified' | cut -d " " -f 4 | xargs bundle exec rubocop` rubocop will check modified files.
`-a` is automatic
