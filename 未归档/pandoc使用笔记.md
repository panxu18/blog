1. 处理参考文献

```bash
pandoc --filter pandoc-citeproc --bibliography=ref.bib --csl=chinese-gb7714-2005-numeric.csl demo-citation.md -o demo-citation.docx
pandoc --filter pandoc-citeproc --bibliography=ref.bib --csl=/home/xupan/Documents/doc/styles/chinese-gb7714-2005-numeric.csl 小论文初稿.md -o xiaolunwen.docx
pandoc --filter pandoc-noxs 小论文初稿.md -o xiaolunwen.docx
```

2. pypandoc https://github.com/bebraw/pypandoc
3. pandoc-eqnos https://github.com/tomduck/pandoc-eqnos
4. pandoc-crossref http://lierdakil.github.io/pandoc-crossref/

