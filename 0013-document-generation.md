---
Title: Document generation using Markdown and Pandoc
Date: 2016-9-13
Status: draft
Category: Misc
Tags: markdown, documentation, pandoc, docker
---

Maintaining documentation can be an involved process,  especially when different versions and output formats are needed.  One of the documents I, and probably you also, have to maintain is your work history or resume/CV.  Some while back I changed this document from an OpenOffice document to plain text and ever since,  this workflow has influenced how I deal with other documentation.


## Requirement and timeline
If i remember correctly the original version of my resume was written in Microsoft Word. As you can imagine, this limits the usage to Windows as the platform for making small corrections. Of course I could use CrossOver Office or Wine and install Office 2003. But, this is never a long-term solution.  So, I moved to OpenOffice as the `.odt` format looked promising. It was easy to use to generate the needed PDF output, but still... the workflow was  far from ideal.

I wanted a workflow in which I can use just plaintext, which could be converted into the final format. Text is easy to index and to story under version management. This lead me to using XML directly and the use of FO (Formatting Objects), and Apache FOP. This was painful, but the results were decent to even very acceptable. It allowed me to describe elements with metadata, such as 'suffix', 'employment-type', etc. And all could be validated and transformed using XSLT. Although the conclusion was that the workflow was slow as there was no preview and transforming just took long. Also, describing elements was not useful as no-one would use the source material directly. Most recruiter use analysis tools that rely on extracting keywords and generate  a profile based on this information. This can lead to incorrect results, but since they analyse hundreds of resumes this seems one of the best methods, besides being introduced by a reference.

So I moved to a simpler approach which allows me to change the document and have a partial, and acceptable  preview. By this time Markdown got more popular and I tried to convert my resume and it seemed like an acceptable approach. Using a standard parser the documents looked OK, but it missed the oomph. Then `pandoc` arrived and it changed everything.


## Markdown and Pandoc
By writing in Markdown I would be able to write the following

```
# _**Gerard Braad**_


## Personal information

  * Date of Birth
    : 22 February, 1981
  * Nationality
    : Dutch
  * Email
    : [me@gbraad.nl][personal email]
```

This would be both humanly readable, as well as parsed by something like the Webinterface of GitHub into a decent representation. And when using [Markdown Preview](https://github.com/revolunet/sublimetext-markdown-preview) for Sublime Text, this became a pleasure to edit.

Now for introducing [Pandoc](http://pandoc.org/). This tool is like a Swiss knife for handling documents. It can convert between different formats and allows you to specify a template for specific formats, such as HTML. This tool allows me to use:

```
$ pandoc -o resume.html resume.md
```

This will generate an unformatted HTML output from `resume.md` as input. Headings are converted to `h6`, `h5`, etc. and links become `a`, etc. But the beauty shows when a template is used:

```

```

This will generate a similar output, however the template refers to a stylesheet to style the look of the output. Very advanced scenarios are possible, such as conditionals, etc.

## Styling and layout
Using a template and stylesheet I am able to format the document to my liking. And since the output of markdown auto-generates names for elements in a consistent way, it is easy to style and format the document.

Take for instance the photo. Although I personally do not like photos on a resume, it is often requested. Placing it inline with the text does not look very nice. Using CSS it is possible to convert the following:

`
`

With the styling:

`
`

Into:

![]()


## pandoc distribution
Pandoc itself is written in Haskell. Which makes for some trouble when installing. It means that a distribution like Fedora or Ubuntu can not always provide the latest version. This is due to possible dependency issues, but also the effort a packager has to put into it. I work mostly on Fedora and CentOS and since the developer of this tool only provides a .deb package I had to find an alternative solution.

Spinning up a Virtual Machine with Ubuntu would be heavy weigh
Docugen
  Docker container
  CI runner

PDF generation
  Latex -> Chinese
  Headless Chrome

general documentation
  Handsonlabs
  Blog
  Training material


## Links

  * https://gitlab.com/gbraad/resume/
  * https://github.com/gbraad/resume/
  * https://gbraad.nl/resume/
  * https://gbraad.gitbooks.io/resume/content/


## Endnotes
I looked for some of the older formats and output, but was unable to track them down. I do remember my businesscard was used as a reference for '[microformats](http://microformats.org/wiki/selected-test-cases-from-the-web)'. And someone tried to introduce an XML standard for resume, called xmlresume if I recall correctly. From experience I could conclude this would not work; no applicant will provide XML and converting it in a tool seems pointless.
