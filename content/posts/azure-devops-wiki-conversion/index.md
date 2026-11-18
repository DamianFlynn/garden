---
title: Index En
date: '2026-07-25'
draft: true
slug: azure-devops-wiki-conversion
author: damian
authorLink: https://linkedin.com/in/damianflynn
neocortex_id: areas/damianflynn/content/azure-devops-wiki-conversion/index.md
neocortex_source_sha: 8e0fc731e3ad8d891eb050cd199b78d22b34f0dbae3ca324f712be3d27e3b614
description: ''
tags: []
---

# Azure DevOps Wiki Conversion

Clone down a copy of the wiki, 


```powershell
function Get-DocfxFolderItems {
	param(
		[string]$Path,
		[string]$Include = '*.md',
		[string[]]$ExcludeFolders
	)

	# Get .order file in Path, if it exist
	$orderFile = Join-Path -Path $Path -ChildPath '.order';

	$orderContent = @(
		if (Test-Path -Path $orderFile -PathType Leaf) {
			Get-Content -Path $orderFile;
		}
	)

	# Set lastItem to add items not in .order file

$lastItem = $orderContent.Count;

# Get all the files in Path, no recurse

$files = @(Get-ChildItem -Path $Path -File -Include $Include -Recurse -Depth 0);

# Get items for toc file

# Process files in the Path

foreach ($file in @($files | Sort-Object -Property Name)) {

# Get the order of the item as specified in .order file

$order = $orderContent.IndexOf($file.BaseName);

# If item was not in .order file

if ($order -eq -1) {

# Set order to last item

$order = $lastItem;

# Increase last item by one

$lastItem++;

}

# The DevOps Wiki allows special characters in file and folder names, but this is not supported by docfx build

if ($file.BaseName -match '\%') {

Write-Warning -Message "The '%' character is unsupported by docfx build: '$($file.FullName)'";

}

if ($ExcludeFolders -notcontains $file.BaseName) {

$childFolder = Join-Path -Path $Path -ChildPath $file.BaseName;

$items = @(

# If a folder exist with same name as file basename

if (Test-Path -Path $childFolder -PathType Container) {

# Get child items of that folder

Get-DocfxFolderItems -Path $childFolder -Include $Include -ExcludeFolders $ExcludeFolders;

}

)

$href = $file.FullName.Replace($Script:BasePath, '').Replace('\', '/').Trim('/');

# Create the item object

[pscustomobject]@{

Order = $order;

Name = $file.BaseName -replace '-', ' ' -replace '%3A', ':' -replace '%3C', '<' -replace '%3E', '>' -replace '%2A', '*' -replace '%3F', '?' -replace '%7C', '|' -replace '%2D', '-' -replace '%22', '"';

Href = $href;

Items = $items;

#TopicHref = '';

}

}

}

}

function Get-DocfxItem {

param(

[pscustomobject]$Item,

[int]$Depth = 0

)

if ($Depth -eq 0) {

$spaces = '';

$content = "- $($Item.hRef)";

} else {

#$spaces = ''.PadLeft($($Depth * 2), ' ');

$spaces = ''.PadLeft($(1 * 2), ' ');

$content = "`n";

$content += "$($spaces)- $($Item.Href)";

}

if ($Item.Items.Count -gt 0) {

$items = @(

foreach ($child in $Item.Items | Sort-Object -Property Order) {

Get-DocfxItem -Item $child -Depth $($Depth + 1);

}

)

$content += "`n";

$content += "$($spaces) $($items)";

}

$content;

}

  
  

function Invoke-DocfxTocProcessing {

param(

[string]$Path,

[string]$Include = '*.md',

[string[]]$ExcludeFolders = @()

)

# Todo:

# read .md files and get metadata: title and order

# support replace for custom documents to customer

#

# Example:

# Invoke-DocfxTocProcessing -Path 'C:\Repos\innofactor\Internal\Docs\markdown'

  

# Get the folders in Path (no recurse)

$ExcludeFolders += '.*';

$ExcludeFolders += 'obj';

$ExcludeFolders += 'pdf';

$basePaths = @(Get-ChildItem -Path $Path -Directory -Exclude $ExcludeFolders -Recurse -Depth 0);

  

# Create toc file in root Path

$tocPath = Join-Path -Path $Path -ChildPath 'pandoc.yml';

Write-Host -Object "Creating '$($tocPath)'";

$i = 0;

$tocContent = $basePaths | ForEach-Object {

$i++;

if ($i -eq $basePaths.Count) {

" - $($_.Name)/";

} else {

" - $($_.Name)/`n";

}

}

$null = Set-Content -Path $tocPath -Value "$($tocContent -join '')" -Force;

  

# Create toc files in each basepath of root path

foreach ($basePath in $basePaths) {

$Script:BasePath = $basePath.FullName;

$items = Get-DocfxFolderItems -Path $basePath.FullName -Include $Include -ExcludeFolders $ExcludeFolders;

$tocPath = Join-Path -Path $basePath.FullName -ChildPath 'toc.yml';

$tocContent = $(

foreach ($item in @($items | Sort-Object -Property Order)) {

$content = Get-DocfxItem -Item $item;

"$content`n";

}

)

# Get the file in root Path that match basepath Name

$basePathFiles = @(Get-ChildItem -Path $Path -File -Include "$($basePath.Name).md" -Recurse -Depth 0);

$tocContent = $(

if ($basePathFiles.Count -eq 1) {

" - ../$($basePathFiles[0].Name)`n$($tocContent -join '')";

} else {

"$($tocContent -join '')";

}

);

  

Write-Host -Object "Creating '$($tocPath)'";

$null = Set-Content -Path $tocPath -Value "$tocContent" -Force;

}

}
```


## Pandoc Defaults File
The [`--defaults`](https://pandoc.org/MANUAL.html#option--defaults) option may be used to specify a package of options to apply on our conversion, in the form of a YAML file.

Fields that are omitted will just have their regular default values. So a defaults file can be as simple as one line:

```
verbosity: INFO
```

In fields that expect a file path (or list of file paths), the following syntax may be used to interpolate environment variables:

```
csl:  ${HOME}/mycsldir/special.csl
```

`${USERDATA}` may also be used; this will always resolve to the user data directory that is current when the defaults file is parsed, regardless of the setting of the environment variable `USERDATA`.

`${.}` will resolve to the directory containing the defaults file itself. This allows you to refer to resources contained in that directory:

```
epub-cover-image: ${.}/cover.jpg
epub-metadata: ${.}/meta.xml
resource-path:
- .             # the working directory from which pandoc is run
- ${.}/images   # the images subdirectory of the directory
                # containing this defaults file
```

This environment variable interpolation syntax _only_ works in fields that expect file paths.

Defaults files can be placed in the `defaults` subdirectory of the user data directory and used from any directory. For example, one could create a file specifying defaults for writing letters, save it as `letter.yaml` in the `defaults` subdirectory of the user data directory, and then invoke these defaults from any directory using `pandoc --defaults letter` or `pandoc -dletter`.

When multiple defaults are used, their contents will be combined.

Note that, where command-line arguments may be repeated ([`--metadata-file`](https://pandoc.org/MANUAL.html#option--metadata-file), [`--css`](https://pandoc.org/MANUAL.html#option--css), [`--include-in-header`](https://pandoc.org/MANUAL.html#option--include-in-header), [`--include-before-body`](https://pandoc.org/MANUAL.html#option--include-before-body), [`--include-after-body`](https://pandoc.org/MANUAL.html#option--include-after-body), [`--variable`](https://pandoc.org/MANUAL.html#option--variable), [`--metadata`](https://pandoc.org/MANUAL.html#option--metadata), [`--syntax-definition`](https://pandoc.org/MANUAL.html#option--syntax-definition)), the values specified on the command line will combine with values specified in the defaults file, rather than replacing them.

## Concatenate multiple markdown
To concatenate multiple files in a specific order, we can leverage the `defaults.yaml` file, with the `input-files:` option. 

Here is a simple example file called `defaults.yaml` with the following content:

```yaml
input-files:
  - file1.md
  - file3.md
  - file2.md
```

Then call pandoc with `pandoc --defaults defaults.yaml`

DevOps Wiki,
## Page Formatting
Create the following `head.tex` file, to define some settings for pandoc to apply during converstion. 

```tex
\usepackage{fancyvrb,newverbs}
% set up page geometry
\usepackage[top=2cm, bottom=1.5cm, left=2cm, right=2cm]{geometry}

% change background color for inline code in
% markdown files. The following code does not work well for
% long text as the text will exceed the page boundary
\definecolor{bgcolor}{HTML}{DADADA}
\let\oldtexttt\texttt

\renewcommand{\texttt}[1]{
  \colorbox{bgcolor}{\oldtexttt{#1}}
}

%% color and other settings for the hyperref package
\hypersetup{
    bookmarksopen=true,
    linkcolor=blue,
    filecolor=magenta,
    urlcolor=NavyBlue,
}
```

We can then reference the file with 

```sh
pandoc --defaults defaults.yaml -H head.tex -o output.docx
```


## Word Template
With the option `--reference-doc=FILE` parameter, we have the option to define a `docx` (Word) or `odt` (LibreOffice) document which is used as a format template file.

This setting will enable **Pandoc** to apply the formatting from our template Word files to the created output. 

Copy a `docx` file (not a `dotx` template, save the template as document) and name it as `reference.docx` in either
* The current folder
* A folder of your choice, defined by using `--data-dir` 

Note: The system default folder for `data-dir` is
- On Linux or MacOS the folder is  `$HOME/.pandoc` 
- On Windows use  `C:\Users\USERNAME\AppData\Roaming\pandoc`

The results of running Pandoc with the following command, is a document that follow the style of our company documents: fonts, font colors, and even the company logo

```sh
pandoc --defaults defaults.yaml -H head.tex --reference-doc=reference.docx -o skbo.docx
```

## Highlighting

Pandoc supports block code syntax highlighting for many languages and offers several highlight themes. To list the highlight themes that Pandoc provides, use the following command:

```fallback
pandoc --list-highlight-styles
```

To list all the languages that Pandoc supports, use the following command:

```fallback
pandoc --list-highlight-languages
```

To use syntax highlighting for different languages, you need to specify the language in the block quote and use `--highlight-style`, e.g.,:

```fallback
pandoc --pdf-engine=xelatex --highlight-style zenburn test.md -o test.pdf
```

In the above command, we use the `zenburn` theme, I also recommend using the `tango` or `breezedark` theme.

## Add colors to links

According to the Pandoc user guide, we can add colors to different links via the `colorlinks` option to separate the links from the normal texts. To customize the color of different types of links, Pandoc offers different options:

> linkcolor, filecolor, citecolor, urlcolor, toccolor color for internal links, external links, citation links, linked URLs, and links in table of contents, respectively: uses options allowed by xcolor, including the dvipsnames, svgnames, and x11names lists

For example, to set the URL color to `NavyBlue` and set the TOC color to `Red`, we can use the following command:

```fallback
pandoc -V colorlinks -V urlcolor=NavyBlue -V toccolor=Red test.md -o test.pdf
```

Note that the `urlcolor` option will not color the raw URL links in the text. To color those raw links, you can enclose those links with `<>`, e.g., `<www.google.com>`.


Finally, my docuements have some code snippets embedded, so we can also tell pandoc to apply a little extra formatting on these for us

```sh
pandoc --defaults defaults.yaml -H head.tex --reference-doc=reference.docx -N --highlight-style=espresso -V colorlinks  -o output.docx
```

[Converting Markdown to Beautiful PDF with Pandoc - jdhao's digital space](https://jdhao.github.io/2019/05/30/markdown2pdf_pandoc/)

---
<!-- enrich:containment -->
*Part of: damianflynn*
