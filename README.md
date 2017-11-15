# What is it?

GHCI offers a :cmd directive that lets you evaluate the contents of a file. It cannot process multi-line expressions unless they are bracketed by (on separate lines) ":{" and ":}". Adding those brackets by hand is boring, and it confuses the indentation routines in at least some (maybe all?) editors.

readHsAsGhciCmd lets you pass ordinary Haskell-formatted single- and multi-line expressions, without those surrounding brackets, to `:cmd`. Use it on a file like this `:cmd ReadHsAsGhciCmd.main "folder/filename.hs"`.

If you plan on using it a lot, you could make a macro:
```
:def! . ReadHsAsGhciCmd.main -- in this case the macro is named "."
```
and then call the macro like this:
```
:. folder/file.hs
```
(Note that no quotation marks surround the filepath in the macro.)


# Known issues

This code assumes "--" indicates a comment. If there was an operator that began with "--", and it was the first thing on a line, the whole line would be ignored.


# Where's the commit history?

It's in my [fork of Tidal](https://github.com/JeffreyBenjaminBrown/Tidal/commit/fa99bbb86ff05ed494fd5bb15f1f26b7e27dbcb9).
