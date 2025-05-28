# How to prepare a response for the referees (PASP)

## General considerations
- Address all of the referees’ comments.
- Using inclusive or gender-neutral titles.
- If you are using LaTeX you can use the “latexdiff” command.
- Referees voluntarily give up their time to review manuscripts. We recommend that you thank the reviewers for their time and input.
- Note that there is an incompatibility between amsmath.sty and iopart.cls which cannot be completely worked around. If your article relies on commands in amsmath.sty that are not available in iopart.cls, you may wish to consider using a different class file.


## What files to submit for your revised article
- A document (Word file) containing a list of all the changes made (if your changes are not highlighted in the manuscript) and a point-by-point response to each referee comment.
- A PDF of the complete revised manuscript (designated 'Complete Document for Review (PDF Only)', both a clean version of the revised manuscript, and also a version with the changes highlighted). 
- The latest set of source files, e.g. TeX/LaTeX files (which includes figure/table captions) and individual figure files. You may combine all your files into a single compressed zip file. To upload this file type, choose the ‘source files’ designation when you submit. 


## Naming your files
- Use only characters from the set a to z, A to Z, 0 to 9 and underscore - Do not use spaces in file names
- Do not use any accented characters (for example, à, ê, ñ, ö, ý, etc)
- Name figure files indicating the numbers of the figures in the text(ex: figure1.eps). If a file contains a figure with multiple parts, give it a name such as figure2a_2e.jpg, and so forth.

## Addons to the source files
-  List of all the changes made 
-  Polite point-by-point response to each referee comment 
    -   You should copy each referee comment into a separate document and add a response below each comment (and refer to the manuscript line numbers when referring to changes in the main text)
    - For each point, take the time to detail exactly what you have changed (quote the exact text before and after)
    - You should justify your responses
    - Any additional material should appear in the manuscript.

## latexdiff
- I had some problems dealing with this library. I needed to add the follwoing commando to the preamble of the main.tex file (before \begin{document}):


```latex
\providecommand{\DIFadd}[1]{\textcolor{blue}{#1}} % Change added text color to blue
\usepackage[normalem]{ulem}
\providecommand{\DIFdel}[1]{\begingroup\color{red}\dashuline{#1}\endgroup}
\sloppy
```

## Useful links
- [How to prepare your revised article](https://publishingsupport.iopscience.iop.org/questions/how-to-prepare-your-revised-article/)
- [Preparing your source files for journal articles](https://publishingsupport.iopscience.iop.org/questions/preparing-your-source-files-for-journal-articles/)
- [Author rights and article sharing](https://publishingsupport.iopscience.iop.org/author-rights-policies/)
- [PASP Author Rights](https://iopscience.iop.org/journal/1538-3873/page/PASP_author_rights)
- [Software citation](https://www.tomwagg.com/software-citation-station/)
- [ASCL](http://ascl.net/)
- [Journal of Open Source Software](https://joss.theoj.org/)