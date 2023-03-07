Some thoughts on making high-quality figures for publication.

The key is to preserve as much resolution in the your figure-production pipeline going to the final pdf, i.e., you want to be careful not to accidentally pixelate text or reduce resolution of images.

The easiest and most reliable pipeline I have found involves using Powerpoint to do final layout of figures, and then exporting from Powerpoint to pdf, and then importing pdfs of the figures into LaTeX.  This gives you good artistic control while still reliably keeping all the things like splines and lines and arcs and fonts as smooth line-art.  There are a few other details.  Here is what I suggest, in detail:

  1. Generate images as pngs (not jpegs, if you can avoid them).  Generate matplotlib plots as pdfs.
  2. Insert the pngs and pdfs into a single-slide Powerpoint slide deck, and lay things out neatly, and resize and add things like text and arrows and dividers as needed.  Be sure to use the Powerpoint "Align to top/left/right/left" features and other precision-layout controls rather than just aligning things by eye.  I often find that it is convenient to expand the powerpoint slide size to 25 inches by 25 inches to make lots of space for editing.
  3. Save the pptx file in the project folder (e.g., upload it to overleaf) to preserve the original "source".  Then print-to-pdf or export from powerpoint to a single-page pdf.
  4. Crop the pdf tightly with the `pdfcrop` command-line tool.  If you don't have this on your machine, you can copy the pdf to any of the cluster machines; we have it there.
  5. Use \includegraphics in latex to bring the cropped pdf into latex, copying a previous example.