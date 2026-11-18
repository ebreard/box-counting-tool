# Box Counting Tool

Box-counting (Minkowski-Bouligand) dimension of outlines and sets in images, in one HTML page.
E.C.P. Breard (School of GeoSciences, University of Edinburgh) and
Dr Johan (Yoshi) Gilchrist (Dartmouth College).

## Open it

Download `boxcount_tool.html` (on GitHub: open the file, then Download raw file) and
double-click it. It is one self-contained file (about 440 kB): the code, the validation
references and the logo are inside it, and images are read in the browser, never uploaded.
The IBM Plex fonts come from Google Fonts when online and fall back to system fonts when
offline. Checked in a Chromium browser, not yet in Firefox or Safari.

### In the browser, on GitHub

This repository is private, so there is no public web link. The button below starts a
GitHub Codespace under your own account (it uses your free monthly Codespaces hours),
serves the page and opens it in a new browser tab. Sign in to GitHub with access to this
repository; the first start takes about a minute.

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/ebreard/box-counting-tool)

If no tab opens (a pop-up blocker can stop it), open the Ports tab and click the globe
beside port 8000. Images you load stay in your browser; the codespace only serves the page.
Stop the codespace from github.com/codespaces when you are done; it also stops itself when
idle.

## Use it

1. Open image (or drop or paste one).
2. Draw the mask with the brush, polygon or rectangle (the eraser removes). For the
   dimension of one outline, draw a band along the edge with both sides of it inside; a
   mask drawn inside the object measures the texture within it.
3. Choose the channel, the threshold (Otsu button: Otsu inside the mask) and the mode.
   Interface counts the boxes that hold the boundary between the two classes; Set counts
   the boxes that hold any foreground pixel.
4. Analyse: box counts N(s) at 16 grid origins, the local slope, D over the automatic
   straight window, and a quality flag. Threshold scan: D against the threshold level.
5. Export: CSV, PNG of the plots, JSON, or a text summary.

## Help me choose

- **Suggest settings** picks the channel, blur, mode, smallest box and work resolution
  from rules fixed in advance that never look at D, and warns when the mask holds a
  texture rather than one outline. **Apply and analyse** sets them and runs the fit.
- **Check robustness** reruns the fit about a dozen times (threshold moved so that the
  area above it changes by 5 and 10 % of the mask either way, blur 1 px up and down, half
  resolution, smallest box x2 and x4, each half of the mask, and the other mode for
  information) and gives a verdict (robust, moderately sensitive, sensitive), the ± to
  quote and a dot plot.
- **Calibrate with synthetic outlines** sends level sets of Gaussian random fields of
  known dimension (4/3, 3/2, 5/3; four seeds each) through the same mask, area share,
  blur, mode and fit window, and says whether this chain ranks and separates them and
  where the current D falls among them.

The Method tab writes out every rule. Quote a D only when the verdict is robust and the
calibration ranks and separates the three dimensions.

## Reading a result

- D is the box-counting (Minkowski-Bouligand) dimension over a finite range of box sizes.
  It equals the Hausdorff dimension for self-similar sets that satisfy the open set
  condition; for random sets such as turbulent outlines that equality is an assumption.
- On test sets of known dimension D is off by up to 0.1, usually low: quote it to ±0.1 at
  best. D_err measures how stable the fit is, not how right it is.
- A flagged fit (no straight range of log N against log s) is not a dimension; the page
  then withdraws the comparison with 4/3 and 5/3.
- A thresholded texture of many pieces gives an average over a crossover between scales.
  Set mode on a texture counts area as well as outline, so it does not follow the
  roughness of an outline.

## Validation

The Validation tab rebuilds 165 test images in the page (Sierpinski carpet, Vicsek cross,
Koch snowflake outline and two jet masks, 24 to 2187 px), counts each with this tool and
with a zero-padded power-of-2 counter like the one of D'Orazio et al. (2025, Experiments in
Fluids 66:70), and compares both with the exact D and with values from the Python
implementation stored in the page. All 165 agree with Python; a run takes 10 to 20 s.

## Source

This repository holds the page and this file. The Python implementation, its benchmark and
the tests of the page are kept by E.C.P. Breard; ask for them.
