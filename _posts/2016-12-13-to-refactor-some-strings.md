---
layout: post
title: "To refactor some strings..."
date: 2016-12-13
---

### Abstract

This post is a written analysis on a refactoring which is to take place (if you are reading this post in 2017, it has already taken place). This post builds on a context from [that other post explaining rdtool](/2016/12/04/rdtool-1) and this one shows the technical reasoning on that refactoring.

This post is supposed to show a way of approaching a non-trivial refactoring. To show how the final conceptual result was achieved and what it has finally become in the end.

_This analysis is not refined on purpose. I am making things up while I am writing this post. This is a feature - I am trying to show how I am thinking while making this type of analysis. You will be able to see how the analysis is expanding my understanding of this problem and how I am narrowing the cone of potential solutions._

(I did correct typos and some stylistical things, though; I simply did not edit the facts or reasoning). 

### The value of this post to you:

1. If you :
    1. are a programmer who knows words like 'factory' or 'composite' or 'layer' who likes to co-design non-trivial refactorings in an exploratory way, I hope to show you:
        1. multiple approaches applied more or less in the same time to pinpoint the least risky and most impactful refactoring towards the domain
        2. how my understanding of the problem really changes with writing this document
        3. and I hope you will be able to pick a trick or two from what I am doing here
    2. are a technical person who prefers to refactor **code**, not refactor **towards the domain**, I hope to show you:
        1. that 'refactoring strings' very often is something different to what is stated
        2. that without knowing the domain it is very hard to get an acceptable solution; too easy to get a local maximum and get stuck in it
    3. are a technical person who does not see the value in drawing on the whiteboard, paper & pencil or other forms of analysis, I hope to show you:
        1. how to use an analysis as a tool supporting the code, separating the concept from implementation
        2. why it is worth it to use an analysis like that - how it saves me time and frustration
        3. how the analysis changes with time; how trying to draw the problem changes its understanding and makes it easier to reason about it.

### Why did I write this post?

When I have a complex change to perform in the program, I usually try to create an analysis allowing me to get to a sufficiently good approach to the problem.

This analysis does not have to be perfect. 

This post is an example of this type of analysis. 

Don't get me wrong - I am not doing this type of analysis every time I’m making a change in the code. However, those changes which have a lot of impact I usually do analyze like this. 

Instead of using the computer and word processor, however, I use several pieces of paper and a pen before I start writing the code. And although I don't type as much as in this post, I definitely try to get to the 'final output of this analysis' I present here.

So this post is, in reality, a written down analysis of a refactoring which is going to take place. Technically, it should take place somewhere in between 16 Dec 2016 and 1 Jan 2017.

### Business case - why refactor at all?

##### Context

The context and the domain are described in the previous post. Let me warn you, [that one](/2016/12/04/rdtool-1) is as long as this one. This one has more pictures though ;-).

Currently, rdtool (and all the data governed by it) is deeply connected with the wiki, using the wiki markup internally. 

##### Domain problem, in short

I want to move the data from the wiki to Jekyll - which uses a particular type of markup called markdown.

So at this point I have two different data sources and two different data destinations - wiki and Jekyll.

Between different static site generators - all of them using markdown - the implementation of markdown varies. This means, that a well written markdown for one site may render differently on the other site.

To make it worse, currently rdtool doesn’t have a complex, intelligent structure internally. It operates on flat pre-formatted strings, something like this:

    +++ Header H3 (wikidot format)

Which should look like this in markdown:

    ### Header H3 (markdown)

##### First hypothesis - setting the tactical goals

So the refactoring requires the following:

* Create a parser/formatter pair on input and output of rdtool
* Change the internal data structure in rdtool
* Make it all sufficiently configurable

And I will know I succeeded when:

* I can swap between wiki and Jekyll seamlessly
* I can add a new format for rdtool to operate on without issues or obstacles
* The readability of the code does not take a hit

### You know, changing some strings shouldn't be so hard...

It is not as easy as changing some strings.

This whole program takes some strings (reading them from text files) and saves some other strings (writing them to text files).

Those same strings mean something very different in different areas of the system. There are different _views_ made of the same strings which let us slice the data differently.

The question here is not about the strings, but about the internal data representation. In short, how to read something like this:

    +++ Inclinations:

    Base: normal (6)

    ||~ SCA ||~ SCD ||~ SCF ||~ KNO ||
    ||  -2  ||   2  ||   0  ||   0  ||

In such a way that we do not lose any important information, yet be able to extract all the value as necessary. And to be able to write it back.

While exploring the problem, first obvious question usually is...

### Where should the change be made?

##### Sequence diagram explanation

To continue, I have to explain a sketch of a sequence diagram. Those tools are usually used to show interactions between participants; I use them very often to verify if I have everything I need at my disposal. Basically, I use them to program without writing code.

In the picture below, you can see a relation between a plane trying to land and a control tower governing the runways. Let’s assume that everything goes okay. Note, that time flows up to down (so you read top to down in terms of chronology).

![Plane sends message to a tower, which sends message to runways. Sequence diagram.](/img/161213/example_1.png)

This diagram can also represent business processes, not only interaction between objects. For example, the diagram below showing the process of making tea:

![Sequence diagram showing how to make a tea](/img/161213/example_2.png)

The great advantage of using sequence diagrams is the fact that I am able to pinpoint what I’m going to need (in the case presented in the diagram, to make tea I need whatever is delivered on the leftmost arrow: water, kettle, teabag, cup).

##### The problem we have:

Rdtool is a program which operates like this:

![rdtool flow: start - load all business commands - run all business commands - end](/img/161213/rdtool_flow.png)

So there is no UI, there is no selection. It just tries to run all existing business commands. Every business command is self-contained; it reads all it needs and it saves all it needs (horrible in terms of optimization; I know).

An example of the code of a business command: BuildProfiles:

![single rdtool command: BuildProfiles, source code](/img/161213/single_rdtool_command.png)

And its corresponding sequence diagram:

![single rdtool command: BuildProfiles, sequence diagram](/img/161213/current_flow.png)

By "corresponding" I mean that the sequence diagram above is exactly the same (well, a bit generalized) as the code.

When I said that business commands are self-contained, it is because every business command requests some information directly from the data source (using the ‘provide’ verb) and saves the result to hard drive (using the ‘persist’ verb).

If we apply color code corresponding to the layers onto the above, we get the following results:

![single rdtool command: BuildProfiles, sequence diagram with color code: BuildProfiles is red, Provide/Persist is blue, other things are yellow](/img/161213/current_flow_2.png)

The red part means ‘what I want to achieve’ - I want to build profiles. The yellow and blue parts are how I want to achieve the red one. 

Blue parts - the infrastructure - is an interaction with the hard drive, data sources, data destination… overall, with things outside this program.

Yellow parts - the domain - is an internal representation of the model of the data. It takes what infrastructure provides and creates new insight from the data we have. This is where the ‘true program logic’ lies (vastly simplified).

So the question here is: where should the formatter and parser be inserted? Are they things from the red, yellow or blue areas?

Formatter and parser are something which are definitely not on the business level. They are really about the way the data is read and written. This means that they live in the blue parts of the program. In the providers and persistors.

![single rdtool command: BuildProfiles, sequence diagram, with only blue parts - infrastructure - highlighted](/img/161213/current_flow_3.png)

Which corresponds to the code in such a way:

![single rdtool command: BuildProfiles, source code with infrastructure colored blue](/img/161213/single_rdtool_command_changes.png)

The above show us directly:

* All changes in the formatters and parsers should be contained to Blue area. Any change outside of Blue area requires analysis - why did it happen?
* Thus, the internal representation of data in rdtool should allow parsers and formatters to be contained to Blue area.
    * This is a new requirement on the refactoring of internal representation
* Worst case scenario? Blue area will have to be rewritten (very unlikely).

### Let's go deeper

Provider uses a source. At this moment, the relation is quite simple – the source returns the data, the provider returns domain objects made from the data.

Just like this:

![business command calls 'provide' which uses the source](/img/161213/provide_source_relation.png)

To try to narrow the impact of the change, where should the parser be located?

And this proved to be a truly interesting question.

![business command calls 'provide' which uses the source, add 'parse' without linking it](/img/161213/provide_source_parse_relation_1.png)

Intuitively, I understand the parser as something which takes a formatted text and returns ‘something’ - usually a [tree structure](https://en.wikipedia.org/wiki/Parse_tree) of some kind (but... tree containing **what type of data** in this case?). 

This is also reinforced by a very interesting article by Steve Yegge, called ['The Emacs Problem'](https://sites.google.com/site/steveyegge2/the-emacs-problem) which shows an evolution of working with text from regular expressions, via XML to holding data directly in LISP - as in, holding data as a part of the code itself. Kind of what I want to achieve in rdtool by making intelligent objects.

Having the above in mind, let's see our options.

##### 1. Source calls 'Parse' and returns the DataTree

![business command calls 'provide' which uses the source; source calls 'parse'](/img/161213/provide_source_parse_relation_11.png)

Instead of returning text, our source returns DataTrees now. This means that every source is expected to return DataTrees in the same format, no matter the original resource.

Symmetrically, our destinations will request DataTrees now. So the providers change the DataTrees from an appropriate source into objects using factories, while the persistors pass the DataTrees from objects directly into destinations.

Advantages:

* tight coupling of the source and a formatter means that if our source is very unusual, it may require a different formatter; this scenario works sufficiently well.

Disadvantages:

* if we moved from Jekyll to Hugo, for example, from my observations there are differences in the way lists are written in markdown (has to do with whitespaces...). This would require making _Hugo source_ and _Jekyll source_ with possible code duplication

##### 2. Provide calls 'Parse' as a part of its operations

![business command calls 'provide' which uses the source; receives TextFile. Then, 'provide' calls 'parse'](/img/161213/provide_source_parse_relation_12.png)

Our sources and destinations did not change. They work with something which looks like a set of text files - well, technically, something which looks like strings read from the text files.

The providers and persistors are responsible for proper transformation between the text and the DataTree. Parsers and formatters are visible in the provider and persistor respectively.

Advantages:

* you can swap the parser as needed
* it is easier to make end-to-end tests including parsers and sources

Disadvantages:

* you can (accidentally?) use a parser which does not correspond to the source
* in case of a strange source, it might have to generate the text files first instead of generating DataTrees from the very start

##### 3. Object creation calls 'Parse' as a part of its operations

![business command calls 'provide' which uses the source; receives TextFile. Then, while objects are constructed, appropriate factories call 'parse'. Potential of some negotiated knowledge between factories-parsers-data](/img/161213/provide_source_parse_relation_13.png)

Those parsers over here – and those formatters over here - would be doing something different. Every single factory would have to extract the meaning from the set of input data. This could work, but I do not see the advantage of choosing this approach over the other ones.

Formatters in this approach would belong to the objects, not infrastructure layer. This means that the parsing and formatting would be between yellow and blue areas; not directly in the blue area.

Advantages:

* very high level of control
* maybe it is possible to skip the DataTrees at all; it would reduce the scope of the refactoring

Disadvantages:

* Very brittle; changes in formatters could cascade between factories
* a lot of support code in places where it should not really be expected
* I don't really _understand_ how this is supposed to look like; a new parser per entity?

##### 4. Returning to 1, but with a different view

![business command calls 'provide' which uses the source; source calls 'parse'; this time 'DataTrees' were swapped for 'Information' - not working with code but with its intention](/img/161213/provide_source_parse_relation_14.png)

I have been making a major mistake from the very beginning. I have been looking at a current implementation of the code, not at the **intention** of the current implementation of this code.

If you look at this picture, it is quite obvious that the source is supposed to deliver whatever is needed for further objects to function. In current rdtool this is the text content of the text files. In future rdtool those may be DataTrees.

If you look at this picture, it is quite obvious that the source is supposed to deliver whatever is needed for further objects to function. In current rdtool those are text files. In future rdtool those may be DataTrees.

Also, if Hugo and Jekyll and Lektor and other static sites generators really differ with the rendering of the markdown format, it might be better to create proper formatters and parsers inside the respective sources. That way it is impossible to generate something which does not work by accident.

So, (1) it is.

### ...but trees containing WHAT?

##### The problem of the data model

The data I am dealing with will be rendered in a following way, more or less:

![wiki render of dramatis personae section and locations section; Dramatis Personae is a H1 Header with text having a form 'mage: Kate, who did something' where Locations is a H1 Header with text being an ordered list](/img/161213/data_model_render.png)

which is represented on wiki like this:

![DPersonae has a structure '+++ Dramatis Personae, newline: mage: Kate, who did X' Locations have a structure: '+ Locations, # First level of a list, ## Second level of a list](/img/161213/data_model_wiki.png)

and in markdown like this:

![DPersonae has a structure '### Dramatis Personae, newline: mage: Kate, who did X' Locations have a structure: '# Locations, 1. First level of a list, 4 spaces, 1. Second level of a list](/img/161213/data_model_markdown.png)

and, really, represents the following data model:

![DPersonae has a structure made of Records, each record has: Qualifier -mage or human-; Who -unique name-; Deed -saved a village. Locations have a structure: of a nested list - tree, really, where every element of a that tree is a link to an appropriate page](/img/161213/data_model_real.png)

And here, the question. What should the parser output? As in, how aware of the domain should the parser be?

There is also one more complicated issue. That one is the Read Profile. Let's see an example of Angela:

This is how Angela is rendered:

![Angela is rendered with several H headers; H1 as a name, H3 for 'impulses' and 'abilities' with H5 for sublevel abilities](/img/161213/data_model_angela_render.png)

To get this, I would need, respectively, with corresponding syntax:

wiki:

![Angela in wiki syntax; every H1 is '+', H3 is '+++' and H5 is '+++++'](/img/161213/data_model_angela_wiki.png)

markdown:

![Angela in markdown syntax; every H1 is '#', H3 is '###' and H5 is '#####'](/img/161213/data_model_angela_markdown.png)

Thing is, the underlying data model looks like this:

![Angela as a data model; currently the 'read' part of the profile is read as a plain text. It is in red colour.](/img/161213/data_model_angela.png)

So the real problem here is as follows:

**Given the following:**

* Some of the formatting represents a particular MEANING in some areas of the code.
    * 'Sections' - 'Dramatis Personae' or 'Locations' in Stories
* Some of the formatting represents just the WAY how the text should be written.
    * **For now** - 'Impulses' or 'Abilities' for a Profile
    * Always - some formatting in particular Stories

**The question stands:**

* How aware of the text / domain should the parser be?
* What should be the internal data model in the application?

Note, that if we return to the previous problem, this is **not** a vertical, 'blue' problem":

![single rdtool command: BuildProfiles, sequence diagram, with only blue parts - infrastructure - highlighted](/img/161213/current_flow_3.png)

This is a horizontal problem; it cuts most layers (if not all of them):

![single rdtool command: BuildProfiles, sequence diagram, with only blue parts - infrastructure - highlighted, but with a green horizontal line cutting through all the layers](/img/161213/current_flow_3_horizontal_problem.png)

But this actually means it is very, very important to get this thing right.

##### 1. Pure, perfect text parser

I know! Let's make a 100% powerful pure text parser! The data model will be hidden inside JSon, XML or something, and it will represent something like this:

    This text is a bolded section H3 | 32 H3, 17-21 bold, newline
    This text is a H5 section        | 25 H5, newline
    This text is bolded here         | 14-24 bold, newline

This would, of course, work. This is a heavyweight approach; I guess this is how the word processors do it - with text and... tactically applied text decoration? (incidentally, I should look into LibreOffice codebase if I ever decide to go this way; that is, to see how they made it work).

So having this approach the code is very easy to convert, but the underlying data model might be a small nightmare to work with. That is, not only will the regular expressions be less reliable (because at this moment I search for a pattern involving the special section symbols), but also the information about the data will be located in parallel to the data itself.

This would work, but let's explore further options.
   
##### 2. Pure domain aware parser

To expand a possible set of options, I always try to have two polar opposites before iterating on a 'perfect' solution. So, this one would be an impractical opposite of the pure text parser approach. In this approach the parser really is aware about the domain and it already builds the data model properly. Something like this:

    This text is a bolded section H3 | copied verbatim
    This text is a H5 section        | copied verbatim
    This text is bolded here         | copied verbatim

In effect, it does nothing at all when dealing with 'text'. But when it deals with a pattern it knows about, something like this:

    +++ Abilities:   | start of Abilities section
    +++++ Expert:    | Ability level Expert starts
    hiding tracks    | Single ability at Expert level
    
it is able to deal with it, because it seeks particular patterns to parse, not the text itself. 

By being perfectly aware of the domain, however, there are several disadvantages:

* it needs to know the patterns (schemas) precisely; they cannot change
* it needs to take the role of the Factory, too (it will be semi-responsible for building domain objects)
* text which is outside the 'domain entities' remains untouched at all

Impractical and does not serve our needs at all. But after several iterations and ideas...

##### 3. Hybrid text section / plaintext parser

When I recall what I know of the domain, I can see the following:

* the **bold** and _italics_ do not carry a meaning
* the sections (represented by H1, H3, H5 headers) usually carry a meaning
* important text is **always** prepended by a meaningful section
* **bold** is identical in wiki and markdown formats
* _italics_ differ, but it doesn't matter AS MUCH, as I don't use it often
* lists and tables are usually written, not read (for now)

So if I was to prioritize, I would do the following domain-wise:

    This text is a bolded section H3 | Text Header H3
    This text is a H5 section        | ...containing Text Header H5
    This text is bolded here         | ......containing this verbatim Text
    
and:

    +++ Abilities:   | Text Header H3 "Abilities"
    +++++ Expert:    | ...Text Header H5 "Expert"
    hiding tracks    | ......"hiding tracks" copied verbatim

In short, a tree structure [(like Steve Yegge wrote)](https://sites.google.com/site/steveyegge2/the-emacs-problem), with 'Document' being a root and headers being the nested sections - but at least now I know what this tree is supposed to hold - text **sections** denoted with **headers**.

**Advantages:**

* Meaningful information (sections, headers) is parsed and transcoded between wiki and Jekyll
* Factories are responsible for creation of Domain Objects from DataTrees (to be precise: from the text held by sections denoted by headers) - separation of roles
* Much less work and much more readable than case #1

**Disadvantages:**

* Meaningless information (formatting) is rendered in wrong ways
* ...which means I have [Yet Another Subset Of Markup/Markdown in this program...](https://imgs.xkcd.com/comics/standards.png)

**Observations:**

* It is possible to transcode things like 'italics' or 'lists' if absolutely needed by adding elements of solution '1' into '3'.

This would mean, that the 'Dramatis Personae' and 'Angela' problem really looks like this:

'Dramatis Personae'

![Dramatis Personae: header is PARSER's problem, record is domain object factory's problem; parser will ignore it. Same with location - header is parser's problem, content - nested list - is copied verbatim](/img/161213/data_model_markdown_explained.png)

'Angela'

![Angela: name is H1 header - parser's problem. Impulses is H3 header - parser's problem. Lines inside are copied verbatim - domain object factory's problem](/img/161213/data_model_angela_markdown_explained.png)

Note how the parser does not really care about everything in this approach. It only tries to isolate the meaning from between the formats.

But this also means that the formatting (opposite transformation) will not be omnipotent. The formatter receives DataTrees, so it will never receive something identified as a 'nested list'. This means that the raw data which comes from the text inside the section is a responsibility of a Domain Object, the formatter is just responsible for proper writing down the _meaningful_ stuff.

So the X -> X operations (wiki -> wiki, markdown -> markdown) should be rather safe. Only X -> Y operations (wiki -> markdown, markdown -> wiki) should have some problematic issues.

Which leads to the final question.

### Business impact of the refactoring

##### Current refactoring choices

I have selected a solution which looks as follows:

* The solution should have a Parser injected into a Source and a Formatter into a Destination
* Internally, rdtool should not operate on wiki-markup text but DataTrees with a structure of Document-H1-H2...H5 nested sections.

This has the following advantages:

* The particular implementation of the domain is still flexible; the parser does not have to understand the domain
* It should be possible to operate on any type of format in the application to come (in terms of meaningful stuff - headers)
* Formatting to save on a hard drive should usually work seamlessly - tight integration with the Destination and being aware of context from the domain object
* Clean separation between the role of infrastructure parsers and domain object factories
* Sensibly easy to write; corresponds with my current understanding of the domain

And the following disadvantages:

* Currently in the code the infrastructure layer is not as clean as it should be. This will require moving some parts between domain and infrastructure
    * On the other hand, this is simply called 'cleaning' and has to be done once in a while
* X -> Y operations (between formats; wiki -> markdown) will not be converted perfectly
* Formatting of _non-meaningful_ information must be governed by the Domain Objects

##### Is this an acceptable solution?

The real question, really, is:

1. is it better to write the code making sure the transition is 100% seamless? (hardest)
2. is it better to write a formatter and parser and change the data model, but accept imperfections in translation?
3. is it better to find a program transcoding from wiki to markdown and 'just' change the data model?
4. is it better to find a transcoding program AND just change the references to wiki syntax into markdown syntax? (easiest)

How often, realistically, is rdtool going to be used between Jekyll and Wikidot? Or between Jekyll and Hugo?

In our case, probably... once. Well, if a very compelling reason appears to move from Jekyll to something else, second time. This strongly hints towards 'full seamless transcoding is not where the business value lies here'. 

But maybe - just maybe - we can simply make some edits and skip this refactoring at all?

Because of many regular expressions hidden in rdtool, very often connected with the wikidot format, like those ones:

    '\n\+\+\+\s' - starts a H3 section (which HAS to have H1 earlier)
    '\s*#.+' - numbered list single line
    
any change to the format of rdtool would require changing the above to correspond to markdown syntax. If so, I have to make those changes _anyway_ by moving from wiki to Jekyll.

If so, let's do it _properly_ and save that pain in the future.

Thus, introduction of parsers and formatters will be a pain, but this pain is unavoidable, really. It's paying the [technical debt](http://wiki.c2.com/?TechnicalDebt) which is the result of not knowing the domain well enough when I started - and working to get results as fast as possible.

Also, with rdtool doing more and more, I am slowly starting to think that I may not be the only user of this application. If so, why not make it easier for others to integrate it with their workflows - by supporting adding formatters and parsers they need? It won't cost me that much time.

##### 2. is it better to write a formatter and parser and change the data model, but accept imperfections in translation?

Yes, I believe this is the best option for now.

I estimate I should be able to finish before January 2017. Let's see how it goes...

### Risk assesment of this refactoring

1. The riskiest part is achieving parity between wikidot and Jekyll
2. The riskiest part is introducing parsers and formatters for multi-formats
3. The riskiest part is changing the internal data model

##### 1. The riskiest part is achieving parity between wikidot and Jekyll

Actually, the riskiest **and** most difficult part up to now was... selecting a Static Site Generator. I have tried many implementations of SSG on Gitlab. From those I tried, the easiest to install locally (on Windows) and the easiest to achieve parity with the wiki (especially with tables, unspecified in markdown syntax) was Jekyll.

So the riskiest and most impactful of steps is already passed. I have a guarantee that if I do everything right, Jekyll will work as I need it to.

##### 2. The riskiest part is introducing parsers and formatters for multi-formats

Nope. This is infrastructure level. This below is a source code of a provider:

![Provider is made of 16 lines and 2 of them have a Source - only those would be affected](/img/161213/provider_source_code.png)

The risk of this change is contained in the Source itself. If I wanted a multi-format or to 'just' change the format from wiki to markdown, the core of the change would be very deep in the parts of the code nobody really looks at.

_(except changing all formatted strings in most factories in the whole application - it is not 'hard'; I have a test harness; but it is tedious)_

##### 3. The riskiest part is changing the internal data model

Yes, this is it. Look at the provider again:

![Provider is made of 16 lines and 4 of them are affected - but everything 'internal' is; change spills outside the provider](/img/161213/provider_source_code_2.png)

Even assuming perfect encapsulation (this is going to be a good test if I have it, actually), every factory building those entities will have to be changed. All legacy areas I did not touch before will have to be reconstructed. The changes will spill outside the provider and they may touch quite a lot of places.

...fortunately, with sufficient encapsulation and strong objects this should not happen.

...unfortunately, I have some older code in some places.

Anyway, this is definitely the most dangerous and difficult change.

### How to perform this refactoring

##### Current state of the application

At this moment I have my tests. They are green - they prove that my application works.

##### What happens if I start from data model change?

Let's assume I start from the data model change (create a parser and make it return the DataTrees instead of plaintext, doing nothing more). What will happen to my tests? They will definitely stop showing 'everything works'.

How easy is it going to be to get them back to green? Pretty easy - I simply have to get the underlying data model fixed.

I will start regreening the tests, one by one, by rebuilding functional slices of the application, business commands one after the other (starting from the easiest ones, of course). 

Where the old data model is present and it spills too much, it will be rewritten. 

Where the tests are lacking, they will be added.

So the tests will show me which parts of the code do not work yet.

Let's assume it is done. All tests are green. I am working on injecting multi-format to the parser. Hard or easy? Should be sufficiently easy - the change is localized to the Source (well, parser, to be exact). If I have accidentally skipped a part of code with old format somewhere, this is when I notice it.

I will also start to notice meaningful things in the code which do not get parsed properly, because they are not text - they are just represented as text. For example, links. This will force me to go back to the parser and modify the underlying data model from:

    Section H1
    .. Section Hx
    ....Plaintext

To:

    Section H1
    .. Section Hx
    ....Text with special 'hooks'
    ....Parsed Things inserted in text (links, lists...)

And then the Domain Object Factories can expect both the plaintext and the 'Things' which got parsed and which are intertwined with the code. And - possibly - the Factories will simply inject those Things directly into Domain Objects.

_So I see the way forward if I select this approach._

I also expect I should write a lot of tests at this stage. Those tests would look more or less like this:

    text_1 -> parser -> formatter -> text_2
    is text_1 == text_2 ?

To verify the particular parser / formatter operation. Especially in the area of 'parsed special Things with hooks in Text under a Section'.
    
Anyway, this refactoring should not be too difficult in this sequence.

##### What happens if I start from multi-format?

I am starting from writing a parser and a formatter, they still operate on text and they are slowly starting to work with two formats.

Because I am adding a format, it is unlikely for my tests to stop working (because the old format did not stop working). However, I may have to **duplicate** some tests I currently have to cover a new format - I want to make sure both wiki and markdown syntax are working. And starting from multi-format, my program is not format-agnostic, so the change of format cascades through most of the factories in the whole application.

Why does it matter so much? As in, what can go wrong with a few factories?

For example, this:

![The source code of a single factory method using regular expressions](/img/161213/regex_factory.png)

If you REALLY want to know, this is a regular expression for a link in wiki format. 

    def profile_masterlist_metadata_record(self):
        return '(\[\[\[)(.+)(\|)(.+)(\]\]\])(.*)'

It will have to look like this in markdown (tested in [Regex101](https://regex101.com/#python)):

    def profile_masterlist_metadata_record_in_markdown(self):
        return '(\[)(.+)(]\()(.+)(\))(.*)'

Don't get me wrong - I will have to do it anyway if I start from changing the data model (at the level of parser, though) - but I will have to change **most** factories and make two variants - one with wiki and one with markdown syntax. Then I will have to do the same thing to both types of formatters.

When I finish introducing the second format the code will have a lot of 'if-else' structures (or strategy patterns, or factories-inside-factories...). And **then** I am starting to change an **underlying higher-risk data model** and all the tests go red.

And the code is less readable than in variant 1 at that point. Because of if-else structures and the need to consolidate.

Not only I have more tests to turn green, but also more code to move to parsers and formatters.

It has just become far, far more difficult.

And after I finish - I can delete some tests which I wrote before to add a new format.

_This should also work. Seems to be much more work though - and seems harder._

##### What to select, then?

In the world where all programmers are beautiful, nice and wise, I would give you an awesome answer. However, in the real world we are dealing with me... 

I will select variant 1.

Why?

Variant 1 gives me something awesome - much less work than variant 2. 

My tests are also failing from the start, so I can see which parts of the rdtool are not working yet. This gives me a clear-cut progression.

It is less risky - if I have a fundamental misunderstanding of a problem, I should be able to spot it faster and go back to the drawing board faster after a single change revert.

I am working on a harder and riskier problem earlier on, when there is less code and the code is simpler.

And... although at this point I am still not sure how should a 'link' look like in this type of DataTree (which kind of is a major problem, really - if I don't know what I refactor _into_, how do I know if I _succeeded_?), I have a few ideas. This is really a problem of inlining Things into plaintext. This problem has been solved before. Say, in C++ (I hope I still remember the syntax):

    string location = "out there";
    string result = string.Format("The Truth is {0}", location);

This way I would have the following:

    H1 section
    .. H2 section
    .... "The Truth is {0}"
    .... {0}: <Link object pointing at "out there">
    
I am probably going to use this C++ syntax or a [markdown syntax for a reference link](http://daringfireball.net/projects/markdown/syntax#link). That way I have both Things where needed and I do not lose the information where those Things were located - if I need that information.

If this is unsuitable, I can iterate on this idea. But if something works for C++ or for markdown, I don't think it will fail me here - my needs are smaller.

The most important part - I will not be surprised with this problem. I can defer the actual solving of that problem when I can test it. And I have a solution which 'potentially works' - even if it is not perfect yet.

### The final output of this analysis

##### Solution I have selected: how does it look like

Currently, rdtool operates internally on plaintext (strings) and factories are the entities which extract the meaning from the text using the formatting based on wikidot syntax.

This solution changes the internal data model from the plaintext with specific format to some kind of DataTrees.

For the data which looks like this:

![DPersonae has a structure '+++ Dramatis Personae, newline: mage: Kate, who did X' Locations have a structure: '+ Locations, # First level of a list, ## Second level of a list](/img/161213/data_model_wiki.png)

I expect to have the following corresponding data structure (DataTree example):

![Root has two Sections. Every Section has a Header, Text, Supporting Things and Child Sections. Example: Header is "Dramatis Personae", Text is copied verbatim from the picture, Supporting Things is none and Child sections is None. Example 2: Header is "Locations", Text is "{0}", Supporting Things is a Composite pattern python object and Child Sections is None](/img/161213/final_data_structure.png)

The impact of this change is quite large; this requires changing the whole internal model of rdtool. Fortunately, the current code structure supports this type of change by making all objects built by factories. So the change truly impacts the 'text goes into factories' and 'text goes outside the objects' areas. Unfortunately, this is quite a lot of code.

![single rdtool command: BuildProfiles, sequence diagram, with only blue parts - infrastructure - highlighted, but with a green horizontal line cutting through all the layers. The line stops at the level of 'Extract relevant data' where domain objects start and appears again at 'send data to Formatter from Business layer', more or less. The size of the green line is reduced to 66% of the original length](/img/161213/current_flow_3_true_horizontal_problem.png)

The parsers, formatters and other 'environment-based syntax' (Jekyll, wikidot, wikimedia...) should be located in the 'blue' area of the picture - infrastructure level. 

The new internal data model is the green line cross-cutting all the layers. 

If I am lucky, the line will stop at the Domain Objects when factories start doing their job (thus it stops at the level of Domain Objects).

Still lots of stuff (repeat for every command, for every domain object...), but not as much as I initially thought.

##### Will the goals be met?

1. I can swap between wiki and Jekyll seamlessly
    1. No, this one is unlikely to be met. Jekyll -> Jekyll will work, wiki -> wiki will work, but transitions will require some manual intervention. All the _meaningful_ things (things the rdtool is aware of) will be able to work out of the box, but _meaningless_ formatting will not be transcoded.
2. I can add a new format for rdtool to operate on without issues or obstacles
    1. Yes, as long as we are talking about the _meaningful_ areas of the new format. _Meaningless_ formatting will require modifying the internal data structure.
3. The readability of the code does not take a hit
    1. This should be met quite easily. The readability should even improve.
4. The internal representation of data in rdtool should allow parsers and formatters to be contained to Infrastructure area
    1. This should be met quite easily.

This means that out of 4 goals of this refactoring, only 2 are completely met. And those which are not met are really the business 'core' goals of this refactoring. Is this really acceptable?

Yes. 

What was the true reason behind this refactoring?

_We wanted to move from a wiki to a static site generator and we chose Jekyll_.

So there exists a single most important goal, superior to all those presented above:

* Will this refactoring let me move from wikidot to Jekyll?

And the answer is:

* After the transition is done, yes.

Never forget why we are trying to do something. Sometimes the way we are _phrasing_ our goals means that the _meaning_ behind those goals gets lost.

##### Solution I have selected: how will the refactoring - and the business goal - be performed

Obviously, I am skipping 'add tests where relevant'. I would have to repeat it everywhere.

1. Start from introducing DataTree object in rdtool with a parser and a formatter
2. When everything works, add multi-format to a parser and a formatter
3. Then make (1) and (2) work together
4. When it does, start testing on real data
5. And finally, migrate to Jekyll, assisting manually where needed

### Conclusions

##### 0. First meta-conclusion

The conclusions will not be about the result of the analysis itself; it will be about the act of making the analysis and making it work for you.

If this particular analysis feels unwieldy and clumsy, well, it does feel like it because [it is clumsy and unwieldy](http://edw519.posthaven.com/what-tools-do-you-use-for-analysis). I am used to using pen and paper and dictating things to the computer while walking around the room, without any corrections, to be able to see more or less where has my mind wandered THIS time.

It is much slower and much harder to make all the diagrams 'readable', for someone to read 'forever' (as in, put them on the internet) while being in the mode of 'problem solving'.

It is even harder to have to edit the text from all the gibberish AND to finally get to a result which is at least usable.

##### 0. Second meta-conclusion

This type of analysis is so much easier and more efficient if it is done with someone else. [This post](https://8thlight.com/blog/daniel-irvine/2016/11/28/code-is-better-when-we-write-it-together.html) is about pair programming, but really, same reasoning applies here.

When there are many people suggesting ideas, walking around to check something, where there are discussions about tradeoffs... basically, it is both more fun and more efficient compared to one person having to wrestle with their thoughts.

But people are on different skill levels! How can they help!
 
You might have noticed that most of this analysis follows those steps: 

1. find something similar (in programming or real life)
2. compare it to the mental model of the application today
3. how would this 'something similar' look like in the code
4. how is this currently held mental model implemented in the code today. 
5. how to make a transition path?

Which leads us to:

* Even the least experienced person can find 'something similar' in the real world. 
* A business person not knowing the code usually knows the domain very well, so the solutions designed with that business person will usually be more accurate.
* A programmer knowing a part of the code very well can help making a transition path between 'today' and 'expected solution', etc.
* Someone dealing with customers knows what is important to them; will let us prioritize the paths from their point of view.

In short, I strongly recommend doing analysis like this one with other people. Everyone can pitch in something useful and everyone can learn in the process.

##### 1. The goals of refactoring are important, but its impact is even more important

It is very easy to set the goals of refactoring. It is also very easy to forget that there exists a reason why we started to refactor that code in the first place.

Very often, it is so tempting to just go with the flow and repair the reality... just make it _right_. At least once.

However, there is always a reason why we are refactoring something. Something more than "oh, I want to change some code". This 'something more' is your metric for success. Your **observable impact** which also shows you _when the refactoring is done_:

1. Maybe working with the code became something similar to swimming in the swamp and you want to make it hurt less? Nobody knows how to work in that swamp?
    1. If you succeed, you will work faster. So optimize those changes in the code which let you work faster. Optimize developer productivity (hard metric) and developer morale (soft metric).
2. Maybe the code is too slow... or fires OutOfMemory errors?
    1. If you succeed, the program will execute faster and there will be no OutOfMemory errors. Optimize eliminating those things, do not touch other parts.
3. Maybe you need to add a new functionality to a system not designed to expand in this area?
    1. If you succeed, you will be able to add a new functionality. Optimize this particular action. Do not touch the code outside this area unless you really have to 
        1. note: I had to, because of a mismatch between code structure and business vision
        2. note: If I had no time and it was mission critical, I would go for 'change internal format from wiki to Jekyll' only. Really.

You can never do _everything_. So focus on one thing at a time and do it well. The pain points will move with time. Even more time will pass. 

One day you will blink and you may notice with wonder that you have, actually, done 'almost everything' and you have no idea when it happened.

##### 2. Analysis is iterative and it has to stop one day

Refactoring is inherently an _exploratory_ activity.

I want to get to the _library_, but I don't know exactly where it is. So I am moving more or less in the _direction_ of the library, adapting to existing buildings and the flow of the streets. Eventually I will get there, but the exact path is unknown.

I want to achieve my _purpose_ (here: move RD to Jekyll), but I don't exactly know how. So I am making different hypotheses, moving more or less in the _direction_ of the purpose setting tactical goals (the goals of this refactoring). But the tactical goals are secondary to the purpose; the true goal is _observable impact of the refactoring_. Eventually I will get there, but I am not really sure how exactly.

And it is totally fine as long as I have managed to not lose anything important in the process and not invest too much for too little observable impact.

This also means that the closer to the _start_ of the analysis we are, the least likely it is we are close to the solution. Some iterations have to pass.

##### 3. No amount of analysis has ever survived the contact with the code

If I spent more time on this analysis I could get to a very low level of pseudo-code. Even more, I could - technically - reconstruct rdtool into diagrams and rebuild it into something new and beautiful. On paper, of course.

The truth is, no whiteboard design has ever survived the contact with the real code - and especially with the real data with all its edge cases and horrible malforming.

As long as I am writing an analysis, I am actually not providing any 'value' to the project. There is no 'progress'.

At some point one has to stop writing _about_ the code and start writing _the code_ itself.

When?

At the point you feel you understand whatever is to be understood. I have reached that point in the 'final output of this analysis' section of this article. I know:

* how to proceed in terms of sequence - what to start from
* which parts of code are to be changed, _more or less_
* what I am refactoring into, _more or less_
* what to expect, _more or less_

This is enough for me to start. Not knowing some of those things could make me waste time while fumbling with the code, reverting the changes and raging at my own failure to 'refactor some strings'.

If I make a mistake in the process, if my assumptions are wrong - I will simply scratch what I have written and return to the drawing board, knowing more.

Sooner or later, I will make it work. It is just a matter of time and knowledge I may lack at this moment.

##### 4. So what is the purpose of the analysis, really?

Note how close I was to some local maxima while writing this article:

* I thought this is easier (changes localized only in the Parser/Formatter areas in Infrastructure level)
* THEN I thought it is harder - I will have to change all Domain Objects
* THEN I remembered I have my factories; so I just have to change those
* THEN I realized my trees may have to work like in word processor (keeping formatting)
* OH WAIT, Sections are really enough
* OH WAIT, Sections are NOT enough. What about links and lists?
* ...there was that C++ thingy with inlining ints...
* ...ah, so now I understand the data structure...
* ...and the Parser - Factory - Object - Formatter relation

If I started this refactoring from writing code, what would I save? 4-8 hours? _(I am not counting the time spent writing this article in a readable form)_

Stumbling upon those questions while writing the code could cost me much more time and energy, especially in rewriting tests and in trying to think my way out of the situation without having higher level of conceptual understanding. 

What gave me that understanding? An analysis.

And this is a purpose of an analysis.

To _understand_ what I am doing here. Nothing more. To give me _direction_ towards my impact.

##### 5. 'Refactoring some strings' rarely is what it seems

Which leads to the final point.

How hard can 'refactoring some strings' be?

**It can be very hard.**

'String' is just an in-code representation of some kind of a domain concept. 

'Refactoring strings' often really means 'adjusting the rigid, crystallized code - and _everything what relies on that code_ - to the ever-evolving mental model of the domain'. 

Sometimes it is an easy task. Sometimes it is a very difficult task. It depends on the deviation between: 

* _business vision_ of the purpose of the program

and

* the _actual implementation_ of the program itself.

Fortunately, it is possible to be done.

Sometimes it is just not feasible ;-).