---
Title: Document generation using Markdown and Pandoc
Date: 2016-9-13
Status: draft
Category: Misc
Tags: markdown, documentation, pandoc, docker
---

Maintaining documentation can be an involved process,  especially when different versions and output formats are needed.  One of the documents I, and probably you also, have to maintain is your work history or resume/CV.  Some while back I changed this document from an OpenOffice document to Markdown and ever since,  this workflow has influenced how I deal with other documentation.

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

Which would be both humanly readable, as parsed by something like the Webinterface of GitHub.


Note: I looked for some of the older formats and output, but was unable to track them down. I do remember my businesscard was used as a reference for '[microformats](http://microformats.org/wiki/selected-test-cases-from-the-web)'.

[Pandoc](http://pandoc.org/) is like a Swiss knife for handling documents. It can convert between different formats and allows you to specify a template for specific formats, such as HTML.