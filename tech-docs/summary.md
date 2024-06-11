# Confluence to Backstage TechDocs

Adding documentation to Backstage TechDocs requires Markdown files.

Confluence provides the ability to export pages in three formats:
- Export in PDF
- Export in Word
- Export in HTML

In order to convert the above-exported formats to markdown, different converters are available:
- Pandoc
- Aspose using C#
- Markdown Exporter for Confluence
- Node-html-markdown package

## Pandoc

Pandoc is a free and open-source command-line tool used for converting documents from one format to another.

### General Command to Perform Conversions

```bash
pandoc --extract-media -f "current format" -t "required format" {input file name} -o {output file name}
```

- `-f` → from
- `-t` → to
- `-o` → output

### Example

```bash
pandoc --extract-media -f docx -t markdown "input.docx" -o output.md
```

### PDF to Markdown

Pandoc can convert to PDF, but not from PDF.

### Word to Markdown

Pandoc converts `docx` format to markdown format. Confluence exported Word file is in `doc` format, so we need to save the file as `docx` from `doc`. Pandoc has a lot of extensions to customize the conversion process.

Please refer to the following link for a detailed explanation: [Pandoc extensions](https://pandoc.org/MANUAL.html#extensions).

#### Advantages

By disabling the implicit figures extension, we were able to convert images properly:

```bash
pandoc --extract-media ./ -f docx -t markdown-implicit_figures "input.docx" -o output.md
```

Links were also converted to the required format.

#### Disadvantages

Tables and code blocks were not being converted properly. Tried disabling and enabling different extensions such as `simple-tables`, and `grid-tables` but didn’t work.

#### Result

Not acceptable because it cannot convert code blocks and tables, which are essential parts of the documentation, to markdown.

### HTML to Markdown

When exporting to HTML from Confluence, it provides us with a zip file. It consists of an HTML file named "page" that needs to be converted to markdown.

#### Advantages

We can attach a CSS file while conversion:

```bash
pandoc -s -o output.md --css=combined.css --extract-media=. index.html page398697717.html
```

We can give more than one HTML file. The code block was successfully converted to markdown syntax.

#### Disadvantages

Only the code block was converted to proper markdown syntax.

#### Result

Not acceptable as it is not able to convert other essential elements such as images, tables, links, and so on.

## Aspose Using C#

Aspose is a professional software solution to import and export Word, Markdown, and many other document formats using C#, F#, VB.NET.
[Learn more about Aspose](https://products.aspose.com/words/net/conversion/word-to-markdown/).

### PDF to Markdown

Converted markdown file had extra spaces at the start, changed image color, considered code block as plain text, and borders were missing from tables.

#### Result

Not acceptable.

### Word to Markdown

Works well when converting a Word file into a markdown file. Only the code block was not converted properly.

#### Result

Not acceptable.

### HTML to Markdown

It was unable to load the image and the code block was not converted properly.

#### Result

Not acceptable. Aspose couldn't convert all the elements to proper markdown syntax from any of the exported formats.

## Markdown Exporter for Confluence

It is a free and open-source plugin that allows you to export Confluence pages and blog posts to Markdown format. The plugin supports custom templates, export of attachments, and export of metadata such as labels and page properties.

### Performance

Successfully converted all the elements to markdown syntax.

### Result

Acceptable, but we cannot utilize this plugin since it is only applicable to cloud Confluence.

## Node HTML Markdown Package

NHM is a fast HTML to markdown converter, compatible with both Node and the browser.

Refer to the following Confluence page for [Getting started](https://www.npmjs.com/package/node-html-markdown).

NHM was successfully able to convert all the elements to markdown syntax except the code block.

### Reason

HTML uses `<code>` tag to define a code block. Confluence exported HTML files did not contain the `<code>` tag. After manually adding the `<code>` tag, NHM was able to generate the code block.

### Result

Acceptable, but need to do some manual changes in the HTML file.

## Getting a Confluence Page through API

Since the exported HTML file from Confluence did not contain the `<code>` tag, we considered exporting the HTML file through API and comparing it with the exported HTML file.

### Result

It also did not contain the `<code>` tag which we were expecting. The only difference and advantage of exporting through API was that it had CSS included in the HTML code rather than providing a separate file for CSS.

Please refer to the following Confluence page for a detailed explanation: [Confluence API for getting pages](https://developer.atlassian.com/cloud/confluence/rest/).

## Conclusion

- Converting code blocks into markdown syntax was difficult for most converters.
- The Confluence plugin was able to convert the Confluence page to the proper markdown syntax successfully.
- The Node HTML Markdown package worked well when given HTML files with appropriate syntax.

### Note Points

- TechDocs doesn’t accept HTML tags in markdown files.
- Code tags were missing in the Confluence exported HTML files.
- Confluence exported Word file is in `doc` format. We need to convert it to `docx` format to use Pandoc for conversion.