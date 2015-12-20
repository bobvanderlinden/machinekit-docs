<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="../index-developer.md">Back to the Developer index</a></p></td>
<td align="left"><p><a href="../../index.md">Back to the main Documents index</a></p></td>
<td align="left"><p><a href="../documentation-matrix.md">Documentation matrix</a></p></td>
</tr>
</tbody>
</table>

Buildbot
========

The buildbot grid can be found [buildbot.dovetail-automata.com/grid](http://buildbot.dovetail-automata.com/grid) and this knowledge is very usefull if you make a PR because you can see if or why a build fails.

As you are making a change or improvement you are the one who knows what your change is about. So it should be relatively easy for you to fix your PR in case of a failed build so that your PR can be built.

Here’s an example of a PR which initially failed. It was a PR where a lot of files were renamed from `.txt` to `.md`.

Step 1
------

go to your PR and take note of the number (in this case it was [\#400](https://github.com/machinekit/machinekit/pull/400)). This you need to know if you want to see interpret the buildbot.

Step 2
------

go to the [buildbot grid](http://buildbot.dovetail-automata.com/grid) where you can find the specific PR number. you can clearly see that it failed.

![](images/grid-view.png)

Step 3
------

now click (like the blue link in the picture above) the link to the failed build (\#6). This will give you another link to the `stdio`, the output of the build.

![](images/link-to-stdio.png)

Step 4
------

depending on your experience you search for specific errors. In this case I use `ctrl`+`f` to search for `error` and I pick the last mention. But you might need some different stuff.

![](images/search-stdio.png)

Step 5
------

correct your error (like renaming an line with `.txt` to `.md` reference) and push your commit to the branch for which you made the PR. This will ensure that the updated branch gets rebuilt by the buildbot. Enabling you to see if the correction was sufficient.

![](images/fix-error.png)

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="../index-developer.md">Back to the Developer index</a></p></td>
<td align="left"><p><a href="../../index.md">Back to the main Documents index</a></p></td>
<td align="left"><p><a href="../documentation-matrix.md">Documentation matrix</a></p></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


