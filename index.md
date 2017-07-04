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
