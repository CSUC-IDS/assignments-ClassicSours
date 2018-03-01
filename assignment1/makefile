all: gapminder

gapminder:
	Rscript -e "knitr::knit('gapminder.Rmd');rmarkdown::render('gapminder.Rmd','html_document')"

clean:
	rm -rf *.md
	rm -rf *.html
