# Playing with `git` internals

Here I'm playing with `git` internals.

## Selecting a subset of the tree

I'm using `git ls-tree -r HEAD a/ab/5 c/3 b/2` to select a few objects from the tree. I then intend to use `git mktree` to 
create a tree, but don't associate a commit or anything "semantic" like that. My end goal is to `ref` to said subtree.

This works conceptually, but `git mktree` doesn't take directory structures: it complains about slashes. So you would need to
recreate the structure by selecting each `blob` in the deeper `trees` and `mktree` new ones ("masked" trees) and work your way
up until you have a final `git mktree` that only ingests `blobs` in the current directory, and all the "masked" subtrees. That
gives you the final hash of a `tree` that represents the subset of objects in the tree.

(It's unclear to me whether [appending](https://stackoverflow.com/questions/27279136/create-tree-without-using-staging-area#comment43107051_27283328) to the tree would work.)

It's likely that using the index file makes this easier, but that would mess "semantically" with the repo. See [this note] from
the `git-annex` folks on potentially moving to `mktree` but running into the same concern as above.

I proceeded with the simplest example:

```
git ls-tree HEAD b | git mktree
b9645f5603b4efe0368929ccf313145818fdc245

git ls-tree b9645f5603b4efe0368929ccf313145818fdc245
040000 tree 9020154eb21da8cb3fd6860ec7065e962c446ed6	b
```

## References

* https://jwiegley.github.io/git-from-the-bottom-up/1-Repository/4-how-trees-are-made.html
* https://github.com/git/git/blob/2d677e5b1537692a4f875740a6e9853c24285f02/refs.c#L847
* https://git-scm.com/docs/git-for-each-ref
* https://git-scm.com/book/en/v2/Git-Internals-Git-References
* https://alx.github.io/gitbook/7_raw_git.html
* https://cirosantilli.com/git-tutorial/internals
