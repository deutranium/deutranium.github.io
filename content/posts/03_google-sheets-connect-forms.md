---
title: "Connect Google Forms to Google Sheets"
date: 2024-12-09
description: "...and use them for setting up a human annotation \"platform\""
tags: ["programming", "misc"]
draft: False

---

## Story time
For a project, we need to get some data annotated by, well, humans. Using Google Sheets is the easiest and probably the simplest way, but Google Forms seems to be have _better vibes_ (?) when it comes ot human annotation? Out of the following options,
1. Every annotator has a column/sheet in a sheet/spreadsheet where they enter the label of corresponding data point
2. There is a form where you see one data point at a time, and press enter to navigate to the next data point

I would argue that option #2 is better as the data points seem _independent_ + the annotators would not be influenced by responses from other annotators. Of course there are other ways like making copies of the google sheet and sharing one per annotator, or using some platform specifically made for annotation stuff, and yada yada.. BUT, why can't we just use the good old google forms and connect it to the sheet which already has all the data? I was really hoping for having such a functionality as a plug-and-play thing in google sheets itself, but guess not :') There are a few add-ons that seeeeeeem to get the job done, but I wanted to try creating something on my own instead :P
Plus, I haven't worked with JS or Apps Script in a while, so this seemed to be a good excuse to brush up those skills as well!

Dear reader, after this huge disclaimer, I welcome you to this blog. Today, we shall study how to create a simple form using App Script in Google Sheets to connect it to Google Forms. I'll also track the time used in this mini project just for the kicks, and might add a few milestones + their corresponding time here.

**20:06 hrs** \
We begin :tada:

**20:08 hrs** \
I wish JS had list and dict comprehensions

**20:10 hrs** \
Why does JS not have an inbuilt shuffle function for lists?!

**20:12 hrs** \
How do I collapse the sidebar in App Script lol. Also, is it Apps Script or App Script

**20:13 hrs** \
It's Apps Script.

**20:17 hrs** \
I thinkkk we have our first version ready. Time to test!

**20:18 hrs** \
Umm, how do we run this? Do I have to `deploy`?

< smol break >

**20:21 hrs** \
Am back!

**20:25 hrs** \
Ahhh, everything needs to be in one function for it to run. lol.

**20:26 hrs** \
First bug, but it kinddaa works ig? still executing

**20:29 hrs** \
Oopsie, the structure is correct but the content is not


**20:31 hrs** \
Reduced the size of data from 280 to 6 rows coz didn't wanna wait so much for execution during testing. Which leads to another thought: Is this really as scalable as I hoped it would be? Because:
1. The execution itself seems to take a while
2. The entire form was lagging when I tried to view it in Google Forms


**20:33 hrs** \
LOOOOOOLLLLLL it's not reading my sheet XD coz i did `i[col_idx]` instead of `rows[i][col_idx]` :')


**20:35 hrs** \
Cool, content is rendering properly. Time to refine stuff and tada!

**20:39 hrs** \
Why is my `setHelpText()` not working!? even though it should according to [this](https://developers.google.com/apps-script/reference/forms/page-break-item#sethelptexttext)

**20:45 hrs** \
BRUH. this should not be _that_ difficult. Why is `.setHelpText()` working for `.addMultipleChoiceItem()` but not for `.addPageBreakItem()`?! Well, I'll add the other stuff for now, and deal with this after everything else.

**20:48 hrs** \
BRUUHHHH... I was using `.setHelptext()` with `.addPageBreakItem()` instead of `.setHelpText()` :')
Also, can we use markdown to format this text?

**20:54 hrs** \
:( why [this](https://issuetracker.google.com/issues/240105054) and [this](https://issuetracker.google.com/issues/240108930)

**20:57 hrs** \
JS being JS :') \
`ReferenceError: True is not defined`

**20:58 hrs** \
Damn, ELIO and Valley collaborated! [here](https://open.spotify.com/track/6oJA7O8LP862sS5ZlOjkF1?si=3307b0f766b04d37)

**20:59 hrs** \
Why is my `isRequired()` not working for `.addMultipleChoiceItem()` lol

**21:00 hrs** \
Aaaaahhhhh there's a `.setRequired()` and an `.isRequired()`.  Maybe renaming `.isRequired()` to `.getRequired()` will clarify many things

**21:08 hrs** \
I thinkkkk this should be the final version?

**21:09 hrs** \
HAHAHAH guess not XD Who knew `Name` is not the same as `"Name"`

**21:10 hrs** \
Ohh JS linebreaks don't work the same as markdown linebreaks

**21:12 hrs** \
Alles gut! just need to format text and tada!

**21:14 hrs** \
ngl i'm not a huge fan of string formatting in JS. Time to clean up the code and we're ready!

**21:20 hrs** \
Why did they [deprecate](https://developers.google.com/apps-script/reference/forms/form#setRequireLogin(Boolean)) `.setRequireLogin()` :(

**21:37 hrs** \
`Exceeded maximum execution time` well well well. I'll figure [this](https://stackoverflow.com/questions/72628571/google-apps-script-exceeded-maximum-execution-time) out in some time

**11:16 hrs** \
Good morning Zürich! We're back in action! \
P.S. reduced the number of calls by changing the structure a lil, but I'll probably need to find a better way as this doesn't seem scalable enough :')

**13:22 hrs** \
Hungry stomach leads to a starving brain. \
Full stomach leads to an eepy brain

**13:40 hrs** \
it's done :) \
Instead of having one data point per page with `2 (annotation checkbox + title) * 280 (number of data points) = 560` elements, I now show five data points per page, leading to `6 (title + 5 annotation checkboxes) * 56 (number of data points/5) = 336` elements. This kinda speeds things up and is good enough for the curent use case of 280 data points. Both the cases of course also have one intro page with 2 elements. \
Eventually, I would need a solution for 2,800 queries as I don't think I can scale this approach to 10x the number of queries. But, I'll deal with it after I finish other things that are higher up on my priority list. \
Is this the best solution ever? Nope \
Is this _a_ solution? Yup 

## For future reference

A few things I would love to try eventually would be:
1. Since timeout became one of the major issues, perhaps running google connectors locally might be a good idea? I have a vague memory of there being some APIs that let you interact with Google services, but I'll have to check the constraints etc. beforehand
2. Some sources suggested using batching, but I couldn't find any great resources specific to Google Forms (there were many for Google Sheets though), so I'll probably check it out as well. I doubt it'll be enough, but it can help speed things up nonetheless
3. I also remember seeing something where you could use a JSON to create a Google Form. Perhaps I can create a JSON programatically using the CSV and send it to another script that creates a Google Form
4. For some reason, whenever a form was created, it was directly published on the internet. I would've guessed that I need to call another function to _publish_ the form, but that's not really the case here. Maybe it's just the default settings in my account (I vagueellyyy remember seeing that) where the forms are configured to publish automatically, but I'll check it out. Thankfully there isn't any sensitive information here so it doesn't matter much, but _what if_ (the chances are very less, yes). Tbh I'm not sure if this is a big deal or not, but no harm in confirming. According to a few Google Support QnAs, Google will index your stuff only if it is published or embedded/linked to from any other website, so should _probably_ not be an issue here.\
Relevant sources: [here from 2009](https://www.networkworld.com/article/767157/opensource-subnet-your-google-docs-will-soon-appear-in-public-search-results.html) and [same thing but from 2014 (?)](https://www.cnet.com/tech/services-and-software/your-google-docs-soon-in-search-results/)

## Some lessons
1. I should've probably checked out the limits and constraints that exist in Apps Script (timeout) and Google Forms (the forms edit UI becomes laggy if there are too many elements)
2. I'm not really concerened about the time all this took (2.5-3ish hours (?)), but it was still good to know how much time I spend on seemmingly easy tasks.
3. There is always a scope for refinement (outside the main code logic), and it's fine if that refinement takes a while :')

## Codebase
Here we go: [Github Gist](https://gist.github.com/deutranium/90f63eb010e6835f75399eeb60d5758c)
