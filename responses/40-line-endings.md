Line endings are important for .ind files and occasionally also .R files (really, any git-versioned files that are also `depend`ed on by **scipiper** targets), which get (1) transfered to and from **git**, sometimes with line-ending transformations, and (2) hashed by **scipiper** to determine whether they need to be rebuilt. If **git** changes the line endings (e.g., from Linux/Mac-style LF endings to Windows-style CRLF endings) then the file hash will be different even if the text contents are the same. This can cause unnecessary rebuilds.

You can see which type of line endings you have with `cat -e filename`. `$` at the end means LF (Mac/Posix/Linux style); `^M$` means CRLF (Windows style). Our team has picked LF as the team standard, perhaps out of Mac/Linus snobbiness but mostly because we had to pick one.

To avoid clashes in line endings across operating systems, always do three things (and ideally do them at the START of a project):

* Create a *.gitattributes* file at the top level of your project. Its contents should be:
  ```
  *.ind text eol=lf
  ```
* Git version the *\*.Rproj* file for your repository so that everybody has the same project settings. Within those settings, include the options to (1) insert 2 spaces for tab, (2) ensure that source files end with newline, (3) strip trailing horizontal whitespace when saving, (4) use line ending conversion Posix (LF), and (5) use text encoding UTF-8. These settings (most critically #4) reduce the frequency with which shared code or YAML files are changed just because somebody opens a file on a different operating system, which in turn reduces builds (and has the nice side effects of making code files more uniform and git pull requests cleaner, too). You can edit these options graphically:
  ![image](https://user-images.githubusercontent.com/12039957/95337837-41efc200-0880-11eb-8e44-52e20b6e223a.png)
  and the relevant lines should look like this when you `git commit` the .Rproj file:
  ```yml
  UseSpacesForTab: Yes
  NumSpacesForTab: 2
  Encoding: UTF-8
  AutoAppendNewline: Yes
  StripTrailingWhitespace: Yes
  LineEndingConversion: Posix
  ```
* always use `readr` functions (`write_lines`, `write_csv`, etc.) instead of base R file-writing functions. The former are more consistent in writing LF files.


If you've already starting working in a team repository without following these guidelines, consider making one big team-level switch to the above. This will probably require a rebuild of some or many of your pipeline targets, depending on where your pipeline currently depends on files with CRLF line endings, but after that single rebuild the issue will go away forever, finally, and you will no longer have to suffer unnecessary rebuilds nearly every time someone with a different operating system tries to build the pipeline.
