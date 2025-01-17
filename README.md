## Cyper updates

1. upgrade puppeteer to latest 2022 edition
2. waitFor -> waitForTimeout, and change time unit to second
3. add a new `lastDocURL` paramater

usage:

```bash
# compile
npm run build
npm link --force

# usage quick
node lib/cli.js \
--initialDocURLs="http://localhost:3000/docs/intermediate/BFF/bff-prepare" \
--lastDocURL="http://localhost:3000/docs/intermediate/BFF/bff-verification" \
--paginationSelector=".pagination-nav__item--next > a" \
--contentSelector="article" \
--coverImage="http://localhost:3000/img/docusaurus.png" \
--coverTitle="Java Backend Developer Guide" \
--coverSub="-- for BFF" \
--outputPDFFilename="MyBook.pdf"


# usage full
mr-pdf \
--initialDocURLs="http://localhost:3000/docs/intermediate/BFF/bff-prepare" \
--lastDocURL="http://localhost:3000/docs/intermediate/BFF/bff-verification" \
--contentSelector="article" \
--paginationSelector=".pagination-nav__item--next > a" \
--excludeSelectors=".margin-vert--xl a,.theme-doc-footer" \
--coverImage="http://localhost:3000/img/docusaurus.png" \
--coverTitle="Java Backend Developer Guide" \
--coverSub="-- for BFF" \
--waitForRender=3 \
--outputPDFFilename="Java Backend Developer Guide 03 - BFF.pdf"

```

## TOC

```html
<div class="toc-page" style="page-break-after: always;">
  <h1 class="toc-header">Table of contents:</h1>
  <ul class="toc-list">
    <li class="toc-item toc-item-1" style="margin-left:0px">
      <a href="#9pcyx-0">准备bff项目</a>
    </li>
    <li class="toc-item toc-item-2" style="margin-left:20px">
      <a href="#bhbrd-1">导入 bff​</a>
    </li>
    <li class="toc-item toc-item-1" style="margin-left:0px">
      <a href="#fe12j-2">Service层(bff)</a>
    </li>
    <li class="toc-item toc-item-2" style="margin-left:20px">
      <a href="#er582-3">创建 TodoClient​</a>
    </li>
    <li class="toc-item toc-item-1" style="margin-left:0px">
      <a href="#ldlth-4">Controller层(bff)</a>
    </li>
    <li class="toc-item toc-item-2" style="margin-left:20px">
      <a href="#dfpsz-5">创建 TodoController​</a>
    </li>
    <li class="toc-item toc-item-1" style="margin-left:0px">
      <a href="#w6t7a-6">验证bff项目</a>
    </li>
  </ul>
</div>
```

## Bookmark

观望(js)： https://github.com/Hopding/pdf-lib

暂时的方案（python)：https://github.com/RussellLuo/pdfbookmarker

等待 Puppeteer 支持： https://github.com/puppeteer/puppeteer/issues/1778

pagedjs: https://www.pagedjs.org/posts/2020-02-19-toc/

puppeteer + parse(mozilla/pdf.js) + merge(rkusa/pdfjs): https://medium.com/@pofider/generate-pdf-with-toc-using-chrome-c3b44f924ff9

The fundamental idea for adding TOC is the following…

1. 👉 Prepare html with TOC at the top, just skip the page numbers. Include the anchors so the TOC links are clickable and navigates reader to the particular chapter (mr-pdf 已经实现)
2. 👉 Let chrome convert this html into pdf (mr-pdf 已经实现)
3. 👉 Parse the output pdf and find out the page numbers of the particular chapters
4. 👉 Prepare another html which represents just TOC, but this time it will also include page numbers which we parsed in the previous step.
5. 👉 Convert the TOC html into pdf using chrome
6. 👉 Merge both pdfs together.
7. 👉 Now we have pdf with the TOC at the beginning.

## 📌 Introduction

This is a PDF generator from document website such as `docusaurus`, `vuepress`, `mkdocs`.

## ⚡ Usage

```shell
npx mr-pdf --initialDocsURL="https://example.com" --paginationSelector="li > a"
```

## 🍗 CLI Options

**❗NEED DOCS UPDATE!**

- `--initialDocsURL`: set url to start generating PDF from.

- `--paginationSelector`: used to find next page to be printed for looping.

- `--excludeSelectors`: exclude selectors from PDF. Separate each selector **with comma and no space**. But you can use space in each selector. ex: `--excludeSelectors=".nav,.next > a"`

- `--cssStyle`: css style to adjust PDF output ex: `--cssStyle="body{padding-top: 0;}"` \*If you're project owner you can use `@media print { }` to edit CSS for PDF.

- `--outputPDFFilename`: name of the output PDF file. Default is `mr-pdf.pdf`.

- `--pdfMargin`: set margin around PDF file. Separate each margin **with comma and no space**. ex: `--pdfMargin="10,20,30,40"`. This sets margin `top: 10px, right: 20px, bottom: 30px, left: 40px`.

- `--pdfFormat`: pdf format ex: `--pdfFormat="A3"`. Please check this link for available formats [Puppeteer document](https://pptr.dev/#?product=Puppeteer&version=v5.2.1&show=api-pagepdfoptions)

- `--disableTOC`: Optional toggle to show the table of contents or not.

- `coverTitle`: Title for the PDF cover.

- `coverSub`: Subtitle the for PDF cover. Add \<br/> tags for multiple lines.

## 🎨 Examples and Demo PDF

### Docusaurus v1

https://docusaurus.io/en/

`initialDocsURL`: https://docusaurus.io/docs/en/installation

`demoPDF`: https://drive.google.com/file/d/1HK5tBKmK0JBsFMNwoYRB9fDs9rkJhGRC/view?usp=sharing

`command`:

```shell
npx mr-pdf --initialDocsURL="https://docusaurus.io/docs/en/installation" --paginationSelector=".docs-prevnext > a.docs-next" --excludeSelectors=".fixedHeaderContainer,footer.nav-footer,#docsNav,nav.onPageNav,a.edit-page-link,div.docs-prevnext" --cssStyle=".navPusher {padding-top: 0;}" --pdfMargin="20"
```

### Docusaurus v2 beta

![20210603060438](https://user-images.githubusercontent.com/29557494/120552058-b4299e00-c431-11eb-833e-1ac1338b0a70.gif)

https://docusaurus.io/

`initialDocURLs`: https://docusaurus.io/docs

`demoPDF`:
https://drive.google.com/file/d/12IXlbRGKxDwUKK_GDy0hyBwcHUUell8D/view?usp=sharing

`command`:

```shell
npx mr-pdf --initialDocURLs="https://docusaurus.io/docs/" --contentSelector="article" --paginationSelector=".pagination-nav__item--next > a" --excludeSelectors=".margin-vert--xl a" --coverImage="https://docusaurus.io/img/docusaurus.png" --coverTitle="Docusaurus v2"
```

### Vuepress

https://vuepress.vuejs.org/

`initialDocsURL`:

https://vuepress.vuejs.org/guide/

`demoPDF`: https://drive.google.com/file/d/1v4EhFARPHPfYZWgx2mJsr5Y0op3LyV6u/view?usp=sharing

`command`:

```shell
npx mr-pdf --initialDocsURL="https://vuepress.vuejs.org/guide/" --paginationSelector=".page-nav .next a" --excludeSelectors="header.navbar,aside.sidebar,footer.page-edit .edit-link,.global-ui,.page-nav"
```

### Mkdocs

https://www.mkdocs.org/

`initialDocsURL`: https://www.mkdocs.org/

`demoPDF`: https://drive.google.com/file/d/1xVVDLmBzPQIbRs9V7Upq2S2QIjysS2-j/view?usp=sharing

`command`:

```shell
npx mr-pdf --initialDocsURL="https://www.mkdocs.org/" --paginationSelector="ul.navbar-nav li.nav-item a[rel~='next']" --excludeSelectors=".navbar.fixed-top,footer,.homepage .container .row .col-md-3,#toc-collapse" --cssStyle=".col-md-9 {flex: 0 0 100%; max-width: 100%;}"
```

### Material for mkdocs

https://squidfunk.github.io/mkdocs-material/

`initialDocsURL`: https://squidfunk.github.io/mkdocs-material/getting-started/

`demoPDF`: https://drive.google.com/file/d/1oB5fyHIyZ83CUFO9d4VD4q4cJFgGlK-6/view?usp=sharing

`command`:

```shell
npx mr-pdf --initialDocsURL="https://squidfunk.github.io/mkdocs-material/getting-started/" --paginationSelector="a.md-footer-nav__link--next" --excludeSelectors="header.md-header,.announce,nav.md-tabs,.md-main__inner .md-sidebar--primary,.md-main__inner .md-sidebar--secondary,footer" --cssStyle=".md-content {max-width: 100%!important;}"
```

#### PR to add new docs is welcome here... 😸

## 📄 How this plugin works

This plugin uses [puppeteer](https://github.com/puppeteer/puppeteer) to make PDF of the document website.

![mr-pdf-diagram](https://user-images.githubusercontent.com/29557494/90359040-c8fb9780-e092-11ea-89c7-1868bc32919f.png)

## 🎉 Thanks

This repo's code is coming from https://github.com/KohheePeace/docusaurus-pdf.

Thanks for awesome code made by [@maxarndt](https://github.com/maxarndt) and [@aloisklink](https://github.com/aloisklink).

[@bojl](https://github.com/bojl) approach to make TOC was awesome and breakthrough.
