# Playing with `git` internals

Here I'm playing with `git` internals.

## Selecting a subset of the tree

I'm using `git ls-tree -r HEAD a/ab/5 c/3 b/2` to select a few objects from the tree. I then intend to use `git mktree` to 
create a tree, but don't associate a commit or anything "semantic" like that. My end goal is to `ref` to said subtree.

## References

* https://jwiegley.github.io/git-from-the-bottom-up/1-Repository/4-how-trees-are-made.html
* https://github.com/git/git/blob/2d677e5b1537692a4f875740a6e9853c24285f02/refs.c#L847
* https://git-scm.com/docs/git-for-each-ref
* https://git-scm.com/book/en/v2/Git-Internals-Git-References
