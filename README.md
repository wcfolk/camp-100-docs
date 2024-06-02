# Camp 100 Documents

> This repository stores the documents produced for Camp 100, which have been typeset using LaTeX

## Jump To Section
* [Folder Structure](#folder-structure)
* [Compiling Documents](#compiling-documents)
* [The Templates](#the-templates)
* [Custom Document Elements](#custom-document-elements)
* [Watermarks](#watermarks)
* [Styling](#styling)
* [References](#references)

## Folder Structure
Each document is stored within it's own folder in the repository. Different versions of the same document are also stored in the same folder - as to keep all of one document in one place.

Within each folder, the main document which gets compiled should be called `main.tex`, to achieve consistency between the documents. It's up to the person who is typesetting to decide if they want to place all the document's content into the `main.tex` file or to make a folder called `sections` and put the content in suitably named `.tex` files within there. Generally good practice is to use the subfolder method for long documents as this keeps the project tidy.

Assets for each document are to be stored within a `assets` folder within the document's folder.

There are a number of assets which all the documents will require access to, for example the logos used on the front & back pages as well as the global preamble. These are stored in the root of the repository with their names prepended with `global-`. 

Names of all files and folders should be lowercase with hyphens used in place of spaces where required. 

## Compiling Documents
The documents are to be compiled using LuaLaTeX. The documents will require a compile chain of at least two compiles, as this ensures cross-references (page numbering, table of contents, etc) and the graphics are correct. The compiler should complain if it thinks you need to re-run the compilation.

The documents are setup to use the Quicksand font, as in line with the Camp 100 style guides. You will need to install this font on your computer to be able to compile the document and ensure that LuaLaTeX is able to identify where the font has been installed. If this is in a non-standard place, you will need to indicate this in the preamble. 

### Installing Quicksand
It is suggested to install the Dafont distributed variant of Quicksand as this comes with a Light-Oblique variant of the font which replicates italics to an acceptable degree. Quicksand does not come with an italic variant from any font distributor; and the Google Docs implementation of Quicksand does not contain a italic variant, rather it leans the font slightly - replicating italics. [Download Quicksand from Dafont](https://www.dafont.com/quicksand.font).

On a Fedora 39 (KDE) machine, the Quicksand default installation path, which LuaLaTeX was able to identify is as follows:
```sh
/usr/local/share/fonts/q
```

## The Templates
There are two templates available, a shorter and a longer template. Both use the same front title page and back title page.

The templates are setup such that the only thing which differentiates between them is the document class provided to LaTeX's `\documentclass{}` command. Examples of the two document templates are given in their respective sections, both use common placeholder text:
* `[doc title]` - the title of the document
* `[doc subtitle]` - the subtitle of the document
* `[publish date]` - the date at which the document will be published
* `[internal doc id]` - an internal id used to represent the document, for example `payment-policy-v1a`
* `[version]` - the version of the document (see [Watermarks](#watermarks) section below)
* `[content]` - the content in the document

### Long Document Template
The long document template uses the `report` LaTeX class, thus allowing us to use Chapters & Parts. Chapters are, realistically, the top-most heading that we will need to use; they are setup to force a new page at the start of a chapter and add a graphical element in the top right hand corner of the page in keeping with the overall document styling. [Example of the Long Document Template](https://wcfolk.github.io/camp-100-docs/00-templates/dev-doc-report.pdf).

An example of the Long Document Template outline code can be seen below:
```latex
\documentclass[a4paper, 11pt]{report}

\input{../global-preamble.tex}

\documentsetup{[doc title]}{[doc subtitle]}{[publish date]}{[internal doc id]}{[version]}

\begin{document}
\makedocumenttitlepage

\tableofcontents

[content]

\makedocumentbackpage

\end{document}
```

### Short Document Template
The short document template uses the `article` LaTeX class, therefore the top-most heading we can use are Sections. These are styled the same as Sections in the long document therefore there is no page-break before the start of a new section. There is no table of contents in the Short Document Template. [Example of the Short Document Template](https://wcfolk.github.io/camp-100-docs/00-templates/dev-doc-article.pdf).

An example of the short document template outline code can be seen below:
```latex
\documentclass[a4paper, 11pt]{article}

\input{../global-preamble.tex}

\documentsetup{[doc title]}{[doc subtitle]}{[publish date]}{[internal doc id]}{[version]}

\begin{document}
\makedocumenttitlepage

[content]

\makedocumentbackpage

\end{document}
```

## Custom Document Elements
A number of custom elements have been designed for the Camp 100 Document Template as this makes the documents look more visually interesting than the standard LaTeX document.

### Front Page
A front page has been developed using TikZ which inserts coloured bars at the top of the page, the title and other relevant text in the middle then the Woodcraft Folk and Camp 100 logos at the bottom of the page.

The front page is generated using the custom command `\makedocumenttitlepage`, with all the information provided in the preamble using the `\documentsetup` command.

### Back Page
A back page has been developed using TikZ which inserts coloured bars at the top of the page, then a series of information containing boxes at the bottom of the page. The boxes contain some basic information about Camp 100 and Woodcraft Folk as well as social media links for both. At the very bottom of the page is a box which contains some information about the build of the document including the filename which has been compiled, the version and the compile time. 

The back page is generated using the custom command `\makedocumentbackpage`, with all the information provided in the preamble using the `\documentsetup` command.

### Callouts
Callouts have been developed to be used to emphasise content to the reader of the document, three different colours have been made available which use shades of the Camp 100 orange, red and green colours. They are inserted as follows, where `color` is substituted for either `green`, `orange`, or `red`:
```latex
\begin{callout-color}{callout title}
callout content
\end{callout-color}
```

### Internal Links
Due to the desire to apply non-standard styling to links, a command has been developed to be used where an internal link is required. The command will apply the desired styling then pass the content of the link to the `\hyperref` command for that to insert the actual link. The `\internallink` command is used as follows:
```latex
\internallink{sec:link-to}{link text}
\internallink{fig:link-to}{link text (Figure \ref*{fig:link-to})}
```
NB. references within the link text section should be un-linked (ie use the `\ref*{}` command).

### Tables
Tables should be defined using `tabular` environments, which are encased in `table` environments. The contents of the `table` environment should be centred, and the content of the table set to `RaggedRight`. 

The header cells should be defined using the `\tablehead{}`. The body cells of the table do not require special styling, other than a `\hline` after every row other than the header row. There should be no vertical lines in the table.

The column widths should be defined using the `p{decimal\textwidth}` option. The table width, in totality should be between `0.8\textwidth` and `0.9\textwidth`.

An example of a table is shown below:
```latex
\begin{table}[ht]
    \centering
    {\RaggedRight
    \begin{tabular}{p{0.2\textwidth} p{0.2\textwidth} p{0.5\textwidth}}
    \tablehead{Left} & \tablehead{Middle} & \tablehead{Right}\\
    a1 & a2 & a3 \\
    \hline
    b1 & b2 & b3 \\
    \hline
    c1 & c2 & c3\\
    \hline
    d1 & d2 & d3\\
    \hline
    \end{tabular}
    } % end of rr     
    \caption{Example Table}
    \label{tab:example-table}
\end{table}
```

## Watermarks
A watermark has been configured to be automatically applied to the document when it is either in `draft` or `proof` status. This status is set through the `\documentsetup` command (further information on this available in [The Templates](#the-templates)). 

A watermark will not be visible if the document is in any other version, however the document version will be printed on the back page of the document in the document build information box. 


## Styling
LaTeX allows for the majority of the styles in a document to be defined once and used across multiple different documents, which is precisely what we're doing with the global preamble. However, there are some components which have to be styled locally (urgh!). 

### Emphasising Text
As mentioned above, the font which is used doesn't actually have an Italic variant (yes, it does have the Light-Oblique variant which sorta works but does look a bit strange). Therefore, any emphasised text should be made bold. 

### Floats
In an ideal world floats should be placed using the `ht` location. Floats must be centred within their environment using the `\centering` command.

Figures should be landscape and should occupy `0.8\textwidth` horizontal space and tables should be at least `0.8\textwidth` in total, however no more than `0.9\textwidth` (to account for vertical spacing). See [Tables](#tables) above for information on styling of tables and a template.

All floats should have a caption defined below the float. A label is optional, as the necessity for this depends on if you need to be able to reference the float or not.

## References
Producing this template took a lot of google-ing, here's some of the websites which heavily influenced the template / provided relevant, useful, information about a particular component of it:
* [My First Cover Page Done In LaTeX (reddit.com)](https://www.reddit.com/r/LaTeX/comments/faij1n/my_first_cover_page_done_in_latex_is_it/)
* [Customisation of section with TikZ and titlesec (tex.stackexchange.com)](https://tex.stackexchange.com/questions/271563/customization-of-section-with-tikz-and-titlesec)
* [Fancy Chapter Headings with TikZ (texblog.net)](https://texblog.net/latex-archive/uncategorized/fancy-chapter-tikz/)

(I'll try to add to this list as I use other resources, especially useful / harder to find ones!)