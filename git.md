Deleting a branch doesn't delete it's sub-branches. Branches are pointers to commits rather than ordered containers. So if I delete a branch without merging, the sub-branch will take the last known commit (pointer) on the repository's git tree.

If you merge, then delete the branch, the sub-branch isn't deleted. Remember, branches are pointers to commits rather than pointers to another container.
