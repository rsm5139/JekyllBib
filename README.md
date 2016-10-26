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

## Adding References to a Webpage

References can be added to any page by using the ```include``` tag with the file ```jekyll-bib.html```.

Example:  
```
{% include jekyll-bib.html %}
```
This will serve up a reference list based on the default values in ```_config.yml```. The content and look of the references can be customized with the following parameters:

- **collection**  
  - The name of the collection file in ```_jekyll-bib```, minus the extension
- **citekey**  
  - The cite key of a specific publication
- **view**  
  - The name of a view found in ```_includes/jekyll-bib/views```
- **style**  
  - The name of a style found in ```_includes/jekyll-bib/styles```

If ```collection```, ```view```, or ```style``` are not defined, default values from ```_config.yml``` will be used. (```bib-collection``` for ```collection```, ```bib-view``` for ```view```, and ```bib-style``` for ```style```.) 

## Customizing Output

While there are several options for displaying references included in this package, creating new display options isn't difficult. This section will go through how JekyllBib works, and will provide insight into creating custom views and styles.

### jekyll-bib.html

Everything starts with an ```include``` tag which calls ```_includes/jekyll-bib.html```. 

**Example:**  
```
{% include jekyll-bib.html collection="my-collection" view="year-author" style="default" %}
```

The purpose of ```jekyll-bib.html``` is to serve the correct data to the requested view. In the above example, the data in ```_jekyll-bib/my-collection.md``` in the ```publications``` field will be served to the view file ```_includes/jekyll-bib/view/year-author.html```. It determines which collection file to use by checking the value in ```include.collection```, or falling back to ```page.bib-collection```. If ```include.citekey``` is defined, only the publication with the matched cite key will be served. The publication/s data will be stored in an array called ```results```. 

At the end of ```jekyll-bib.html```, a call will be made to the view file based on the value of ```include.view```, or ```page.bib-view```, in that order. This example would look as follows:

```
{% include jekyll-bib/view/year-author.html style="default" %}
```

Please note that the requested style is just passed along to the view.

### The View

The purpose of a "view" is to organize and display the formatted data, perhaps with the assistance of a "style", (*more on styles later*). All of the data are stored in a array called ```results```, so displaying data can be as simple as using a Liquid ```for``` loop.

For example, to display all of the reference titles, the following code block could be used:

```
{% for result in results %}
{{result.title}}
{% endfor %}
```

This is great, but what if the titles need to be in alphabetical order? Just sort them using Liquid's ```sort``` filter:

```
{% assign sorted = results | sort:'title'}
{% for result in sorted %}
{{result.title}}
{% endfor %}
```

The view can be customized further by adding more data to the output, and adding HTML:

```
{% assign sorted = results | sort:'title'}
{% for result in sorted %}
<strong>{{result.title}}<strong> ({{result.year}})
{% endfor %}
```

So there are many ways that references can be displayed to a website by creating custom views. However, sometimes the difference between one view and another is minimal. For example, what if sometimes the title needs to be bolded, and sometimes italicized, depending on the webpage? Well, this is where "styles" can be used. Just add an ```include``` tag inside the for loop, for example:

```
{% assign sorted = results | sort:'title'}
{% for result in sorted %}
{% include {{style}} %}
{% endfor %}
```

Now, the data in ```result``` can be formatted and displayed by the requested style.

### The style

The purpose of a "style" is to individually format references. As seen above, the view receives an array of references and their data. It can then sort and output them. But if there is variance to how a reference should be formatted, creating styles can save a lot coding.

For example, there could be a style for MLA and APA format. Then, the same view could display references using either style. This also means that other views can use these styles, saving a lot of copying and pasting. So one view may sort the data by title, and another by author names. Both views will have access to the MLA and APA styles, though. 

### The Formatting folders

Beside the ```views``` folder and ```styles``` folder, there is one more folder called ```formatting```. This folder just contains other useful snippets of code that can be used by different views and styles. Just use an ```include``` tag to add the snippets. 