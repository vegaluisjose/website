luisvega-cv.pdf: luisvega-cv.tex
	latexmk -pdf -pdflatex='pdflatex -interaction=nonstopmode -halt-on-error' luisvega-cv.tex

.PHONY: clean
clean:
	latexmk -C
