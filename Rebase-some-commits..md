**Kevin comment**

> For ease of review, to address Arun's concern, can you please squash your commits into a single commit ?
> You should do this on your existing branch (meaning you will need to use `git push --force` after you squash it), so that you don't need a new PR.
> If you are worried about losing information, you can create a new local branch to save all of your commits.

> Presuming you have an upstream remote pointing to javafxports/openjdk-jfx called 'upstream" the basic recipe is :

```
git fetch upstream
// this will put you into an editor where you can select which commits are squashed
git rebase -i upstream/develop
```

> and then change the `pick` to `squash` in your editor for all commits except the first one (leaving that first commit as 'pick') like this :

But I prefered to use `rewrote` and `fixup`.
- `rewrote` opens my editor (`kwrite`) and allowed me to change de commit message that resumes all next following commits :
   - Bug JDK-8147476, commit of the following :
     FontJava.cpp :
     - Completes the implementation of the platformBoundsForGlyph(Glyph c) function in FontJava.cpp.
     - Previous implementation simply returned a (0,0,0,0) rectangle, which explains the bad rendering height of MathML elements.
     - This affects only MathML elements due to specific mathematic glyphs behavior : WebCore MathML rendering can't use the font ascents and descents which causes gap into assembled glyphs. So getAscent(), getDescent() ... font methods are not appropriate.
     - WebCore uses bearY and heights informations from the glyph bounding box.
    ...
    
```
rewrote 78bf9df Bug JDK-8147476, commit of the following:
fixup 47d9135 Update FontJava.cpp
fixup 91023c1 Update FontJava.cpp
fixup 2c89846 Update FontJava.cpp
...
```

> Alternatively, if you use a GUI front-end to git there will be options to allow you to squash the commits into a single commit.
