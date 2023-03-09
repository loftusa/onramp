Uploading files on arXiv requires you to upload the raw latex source.  However, the latex source that you need is slightly different from what overleaf will produce or accept for you, because:

  * There is a size limit of 10mb for any file on arXiv.
  * ArXiv will run LaTeX to compile, but it will not run bibtex.  Therefore your project must be packaged with the bbl file.
  * You probably want to strip out unused stuff.

There is a nice script to remove unused stuff and deal with image size limits, called `arxiv_latex_cleaner`.

  1. Use python 3 (e.g., set up a python 3 conda environment)
  2. `pip install arxiv-latex-cleaner`
  3. `git clone https://git.overleaf.com/[63f24a12347123469211234] my_paper`
  4. `./make_arxiv.sh my_paper`

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
