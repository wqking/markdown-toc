# markdown-toc.py -- Adds table of contents to Markdown files

## Overview

markdown-toc.py creates and insert table of contents to Markdown file.  
The TOC is generated from the headings in the markdown document.  

## Features  

* Use `<a>` anchor tag as the TOC target. It doesn't depend on Github auto anchor feature.  
* Can selectively generate TOC for lengthy article with enough headings. (The `--min-headings` option)
* Repeating the command on the same file will replace the previous TOC instead of adding more TOC.
* Code blocks can be handled correctly, the code lines start with '#' won't be treated as headings.  

## Command line usage
```
python markdown-toc.py ACTION [--help] [-h] --file FILE [--before BEFORE] [--after AFTER] [--min-headings MIN_HEADINGS] 
                       [--min-level MIN_LEVEL] [--max-level MAX_LEVEL] [--front FRONT] [--exclude EXCLUDE]
```

#### ACTION, required

The action can be `update` or `remove`.  
If the action is `update`, it will add TOC to the markdown files if it's not there, or update the TOC if the TOC is already there.  
If the action is `remove`, it will remove the TOC from the markdown files.  

#### --file FILE, required

FILE is the file pattern of the markdown files, the pattern can include wildcard `*`, and may include `**` to indicate recursive folders.  
There can be multiple `--file FILE`.  

#### --min-headings MIN_HEADINGS

Set minimum heading count to MIN_HEADINGS. If the valid heading count is smaller than MIN_HEADINGS, no TOC is created. Default is 6.

#### --min-level MIN_LEVEL

Set minimum level to MIN_LEVEL. A heading level smaller than MIN_LEVEL is invalid and not included in TOC. Default is 2.

#### --max-level MAX_LEVEL

Set maximum level to MAX_LEVEL. A heading level larger than MAX_LEVEL is invalid and not included in TOC. Default is 3.

#### --front FRONT

FRONT can be 0 or 1, default is 0. If FRONT is 0, the TOC is put after the first valid heading. If FRONT is 1, the TOC
is put in front of the document. The option is not used if there is `<!--toc-->` tag in the document.

#### --before BEFORE, optional

BEFORE is the text that will be added before the TOC block.  
If there are spaces, BEFORE should quoted with ".  
Any \n in the text will be replaced with line break.  

#### --after AFTER, optional

AFTER is the text that will be added after the TOC block.  
If there are spaces, AFTER should quoted with ".  
Any \n in the text will be replaced with line break.  

#### --exclude EXCLUDE, optional

Special the file names to be excluded from the markdown files.  
`EXCLUDE` can't contain any wildcard. The tool checks if any markdown file name or path contains `EXCLUDE`, then the tool skips that file.  
There can be multiple `--exclude EXCLUDE`.  

## Special tags

Several tags can be added to the markdown document to help markdown-toc to generate the TOC.

### Tag toc

Syntax: `<!--toc-->`  
It indicates where to put the TOC. The generated TOC will replace `<!--toc-->`.  
`<!--toc-->` can be omitted. If there is no `<!--toc-->`, then the place of TOC is determined by the command option `--front`. If `--front` is 0 (the default value), the TOC is placed right after the first valid heading (so if the first heading is a H1, the TOC is placed after the H1). If `--front` is 1, the TOC is placed in front of the document.

### Tag begintoc and endtoc

Syntax: `<!--begintoc-->`, `<!--endtoc-->`   
These two tags are created by markdown-toc and usually don't need to add manually. They indicates where the TOC is in. The script can replace the TOC correctly next time the script runs.  
Don't remove the tags from the markdown.  

## Why anther tool while there are existing tools to add TOC to markdown?

Because I need some of the features missing from the other tools,

1. Use `<a>` anchor tag as the TOC target. So that the existing markdown-to-HTML converter is happy with the TOC.  
2. Selectively generate TOC for long articles that contain enough headers. I don't want to add `<!--toc->` for every article manually, and I want only longer article has TOC. This is how the command option '--min-headings' is useful.
3. I created a tool called addtoc2md.pl written in Perl, which has equivalent features with markdown-toc. But the Perl script doesn't work with some newer Perl version, and it's difficult to maintain. So I rewrite it using Python.
