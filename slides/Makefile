# The deck. What to SAY is in script.md, which needs no build.
#
#   make        -> build/main.pdf
#   make clean

.PHONY: all clean

all:
	latexmk main.tex

clean:
	latexmk -C
	rm -rf build
