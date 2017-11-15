GitHub-Flavoured Markdown to HTML Conversion
==============================================================================

Takes a directory of GitHub-flavoured markdown documents and converts them to HTML documents.

* Applies styles similar to those used when viewing online at GitHub.
* Performs a simple search/replace to maintain relative links between files.
* For the HTML document title we use whatever is on the first line of the markdown file.

## **INCOMPLETE**

Doesn't work well/at all with syntax highlighting in code blocks.

Related project might be of help..?
https://github.com/Voleking/pandoc-templates


## Prerequisites

You'll need [Pandoc](https://pandoc.org):

```sh
# MacOS install with brew
brew install pandoc
```

## Process

Something like this:

```sh
# Edit these
INPUT_DIR=/Users/molomby/codebases/molomby/md2html-convertion/
OUTPUT_DIR=/Users/molomby/temp/groki-docs/
TEMPLATE_DIR=/Users/molomby/codebases/molomby/md2html-convertion/

cp ${TEMPLATE_DIR}styles.css ${OUTPUT_DIR}

for FILEPATH in $INPUT_DIR*.md; do
  FILE=$(basename "$FILEPATH")
  TITLE=$(head -n 1 ${INPUT_DIR}${FILE})

  printf "Processing ${FILE} ..."

  pandoc --standalone --from gfm --to html \
    --metadata=title:$TITLE \
    --template=${TEMPLATE_DIR}template.html \
    ${INPUT_DIR}${FILE} |\
    sed 's/\(href="[^"\.]*\)\.md/\1.html/g' >\
    ${OUTPUT_DIR}${FILE%.*}.html

  printf "done\n"
done
```
