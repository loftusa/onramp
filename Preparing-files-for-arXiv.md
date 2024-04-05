ArXiving a paper is a special moment. Before you start the process, read [the 10 steps to follow](Ten-Steps-for-arXiving-a-Paper). Then when it's time to physically post the paper on arXiv, use the technical guidelines on this page for wrestling with the arXiv LaTeX system.

## ArXiv restrictions on LaTeX

Uploading files on arXiv requires you to upload the raw latex source.  However, the latex source that you need is slightly different from what overleaf will produce or accept for you, because:

  * There is a size limit of 10mb for any file on arXiv.
  * ArXiv will run LaTeX to compile, but it will not run bibtex.  Therefore your project must be packaged with the bbl file.
  * You probably want to strip out unused stuff.

In addition you'll want to follow the latex formatting advice on [this arXiv help page](https://info.arxiv.org/help/submit_tex.html)

## Avoiding the arXiv "hold" error

ArXiv will sometimes get a latex compilation error after you successfully submit, and then fail to publish your submission the next day.  We think this might often be caused by failure of automatic detection that we are using pdflatex processing. So be sure to follow the following advice from [the arXiv help page](https://info.arxiv.org/help/submit_tex.html):

> arXiv fully supports and automatically recognizes PDFLaTeX. **You can ensure pdflatex processing by setting the flag \pdfoutput=1 within the first 5 lines of the preamble of the main .tex file.** You should not need any other special flag.


## Using arxiv_latex_cleaner

There is a nice script to remove unused stuff and deal with image size limits, called `arxiv_latex_cleaner`.

  1. Use python 3 (e.g., set up a python 3 conda environment)
  2. `pip install arxiv-latex-cleaner`
  3. Make sure your main tex file in overleaf is in the top-level directory and called `main.tex`.
  4. `git clone https://git.overleaf.com/[63f24a12347123469211234] my_paper`
  5. `./make_arxiv.sh my_paper`

The following script is the `make_arxiv.sh` script.  It runs bibtex and latex iteratively to get all the references resolved, and it runs arxiv_latex_cleaner with good settings; then it zips up the results for uploading to arXiv.


```
#!/bin/bash

# To make a my_paper_arXiv.zip, grab your overleaf source, then run this script on it.
# git clone https://git.overleaf.com/63f24a12347123469211234 my_paper
# ./make_arxiv.sh my_paper

DIR="$1"

rm -rvf ${DIR}_arXiv ${DIR}_arXiv.zip
pushd ${DIR}
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
popd
python -m arxiv_latex_cleaner --compress_pdf --pdf_im_resolution 600 ${DIR}/
zip -r ${DIR}_arXiv ${DIR}_arXiv
pushd ${DIR}_arXiv
pdflatex main.tex
pdflatex main.tex
cp main.pdf ../${DIR}_arXiv.pdf
popd
```

We've noticed a couple little quirks in the tool (add more tips here if you encounter issues).
   * Make sure the main tex file is called `main.tex`.
   * When you have references between files in latex, do not quote them.

## When to upload things on arXiv

By the way, you don't need to worry too much when testing your upload on arXiv; you should go ahead and try, and then feel free to erase things when it's not looking the way you want. There are many steps before anything uploaded is posted in public, and even after you go through the steps, it won't be posted for about a day.  (However: once it is posted, it is permanent and you are not allowed to take it down - so do be careful what you post.)

One tidbit of lore: arXiv's daily cutoff time is 2pm ET daily, and the last things to be posted are the first ones to appear on the `recent new articles` lists, so if you want your post to be visible, the best time to hit the final "submit" button in the last step on arXiv is 1:59:59pm ET. Some people think the best day to do that is Thursday, since it will visible top on the recent lists all weekend.

## Keeping and sharing your arXiv paper password

After your paper is up on arXiv, they will send an email to you containing your "paper password" which will allow you to do things like post a revision of your paper (you cannot take down your original revisions, but you can post updated versions that will become the default, and all the versions will be available to the public).

You should forward this email with your paper password with all your coauthors so that they can register themselves as coauthors.

