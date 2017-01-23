---
layout: post
title: "Trees - musings on rdtool data"
date: 2016-12-19
---

### Abstract


### The value of this post to you:

1. If you are:
    1. a person who **liked** the previous articles about rdtool in this workshop either because of the depth or because of the story, I hope to keep you entertained ;-).
    2. a programmer interested to see how to design an untrivial object from internal and external points of view, I hope to show you:
        1. how and **why** to use formal grammar to analyze internal data (in this case, EBNF notation)
        2. how to reason about the methods / functions operating on the object
    3. a programmer not sure that tests can be used as a design tool, I hope to show you exactly that; without the tests this post would have never been written.
    
This post is more technical than the previous ones. It is deeper intertwined with the domain than the previous parts. What is worse, this post follows a particular way of reasoning of mine and the solution kind of unravels itself on its own. Thus, length and difficulty.

...just don't say I didn't warn you ;-).

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

![Structure of domain: book into chapters, chapters into stories](/img/161219/domain_picture.png)

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

![The overall flow: Original Documents -> Source -> Factories building Domain Objects -> Actual transformations of Domain Objects -> 'Deserializers' preparing Domain Objects to be saved, Destination persisting data -> Saved Documents. Our UniversalDataStructures are between Source and Factories and between 'Deserializers' and Destination.](/img/161219/lifecycle_universal_data_structure.png)

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

![A box representing an object and two sets of arrows: Incoming Commands or Queries (and corresponding Return Result) and Outgoing Commands or Queries (and corresponding Return Result)](/img/161219/what_is_object.png)

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

What do those Factories need?

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

#### Let's generalize those a bit

We are dealing with [Queries](http://martinfowler.com/bliki/CommandQuerySeparation.html) here. Quoting Martin Fowler, queries "return a result and do not change the observable state of the system (are free of side effects)."

From the prototype methods above it seems we query only the direct 'child nodes' of the object we are querying (give me something from those things you _directly_ hold).

#### Also, hi, Story Masterlist Core method 'to_rd_text'

In the current code there are no 'Deserializers' - the method 'to_rd_text' is enough. Up to this point, that is. In the near future, we want to have factories changing Domain Objects into UniversalDataStructures - I call them 'Deserializers' in this article to avoid confusion with Factories Building Domain Objects from UniversalDataStructure (another question to consider: which context does the Deserializer belong to - the context of a Domain Object or the context of UniversalDataStructure).

Back to the StoryMasterlistCore.to_rd_text():

* **Story Masterlist** orders the Prelude, Core and Text to call their to_rd_text() and combines the results
* **Story Masterlist Core** adds a Header, then orders all Campaigns (chapters) to call their to_rd_text() and appends the results to the Header
* **Campaign** (chapter) adds:
    * a Header
    * text like 'amount of stories' 
    * a Link (target = appropriate CampaignSummary; generated from the Campaign name)
    * then orders all Metadata to call their to_rd_text() methods
    * and finally combines the results
* **Single Story Metadata** does this: _rd_text = "* " + "[[[" + self.weblink + "\|" + self.sequence_number + " - " + self.date + " - " + self.name + "]]]" + self.extraComments_
    * so: it is an element of the List (should be governed by Story Metadata Collection, really; this is a modeling error on my part)
    * it inserts a Link (target = weblink, key = lots of stuff)
    * and it adds a text after that Link

What do those Deserializers need?

* **Story Masterlist**: Create Document, expand it with Sections.
* **Story Masterlist Core**: Create a Section, then expand it with lower level Sections.
* **Campaign**: Create a Section, expand it with SmartText, append SmartText to SmartText
* **Story Metadata Collection** (which should be here): Create a List, Append SmartText to that List
* **Single Story Metadata**: Create SmartText, insert Link in SmartText, append Text to SmartText

#### Which lets me infer the following methods and functions:

* def create_document() -> Document
* def create_section_having(header, level) -> Section
* def expand_section_with(self, lower_level_section) -> void
* def expand_section_with(self, smart_text) -> void

annnnnd here (at Campaign) we have an interesting observation:

* I want to append Section to a Section; I can use a SectionList, or Section[]
* I want to append SmartText to a SmartText; I can use a SmartTextList, or SmartText[]

but also:

* I want to append SmartText to a Section:

![+++ Section H3 \n some text, [link key](link-to-something) \n +++++ Section H5 \n some text corresponding to the tree, where we want to add a Section to SmartText](/img/161219/H3_ST_H5_append.png)

This implies I want to create collections not of 'Sections' of 'SmartText' but of 'GeneralElements'. Fortunately, what I am doing here closely follows what [Document Object Model](https://en.wikipedia.org/wiki/Document_Object_Model) in XML was doing long time ago, so I can always look there for inspiration.

back to the functions:

* def create_list() -> Element[]
* def append_element_to_list(list, element) -> void
* def expand_smarttext(smart_text_1, smart_text_2) -> SmartText
    * reason: list is rendered as list, with '* '; so if we have two blocks of SmartText, we want to merge it
    * if we have a list of leading SmartText and then Sections, formatter can render it properly without '* ' sign
* def create_smarttext() -> SmartText
* def create_link(key, url) -> Link
* def expand_smarttext_with_link(smart_text, link) -> SmartText

Those are just function prototypes, of course; it will be refined by tests, use and everything. But it is worth it to see what we expect from our UniversalDataStructure.

#### Let's generalize those a bit

Here, we have several operations:

* Create a _Thing_ (Document, List, Section...)
    * def create_section_having(header, level) -> Section
* Append a _Thing_ to a _Thing_ as a sibling
    * def expand_smarttext(smart_text_1, smart_text_2) -> SmartText    
* Add a _Thing_ to a _Thing_ as a child
    * def expand_section_with(self, lower_level_section) -> void

Visually, it maps to the following:

![Visual form of the above list showing, that append_child adds a nested node while append_sibling adds a node on the same level](/img/161219/create_add_child_sibling.png)

If we generalize it sufficiently, the above operations should be enough for our needs.

### What type of data will this object hold?

#### The internal point of view

Another way to approach the creation of an object is to reason about the data it is going to hold.

* [Linus Torvalds](http://softwareengineering.stackexchange.com/questions/163185/torvalds-quote-about-good-programmer): 'Bad programmers worry about the code. Good programmers worry about data structures and their relationships.'
* [Fred Brooks](https://mperdikeas.github.io/wise-technology.html.files/data-vs-code.html): 'Show me your flowcharts and conceal your tables, and I shall continue to be mystified. Show me your tables, and I won't usually need your flowcharts; they'll be obvious.'

In this understanding, the main goal of our UniversalDataStructure is to hold data. The data will not really change - our requirements for the actions performed on the data may change, but we should _understand_ what is the data in our system so we can properly design the structure which will stand the test of time (read: changes).

#### Time for... Formal grammar

_Wait, what? Why? What did I do to you?!_ 

Formal grammar only _sounds_ boring. I consider it to be a very fun and fascinating topic; please, give me a chance to show you how it works. It is just a way to describe the _rules_ governing the syntax of something.

We are going to use an [Extended BNF (EBNF)](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form) notation.

As a simple example, look at this grammar:

* word = (letter)
    * a word can be composed only of letters
    * (letter) means that letters can repeat (without this a word is made of one letter only)
* letter = a \| b \| c 
    * 'x' \| 'y' means that it is either 'x' or 'y'.

So this grammar lets us generate the following words:

* word = a
* word = aaaaaaaaaaaaaaaaaaaa
* word = acbbabcccb

All are valid.

The following are invalid:

* word = abcd - invalid, because 'd' is outside the possible options for a letter
* word = aaa aaa - invalid, because word can be made only of letters and there is a space

Thanks to using this type of formal grammar I am able to look through existing documents and samples to be able to create the _meaningful, unchanging data_. Something which will work and will correspond to my use of the formatting in this domain. 

And having that I should be able to derive a sensible structure.

#### Preface

This is a long-ish process in its own rights. Therefore I have moved the full analysis and reasoning [to the auxillary post in scraps](/scraps/161215-constructing-formal-grammar). If you want to see how I do things like those, visit that link. I would ;-).

#### Result

The current 'final' (read: good enough) form of the grammar looks like this:

For the following legend:

|  Special symbol   | origin |  Meaning                 |
|:------------------|:------:|:-------------------------|
| [Section]         | EBNF | Section is optional; it may not appear |
| {Section}         | EBNF | Section can appear more than once |
| X = A, B          | EBNF | X is made of both A and B |
| X = A \| B        | EBNF | X is either A or B, never both |
| _italics text_    | custom | a terminal symbol (does not split further) |

(I have added _italics for terminal symbols_ to make it more readable. It is outside the standard, but as this is just an informal article I think nobody will mind.)

The final grammar looks like this:

|  Element          | Operator|  What can it contain? |
|:------------------|:-------:|:----------------------|
| Document          | = | _Identifier_, {[Section]}, [SmartText]              |
| Section           | = | Header, [SmartText], [{Section}]      |
| Header            | = | _RawText_, _Level_        |
| SmartText         | = | _RawText_, [EmbeddedObjects] |
| EmbeddedObjects   | = | {EObject} |
| EObject           | = | _Position_, EValue    |
| EValue            | = | Link \| UList \| OList |
| Link              | = | _Key_, _URL_  |
| UList             | = | {SmartText} |
| OList             | = | {SmartText} |


#### Proving the grammar with real data

Let us take a simple story with a link in Dramatis Personae section:

![H3 header "Before this report" and text "gasp, something happened". Then H1 header "Actual report" with the text "raw text". Then H1 header "Dramatis personae" and text: a link to the person, afterwards with some text.](/img/161219/r03_story_with_links.png)

Deconstruction according to the rules of formal grammar:

* Document = Identifier, Section_1, Section_2, Section_3
* Section_1 = Header_1, SmartText_1
* Header_1 = RawText "Before this report", Level "3"
* SmartText_1 = RawText "gasp, something happened"
* Section_2 = Header_2, SmartText_2
...

See how it works? For visual form:

![Visual form (in the form of a tree) expanding the list above](/img/161219/r03_story_with_links_names_cleaned_struct.png)

So now we have the information on what type of data we can expect for our UniversalDataStructure.

### Our current knowledge about the object

#### In terms of purpose

I am trying to write an intermediate 'UniversalDataStructure' type of object. A universal representation of the data which separates the formatting from the meaning of that formatting. In short, a transport intermediary object, something separating the bounded context of the Domain Objects from the bounded context of the ways the data is stored (read or written).

#### In terms of use

We are going to use it more or less like this:

For factories:

* queries on the data
    * probably only one level deep

For deserializers:

* Create a _Thing_ (Document, List, Section...)
    * def create_section_having(header, level) -> Section
* Append a _Thing_ to a _Thing_ as a sibling
    * def expand_smarttext(smart_text_1, smart_text_2) -> SmartText    
* Add a _Thing_ to a _Thing_ as a child
    * def expand_section_with(self, lower_level_section) -> void

#### In terms of data

The formal grammar for the internal data looks more or less like this:

|  Element          | Operator|  What can it contain? |
|:------------------|:-------:|:----------------------|
| Document          | = | _Identifier_, {[Section]}, [SmartText]              |
| Section           | = | Header, [SmartText], [{Section}]      |
| Header            | = | _RawText_, _Level_        |
| SmartText         | = | _RawText_, [EmbeddedObjects] |
| EmbeddedObjects   | = | {EObject} |
| EObject           | = | _Position_, EValue    |
| EValue            | = | Link \| UList \| OList |
| Link              | = | _Key_, _URL_  |
| UList             | = | {SmartText} |
| OList             | = | {SmartText} |

So now we are ready.

### The structure of this object

#### Why did we have to wait so long?

Let me modify the picture with the object...

![A box representing an object and two sets of arrows: Incoming Commands or Queries (and corresponding Return Result) and Outgoing Commands or Queries (and corresponding Return Result). Object has a blue circle inside with text 'What data goes here' and the arrows have a red circle around them with text 'how will others use this object'](/img/161219/what_is_object_2.png)

Our object **holds and guards access** to data (blue) while **presenting a way to interact** with the data using methods and functions (red).

If we don't know why does an object exist, how will we define the boundary of this object against all the other possible objects?

If we don't know what will others need and how are they going to use it, we don't know how to design user's interface for this object - its methods or the functions operating on it. Incidentally, we also cannot write tests.

If we don't know what data is this object going to hold, we cannot build a sensible internal structure.

#### How to understand the structure?

The most important thing is to create a structure which will make it _the easiest_ to do the right thing, while making it _the hardest_ to make things which do not correspond to its purpose. 

#### Identify the difficult part here

Will the actions (methods, functions) be the hard part here or the way the data is held? I think the data is a more difficult problem in this case; let's try to tackle that one first.

#### How to solve that one?

There were several subtle hints throughout those posts and musings here:

* [Steve Yegge's post](https://sites.google.com/site/steveyegge2/the-emacs-problem) showing, that usually one progresses from regular expressions (my current state) to XPath and Document Object Model.
* Many references to [XML composite structure](https://en.wikipedia.org/wiki/Document_Object_Model)

The possibly **worst** thing I could do right now would be to actually try to reimplement XML or JSON.

1. This is not the problem I am solving.
2. I will do it far worse than the people who implemented XML or JSON in the first place.

So let us see how would the data map to a JSON object. I already know that [JSON objects map pretty nicely to Python dictionaries](https://docs.python.org/3.5/library/json.html) and if I have only one depth level of queries this should be quite easy to work with.

Ok, so... hypothesis.

##### The data - internal JSON object?

If we look at this again:

![H3 header "Before this report" and text "gasp, something happened". Then H1 header "Actual report" with the text "raw text". Then H1 header "Dramatis personae" and text: a link to the person, afterwards with some text.](/img/161219/r03_story_with_links.png)

We can try to represent it in JSON format (all my JSON text files are validated with [JSONLint](http://jsonlint.com/):

([JSON text file](/img/161219/JSon_approach_1.json) if you don't like pictures)

![JSON corresponding to the above](/img/161219/json_approach_1.png)

There are several things I don't like about that JSON above:

* Are 'Section_1', 'Section_2' etc necessary? They serve no purpose here
* Same with 'EObject_1'
* Also, 'EObject_1' has Type inside Value; it is too deep (think of the queries; preferably we want to deal with 'query your children only' approach)
* Once again I have forgotten about _Identifier_ from the Document ;-)

Several iterations later:

([JSON text file](/img/161219/JSon_approach_3.json) if you don't like pictures)

![JSON corresponding to the above](/img/161219/json_approach_3.png)

But those changes modified the grammar. Question is, does the second approach function as well as the first one? Back to the grammar.

(X, Y) means they are a logical group and need to appear together. Saying it now, as it has never been needed before ;-).

|  Element          | Operator|  What can it contain?       |
|:------------------|:-------:|:----------------------------|
| Document          | = | _Identifier_, Elements            |
| Elements          | = | {Element}                         |
| Element           | = | _Type_, {[TerminalValue]}, [EmbeddedObjects], {[Element]} |
| TerminalValue     | = | _Level_ \| _RawText_ \| _any_domain_named_KeyVal_pair_ |
| EmbeddedObjects   | = | {EmbeddedObject}                  |
| EmbeddedObject    | = | _Type_, _Position_, EmbeddedValue |
| EmbeddedValue     | = | (ESimpleValue \| EComplexValue)   |
| ESimpleValue      | = | (_Key_, _URL_) \| _(* any set of TerminalValues *)_ |
| EComplexValue     | = | {[TerminalValue]}, {[EmbeddedObject]}, {[Element]} |

What can we see from the document and the grammar above?

* Our grammar became much more flexible (which is good for extension and bad for validation) - it is much easier to malform the Documents now
    * for example, I can alternate {Element} being a Section and one being a SmartText which would change its meaning: save it, reload it and the meaning would be different.
* 'Section' and 'SmartText' became just _Type_ mandatory fields
    * 'Document, get me the Section with header Dramatis Personae'
    * dp_section = [element for element in Document["Elements"] if element["Type"] == "Dramatis Personae"][0]  __(simplified Python code)__
* 'EmbeddedValue' is either a (_Key_, _Url_) pair or a group of 'Element'. Which means it repeats _Type_ field.
    * and the _Type_ field will govern how should this object be rendered
    * which means that internally we should treat all lists as ordered lists not to lose information
* Using 'EComplexValue' might be very unwieldy. This one will require a lot of tests in case of extension.

Fortunately, our current 'EComplexValues' are List and a Link. And it is possible to make a List in the JSON corresponding to this grammar:

([JSON text file](/img/161219/list_json.json) if you don't like pictures)

![JSON corresponding to the above](/img/161219/list_json.png)

##### So the internals are done?

Kind of, yes. I believe having the grammar I have right now it is possible to hold any type of rdtool data in a JSON document. 

Note how introduction of JSON means that the actual _data container_ was not a problem to be solved. If we go back to the solution from the previous article, "To refactor some strings...":

![Root has two Sections. Every Section has a Header, Text, Supporting Things and Child Sections. Example: Header is "Dramatis Personae", Text is copied verbatim from the picture, Supporting Things is none and Child sections is None. Example 2: Header is "Locations", Text is "{0}", Supporting Things is a Composite pattern python object and Child Sections is None](/img/161213/final_data_structure.png)

That one could require writing some code and worse - relations between entities. The current JSON based solution requires having a Dictionary - which is already included in Python. The less code I write, the easier to maintain it is ;-).

#### Now, the methods (or functions)

##### Introduction (again)

We have two sets of methods:

For factories:

* queries on the data
    * probably only one level deep

For deserializers:

* Create a _Thing_ (Document, List, Section...)
    * def create_section_having(header, level) -> Section
* Append a _Thing_ to a _Thing_ as a sibling
    * def expand_smarttext(smart_text_1, smart_text_2) -> SmartText    
* Add a _Thing_ to a _Thing_ as a child
    * def expand_section_with(self, lower_level_section) -> void
    
From the previous analysis we see, that actually at the level of those methods we are operating on Python base entities:

Quoting from [Python JSON documentation](https://docs.python.org/3.5/library/json.html):

| Python        | JSON |
|:-------------:|:----:|
| dictionary    | object |
| list, tuple   | array |
| string        | string |
| int, float... | number |
| True, False   | true, false |
| None          | null |

Which leads to the following problem to be solved:

* Parsers: How to change formatted text into Dictionaries?
* Factories: How to query a dictionary?
* Deserializers: How to change a Domain Object into a dictionary?
* Formatters: How to output Dictionaries as formatted text?

Which are much simpler problems than the ones which we were exploring in the very beginning - especially as it is quite easy to query or build a Dictionary.

##### Custom class or a Dictionary?

I am usually opposed to allowing primitives move inside the application on their own. An 'int' is never an 'int' - it always represents _something_ in the domain. However, this time we are dealing with the Dictionary - and that Dictionary is representing an IntermediateUniversalDataStructure. And how would one implement an IntermediateUniversalDataStructure? As a Dictionary.

So I am slightly at loss here. I have two opposite rules of programming clashing in the situation I am not used to have:

* Encapsulate _Things_ coming from the language with objects having Domain-Aware names and present the methods allowing others to make important things _easy_ and improper things _hard_
* Do not overengineer, do not overcomplicate, basically - [KISS (Keep It Small and Simple)](https://en.wikipedia.org/wiki/KISS_principle)

If I don't see any apparent advantage of designing a custom class here I will do something I rarely do - I will let the Dictionary fly throughout the application. The moment I notice that it is a wrong approach - this one is fortunately pretty simple to change.

#### But then... what will happen to my methods if there is no class?

In Python, I can write a function which is not connected to any class. Technically, if I programmed in Java or C#, I could also emulate this behaviour with a [Functor](http://stackoverflow.com/a/6029692). 

I have four sets of operations to be done over this Dictionary. Which are in reality four sets of functions not having any side effects:

* Parsers: **given** properly formatted text **return** proper Python objects corresponding to JSON <-> Python conversion
* Factories: **given** Python <-> JSON objects **return** proper Domain Objects
* Deserializers: **given** Domain Objects **return** Python <-> JSON objects
* Formatters: **given** Python <-> JSON objects **return** properly formatted text

Those sound like facade functions operating on that Dictionary. What is more, Parsers and Formatters have 'general', domain-independent functions - things I could extract and share with other project of mine if I ever worked with Markdown or Wikidot.

...which leads me to the question: are there Wikidot -> Dictionary and Markdown -> Dictionary parsers somewhere in Python? (or on the internet)?

Answer: At a glance, there exists a [Markdown -> Html (or XHtml)](http://pythonhosted.org/Markdown/#python-markdown) and then [Html -> Dictionary](https://docs.python.org/3/library/html.parser.html) or [XHtml -> Dictionary](http://code.activestate.com/recipes/410469-xml-as-dictionary/).

Which means that - possibly - I have even less code to write. 

This - an actual implementation - has to be looked into when I start working with the code. For now, it seems I _may_ have less work than I expected.

### So... did we succeed?

Good question.

#### Can this object serve its purpose?

Can a Dictionary with some facade functions serve the following purpose:

* something which allows me to move RD from wikidot to Jekyll?
* something I can expand with a new format if absolutely necessary?
* a 'transport' intermediary object separating formatting from the meaning of the formatting?

Yes:

* having a Dictionary with Python objects instead of strings I can operate on any format I want
* to change the format I simply need to add new Parsers and Formatters
* Dictionary **is** an intermediary transport object

So this one - the most important one - is done.

#### Does the name sound fine?

There is no naming problem, as we are not creating any object right now. Sure, I am really, _really_ tempted to create an object and call it 'The Universal Datanator', but this is my fun-seeking nature speaking here.

#### Can it hold the varied data it needs?

Yes. A Dictionary can hold absolutely everything. And the formal grammar defined here (again):

|  Element          | Operator|  What can it contain?       |
|:------------------|:-------:|:----------------------------|
| Document          | = | _Identifier_, Elements            |
| Elements          | = | {Element}                         |
| Element           | = | _Type_, {[TerminalValue]}, [EmbeddedObjects], {[Element]} |
| TerminalValue     | = | _Level_ \| _RawText_ \| _any_domain_named_KeyVal_pair_ |
| EmbeddedObjects   | = | {EmbeddedObject}                  |
| EmbeddedObject    | = | _Type_, _Position_, EmbeddedValue |
| EmbeddedValue     | = | (ESimpleValue \| EComplexValue)   |
| ESimpleValue      | = | (_Key_, _URL_) \| _(* any set of TerminalValues *)_ |
| EComplexValue     | = | {[TerminalValue]}, {[EmbeddedObject]}, {[Element]} |

should be able to deconstruct every complex object into something flat and simple to work with.

#### Can it use it without any problems?

The grammar is designed in such a way that it should be pretty easy to query data (one level only, preferably).

An intermediary object is a Dictionary. It is not too difficult to work with a Dictionary, as long as one does not get lost.

#### Can we write sensible tests?

* Tests for Parsers:
    * Does _this_ particular string map into _this_ dictionary? Use dict.items() to make sure it maps 1:1
* Tests for Formatters:
    * Does _this_ particular dictionary map into _this_ string? Compare strings.
* Test for a pair
    * Does text -> dict -> text generate the expected text on the output? Compare the initial text and the resultant text.
* Tests for Factories and Deserializers:
    * Does _this_ particular dictionary create an expected Domain Object?
    * Does _that_ Domain Object deserialize into _that_ dictionary?
    * Compare _this_ dictionary and _that_ dictionary with dict.items()

Yes, we can test if it works. It will be easier than testing wikidot formatted strings.

### Conclusions

#### If I cannot make a test, my analysis is not done

The functions and the public methods are an API of an object. They are, actually, our user interface for this object.

If I cannot make a test or if I do not know how to make one, this means I do not understand how to use this object, really (or a user interface I have designed is simply atrocious and makes this object hardly usable).

An object exists for a purpose. The test should check this particular purpose.

If I cannot write a test - either I don't understand the purpose or it is hard to use that object as it should be used.

In both cases it is a Bad Thing.

Note this: I have started writing this document thinking I am dealing with one object. I have finished writing this document with a dictionary – a standard Python dictionary – and four sets of functions. Therefore - with four sets of tests, which should be pretty easy to write.

#### Prioritize the important

In this particular context, the important problem to be solved was how to store very varied data. 

A key observation was that the most important part of the task was to split the _formatting_ (how things are rendered) from the _meaning of the formatting_ (what does the formatting mean).

So the use of the object was secondary in importance and difficulty (that is, it was unlikely this part will fail) to getting the data storage right. Thus the formal grammar and stuff - this is why I have focused on the held data first and the use of the object second.

Which leads us to...

#### Remember to check it with real data

This is the main reason why I did not try to implement rdtool 'the right way' from the very beginning. Initially, I did not have enough data yet. If you remember [part one](/2016/12/04/rdtool-1) of this analysis, you can see how rdtool changed with time.

But rdtool only followed (and influenced) the business requirements. The format of the data _has also changed with time_. Some data (older stories, for example) have never been modernized - there are documents which are simply malformed according to the standard we have today. In the past, _they_ were the standard.

Note how much I had to calibrate the formal grammar until I got it right. How much I had to check existing data against the grammar until I was satisfied. This was the key parts of this analysis.

The grammar made me realize that any type of custom data structures will be inadequate. It is much better to serialize my data into an existing format like XML or JSON. Especially because I cannot predict the future changes - as much as I could not have predicted the past ones.

And the fact I can make JSON or XML map 1:1 to a Python Dictionary is simply an icing on the cake - it means I have it easy.

#### Do not be afraid to change something that works

##### 1. Discard biases and misconceptions.

I usually say that it is not a good idea to ever let 'technical' objects be passed through an application. Integers, strings, dictionaries - those are technical objects. They are technical implementation of a data structure and they carry no _meaning_ by themselves.

I usually try to model the domain objects with powerful methods operating on the – preferably immutable – data held inside.

In this particular case I am willing to let a technical object (dictionary) be passed throughout an application and I have four sets of functions in four different contexts - parsing, formatting, building domain objects, deserializing domain objects. 

I have already explained why have I gone this way. I can always replace a dictionary with a domain object when I know it is needed.

What I have done here, however, runs contrary to most of the heuristics I am using when programming. Seeing the evidence, if I made an object in place of a dictionary here, it could worsen the readability and take some time I can use for something more constructive (say, playing a game).

Even the best heuristics are just that – heuristics. They are something we use because it works and because we have learned it works; very often we don't even know why it works.

By introducing this exception - letting a mutable dictionary move in an application - I may either confirm my existing heuristics (and learn what was the 'proper' solution here) or expand my existing heuristics and see that sometimes it is worth it to let an object like this dictionary exist on its own.

##### 2. Change our previous solution.

I had a formal grammar which worked with the current data – the one with sections, headers... I mean, really, it was perfectly modeled. However, when I started looking at it from the point of having to use it - it was unwieldy and cumbersome.

I have decided to de-optimize the perfect model to make it easier to work with. I have sacrificed some readability to get ease-of-use and to get it to correspond closer to the Domain Objects I have today.

I mean, nobody is going to read or write this type of JSON without an assisting tool. Not in this program.

So although I have started from modeling the data, I have discarded some advantages after I have started working on how to use that data.

This is a neverending trade-off. This is something which always requires a thought or two. What is important here and how much am I going to sacrifice to get it.

Fun, right?
