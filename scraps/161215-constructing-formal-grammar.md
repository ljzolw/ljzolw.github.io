---
layout: post
title: "Constructing formal grammar for rdtool DTO"
date: 2016-12-15
---

### What is this?

This is a complementary post to [part three of RdTool refactoring](/2016/12/15/trees-musings-on-rdtool-data). This one focuses on constructing formal grammar for RdTool from the existing data. More pictures and screenshots than text.

### Reminder of formal grammar rules

I am going to draw some trees to finally end with [Extended BNF](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form#Basics).

X = (Y) means that Y can appear more than once

X = [Y] means that Y is optional

X = A, B, C means: X is composed of A, B, C; you need all three

X = A \| B means: X is either A or B (but never both; choose one)

### Several simple cases

#### 1. Well-formed simple story

This:

![null](/img/161215/r01_simple_story.png)

Corresponds to:

![null](/img/161215/r01_simple_story_struct.png)

In human language:

* Document requires one or more Sections. 
* Section is made of one Header and one Text.

In formal grammar:

|  Element     | Operator|  What can it contain? |
|:-------------|:---:|:----------------------|
| Document     | = | (Section)             |
| Section      | = | Header, Text          |


#### 2. Single profile

So now we have sections which can contain sections

![null](/img/161215/r02_simple_profile.png)

Which corresponds to this (only partially; too much to draw):

![null](/img/161215/r02_simple_profile_struct.png)

In human language:

* Document requires one or more Sections. 
* Section is made of one Header, one Text (which is optional) and (optional) Sections - may have more then one.

In formal grammar:

|  Element     | Operator|  What can it contain? |
|:-------------|:---:|:----------------------|
| Document     | = | (Section)             |
| Section      | = | Header, [Text], [(Section)] |

##### 3. Dramatis personae with links, depth level relevant

This one gets far more interesting - we have links in dramatis personae and right now I have noticed that the headers require levels to properly function. Thus:

![null](/img/161215/r03_story_with_links.png)

Maps to this:

![null](/img/161215/r03_story_with_links_struct.png)

In human language it means:

* Headers are non-terminal (they split into something)
* Text is split into RawText and SmartText
* We have support objects

|  Element     | Operator|  What can it contain? |
|:-------------|:---:|:----------------------|
| Document     | = | (Section)             |
| Section      | = | Header, SmartText     |
| Header       | = | RawText, Level        |
| SmartText    | = | RawText, [Support]    |
| Support      | = | [Link]   |
| Link         | = | Key, URL              |

##### Merging the tables

|  Element     | Operator|  What can it contain? |
|:-------------|:---:|:----------------------|
| Document     | = | (Section)             |
| Section      | = | Header, [SmartText], [(Section)]     |
| Header       | = | RawText, Level        |
| SmartText    | = | RawText, [Support]    |
| Support      | = | [Link]   |
| Link         | = | Key, URL              |

#### 4. Single Stories Masterlist

I have cut a part of an existing story masterlist, without changing the data this time (too much work), so expect something which looks like gibberish:

![null](/img/161215/r04_masterlist_excerpt.png)

Now THIS one changes a lot:

* We get a list which contains links
* We get multiple objects with different positions

So the structure looks pretty... interesting (by the way, I renamed 'support' to 'embedded objects', like good old ActiveX times):

![null](/img/161215/r04_masterlist_excerpt_struct.png)

In human language it means:

|  Element          | Operator|  What can it contain? |
|:------------------|:-------:|:----------------------|
| Document          | = | (Section)              |
| Section           | = | Header, SmartText      |
| Header            | = | RawText, Level        |
| SmartText         | = | RawText, EmbeddedObjects |
| EmbeddedObjects   | = | (EObject) |
| EObject           | = | Position, [Link], [List]    |
| Link              | = | Key, URL  |
| List              | = | (SmartText) |

##### Merging the tables

|  Element          | Operator|  What can it contain? |
|:------------------|:-------:|:----------------------|
| Document          | = | (Section)             |
| Section           | = | Header, [SmartText], [(Section)]      |
| Header            | = | RawText, Level        |
| SmartText         | = | RawText, [EmbeddedObjects] |
| EmbeddedObjects   | = | (EObject) |
| EObject           | = | Position, [Link], [List]    |
| Link              | = | Key, URL  |
| List              | = | (SmartText) |

Obviously, the above table is wrong. According to that one, EOBject can theoretically have **both** Link and a List in the same time. So, one more change:

|  Element          | Operator|  What can it contain? |
|:------------------|:-------:|:----------------------|
| Document          | = | (Section)              |
| Section           | = | Header, [SmartText], [(Section)]      |
| Header            | = | RawText, Level        |
| SmartText         | = | RawText, [EmbeddedObjects] |
| EmbeddedObjects   | = | (EObject) |
| EObject           | = | Position, EValue    |
| EValue            | = | Link \| List |
| Link              | = | Key, URL  |
| List              | = | (SmartText) |

Now we have only one EValue which is either a Link or a List.

#### 5. Malformed document

Sometimes a Document does not _have_ a Section. In that case it usually is malformed, but it should be able to be read not to lose any information (even if for debugging; real data tends to be very messy).

Sometimes a Document can be slightly malformed - it works; it just has some text before the first Section. And we don't want to lose that information, for example when we write the Document back. 

Therefore, updating the table (and _italicizing_ terminal elements to make it more visible):

|  Element          | Operator|  What can it contain? |
|:------------------|:-------:|:----------------------|
| Document          | = | ([Section]), [SmartText]              |
| Section           | = | Header, [SmartText], [(Section)]      |
| Header            | = | _RawText_, _Level_        |
| SmartText         | = | _RawText_, [EmbeddedObjects] |
| EmbeddedObjects   | = | (EObject) |
| EObject           | = | _Position_, EValue    |
| EValue            | = | Link \| List |
| Link              | = | _Key_, _URL_  |
| List              | = | (SmartText) |

#### 6. Unordered and ordered lists

Location masterlist is ordered and will eventually have to be parsed to extract links, like all masterlists do. Therefore the table needs to split: UnorderedList (UList) and Ordered List (OList):

|  Element          | Operator|  What can it contain? |
|:------------------|:-------:|:----------------------|
| Document          | = | ([Section]), [SmartText]              |
| Section           | = | Header, [SmartText], [(Section)]      |
| Header            | = | _RawText_, _Level_        |
| SmartText         | = | _RawText_, [EmbeddedObjects] |
| EmbeddedObjects   | = | (EObject) |
| EObject           | = | _Position_, EValue    |
| EValue            | = | Link \| UList \| OList |
| Link              | = | _Key_, _URL_  |
| UList             | = | (SmartText) |
| OList             | = | (SmartText) |


#### 7. Try to break the grammar

It seems the grammar works. It is easy to extend by adding a new EValue (for example, I expect to add Table too, one day). Time to generate some documents based on this grammar to try to break it - to see what it cannot deal with (as I cannot find any real data which breaks it).

##### 7.1 Empty Document

In Document, set ([Section]) and [SmartText] to empty.

Well, this works. Not much information in such a document, but it works.

Actually...

The Document should have an origin. Or, if generated, an identifier of any kind. Something which shows what it IS. It is either read from the file, extracted somewhere... I mean, it didn't appear on its own. So even an empty Document contains some information.

Updated table:

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

##### 7.2 Document with many sections and some SmartText

![null](/img/161215/r07_inverted_document.png)

This Document is well-formed. Sections can have Headers only and they actually have them. Because all the Headers have the descending level, all those Headers and SmartText belong to the Document.

##### 7.3 Nested List

I see a nested list (be it UList or OList) as a list where at the end of the text, after a newline (\n or \r\n) sign a new EObject is present - another list.

Look at the picture below to understand my reasoning:

![null](/img/161215/r08_nested list.png)

and the {0} over here is really another UList.

##### 7.4 Mixed List

This one breaks the grammar in terms of **meaning**.

![null](/img/161215/r09_broken_list.png)

I guess if I had a Table object - this would also break the grammar if I inserted it in a List.

So I have to either accept this imperfection in my grammar or create a subset of SmartText which can hold... currently only Links; and let List use that.

Due to very low severity and very low impact I am postponing this issue until it becomes a problem.

### Final grammar table

This should work for now, for the purpose of working with rdtool.

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

