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

[Part one](/2016/12/04/rdtool-1) focuses on the business shift and on the domain and ecosystem. 

[Part two](/2016/12/13/to-refactor-some-strings) shows that 'refactoring strings' may be much more than just that and tries to give a high level overview on this particular refactoring.

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

In other words, what am I actually working on?

I am trying to write an intermediate 'data tree' type of object. A universal representation of the data **for this domain**. Something, which will separate the _meaning_ of the data from the _formatting_ of the data, but something which is not domain-aware yet.

This is a [bounded context](http://martinfowler.com/bliki/BoundedContext.html) issue. 

To explain, let's look at this single line:

    mage: [[[inwazja-karty-postaci-1411:dalia-weiner|Dalia Weiner]]]

* In **domain of rdtool** this line represents the **metadata** about the Profile of a character called Dalia Weiner. This particular Profile is using the mechanics denoted as '1411'. From this metadata record we see that Dalia is a mage. Also, this metadata record can redirect us to Dalia's Profile. There probably exists another Profile of the same character, say, using the '1604' mechanics.
* In terms of **meaning of formatting** this line represents the **link** to another page. That page is located under the address _inwazja-karty-postaci-1411:dalia-weiner_ and has a display key _Dalia Weiner_. Before the link, text 'mage: ' is present, but its meaning is unknown

And having stated the above, UniversalDataStructure we are reasoning about belongs to the _meaning of formatting_ world, not the _domain of rdtool_ world.

In terms of larger application lifecycle, this is where our UniversalDataStructure is located:

![The overall flow: Original Documents -> Source -> Factories building Domain Objects -> Actual transformations of Domain Objects -> 'Deserializers' preparing Domain Objects to be saved, Destination persisting data -> Saved Documents. Our UniversalDataStructures are between Source and Factories and between 'Deserializers' and Destination.](/img/161215/lifecycle_universal_data_structure.png)

So the _current_ name of this object is 'UniversalDataStructure', not 'DataTree'.

Its purpose is to:

* Separate the formatting from meaning in context of _meaning of formatting_
* Properly deliver all _meaningful_ data to Formatters (in Destination) and Factories (from Source).
* Be domain-agnostic to allow more flexible rdtool evolution
* Be generated from Domain Objects and from Parsers

We will know it fulfills its purpose when:

* Every type of _meaningful_ data can be stored in this UniversalDataStructure.
* No data is ever lost; Documents being read by Source can be persisted by Destination in exactly the same form.
* There exists a set of functions (or methods) which allows Factories to easily build what they need from those objects.
* There exists a set of functions (or methods) which allows Formatters to properly output the expected output (say, strings).
* There are simple Factories able to convert Domain Objects into appropriate UniversalDataStructures.
* UniversalDataStructure is domain-agnostic and deals mostly with the 'universal formatting'.

### How will this object be used?

#### The external point of view

In her [excellent talk about testing](https://www.youtube.com/watch?v=URSWYvyc42M), Sandi Metz introduces the concept of testing the messages of an object, not an object itself. This is the external point of view - 'an object is what an object does'.

![A box representing an object and two sets of arrows: Incoming Commands or Queries (and corresponding Return Result) and Outgoing Commands or Queries (and corresponding Return Result)](/img/161215/what_is_object.png)

Being able to understand how we are going to use the object will allow us to design its interfaces (ways of communicating with the world). Being able to understand its interfaces will help us see if we can actually use it in the context of real world usage.

So...

#### Say hello to Profile Masterlist Core Factory ;-)

To construct every single object, the factories are working in a chain of sorts:

* **Profile Masterlist factory** currently takes the context of the whole text file and passes just its part representing the Core to the Core factory.
* **Profile Masterlist Core factory** currently takes the 'Core' content of the text file, splits it into parts of the text representing Factions and passes (...)
* **Faction factory** splits the Faction into 'Faction part' and 'Metadata Collection part' and passes 'Metadata Collection part' (...)
* **Profile Metadata Collection factory** splits the collection into individual Metadata and passes them, one by one, to Single Profile Metadata factory.
* Finally, **Single Profile Metadata Factory** takes the single _line of text_ representing Single Profile Metadata and builds an object out of this.
* Then, after all Single Profile Metadata are built, Metadata Collection can be built
* ...so Factions can be built
* ...so Core can be built
* ...and finally, Masterlist can appear

What do those factories need?

* **Profile Masterlist factory**: Give me whole Document
* **Profile Masterlist Core factory**: Give me the Section corresponding to Core (starts with Header 'XXX' and ends with Header 'YYY')
* **Faction factory**: Give me all the Sections having the Header at Level 3 (+++ Header)
* **Profile Metadata Collection factory**: Give me all the Text Lines which contain a particular **domain** text pattern and which contain a link
* **Single Profile Metadata Factory**: Just give me a Text Line; the Collection validated it for me

#### What can I infer about the methods and functions?

Well, if our UniversalDataStructure is to be usable, it has to contain the following methods:

* def get_all_sections_between(start_header, end_header) -> Section[]
* def get_section_having(header) -> Section
* def get_all_sections_at(level) -> Section[]
* def get_all_smart_text_from(section) -> SmartText
* def get_all_text_lines_satisfying_criteria(smart_text, criteria) -> SmartTextLine[]

So in terms of creating a complete parity between formatting-based operations and meaningful-based operations we should expect to have more or less this type of actions.

Note: SmartText is the text which is generally aware of things like 'links' or 'lists'.

#### Also, hi, Report Masterlist Core method 'to_rd_text()'

In the current code there are no 'Deserializers' - the method 'to_rd_text' is enough.



### What type of data will this object hold?

#### The internal point of view

Another way to approach the creation of an object is to reason about the data it is going to hold.

* [Linus Torvalds](http://softwareengineering.stackexchange.com/questions/163185/torvalds-quote-about-good-programmer): 'Bad programmers worry about the code. Good programmers worry about data structures and their relationships.'
* [Fred Brooks](https://mperdikeas.github.io/wise-technology.html.files/data-vs-code.html): 'Show me your flowcharts and conceal your tables, and I shall continue to be mystified. Show me your tables, and I won't usually need your flowcharts; they'll be obvious.'

In this understanding, the main goal of our UniversalDataStructure is to hold data. The data will not really change - our requirements on the actions performed on the data may change, but we should _understand_ what is the data in our system so we can properly design the structure which will stand the test of time (read: changes).

#### Time for... Formal grammar

_Wait, what? Why? What did I do to you?!_ 

Formal grammar only _sounds_ boring. I consider it to be a very fun and fascinating topic; please, give me a chance to show you how it works. It is just a way to describe the _rules_ on syntax of something.

We are going to use an [Extended BNF (EBNF)](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form) notation.

As a simple example, look at this grammar:

* word = (letter)
* letter = a \| b \| c 

In the above case, we have a word which can be composed only of some letters. The parenthesis around the letter mean, that it can repeat. The \| means, that it is one of those only. 

So this grammar lets us generate the following words:

* word = a
* word = aaaaaaaaaaaaaaaaaaaa
* word = acbbabcccb

All are valid.

The following are invalid:

* word = abcd - invalid, because 'd' is outside the possible options for a letter
* word = aaa aaa - invalid, because word can be made only of letters and there is a space

Thanks to using this type of formal grammar I am able to look through existing documents and samples to be able to create the _meaningful, not changing data_. From this I should be able to derive a sensible structure.

#### Preface

This is a long-ish process in its own rights. Therefore I have moved the full analysis and reasoning [to the auxillary post in scraps](/scraps/161215-constructing-formal-grammar). If you want to see how I do things like those, visit that link. I would ;-).

#### Result

The current 'final' (read: good enough) form of the grammar looks like this:

For the following legend:

|  Special symbol   | origin |  Meaning                 |
|:------------------|:------:|:-------------------------|
| [Section]         | EBNF | Section is optional; it may not appear |
| (Section)         | EBNF | Section can appear more than once |
| X = A, B          | EBNF | X is made of both A and B |
| X = A \| B        | EBNF | X is either A or B, never both |
| _italics text_    | custom \| | a terminal symbol (does not split further) |

(I have added _italics for terminal symbols_ to make it more readable. It is outside the standard, but as this is just an informal article I think nobody will mind.)

The final grammar looks like this:

|  Element          | Operator|  What can it contain? |
|:------------------|:-------:|:----------------------|
| Document          | = | _Identifier_, ([Section]), [SmartText]              |
| Section           | = | Header, [SmartText], [(Section)]      |
| Header            | = | _RawText_, _Level_        |
| SmartText         | = | _RawText_, [EmbeddedObjects] |
| EmbeddedObjects   | = | (EObject) |
| EObject           | = | _Position_, EValue    |
| EValue            | = | Link \| UList \| OList |
| Link              | = | _Key_, _URL_  |
| UList             | = | (SmartText) |
| OList             | = | (SmartText) |


#### Proving the grammar with real data

Let us take a simple story with a link in Dramatis Personae section:

![H3 header "Before this report" and text "gasp, something happened". Then H1 header "Actual report" with the text "raw text". Then H1 header "Dramatis personae" and text: a link to the person, afterwards with some text.](/img/161215/r03_story_with_links.png)

Deconstruction according to the rules of formal grammar:

* Document = Identifier, Section_1, Section_2, Section_3
* Section_1 = Header_1, SmartText_1
* Header_1 = RawText "Before this report", Level "3"
* SmartText_1 = RawText "gasp, something happened"
* Section_2 = Header_2, SmartText_2
...

See how it works? For visual form:

![Visual form (in the form of a tree) expanding the list above](/img/161215/r03_story_with_links_names_cleaned_struct.png)

So now we have the information on what type of data we can expect for our UniversalDataStructure.

### The structure of this object

#### Why did we have to wait so long?

Let me modify the picture with the object...

![A box representing an object and two sets of arrows: Incoming Commands or Queries (and corresponding Return Result) and Outgoing Commands or Queries (and corresponding Return Result). Object has a blue circle inside with text 'What data goes here' and the arrows have a red circle around them with text 'how will others use this object'](/img/161215/what_is_object_2.png)

Our object **holds and guards access** to data (blue) while **presenting a way to interact** with the data using methods and functions (red).

If we don't know what will others need and how are they going to use it, we don't know how to design user's interface for this object - methods. Incidentally, we also cannot write tests.

If we don't know what data is this object going to hold, we cannot build a sensible internal structure.

#### So what do we know?

##### In terms of use

Probable methods will look more or less like this:

* def get_all_sections_between(self, start_header, end_header) -> Section[]
* def get_section_having(self, header) -> Section
* def get_all_sections_at(self, level) -> Section[]
* def get_all_smart_text_from(self, section) -> SmartText
* def get_all_text_lines_satisfying_criteria(self, smart_text, criteria) -> SmartTextLine[]

(I still don't know which objects have those methods - Document? Section? SmartText is unlikely)

##### In terms of data

Our grammar looks more or less like this:

|  Element          | Operator|  What can it contain? |
|:------------------|:-------:|:----------------------|
| Document          | = | _Identifier_, ([Section]), [SmartText]              |
| Section           | = | Header, [SmartText], [(Section)]      |
| Header            | = | _RawText_, _Level_        |
| SmartText         | = | _RawText_, [EmbeddedObjects] |
| EmbeddedObjects   | = | (EObject) |
| EObject           | = | _Position_, EValue    |
| EValue            | = | Link \| UList \| OList |
| Link              | = | _Key_, _URL_  |
| UList             | = | (SmartText) |
| OList             | = | (SmartText) |

#### Building the structure


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

