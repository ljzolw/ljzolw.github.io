---
layout: post
title: "Trees - musings on rdtool data"
date: 2016-12-15
---

### Abstract


### The value of this post to you:

1. If you are:
    1. a person who **liked** the previous articles about rdtool in this workshop either because of the depth or because of the story

### Why did I write this post?

This is the part three of the larger rdtool refactoring.

[Part one](/2016/12/04/rdtool-1) focuses on the business shift and on the domain and ecosystem. [Part two](/2016/12/13/to-refactor-some-strings) shows that 'refactoring strings' may be much more than just that and tries to give a high level overview on this particular refactoring.

So why do I need a part three?

When I sat down and ran the IDE I have noticed I cannot write the tests. I still don't understand enough about the data structure (those "DataTrees") to be able to properly create the first prototypes of the code.

After a short work with a pencil and several pieces of paper I have realized I still don't have the core information - what _actually_ is my data. What do I _expect_ from those "DataTrees".

Back to the drawing board.

### Recap

This is the domain - integrating different 'documents' to create summaries and automatically generated facts based on the things which were written down. Details: [Part one](/2016/12/04/rdtool-1).

![Structure of domain: book into chapters, chapters into stories](/img/161215/domain_picture.png)

This is the current solution in terms of the data (if you want details, [read part two](/2016/12/13/to-refactor-some-strings)): 

For the data which looks like this in wikidot format:

![DPersonae has a structure '+++ Dramatis Personae, newline: mage: Kate, who did X' Locations have a structure: '+ Locations, # First level of a list, ## Second level of a list](/img/161213/data_model_wiki.png)

And like this in markdown format:

![DPersonae has a structure '### Dramatis Personae, newline: mage: Kate, who did X' Locations have a structure: '# Locations, 1. First level of a list, 4 spaces, 1. Second level of a list](/img/161213/data_model_markdown.png)

I expect to have something like this data structure:

![Root has two Sections. Every Section has a Header, Text, Supporting Things and Child Sections. Example: Header is "Dramatis Personae", Text is copied verbatim from the picture, Supporting Things is none and Child sections is None. Example 2: Header is "Locations", Text is "{0}", Supporting Things is a Composite pattern python object and Child Sections is None](/img/161213/final_data_structure.png)

They should be rendered identical.

Now, one of the observations I had - not every formatting has _meaning_. Some does - for example, section headers - they usually say that the section is starting and sections are usually meaningful. In the example above, **Dramatis personae** or **Locations** are typical meaningful section headers.

Some formatting, for example **bold** or _italics_ does not really matter. They are just pretty print. **But:**

* sometimes a list doesn't matter; it is just a pretty print
* sometimes a list is meaningful and it is, well, a list of something
* and this makes is slightly harder to parse

And this is the point where we have finished both part 1 and part 2.

It is time to go into...

### The purpose of this object



### How will this object be used?

#### Main responsibility

#### Lifecycle of this object

#### Typical actions

### How will this object be structured?

#### The purpose of structure

#### Structure is secondary to needs

#### Structure is secondary to data


### Formal grammar

#### Wait, what? Why?

#### A bit of theory

#### Creating the grammar from data

[Full analysis here, long and probably boring.](/scraps/161215-constructing-formal-grammar)

#### Proving the grammar with real data


### Structuring the object, approach #2

#### The use of this object

#### Structuring the grammar


### NAMING this object

#### Why naming is the last step?

#### What was wrong with 'DataTrees'?


### Did we succeed?

#### Can this object serve its purpose?

#### Does the name sound fine?

#### Can it hold the varied data it needs?

#### Can it use it without any problems?

#### Can we write sensible tests?


### Conclusions

#### If I cannot make a test, my analysis is not done

#### If I cannot NAME it, I probably don't understand it

#### How I use an object proves its existence

#### Check it with real data...

#### ...and check if you can use it...

