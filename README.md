# JekyllBib  
*A bibliography formatter for Jekyll websites*

JekyllBib provides an easy way to include formatted bibliographies on a Jekyll-based website. No special plug-ins are needed; just drop the JekyllBib files into the appropriate folders and go!

## Getting Started  

This repository is organized to resemble the source code of a standard Jekyll website. First, transfer all of the files in the repository's ```_include``` directory to the website's ```_include``` directory. Next, copy the ```_jekyll-bib``` directory over to the Jekyll website's base directory (the same level as the ```_include``` directory). This becomes what's known in Jekyll as a "collection." Copy the file ```jekyll-bib.html``` to the Jekyll website. While this file is not required, it serves as an example of how to use this package. Finally, add these lines to the website's ```_config.yml``` file:

```
collections: 
  jekyll-bib: 

defaults:
  -
    scope:
      path: ""
    values:
      bib-view: "no-sort"
      bib-style: "default"
      bib-collection: "my-publications"
```

JekyllBib is ready to go! Serve the website, (this is usually done with ```jekyll serve```), then navigate to the ```jekyll-bib.html``` page. ```jekyll-bib.html``` Contains examples that can be copied and pasted to other pages on the website without much modification. 

## Editing References  

The default collection file for JekyllBib references is ```_jekyll-bib/my-publications.md```. There are several examples of publications and the data fields they use in this package, as well as some additional unused but useful fields for future expansions. The references are an array of data objects in the front-matter of a Jekyll collection file. The field name for this array is ```publications```. This file can be edited with new references, or a new collection file can be created. Just take care to follow the format of this example.