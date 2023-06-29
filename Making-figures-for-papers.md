## Figure Design Tips

It is common to make figures with details that are too tiny for your paper.  Keep in mind that the giant space you have in your desktop is not available for your figure in the paper, and it will fit into two or three inches in the end.  So, design your figures to be scaled way down.

  * Make text very large.
  * Make lines very thick.
  * Avoid visual complexity.

A good size to shoot for is to try to make the text in a figure about the same size as the text in the document, or very slightly smaller. Unless you have a good reason to choose a different font, use the same font for your figures as the font of the main paper.  (I.e., do not use the matplotlib default font; use Times.)

In a two-column paper, keep in mind that there are two types of figure layout: the squarish one-column figure, or the wide two-column figure, and plan your layout accordingly.


## The Figure Production Pipeline

Some thoughts on the pipeline for making high-quality figures for publication.

The key is to preserve as much resolution in the your figure-production pipeline going to the final pdf, i.e., you want to be careful not to accidentally pixelate text or reduce resolution of images.

The easiest and most reliable pipeline I have found involves using Powerpoint to do final layout of figures, and then exporting from Powerpoint to pdf, and then importing pdfs of the figures into LaTeX.  This gives you good artistic control while still reliably keeping all the things like splines and lines and arcs and fonts as smooth line-art.  There are a few other details.  Here is what I suggest, in detail:

  1. Generate matplotlib plots or other line-art as pdfs.  Generate pixel images as pngs (not jpegs, if you can avoid them).  
  2. Insert the pngs and pdfs into a single-slide Powerpoint slide deck, and lay things out neatly, and resize and add things like text and arrows and dividers as needed.
  3. Do draw on your plots!  E.g., when appropriate, go ahead and add a circle around the important data point that you want people to see.  It's often best to make legends by hand instead of taking matplotlib's legends, same for axis labels and titles.
  4. Be sure to use the Powerpoint "Align to top/left/right/left" features and other precision-layout controls rather than just aligning things by eye.  I often find that it is convenient to expand the powerpoint slide size to 25 inches by 25 inches to make lots of space for editing.
  5. Save the pptx file in the project folder (e.g., upload it to overleaf) to preserve the original "source".  Then print-to-pdf or export from powerpoint to a single-page pdf.
  6. Crop the pdf tightly with the `pdfcrop` command-line tool.  If you don't have this on your machine, you can copy the pdf to any of the cluster machines; we have it there.
  7. Use \includegraphics in latex to bring the cropped pdf into latex, copying a previous example.