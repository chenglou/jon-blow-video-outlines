# Talk: Discussion: Iteration

Link: https://www.youtube.com/watch?v=O9xn9h_874U

Auto-transcribed with Whisper

[00:00:00.000 --> 00:00:12.320]   OK, so what I'm going to talk about today is an extension of what was discussed during
[00:00:12.320 --> 00:00:13.320]   the previous demo.
[00:00:13.320 --> 00:00:21.160]   In fact, I want to start with a slightly modified version of that demo, as you recall.
[00:00:21.160 --> 00:00:28.680]   I was showing iteration in cooperation with the built-in for loop as one of the things
[00:00:28.680 --> 00:00:32.220]   that happened last time, the main thing, sort of.
[00:00:32.220 --> 00:00:36.400]   And I'm just going to run that again, because why not, right?
[00:00:36.400 --> 00:00:42.320]   So you recall we had a bucket array thing and a hash table thing, and I was iterating
[00:00:42.320 --> 00:00:45.280]   over them and printing the results and all that.
[00:00:45.280 --> 00:00:49.800]   And in the discussion, someone said, "Hey, why don't you support remove in the iterator?"
[00:00:49.800 --> 00:00:54.480]   And I'd sort of thought about that at the beginning, but then hadn't really followed
[00:00:54.480 --> 00:00:55.480]   through on it.
[00:00:55.480 --> 00:01:01.100]   Yeah, I should make that happen, so the first thing to say is that I made that happen.
[00:01:01.100 --> 00:01:02.640]   So let's look at that.
[00:01:02.640 --> 00:01:10.240]   If you remember, I had these entities, right, here I'm filling out the data for an entity,
[00:01:10.240 --> 00:01:16.360]   and you know, it's just kind of some random values.
[00:01:16.360 --> 00:01:19.560]   I should have changed the string to make it more notifiable, but instead I change this
[00:01:19.560 --> 00:01:20.560]   index y.
[00:01:20.560 --> 00:01:28.080]   You know, it's 55 for one of the entities in 66 and 77, and then remember I did this manual
[00:01:28.080 --> 00:01:36.560]   iteration here, and then, you know, this is sort of using the built-in for loop.
[00:01:36.560 --> 00:01:39.760]   So here I've added another one where I just, you know, it's a very simple instance of
[00:01:39.760 --> 00:01:44.840]   remove, where I just say like, "Hey, look, if it's the one with 66 in index y, then remove
[00:01:44.840 --> 00:01:49.640]   that, right?"
[00:01:49.640 --> 00:01:55.720]   I don't know why I typed that, but you could just say, you don't have to say it, because
[00:01:55.720 --> 00:01:57.480]   it is a default name for a thing.
[00:01:57.480 --> 00:02:01.640]   But anyway, if you say remove it, then it'll call iterator remove, which we'll look at
[00:02:01.640 --> 00:02:06.240]   in a second, which is another procedure that you could provide or not provide if you don't
[00:02:06.240 --> 00:02:11.760]   provide it, then when people try to say remove, you'll just get an error that this iterator
[00:02:11.760 --> 00:02:13.440]   doesn't support remove.
[00:02:13.440 --> 00:02:19.440]   So up here, so the first time we print these out, we've got the 55 and the 66 and the 77,
[00:02:19.440 --> 00:02:24.840]   but then, then, so here I'm removing but not printing anything, and then here I print,
[00:02:24.840 --> 00:02:25.840]   right?
[00:02:25.840 --> 00:02:29.880]   This is the second time I print, the 66 is gone, because I removed it, and then it's
[00:02:29.880 --> 00:02:32.800]   gone throughout the rest of the demo.
[00:02:32.800 --> 00:02:40.640]   Okay, so that's straightforward, let's look at, now, the interesting thing was when I first
[00:02:40.640 --> 00:02:48.920]   sat down to do this, I realized that the model of iteration that I did last time was
[00:02:48.920 --> 00:02:53.840]   not good if you want to remove, because if you recall, I had an iterator make, which
[00:02:53.840 --> 00:03:02.080]   has stayed unchanged, right, or mostly unchanged, unchanged in principle, and we had an iterator
[00:03:02.080 --> 00:03:09.440]   next that also returned the value, like, returned the current value and gave you the
[00:03:09.440 --> 00:03:14.520]   next thing, and I did that so that you wouldn't have to implement very many things to create
[00:03:14.520 --> 00:03:15.520]   an iterator.
[00:03:15.520 --> 00:03:19.920]   The problem with that is that if you've moved on past the current item, and then you say
[00:03:19.920 --> 00:03:27.600]   remove, then you're removing the wrong thing, or one choice that I had was to define remove
[00:03:27.600 --> 00:03:33.280]   in the language in this really hacky way to say, well, if someone calls remove, then you
[00:03:33.280 --> 00:03:39.440]   have to back up one and remove that one, but that seemed very messy and undesirable, so
[00:03:39.440 --> 00:03:44.920]   instead I broke next, I broke next and get value into different things, so next doesn't
[00:03:44.920 --> 00:03:50.640]   return anything, right, this, there's no right arrow here, so this is the void return value,
[00:03:50.640 --> 00:03:58.200]   and here, iterator get value actually returns the value and a bool, and again it will return
[00:03:58.200 --> 00:04:00.520]   false if you're done, right.
[00:04:00.520 --> 00:04:05.040]   We could return the false up here, that would be a pretty straightforward refactoring of
[00:04:05.040 --> 00:04:13.600]   this, and then you're only supposed to call get value if.
[00:04:13.600 --> 00:04:23.520]   If you know that there's a value available, I don't know, okay, so, and then remove, you
[00:04:23.520 --> 00:04:28.280]   know, I happen to have a remove already called bucket array remove, which I think we might
[00:04:28.280 --> 00:04:34.080]   have looked at, but then iterator remove, whoops, iterator remove, is just going to
[00:04:34.080 --> 00:04:37.640]   be a wrapper around that, so bucket, you can look at bucket array remove for a second,
[00:04:37.640 --> 00:04:43.080]   it's not that important because this is just whatever implementation details behind your
[00:04:43.080 --> 00:04:46.760]   particular data structure, right.
[00:04:46.760 --> 00:04:53.000]   Now the thing is, sometimes there's a straw that breaks the camel's back, right, and,
[00:04:53.000 --> 00:04:57.520]   you know, back when I sort of had two procedures that were kind of big, I was like, okay, it's
[00:04:57.520 --> 00:05:02.320]   fine, iterators are still simple enough, but now I'm like, okay, if I'm going to make
[00:05:02.320 --> 00:05:08.160]   an iterator, and I need to implement all this stuff, it's like more than a screen of code,
[00:05:08.160 --> 00:05:13.520]   but it's starting to seem like a lot for something that ought to be really simple, right, and
[00:05:13.520 --> 00:05:20.920]   I'm a little bit unhappy with this at this point.
[00:05:20.920 --> 00:05:26.560]   Now the reason that I'm unhappy is because I designed four, you know, four loops in this
[00:05:26.560 --> 00:05:30.400]   language are meant to be used as simply as this, or as simply as this, their terse, their
[00:05:30.400 --> 00:05:38.880]   quick, you can do a lot with them, right, and I would like iterators to be terse and quick
[00:05:38.880 --> 00:05:45.120]   and let you do a lot with them, right, if they, if we keep iterators around, it's not
[00:05:45.120 --> 00:05:49.640]   a foregone conclusion because I look at this much code to do a simple thing, and I say,
[00:05:49.640 --> 00:05:52.360]   well, that's not necessarily very good.
[00:05:52.360 --> 00:05:55.420]   Now you might ask, well, why is that so much code?
[00:05:55.420 --> 00:05:59.440]   It doesn't necessarily seem like that much, but let's look at a different thing.
[00:05:59.440 --> 00:06:03.520]   Suppose I didn't care about having iterators, and I was going to do things in a LISP style
[00:06:03.520 --> 00:06:09.080]   way, where I just have an iterate function that takes my bucket array and then a visitor
[00:06:09.080 --> 00:06:14.800]   function that I call on every argument in the bucket array, right, that's like map or
[00:06:14.800 --> 00:06:20.120]   map car and LISP or whatever, right, if I want to iterate over every element in the bucket
[00:06:20.120 --> 00:06:25.840]   array, all I do is visit all the buckets, then within each bucket, I visit each by pointer,
[00:06:25.840 --> 00:06:30.680]   I take a pointer to every data element, and I make sure it's occupied, if it's not occupied,
[00:06:30.680 --> 00:06:34.480]   I continue, otherwise I visit the thing, right?
[00:06:34.480 --> 00:06:40.640]   So this, right here, if I were to type this by hand at any point in the program, it gives
[00:06:40.640 --> 00:06:45.480]   me all the functionality of all this giant page of code, apart from the remove, this
[00:06:45.480 --> 00:06:47.880]   doesn't handle remove right now.
[00:06:47.880 --> 00:06:56.840]   Actually, I guess it's sort of, well, we could talk about that, you know, this is just drastically
[00:06:56.840 --> 00:06:58.640]   less code than this, right?
[00:06:58.640 --> 00:07:03.560]   So what's happened here is this sort of ballooned in two steps.
[00:07:03.560 --> 00:07:07.280]   The first step was me saying, well, there's going to be a thing called iterators, and
[00:07:07.280 --> 00:07:11.400]   they have at least a data structure definition and then two procedures, right?
[00:07:11.400 --> 00:07:15.520]   And now that they have four procedures, because I broke this into two and then added one more
[00:07:15.520 --> 00:07:23.160]   optionally, it's just, there's this thing that happens to code universally, or you try to
[00:07:23.160 --> 00:07:28.200]   make it more abstract and applicable or something, and it just becomes a lot more complicated
[00:07:28.200 --> 00:07:29.840]   than it should be, right?
[00:07:29.840 --> 00:07:30.960]   This is all I want.
[00:07:30.960 --> 00:07:37.040]   This is all I want to do to iterate over this bucket array, but it's become this freaking
[00:07:37.040 --> 00:07:39.000]   giant, giant thing.
[00:07:39.000 --> 00:07:42.400]   Now it's not necessarily that bad, because you don't necessarily implement iterators
[00:07:42.400 --> 00:07:49.880]   that often, but even if I look at how much code this is, just to visit each thing, it's
[00:07:49.880 --> 00:07:54.000]   almost as much code as is actually required to implement this data structure, which is
[00:07:54.000 --> 00:07:55.000]   kind of ridiculous.
[00:07:55.000 --> 00:07:56.520]   Now, a little bit of it's optional.
[00:07:56.520 --> 00:08:02.720]   Like here I have a couple of asserts, you know, and these two asserts, basically, because
[00:08:02.720 --> 00:08:07.600]   when you call get value, it's not so tightly entwined with next, like I just want to make
[00:08:07.600 --> 00:08:12.460]   sure that things are happening in the right way and stuff, and so we assert, before we
[00:08:12.460 --> 00:08:18.520]   dereference this array by slot index, we just assert that it's in range, and that probably
[00:08:18.520 --> 00:08:21.360]   comes from more of a C or C++ mindset.
[00:08:21.360 --> 00:08:25.640]   Like if we assume that when we compile our debug build, we're going to have our automatic
[00:08:25.640 --> 00:08:31.000]   array checking turned on, and we demo that in a previous demo, if you don't recall.
[00:08:31.000 --> 00:08:33.080]   I probably don't really need those asserts.
[00:08:33.080 --> 00:08:37.920]   I probably do really want this one though, because this makes sure that if I'm about
[00:08:37.920 --> 00:08:42.880]   to return a value, it's really from an occupied bucket, which says that this should have definitely
[00:08:42.880 --> 00:08:46.080]   stopped on an occupied bucket, right?
[00:08:46.080 --> 00:08:50.360]   So you know, I saved a couple lines by deleting a couple of asserts, but it's still a freaking
[00:08:50.360 --> 00:08:52.640]   page of code to do the basics, right?
[00:08:52.640 --> 00:09:02.640]   Whereas this is very simple, so then we're sort of on the cusp of a paradigm difference
[00:09:02.640 --> 00:09:09.440]   that several languages have approached in different ways, have approached in one way
[00:09:09.440 --> 00:09:17.640]   or the other way, which is just to say that, okay, imagine, okay, what's the problem with
[00:09:17.640 --> 00:09:18.640]   this?
[00:09:18.640 --> 00:09:24.900]   Not from the fact that this is hard to integrate into like a regular for loops syntactically.
[00:09:24.900 --> 00:09:31.800]   You know, calling a function every time I want to iterate over this thing is maybe expensive.
[00:09:31.800 --> 00:09:37.800]   Of course you could force inline it, because this language has force in lines.
[00:09:37.800 --> 00:09:46.960]   But it's still maybe, well, I don't know.
[00:09:46.960 --> 00:09:49.080]   You know, I'm not sure.
[00:09:49.080 --> 00:09:52.880]   I'm not sure at some point how bad that is, right?
[00:09:52.880 --> 00:09:58.720]   Like right now, like encapsulating things into a procedure to pass it is not very convenient.
[00:09:58.720 --> 00:10:04.720]   But if you could just pass like a block of code, and it was automatically a lambda in
[00:10:04.720 --> 00:10:11.760]   the local scope or something like it, it could start to get convenient, right?
[00:10:11.760 --> 00:10:20.840]   Yeah, okay, the thing about this very small and tight version of iteration is that you
[00:10:20.840 --> 00:10:23.240]   don't get to reason about anything, right?
[00:10:23.240 --> 00:10:32.720]   So part of the reason that part of the purpose for which people make an iterator abstraction
[00:10:32.720 --> 00:10:38.840]   is the idea that you can do other stuff with it besides just iterate in a for loop, right?
[00:10:38.840 --> 00:10:44.600]   And I love two minds about that, and that's one of the things I want to talk about.
[00:10:44.600 --> 00:10:49.400]   The main thing I want to do is make for loops really convenient.
[00:10:49.400 --> 00:10:53.520]   That in my day-to-day programming, that's where I get most of my mileage is just iterating
[00:10:53.520 --> 00:10:57.080]   over stuff over and over and doing various things.
[00:10:57.080 --> 00:11:01.720]   However, you know, there are people in the C++ community and in communities of other
[00:11:01.720 --> 00:11:07.680]   languages like D and whatever, most modern languages have some form of iterator abstraction
[00:11:07.680 --> 00:11:10.500]   or range abstraction is now preferred indeed.
[00:11:10.500 --> 00:11:15.080]   It's sort of the same thing in a more convenient form.
[00:11:15.080 --> 00:11:19.240]   And the reason why they do this is so that you can do stuff like this.
[00:11:19.240 --> 00:11:26.960]   So this is sort of the documentation for if you include algorithm in C++, it's so weird
[00:11:26.960 --> 00:11:31.520]   to call something algorithm as if all other code was not algorithms, but, you know, there's
[00:11:31.520 --> 00:11:35.760]   this all this stuff in the standard library in C++, and the idea is you're supposed to
[00:11:35.760 --> 00:11:42.320]   be able to make an algorithm out of abstract operations where you don't care what the data
[00:11:42.320 --> 00:11:43.960]   structure is, right?
[00:11:43.960 --> 00:11:49.720]   So like, you could find the end of a sequence in a range and ask if it's a permutation of
[00:11:49.720 --> 00:11:50.720]   something.
[00:11:50.720 --> 00:11:58.200]   And there's just all these things that if you know how to use them, you can sort of
[00:11:58.200 --> 00:12:00.920]   use them to build an algorithm sort of.
[00:12:00.920 --> 00:12:08.720]   Well, on the one hand, this is not very data-oriented way to think about things.
[00:12:08.720 --> 00:12:12.840]   Because the thesis behind the kind of programming that I do is that you really actually care
[00:12:12.840 --> 00:12:17.160]   very deeply about the representation that you're manipulating, and these are all designed
[00:12:17.160 --> 00:12:23.800]   to not care about the representation, however, sometimes you totally don't care about performance.
[00:12:23.800 --> 00:12:26.840]   Sometimes you just want to try something and make it work, right?
[00:12:26.840 --> 00:12:33.000]   And in those cases, it might be convenient to have the leverage of this kind of high-level
[00:12:33.000 --> 00:12:34.560]   expression, right?
[00:12:34.560 --> 00:12:38.760]   Certainly, there are a lot of people in the world of programming languages that think
[00:12:38.760 --> 00:12:42.520]   that this is sort of the manifest destiny of the direction where programming languages
[00:12:42.520 --> 00:12:43.520]   are supposed to go.
[00:12:43.520 --> 00:12:47.600]   They're supposed to get more and more abstract, and you're supposed to be able to say more
[00:12:47.600 --> 00:12:54.000]   and more things about what to do without caring what the data representation is, or
[00:12:54.000 --> 00:12:55.840]   even the specific code representation.
[00:12:55.840 --> 00:12:58.000]   Now, I have never programmed this way, right?
[00:12:58.000 --> 00:13:06.680]   I've been programming professionally for 20-plus years, 22-23 years, and programming
[00:13:06.680 --> 00:13:11.360]   including amateur time since I was 10 years old, which would be 34 years, I've been programming
[00:13:11.360 --> 00:13:16.600]   in C++ for 20 years, maybe a little more than that.
[00:13:16.600 --> 00:13:19.200]   I have never once included algorithm, right?
[00:13:19.200 --> 00:13:21.080]   And I just don't program this way.
[00:13:21.080 --> 00:13:28.280]   Part of that is 'cause templates stink in C++, and they're just so terrible to work with.
[00:13:28.280 --> 00:13:31.720]   But part of it is just that this kind of programming is kind of ugly to me.
[00:13:31.720 --> 00:13:36.460]   Like, if I try to make an algorithm out of these things, I kind of feel like I'm playing
[00:13:36.460 --> 00:13:43.840]   TIS 100 or something, and I'm trying to, you know, it becomes more like building a circuit
[00:13:43.840 --> 00:13:44.840]   out of boxes.
[00:13:44.840 --> 00:13:49.440]   Like, "Oh, I'm going to take the result of find-end, and I'm going to pipe that into
[00:13:49.440 --> 00:13:53.160]   this other thing, and I'm going to hook that up to this other thing," and I mean, I guess
[00:13:53.160 --> 00:13:56.640]   that's how some kinds of programming are most of the time now.
[00:13:56.640 --> 00:14:01.280]   Like, I think that's what web programmers do all day and stuff, but I don't like it.
[00:14:01.280 --> 00:14:04.920]   It's not really the kind of programming that I enjoy or that I'm interested in.
[00:14:04.920 --> 00:14:10.720]   So I'm skeptical about the use of this kind of a thing, right?
[00:14:10.720 --> 00:14:12.360]   This kind of super abstract thing.
[00:14:12.360 --> 00:14:16.720]   Now, D, like I've said, has its own version of this that is somewhat cleaned up.
[00:14:16.720 --> 00:14:23.560]   I like the D version better, but I still, this is not my vision of, you know, they just,
[00:14:23.560 --> 00:14:30.160]   they have a similar, their abstraction that these algorithm pieces use is more convenient,
[00:14:30.160 --> 00:14:36.280]   in my opinion anyway, but it's still a similar just set of weird pieces that you're supposed
[00:14:36.280 --> 00:14:41.240]   to try to put together and then put blue code in between to build algorithms, and I don't
[00:14:41.240 --> 00:14:44.640]   ever want to do this, almost.
[00:14:44.640 --> 00:14:50.600]   So my first conclusion is, okay, if I'm trying to make iterators better or iteration better,
[00:14:50.600 --> 00:14:58.280]   maybe I just only care about four loops and I give you a four loop and a way to call it
[00:14:58.280 --> 00:14:59.360]   manually, right?
[00:14:59.360 --> 00:15:04.000]   So this iterate function, you could call it manually or a four loop could know how to
[00:15:04.000 --> 00:15:05.000]   decompose it.
[00:15:05.000 --> 00:15:08.320]   We'll talk about that more in a second, right?
[00:15:08.320 --> 00:15:12.600]   And also you actually could maybe reason about this because we have metaprogramming that's
[00:15:12.600 --> 00:15:14.360]   able to look at the syntax of your thing.
[00:15:14.360 --> 00:15:20.920]   So if you wanted to reason about this, that's maybe something that you could do in a metaprogram.
[00:15:20.920 --> 00:15:25.240]   But it just wouldn't really be the same as this kind of abstract iterator stuff.
[00:15:25.240 --> 00:15:31.920]   And so I'm not sure at first blush that I care that this is part of the reason about
[00:15:31.920 --> 00:15:37.600]   than this, because in terms of human ergonomics, I mean, harder to automatically reason about
[00:15:37.600 --> 00:15:38.600]   it.
[00:15:38.600 --> 00:15:40.640]   It's way easier for a programmer to understand this.
[00:15:40.640 --> 00:15:41.640]   What's this?
[00:15:41.640 --> 00:15:44.240]   Why does he actually pop a iterative page?
[00:15:44.240 --> 00:15:48.000]   This is way easier for a programmer to understand than all this, right?
[00:15:48.000 --> 00:15:52.040]   And then multiply that by the number of thing data structures that you would iterate over
[00:15:52.040 --> 00:15:53.040]   in a program.
[00:15:53.040 --> 00:15:54.960]   It's quite pronounced.
[00:15:54.960 --> 00:15:59.320]   So I'm interested in doing some highly compact version like this.
[00:15:59.320 --> 00:16:01.560]   And there exists another language is highly compact versions.
[00:16:01.560 --> 00:16:06.080]   But before I do that, I want to go off on a side thing and say, even though I just said
[00:16:06.080 --> 00:16:12.480]   I never want to do anything like this abstract algorithm stuff, sometimes I do want to do
[00:16:12.480 --> 00:16:18.320]   things that are a little bit like it, a little bit.
[00:16:18.320 --> 00:16:22.960]   For example, and I want to maybe preserve the ability to do these things, all the time
[00:16:22.960 --> 00:16:27.480]   while building this compiler and while building various games and stuff, I'll end up in a
[00:16:27.480 --> 00:16:32.000]   situation where I don't just have one array that I want to iterate over, I'll have like
[00:16:32.000 --> 00:16:36.240]   two arrays, right, two parallel arrays or three arrays that sort of hold different
[00:16:36.240 --> 00:16:37.240]   things that are related.
[00:16:37.240 --> 00:16:40.560]   And I just want to do the same operation to all of them, but I don't want to factor
[00:16:40.560 --> 00:16:48.400]   that code out into a function because that's slower and it loosens the ties between the
[00:16:48.400 --> 00:16:49.400]   various code.
[00:16:49.400 --> 00:16:52.840]   It makes it unclear, more unclear what's happening.
[00:16:52.840 --> 00:16:56.640]   The code is taken out of its context, right?
[00:16:56.640 --> 00:17:01.200]   So I thought for a while about supporting multiple arrays directly in the syntax of
[00:17:01.200 --> 00:17:07.720]   a four loop like this, but after last time's demo, I realized, oh, you can already solve
[00:17:07.720 --> 00:17:10.800]   that by making a very simple iteration.
[00:17:10.800 --> 00:17:16.960]   So I added a little bit to this demo, but I made a few arrays of strings.
[00:17:16.960 --> 00:17:22.240]   So array one is just a string array literal that says I said hello sailor and array two
[00:17:22.240 --> 00:17:26.760]   is a dynamic array of strings, I'm going to add it to you and A3 is going to have some
[00:17:26.760 --> 00:17:27.760]   punctuations.
[00:17:27.760 --> 00:17:33.400]   And then I'm going to make this thing called combined that's just A3A1, A2A3, and all that
[00:17:33.400 --> 00:17:37.520]   is, is just the thing that's meant to be iterated over that doesn't even copy the arrays.
[00:17:37.520 --> 00:17:39.160]   It doesn't concatenate them, right?
[00:17:39.160 --> 00:17:43.800]   If this was LISPR something, you might concatenate all the arrays and then iterate over the combined
[00:17:43.800 --> 00:17:49.040]   array, but that requires memory allocation and stuff and it's more expensive.
[00:17:49.040 --> 00:17:51.160]   Why don't you just go over each element of each array in turn?
[00:17:51.160 --> 00:17:56.080]   So, you know, given this scheme on the right, you can already very easily implement something
[00:17:56.080 --> 00:17:57.080]   like this.
[00:17:57.080 --> 00:17:59.480]   I broke this into two lines.
[00:17:59.480 --> 00:18:05.440]   You should be able to do this for combined print, whatever, but that actually doesn't
[00:18:05.440 --> 00:18:10.560]   work because for some reason right now the compiler doesn't think it can make an L value
[00:18:10.560 --> 00:18:17.280]   out of a temporary result, which was something I thought I'd fixed a while back, but I can't.
[00:18:17.280 --> 00:18:21.760]   So I put it in a variable here for now, but anyway, so I make this combined and then
[00:18:21.760 --> 00:18:29.480]   I iterate over it and you can see here, you know, I print everything in A3 followed by
[00:18:29.480 --> 00:18:32.800]   A1, A2, and then A3 again, right here.
[00:18:32.800 --> 00:18:38.560]   So A3, A1, A2, A3 again, right.
[00:18:38.560 --> 00:18:44.120]   So that's a convenient thing and I don't think I would use that when I have to be absolutely
[00:18:44.120 --> 00:18:45.880]   fastness, but it's fast enough.
[00:18:45.880 --> 00:18:48.920]   I'll show you what it does in a second, but it's fast enough that you can just use it
[00:18:48.920 --> 00:18:51.480]   without caring in most gameplay code.
[00:18:51.480 --> 00:18:53.240]   And that's a little bit abstract, right.
[00:18:53.240 --> 00:18:58.320]   You're saying I'm making a combined array that's sort of the whatever you get when you
[00:18:58.320 --> 00:19:01.040]   put all these things together without actually putting them together.
[00:19:01.040 --> 00:19:06.320]   It's like one step in the direction of this standard algorithm kind of approach.
[00:19:06.320 --> 00:19:11.240]   So I maybe want to preserve the ability to go one or two steps, but I don't want to,
[00:19:11.240 --> 00:19:15.720]   this is just nutballs, like I don't ever want to go all the way this way unless.
[00:19:15.720 --> 00:19:23.600]   So if you are a game engine serious person and you find this kind of approach very useful,
[00:19:23.600 --> 00:19:28.680]   email me, I'll put up the address at the end of the thing and you can tell me in what way
[00:19:28.680 --> 00:19:29.680]   it's useful.
[00:19:29.680 --> 00:19:33.920]   I know that there's all sorts of random programmers out there that think this is useful, but most
[00:19:33.920 --> 00:19:38.600]   of those programmers do not engage in a style of programming that I'm that interested in
[00:19:38.600 --> 00:19:44.240]   and they don't produce results that I'm not interested in, right.
[00:19:44.240 --> 00:19:52.320]   But I'm interested in if anyone who's a serious person in my domain actually uses this stuff
[00:19:52.320 --> 00:19:59.080]   for real, like to solve real problems, you know, maybe, maybe, I don't know, it's almost,
[00:19:59.080 --> 00:20:05.160]   it's very hard for me to visualize a situation when I really want to use that stuff.
[00:20:05.160 --> 00:20:10.400]   Anyway, going back, let's look for a second at what this combined array does.
[00:20:10.400 --> 00:20:15.440]   It's very small, right, here's the data structure.
[00:20:15.440 --> 00:20:22.480]   I made it a dynamic array right now of elements and the problem is just, in theory I could
[00:20:22.480 --> 00:20:31.000]   make it a static array like n elements wide and I could pass n here, right.
[00:20:31.000 --> 00:20:35.560]   And then it wouldn't do any dynamic memory allocation, but my proof of concept version
[00:20:35.560 --> 00:20:36.920]   does dynamic memory allocation.
[00:20:36.920 --> 00:20:42.960]   And the reason I don't do it is because the procedure that generates this type called
[00:20:42.960 --> 00:20:47.840]   combined here, you know, it takes a variable number of arguments, so let's look down what
[00:20:47.840 --> 00:20:48.840]   I call it.
[00:20:48.840 --> 00:20:53.760]   I just call it with a 381883, I could have put fewer or more arguments if I wanted to,
[00:20:53.760 --> 00:20:54.760]   right.
[00:20:54.760 --> 00:20:59.720]   The thing about that is that the compiler right now doesn't know that when you have a
[00:20:59.720 --> 00:21:07.920]   polymorphic procedure with VeraRx, that the number of arguments is essentially a polymorphic
[00:21:07.920 --> 00:21:08.920]   variable.
[00:21:08.920 --> 00:21:13.960]   It's like the way that the declarations works syntactically right now, there's not really
[00:21:13.960 --> 00:21:21.060]   a way to do that.
[00:21:21.060 --> 00:21:23.720]   So I need to figure out how to do that, right.
[00:21:23.720 --> 00:21:29.320]   So there's basically no way for me to get n out of this argument.
[00:21:29.320 --> 00:21:37.080]   I mean I can say arrays.count, which is how you know how many items are in an array, but
[00:21:37.080 --> 00:21:43.880]   the compiler doesn't know that that's like constant for this instantiation of this procedure.
[00:21:43.880 --> 00:21:47.800]   And actually it's not really, you're not guaranteed for it to be if it's only matched
[00:21:47.800 --> 00:21:48.800]   on t.
[00:21:48.800 --> 00:21:50.760]   You would have to say that it's matched on n somehow.
[00:21:50.760 --> 00:21:53.920]   That's the thing that's doable within the language though, right.
[00:21:53.920 --> 00:22:02.720]   So this can be very fast and have no dynamic memory allocation, aside from making space
[00:22:02.720 --> 00:22:07.960]   for this on the stack down here, which is very fast, right.
[00:22:07.960 --> 00:22:15.800]   Anyway, so all I do is I make a result of this type for, I just copy this argument array
[00:22:15.800 --> 00:22:19.120]   into the result arrays and then I return that, right.
[00:22:19.120 --> 00:22:25.080]   So that's very fast.
[00:22:25.080 --> 00:22:30.920]   And you know, the thing is that's a very simple data structure, but now we come down
[00:22:30.920 --> 00:22:35.080]   and look at our iterator and it's almost as big as the iterator for bucket array, right.
[00:22:35.080 --> 00:22:41.120]   Because it's essentially the same, it's iterating over a collection of arrays, which is what
[00:22:41.120 --> 00:22:45.060]   bucket arrays as well, a collection of arrays, and there's just some amount of complexity
[00:22:45.060 --> 00:22:50.460]   involved in that, that again, when to go back to bucket array for a second, when you're
[00:22:50.460 --> 00:22:56.740]   living inside the loop and you don't need to provide access to the outside, it's like
[00:22:56.740 --> 00:22:58.140]   very terse.
[00:22:58.140 --> 00:23:03.060]   But if you need to like open it up like origami, it becomes bigger and weirder, right.
[00:23:03.060 --> 00:23:07.340]   Maybe if somebody at home comes up with a really much more terse way of doing all this,
[00:23:07.340 --> 00:23:11.820]   let me know, but I kind of doubt it'll ever get anywhere near as short as this.
[00:23:11.820 --> 00:23:18.900]   And to be clear, I didn't do it, but I could write an iterate function that's just as short.
[00:23:18.900 --> 00:23:22.380]   That works on combined array, right.
[00:23:22.380 --> 00:23:27.100]   So the question then is what directions, I don't think I'm going to work on this immediately
[00:23:27.100 --> 00:23:32.700]   because like I said, I'm working on practical programs right now and I don't really, like
[00:23:32.700 --> 00:23:37.580]   I can live with big iterator code like this for a little bit, but I don't like to settle,
[00:23:37.580 --> 00:23:38.580]   right.
[00:23:38.580 --> 00:23:41.100]   So I want to keep my eye on what's the best thing to do.
[00:23:41.100 --> 00:23:45.980]   So now some languages, you know, if you're like a Haskell person or something, you know
[00:23:45.980 --> 00:23:51.660]   about generator functions or some languages use co-routines where you can do a loop that's
[00:23:51.660 --> 00:23:57.460]   just like this except, you know, imagine that I had a yield keyword, right.
[00:23:57.460 --> 00:24:06.460]   And I could call iterate and this would sort of run in its own fiber and then yield control
[00:24:06.460 --> 00:24:14.140]   back to my sort of main thread or main fiber passing this result, right.
[00:24:14.140 --> 00:24:20.300]   And then a for loop could actually take, you'd probably want more syntax like, let's say
[00:24:20.300 --> 00:24:25.940]   this is a co-routine or something just so that we know that yield is legal or I don't
[00:24:25.940 --> 00:24:27.500]   know, right.
[00:24:27.500 --> 00:24:31.900]   If we decided to support co-routines, that would be a way to do it, right.
[00:24:31.900 --> 00:24:37.100]   And then you would have this thing called iterate and a for loop would understand what it is
[00:24:37.100 --> 00:24:41.340]   and you could also call iterate in isolation and every time you call it, it would yield
[00:24:41.340 --> 00:24:42.340]   one result.
[00:24:42.340 --> 00:24:47.900]   And then when it's done, you would like yield no or something, I don't know, right.
[00:24:47.900 --> 00:24:51.100]   And that's how you know when it's done.
[00:24:51.100 --> 00:24:58.020]   Or maybe you have a second argument, right, I don't know, whatever.
[00:24:58.020 --> 00:25:04.820]   This is not, I haven't declared a return value here, so this is a sketch, this is a sketch.
[00:25:04.820 --> 00:25:08.460]   So this is approximately the same amount of code, but it's interesting because you can
[00:25:08.460 --> 00:25:09.460]   do cool stuff.
[00:25:09.460 --> 00:25:16.220]   Now the thing is, if you actually use a co-routine and create a fiber or you have a fiber pool
[00:25:16.220 --> 00:25:22.060]   and you grab one every time, you do a for loop or something, it's almost impossible to avoid
[00:25:22.060 --> 00:25:26.960]   performance ramifications there, right.
[00:25:26.960 --> 00:25:36.100]   It's not that cool of a way to do things if you really care about zero overhead.
[00:25:36.100 --> 00:25:47.080]   Oh, here I was doing a different thing and it ended up being the same.
[00:25:47.080 --> 00:25:52.300]   So and then the other thing about, you know, like programming language enthusiasts will
[00:25:52.300 --> 00:25:56.000]   tell you how great generator functions are from something like Haskell or whatever, but
[00:25:56.000 --> 00:25:58.720]   my opinion about them is similarly met.
[00:25:58.720 --> 00:26:02.780]   I feel the same way about those as I feel about this algorithm and standard algorithm
[00:26:02.780 --> 00:26:03.780]   stuff.
[00:26:03.780 --> 00:26:08.860]   Like, yeah, you can put together examples of how you can use it to do things, but I don't
[00:26:08.860 --> 00:26:12.940]   actually want to use it to do real things in the real world usually.
[00:26:12.940 --> 00:26:14.580]   There might be similar use cases actually.
[00:26:14.580 --> 00:26:18.560]   I think generator functions are probably more useful than this stuff, but they're not things
[00:26:18.560 --> 00:26:21.540]   that I actually really want to use.
[00:26:21.540 --> 00:26:30.000]   So here's the thing that I'm thinking may happen and I'm interested to entertain opinions
[00:26:30.000 --> 00:26:31.440]   on this, right.
[00:26:31.440 --> 00:26:38.240]   What may happen is to have a routine like this that's got a visitor or something.
[00:26:38.240 --> 00:26:45.800]   And instead of defining all this iterator stuff, you just define this one routine, but
[00:26:45.800 --> 00:26:50.080]   the for loop actually knows how to dissect it, right.
[00:26:50.080 --> 00:27:05.320]   So if you say for a bucket array dot that, I don't know, do something if x break, right.
[00:27:05.320 --> 00:27:06.880]   I don't know.
[00:27:06.880 --> 00:27:12.180]   That's a very simple thing to do, right, that if you were to do this, then the compiler
[00:27:12.180 --> 00:27:27.580]   would essentially take this code and essentially paste it and then it would know that these
[00:27:27.580 --> 00:27:32.940]   two arguments, this one is the thing you're iterating over and this argument is just a
[00:27:32.940 --> 00:27:38.220]   placeholder for where it should insert the code.
[00:27:38.220 --> 00:27:43.940]   This is actually not that different from how it works right now anyway, actually, but you
[00:27:43.940 --> 00:27:48.140]   know, maybe you would want some more syntactic markup to make this clearer or less error
[00:27:48.140 --> 00:27:49.140]   problem.
[00:27:49.140 --> 00:27:52.900]   I don't know, but it would just know that wherever you see visit, we don't really mean
[00:27:52.900 --> 00:27:53.900]   call it function.
[00:27:53.900 --> 00:27:54.900]   We mean insert this code.
[00:27:54.900 --> 00:28:04.820]   So it would just go bam, bam, delete the visit and then paste your stuff in here, right.
[00:28:04.820 --> 00:28:11.620]   That would be given how much this, that would be sort of polymorphing this into the form
[00:28:11.620 --> 00:28:17.020]   needed for the particular for loop, right.
[00:28:17.020 --> 00:28:20.720]   That would be a very appropriate to this programming language way of doing things and
[00:28:20.720 --> 00:28:22.100]   it's kind of appealing to me.
[00:28:22.100 --> 00:28:26.460]   It's, I think it's going to be an interesting thing to try.
[00:28:26.460 --> 00:28:33.700]   However, you'll notice that what I just did is intentionally a little bit broken, right.
[00:28:33.700 --> 00:28:39.140]   So if I just paste a breaking here, I'm breaking from the wrong loop, I'm breaking from this
[00:28:39.140 --> 00:28:51.420]   loop, if you take break, you know, literally, right.
[00:28:51.420 --> 00:28:57.500]   So I said break here without a label, but really I need the label of the outer thing.
[00:28:57.500 --> 00:29:02.140]   So I need to map this label to this, right, so I need to say break bucket.
[00:29:02.140 --> 00:29:07.180]   We already have main breaks that'll break you out of an outer thing.
[00:29:07.180 --> 00:29:10.420]   But you know, there's probably a few things like that.
[00:29:10.420 --> 00:29:13.460]   What happens if I do remove?
[00:29:13.460 --> 00:29:21.460]   So what, you know, what if I say if why remove it here, right.
[00:29:21.460 --> 00:29:24.020]   Do I give people a way to define remove?
[00:29:24.020 --> 00:29:25.020]   Maybe I do.
[00:29:25.020 --> 00:29:40.060]   Say in the scope of this function, you could define remove is maybe it's just similar, but
[00:29:40.060 --> 00:29:44.620]   it gets an argument, right.
[00:29:44.620 --> 00:29:47.140]   I'm not going to, I'm not going to do the right type right now.
[00:29:47.140 --> 00:29:52.180]   It's probably, it's probably t dot type or element, let's say element type, let's pretend
[00:29:52.180 --> 00:29:54.620]   it's element type for clarity, right.
[00:29:54.620 --> 00:29:59.580]   So you could put something in local scope, again, that's, I think this was just a call
[00:29:59.580 --> 00:30:06.180]   to bucket array remove, right.
[00:30:06.180 --> 00:30:10.580]   And again, this could be a syntactic placeholder where the arguments just assure the compiler
[00:30:10.580 --> 00:30:16.660]   that it knows what it's talking about.
[00:30:16.660 --> 00:30:22.140]   And then if you said remove it, it would complain if there wasn't a remove in this scope.
[00:30:22.140 --> 00:30:23.140]   So I kind of like this.
[00:30:23.140 --> 00:30:32.300]   It's super terse and I don't think it prevents anything, right.
[00:30:32.300 --> 00:30:39.020]   So this example that I talked about wanting to be able to do down here, you could still
[00:30:39.020 --> 00:30:40.020]   do all that.
[00:30:40.020 --> 00:30:43.260]   And in fact, it would be simpler than what I just did because you would still make this
[00:30:43.260 --> 00:30:44.260]   combined array.
[00:30:44.260 --> 00:30:47.780]   You would still make a function that generates a combined array, but you would have an iterate
[00:30:47.780 --> 00:30:54.380]   function that's about as short as the one we were just looking at that, you know, just
[00:30:54.380 --> 00:30:56.380]   does the work of all this stuff.
[00:30:56.380 --> 00:31:05.180]   And then you would just not implement remove.
[00:31:05.180 --> 00:31:09.060]   So I guess what I'm getting at is, you know, so far a lot of the features of this language
[00:31:09.060 --> 00:31:13.700]   that are interesting have to do with the way it does polymorphism, different from other
[00:31:13.700 --> 00:31:14.700]   languages.
[00:31:14.700 --> 00:31:18.260]   Okay, remember how we have bakes where we can grab a function that's just declared as a
[00:31:18.260 --> 00:31:25.140]   regular old function and we can redefine arguments of it, right, to be of, you know,
[00:31:25.140 --> 00:31:29.220]   what if this argument is known to be this particular constant value, please recompile
[00:31:29.220 --> 00:31:30.220]   this function, right.
[00:31:30.220 --> 00:31:34.140]   So this is sort of like doing that, but it's saying we're going to grab this and we're
[00:31:34.140 --> 00:31:37.220]   just going to paste it into the loop.
[00:31:37.220 --> 00:31:40.980]   But in the process of pacing it into the loop, we define these to be known things.
[00:31:40.980 --> 00:31:47.860]   So it's sort of like an in line combined with a bake somehow.
[00:31:47.860 --> 00:31:50.420]   So that's just interesting and that's where I'm at.
[00:31:50.420 --> 00:31:56.700]   So I don't know, there are many programming languages out there in the world and they
[00:31:56.700 --> 00:32:02.100]   all have different approaches and quite possibly some language is done exactly what I'm talking
[00:32:02.100 --> 00:32:03.100]   about.
[00:32:03.100 --> 00:32:08.340]   Very likely there are a number of languages that have their own flavor of either generator
[00:32:08.340 --> 00:32:12.540]   functions or co routines that yield values.
[00:32:12.540 --> 00:32:17.580]   So I'm just interested in thinking about that in the future and I wanted to talk about that
[00:32:17.580 --> 00:32:21.900]   while it's fresh in my mind because like I said, I'm about to go back and do some more
[00:32:21.900 --> 00:32:24.780]   everyday grunt work programming stuff.
[00:32:24.780 --> 00:32:31.740]   So I just wanted to like get a little closure on this.
[00:32:31.740 --> 00:32:38.660]   Let's say that I'm -- and to reiterate, the conclusion is sort of I'm quite possibly much
[00:32:38.660 --> 00:32:45.380]   more interested in having this be the way things work than this as long as this version
[00:32:45.380 --> 00:32:53.620]   doesn't turn out to be too weird and hacky.
[00:32:53.620 --> 00:33:00.860]   However, I mean there's some things that are not quite figured out like to do this remapping
[00:33:00.860 --> 00:33:04.140]   of the break or whatever, how do you know that that's the right label?
[00:33:04.140 --> 00:33:07.380]   Do you always remap to the outermost loop?
[00:33:07.380 --> 00:33:08.380]   That doesn't seem good.
[00:33:08.380 --> 00:33:13.820]   Like what if my particular iteration scheme has multiple loops with multiple visits like
[00:33:13.820 --> 00:33:16.140]   how do we make sure that that all works?
[00:33:16.140 --> 00:33:22.740]   I think I'm pretty sure that can be made to work, but we want to see, right?
[00:33:22.740 --> 00:33:26.820]   So that's where I'm at.
[00:33:26.820 --> 00:33:34.380]   Yes, you have strong opinions about this and are not a random enthusiast of random languages,
[00:33:34.380 --> 00:33:41.860]   but actually serious game programmer who's shipped things that are non-trivial, you can
[00:33:41.860 --> 00:33:43.340]   let me know at this address.
[00:33:43.340 --> 00:33:44.420]   This is not a mailing list.
[00:33:44.420 --> 00:33:47.660]   This is just where I get comments about the language.
[00:33:47.660 --> 00:33:48.660]   I read everything.
[00:33:48.660 --> 00:33:50.860]   I don't necessarily reply to everything.
[00:33:50.860 --> 00:34:01.460]   And so anything I saw a couple questions for all by while I was talking, but I missed them,
[00:34:01.460 --> 00:34:06.660]   so except for the one asking about inheritance, which I'm sure people in the chat answered.
[00:34:06.660 --> 00:34:16.580]   So if you have questions, we could do it now.
[00:34:16.580 --> 00:34:18.420]   Questions?
[00:34:18.420 --> 00:34:22.260]   One says for your current iteration scheme, you could add a keep current item variable,
[00:34:22.260 --> 00:34:30.140]   which the remove keyword will set to false, then pass that to iterator get next.
[00:34:30.140 --> 00:34:33.260]   Yeah, I mean, maybe.
[00:34:33.260 --> 00:34:43.580]   The thing about that is that you can't -- the problem with my old loop was that I said next
[00:34:43.580 --> 00:34:48.980]   immediately, right?
[00:34:48.980 --> 00:34:54.820]   But I guess you could preserve the keep current item through the head of the loop.
[00:34:54.820 --> 00:34:56.340]   It's a possibility, right?
[00:34:56.340 --> 00:35:02.940]   And by doing a manipulation like that, I could maybe get back to approximately the amount
[00:35:02.940 --> 00:35:10.620]   of code we had last time, right?
[00:35:10.620 --> 00:35:13.520]   It would still be a little more complicated than last time, even, because now you have
[00:35:13.520 --> 00:35:18.020]   this extra boolean to deal with as a user.
[00:35:18.020 --> 00:35:21.660]   And if you're not ignoring it -- and if you are ignoring it, you still want to make an
[00:35:21.660 --> 00:35:25.780]   error and tell people that your iterator doesn't support remove, right?
[00:35:25.780 --> 00:35:31.220]   So it's always going to be a little less -- or a little more code than what I demoed
[00:35:31.220 --> 00:35:32.180]   last time.
[00:35:32.180 --> 00:35:38.740]   And so -- but that would keep it to three procedures instead of four, a number of procedures I
[00:35:38.740 --> 00:35:43.900]   think does matter.
[00:35:43.900 --> 00:35:50.060]   What happens when I loop inside of a loop and try to call remove, it works fine unless
[00:35:50.060 --> 00:35:56.400]   you're looping over the same loop twice, in which case it's an implementation detail
[00:35:56.400 --> 00:35:58.740]   of your data structure, what happens, right?
[00:35:58.740 --> 00:36:05.180]   If it's a regular array, like an in-language array, like, say, one of the dynamic arrays,
[00:36:05.180 --> 00:36:11.700]   then remove is unordered, so it'll copy the last element to the current element, right?
[00:36:11.700 --> 00:36:18.580]   And so -- you may skip an item.
[00:36:18.580 --> 00:36:23.220]   You probably won't ever visit an item twice if that's happening, but you may skip an item
[00:36:23.220 --> 00:36:28.980]   because one of your loops was past that element, and now it'll never see it, right?
[00:36:28.980 --> 00:36:34.420]   So these are just things, though -- some people try to solve that kind of problem with iterators,
[00:36:34.420 --> 00:36:37.580]   but I don't think it's the right way to do it, because things just get very messy very
[00:36:37.580 --> 00:36:41.380]   fast, and you still don't ultimately really solve any problems.
[00:36:41.380 --> 00:36:47.540]   So my preference is to just be aware as a programmer that I'm doing that kind of thing
[00:36:47.540 --> 00:36:50.300]   and be careful about it.
[00:36:50.300 --> 00:36:56.020]   You certainly, though, again, because we have very good meta programming in this language,
[00:36:56.020 --> 00:37:01.260]   if you are worried about that being an error that people might get into, you could actually
[00:37:01.260 --> 00:37:05.100]   scan for that occurrence, like iterating over the same list twice.
[00:37:05.100 --> 00:37:10.020]   Now you don't necessarily -- because it compiled time, you don't necessarily know if things
[00:37:10.020 --> 00:37:17.940]   are alias or not, it's undecidable, right, but you could at least catch the obvious cases
[00:37:17.940 --> 00:37:22.140]   of people iterating over something that's clearly the same array twice, right, if you
[00:37:22.140 --> 00:37:35.180]   were concerned with catching that bug.
[00:37:35.180 --> 00:37:38.140]   Someone says, "Is this a call-back pointer or a hard-coded call?
[00:37:38.140 --> 00:37:41.820]   If former, I'm worried about loading the pointer in the overhead."
[00:37:41.820 --> 00:37:45.020]   It's neither -- so here's what I'm thinking, right?
[00:37:45.020 --> 00:37:50.620]   He's just -- first of all, if you define this procedure, let's get rid of the remove for
[00:37:50.620 --> 00:37:51.620]   an error.
[00:37:51.620 --> 00:37:52.620]   It's clarifying.
[00:37:52.620 --> 00:37:56.580]   So this is just a regular procedure, and you could call it passing a -- another procedure
[00:37:56.580 --> 00:38:01.660]   here to visit, and that would be a regular call, the procedure, for every element, not
[00:38:01.660 --> 00:38:03.740]   particularly fast, right?
[00:38:03.740 --> 00:38:09.180]   But if you used it in a for loop, the for loop would -- or if you used a data structure
[00:38:09.180 --> 00:38:17.260]   in the for loop, the data structure would recognize this pattern and actually go into
[00:38:17.260 --> 00:38:20.700]   the syntax tree and yank pieces out and rearrange it.
[00:38:20.700 --> 00:38:28.580]   So this visit then is not actually anything, like it doesn't represent a pointer or any
[00:38:28.580 --> 00:38:29.740]   data type at all.
[00:38:29.740 --> 00:38:34.540]   It represents the place in which you put the user's code, right?
[00:38:34.540 --> 00:38:39.740]   So if I was calling this as a function, the code to run on every item would be in visit,
[00:38:39.740 --> 00:38:40.740]   right?
[00:38:40.740 --> 00:38:44.860]   So in a for loop like this, this is the code to run on every item, so that would just get
[00:38:44.860 --> 00:38:47.020]   pasted where the visit is.
[00:38:47.020 --> 00:38:49.860]   That's sort of the idea.
[00:38:49.860 --> 00:38:55.580]   Someone saying you're worried about overhead with the old way, incur some overhead as well.
[00:38:55.580 --> 00:39:00.220]   Yes, it would incur some overhead, but there's differences in magnitude, right?
[00:39:00.220 --> 00:39:07.740]   Like messing around with a fiber is more heavyweight than having a little bit of overhead because
[00:39:07.740 --> 00:39:14.020]   your compiler cannot necessarily optimize out all the data, structure references in
[00:39:14.020 --> 00:39:15.980]   your iterator or something.
[00:39:15.980 --> 00:39:21.980]   Ideally, if compilers were smart and if we didn't have so many aliasing problems, there
[00:39:21.980 --> 00:39:26.820]   would be basically no overhead to this current iterator scheme because like, hey, these things
[00:39:26.820 --> 00:39:31.300]   are all in lines or whatever.
[00:39:31.300 --> 00:39:34.460]   And they're doing trivial things for the most part.
[00:39:34.460 --> 00:39:38.780]   But again, because of aliasing and because compilation is just always harder, generation
[00:39:38.780 --> 00:39:44.100]   of optimized code is always harder than you think it would be, in reality, it doesn't
[00:39:44.100 --> 00:39:45.300]   work out that way.
[00:39:45.300 --> 00:39:53.340]   So you would only use an iterator like this in cases where the amount of work that you're
[00:39:53.340 --> 00:40:00.940]   doing per iteration is definitely more than the cost of the iterator, right?
[00:40:00.940 --> 00:40:04.260]   Or where you don't necessarily care that much about the cost of the iterator because you
[00:40:04.260 --> 00:40:09.620]   just know that this isn't going to be that expensive for what you're doing, right?
[00:40:09.620 --> 00:40:11.620]   Like I know that I'm iterating over 30 things.
[00:40:11.620 --> 00:40:12.620]   Who cares?
[00:40:12.620 --> 00:40:17.940]   So if I'm iterating over 60,000 things, I might start to care, but even 60,000 is not
[00:40:17.940 --> 00:40:24.660]   that many on modern computers, unless I'm doing it a lot every frame.
[00:40:24.660 --> 00:40:31.220]   One more thing I was going to say there, and I don't remember what it was now, what was
[00:40:31.220 --> 00:40:32.980]   the question again?
[00:40:32.980 --> 00:40:37.260]   Oh, it's about overhead.
[00:40:37.260 --> 00:40:49.500]   Oh, and so if you say there's already overhead, it's like, well, what does it matter if there's
[00:40:49.500 --> 00:40:55.460]   a little more or a little less, but to the extent that you can make the overhead small
[00:40:55.460 --> 00:41:02.580]   within reason, you widen the number of situations in which using an iterator like this is applicable.
[00:41:02.580 --> 00:41:04.180]   So that's a good, right?
[00:41:04.180 --> 00:41:11.860]   You want it to be as applicable as possible without sacrificing too much, I don't know,
[00:41:11.860 --> 00:41:13.140]   convenience or utility or something.
[00:41:13.140 --> 00:41:16.860]   There's lots of things you could sacrifice, right?
[00:41:16.860 --> 00:41:21.700]   That said, I think that almost universally, this will be lower overhead.
[00:41:21.700 --> 00:41:25.780]   This will be way lower overhead than this, which is another reason why it's appealing
[00:41:25.780 --> 00:41:41.380]   because the compiler doesn't have to reason about as much stuff here, not nearly.
[00:41:41.380 --> 00:41:45.140]   So someone's saying, SuitedM73 is saying, so I'm not a serious game programmer, but I'm
[00:41:45.140 --> 00:41:48.060]   a serious shift code, high performance code type programmer.
[00:41:48.060 --> 00:41:55.740]   That counts too, by the way, any kind of high performance serious programming.
[00:41:55.740 --> 00:42:01.020]   He says, I think the only algorithm I've ever used in algorithm, bracket, algorithm, bracket
[00:42:01.020 --> 00:42:02.100]   is binary search.
[00:42:02.100 --> 00:42:06.740]   That's the only one that's more convenient to get out of can than to write from scratch.
[00:42:06.740 --> 00:42:15.420]   Yeah, I buy that.
[00:42:15.420 --> 00:42:18.940]   But even if you're going, if you're going ultimately for performance, I think you can
[00:42:18.940 --> 00:42:23.900]   do a better job on any particular data structure like that bucket array.
[00:42:23.900 --> 00:42:27.980]   There's a structure where each bucket exists somewhere in memory and then the buckets are
[00:42:27.980 --> 00:42:32.140]   possibly far apart in memory because they came from a generalized heap, right?
[00:42:32.140 --> 00:42:37.980]   So if you were going to search on that thing and be as fast as you could, you probably
[00:42:37.980 --> 00:42:43.260]   would have a strategy specific to it.
[00:42:43.260 --> 00:42:52.740]   That said, like I was saying before, often sometimes you don't care about the absolute
[00:42:52.740 --> 00:42:55.500]   fastest performance and you just want to get something done.
[00:42:55.500 --> 00:43:02.860]   That to my mind is where that kind of algorithm.h stuff is for, but I just never have used
[00:43:02.860 --> 00:43:03.860]   it.
[00:43:03.860 --> 00:43:08.980]   If you do serious scientific stuff and have only used it once, then that's a little bit
[00:43:08.980 --> 00:43:12.900]   of a corroboration because look at how much stuff is there, dude.
[00:43:12.900 --> 00:43:19.900]   Let's go to the C++ one like, dude.
[00:43:19.900 --> 00:43:23.300]   All of these are components that are used in the other things, right?
[00:43:23.300 --> 00:43:28.700]   Like I don't want to, yeah, I've already ranted about that.
[00:43:28.700 --> 00:43:32.460]   I don't need to keep ranting.
[00:43:32.460 --> 00:43:34.580]   Have I considered C# approaches to iterators?
[00:43:34.580 --> 00:43:36.620]   They translate iterator functions into state machines?
[00:43:36.620 --> 00:43:37.620]   I don't know.
[00:43:37.620 --> 00:43:38.820]   I don't know C# at all.
[00:43:38.820 --> 00:43:46.260]   So if someone wants to email me a pointer or a link on the web to a good C# primer about
[00:43:46.260 --> 00:43:49.740]   iterators that's smart, I would appreciate that.
[00:43:49.740 --> 00:43:53.740]   It would be good to look at.
[00:43:53.740 --> 00:43:59.540]   Could we use this feature to specify four in terms of while?
[00:43:59.540 --> 00:44:06.220]   Do you mean, this is James Whidman asking, do you mean like in here?
[00:44:06.220 --> 00:44:11.900]   Yeah, I mean, what's inside this iterate could be anything, like there doesn't even have to
[00:44:11.900 --> 00:44:12.900]   be a loop.
[00:44:12.900 --> 00:44:13.900]   You could put a while here, right?
[00:44:13.900 --> 00:44:18.420]   While I was less than 1,000, right?
[00:44:18.420 --> 00:44:19.420]   You could put anything.
[00:44:19.420 --> 00:44:26.100]   The idea is you can print that print hello sailor, right?
[00:44:26.100 --> 00:44:30.020]   The idea is that it wouldn't care very much about the structure of what is actually can
[00:44:30.020 --> 00:44:31.660]   go bananas, right?
[00:44:31.660 --> 00:44:35.980]   It wouldn't care very much about the structure of what's in this code.
[00:44:35.980 --> 00:44:39.660]   It would only be looking for these, right?
[00:44:39.660 --> 00:44:48.220]   And you could maybe have multiple visits, like do something, right?
[00:44:48.220 --> 00:44:56.700]   And then have another visit, you know, and it would just work that out and pace doesn't
[00:44:56.700 --> 00:45:00.300]   necessarily, or if it looked like there were too many visits, you might actually want to
[00:45:00.300 --> 00:45:06.460]   turn it into a procedure call, or at least a jump to a known point with a return branch
[00:45:06.460 --> 00:45:08.660]   point in a register, right?
[00:45:08.660 --> 00:45:18.620]   So you can do an indirect jump without taking all the procedure call overhead.
[00:45:18.620 --> 00:45:21.680]   Someone says the iteration scheme you've been looking at seems like it could just be implemented
[00:45:21.680 --> 00:45:24.700]   by smartly replacing the four and the other syntax with the iteration code.
[00:45:24.700 --> 00:45:27.380]   That's exactly what I'm talking about.
[00:45:27.380 --> 00:45:32.380]   And this is something you could sort of do at a meta programming level.
[00:45:32.380 --> 00:45:39.900]   So I might actually experiment with this feature in user code rather than compiler code, because
[00:45:39.900 --> 00:45:42.820]   you can make all this happen in user code, right?
[00:45:42.820 --> 00:45:48.020]   And then from there probably the difference between that being in the compiler or not
[00:45:48.020 --> 00:45:54.860]   is maybe just about ergonomics, like how fast does it run, do you get it by default?
[00:45:54.860 --> 00:45:57.140]   How good are the error messages?
[00:45:57.140 --> 00:46:03.100]   Because sometimes the compiler might be generating an error message about something, but if it
[00:46:03.100 --> 00:46:08.500]   knows that it came from the result of the compiler pacing in this code, then it can customize,
[00:46:08.500 --> 00:46:09.500]   right?
[00:46:09.500 --> 00:46:10.500]   So we'll see.
[00:46:10.500 --> 00:46:13.860]   Like I said, I'm not going to do this immediately anyway.
[00:46:13.860 --> 00:46:22.740]   This was more about just getting the idea out there.
[00:46:22.740 --> 00:46:26.620]   Looping through the same array twice, this is in reference to the earlier question, we'll
[00:46:26.620 --> 00:46:32.620]   both remove functions get called just one, a special remove function.
[00:46:32.620 --> 00:46:42.420]   So if you say remove in a loop, the idea, so back when we had remove defined down here,
[00:46:42.420 --> 00:46:45.580]   the idea was this would get called for any removal, right?
[00:46:45.580 --> 00:46:49.580]   So and it wouldn't even get called, it would again just be code that gets pasted in.
[00:46:49.580 --> 00:46:55.380]   So whatever you put in here, we'll just get pasted every time you said remove as a block,
[00:46:55.380 --> 00:46:56.380]   right?
[00:46:56.380 --> 00:46:59.820]   So, yeah, it would happen multiple times.
[00:46:59.820 --> 00:47:12.460]   Oh, so you, let's simplify to one array, if you're iterating over only one array, and
[00:47:12.460 --> 00:47:19.620]   you remove twice, that's an error or it probably, it's not really an error because it'll work
[00:47:19.620 --> 00:47:24.100]   usually unless you're arrayed only, unless the first remove or remove the last element
[00:47:24.100 --> 00:47:31.620]   in the array, but it just may not have semantics that you want because it will remove an item
[00:47:31.620 --> 00:47:32.620]   that you haven't visited.
[00:47:32.620 --> 00:47:38.700]   So you really, even in a one dimensional array iteration, you really only want to call remove
[00:47:38.700 --> 00:47:44.660]   once per time, otherwise you're probably doing something you don't want, but it'll functionally
[00:47:44.660 --> 00:47:45.660]   happen, right?
[00:47:45.660 --> 00:47:50.300]   And so you can extrapolate that to the case of nested arrays or whatever.
[00:47:50.300 --> 00:48:02.220]   I think that's what you're asking about.
[00:48:02.220 --> 00:48:04.900]   Someone says what's the advantage over an inlining step?
[00:48:04.900 --> 00:48:12.380]   I assume you mean like here, because I don't have to wrap the code in a lambda, which is
[00:48:12.380 --> 00:48:14.860]   annoying.
[00:48:14.860 --> 00:48:15.860]   Maybe there's no difference.
[00:48:15.860 --> 00:48:20.660]   I think it's a super convenient way to do lambdas.
[00:48:20.660 --> 00:48:31.620]   Like if I just say, maybe this is automatically a lambda and this is inlined and then that
[00:48:31.620 --> 00:48:35.580]   just works.
[00:48:35.580 --> 00:48:38.660]   Maybe that could be a way that that works.
[00:48:38.660 --> 00:48:48.680]   I don't have lambdas yet, I'm skeptical that it's so easy to optimize lambdas because sometimes
[00:48:48.680 --> 00:48:58.100]   funny things happen, but maybe that might be a way to do it.
[00:48:58.100 --> 00:49:03.180]   Someone says is this a little bit like a LISP macro?
[00:49:03.180 --> 00:49:08.820]   I guess so, but not really.
[00:49:08.820 --> 00:49:14.420]   The stuff we were doing in that one meta programming demo was a little bit similar to
[00:49:14.420 --> 00:49:18.940]   this where you're copying and pasting code and that's kind of like what LISP macros do,
[00:49:18.940 --> 00:49:26.040]   but I mean the reason LISP macros work is because the syntax of LISP is so simple that
[00:49:26.040 --> 00:49:28.260]   it's easy to operate on logically, right?
[00:49:28.260 --> 00:49:38.940]   So an equivalent of LISP macros in this language would be like, well, I mean the equivalent,
[00:49:38.940 --> 00:49:45.100]   really the equivalent is the stuff that I showed where you have all this stuff where
[00:49:45.100 --> 00:49:49.340]   you have data structures for every node, right?
[00:49:49.340 --> 00:49:51.260]   And you manipulate on it.
[00:49:51.260 --> 00:49:54.180]   That's basically what LISP macros do.
[00:49:54.180 --> 00:49:58.180]   It's just that because LISP is such a simpler syntax, you don't have all this stuff, right?
[00:49:58.180 --> 00:50:03.020]   And one of the decisions that I made in building this language is that I value this syntax and
[00:50:03.020 --> 00:50:06.780]   I think it's good ergonomically for the programmer.
[00:50:06.780 --> 00:50:13.140]   So as a result, you get this kind of complexity to do the equivalent of what LISP does.
[00:50:13.140 --> 00:50:19.420]   Now that said, we do more because we have compile time type checking on things and you
[00:50:19.420 --> 00:50:23.580]   get that information in these data structures, which in LISP macros, you don't.
[00:50:23.580 --> 00:50:32.500]   I believe that something like that can depend on the particular flavor of LISP and all that.
[00:50:32.500 --> 00:50:38.300]   But in general, LISP macros are evaluated early, right?
[00:50:38.300 --> 00:50:46.460]   So I don't know.
[00:50:46.460 --> 00:50:50.580]   Someone is asking for function overload and ambiguity compiler error you showed last time.
[00:50:50.580 --> 00:50:55.480]   Is there a way to specify that more specific functions take precedence over more general
[00:50:55.480 --> 00:50:56.480]   ones?
[00:50:56.480 --> 00:50:57.480]   I haven't done that yet.
[00:50:57.480 --> 00:50:59.220]   I've thought about it.
[00:50:59.220 --> 00:51:08.700]   I'm not sure if it's a good idea to get that complex.
[00:51:08.700 --> 00:51:12.860]   Sometimes it's better to just leave it simple and if you want that functionality, you better
[00:51:12.860 --> 00:51:17.700]   not do it or you better do it a different way or something, right?
[00:51:17.700 --> 00:51:22.360]   There already is, right, if you go back and look at the overloading demo, there is an
[00:51:22.360 --> 00:51:30.740]   idea of distance between parameter types and for regular overloads, it will call the thing
[00:51:30.740 --> 00:51:31.740]   that's closest.
[00:51:31.740 --> 00:51:36.940]   And I won't go look at that now, but remember if you should pass an unsigned 8-bit number
[00:51:36.940 --> 00:51:42.980]   as an argument and there's an overload with 32-bit and overload with 64-bit, it'll call
[00:51:42.980 --> 00:51:46.500]   the 32-bit one because 32-bit is closer, right?
[00:51:46.500 --> 00:51:49.660]   So that's how these ambiguities are resolved, but in general, I think there's a very few
[00:51:49.660 --> 00:51:55.500]   cases in the real world where you want to let the compiler do that.
[00:51:55.500 --> 00:52:00.900]   The only other case is sort of in inheritance, right, where you have like the base class
[00:52:00.900 --> 00:52:07.140]   and you have the derived class and if there's a -- you want a base class version to cover
[00:52:07.140 --> 00:52:11.380]   everybody, but you maybe want a particular one for a derived class, but even that, when
[00:52:11.380 --> 00:52:16.260]   I type that in, I'm a little nervous about it, you know?
[00:52:16.260 --> 00:52:22.340]   But that also -- we demoed that in that particular demo too, I don't know if you saw that one.
[00:52:22.340 --> 00:52:29.100]   So the thing is -- so there is that notion of distance between arguments, but the thing
[00:52:29.100 --> 00:52:34.020]   is if all the compiler can see is dollar sign T, like in this example, that type could be
[00:52:34.020 --> 00:52:35.020]   anything, right?
[00:52:35.020 --> 00:52:40.420]   I mean, we reason about the type in here, but that's code that humans need to understand
[00:52:40.420 --> 00:52:45.300]   and that the compiler is never going to completely reason about, at least not in this language.
[00:52:45.300 --> 00:52:48.900]   And if you made it something that could be automatically reason about it, you would be
[00:52:48.900 --> 00:52:52.020]   limiting the expressivity of this kind of thing.
[00:52:52.020 --> 00:52:59.780]   So if you can't ever tell the difference -- the distance between this and int or this
[00:52:59.780 --> 00:53:04.860]   and table, right, if there's a subclass in relation or something, if you can't tell that
[00:53:04.860 --> 00:53:13.700]   distance, I don't know if it makes sense to allow you to express it somehow.
[00:53:13.700 --> 00:53:18.900]   I mean, in this case, this modify doesn't necessarily permit only one type.
[00:53:18.900 --> 00:53:23.580]   In this case it does, in this -- well, in this case it says pointer to a particular -- in
[00:53:23.580 --> 00:53:27.380]   this case it says pointer to any kind of procedure, right?
[00:53:27.380 --> 00:53:33.940]   So you might have to introduce a way to return an integer that represents the priority based
[00:53:33.940 --> 00:53:37.780]   on what type of procedure, but it just seems very messy.
[00:53:37.780 --> 00:53:44.140]   I would rather -- but I don't know, I mean, I'd rather keep it simple, but we just have
[00:53:44.140 --> 00:53:46.700]   to see how it works in real-world code.
[00:53:46.700 --> 00:53:52.740]   And if it's needed, then we'll do something, basically is my policy on that.
[00:53:52.740 --> 00:53:58.140]   But this is complicated and esoteric enough that I'm not going to do it unless it's clearly
[00:53:58.140 --> 00:54:00.980]   needed.
[00:54:00.980 --> 00:54:06.900]   Someone's asking, have I heard of Jenson's device?
[00:54:06.900 --> 00:54:13.140]   I think that used call by name in algal 60 to do an interesting iteration thing.
[00:54:13.140 --> 00:54:15.340]   I've probably heard of that in college.
[00:54:15.340 --> 00:54:22.580]   I don't remember what it is, if you want to email me and remind me, that'd be cool.
[00:54:22.580 --> 00:54:25.580]   Can while loops be labeled and do labels work with continue?
[00:54:25.580 --> 00:54:30.740]   So labels work with continue in for loops because you name the iterator.
[00:54:30.740 --> 00:54:33.060]   While loops do not have labels yet.
[00:54:33.060 --> 00:54:37.900]   I'm up in the air about whether I'm going to add labels, just because I use for loops
[00:54:37.900 --> 00:54:44.740]   so often, I don't really fall back on while loops that often.
[00:54:44.740 --> 00:54:48.620]   So I've never in practical code that I've written so far, I've never needed to break
[00:54:48.620 --> 00:54:52.060]   out of a nested while loop.
[00:54:52.060 --> 00:54:58.500]   And again, if it's while inside a for, you can use a label of the for, right?
[00:54:58.500 --> 00:55:03.660]   So it's only if it's like a for inside a while or a while inside a while.
[00:55:03.660 --> 00:55:07.100]   In fact, there's no go to labels at all in the language yet.
[00:55:07.100 --> 00:55:10.340]   Like I've never had to say go to whatever.
[00:55:10.340 --> 00:55:17.340]   So I assume that labels will be coming for while at some point and that there will be
[00:55:17.340 --> 00:55:22.100]   a syntax for that just because it's dirty to not have them and to have them for for it
[00:55:22.100 --> 00:55:27.220]   like doesn't make that much sense, but I don't know what the syntax would be for it.
[00:55:27.220 --> 00:55:34.660]   So I'm just not going to do it yet.
[00:55:34.660 --> 00:55:39.220]   But if it becomes needed, I'll do it.
[00:55:39.220 --> 00:55:44.380]   Someone's saying maybe introducing a proper macro feature or deriving it from already
[00:55:44.380 --> 00:55:48.620]   existing compile time execution features, provide a mechanism for defining loop, baking,
[00:55:48.620 --> 00:55:51.340]   a rather explicit and transparent manner, maybe.
[00:55:51.340 --> 00:55:59.100]   So at some point soon I was toying around with doing a specific preprocessor demo.
[00:55:59.100 --> 00:56:04.500]   It would be very easy to hook in, in fact, as I demoed at one point a while ago, you
[00:56:04.500 --> 00:56:11.340]   can already build your own preprocessor, right?
[00:56:11.340 --> 00:56:15.260]   Especially if it just operates on program text, that's kind of trivial.
[00:56:15.260 --> 00:56:20.580]   The one step that isn't in yet is to set it by default, like sometimes you just, if you
[00:56:20.580 --> 00:56:26.980]   just say import or load that won't trigger a preprocessor, you would have to call the
[00:56:26.980 --> 00:56:32.700]   load function from the compile time meta program to add your files, but you know, I demoed
[00:56:32.700 --> 00:56:33.700]   that several times.
[00:56:33.700 --> 00:56:37.580]   So if you follow the demos, you know what I'm talking about.
[00:56:37.580 --> 00:56:43.580]   So at that stage, it's easy to add a preprocessor and the one extra step would be to just have
[00:56:43.580 --> 00:56:48.940]   a function called compiler set default preprocessor or something that would make that run or anything.
[00:56:48.940 --> 00:56:52.180]   So I've thought about doing that sometime.
[00:56:52.180 --> 00:56:58.900]   But to do this kind of unpacking a for loop or whatever, you don't necessarily want textual
[00:56:58.900 --> 00:57:04.460]   preprocessing, because unless you say, well, write your for with slightly funny syntax
[00:57:04.460 --> 00:57:06.660]   and the preprocessor will see it, right?
[00:57:06.660 --> 00:57:12.700]   But if you're iterating over a bunch of different things, like if I say for a and that's an
[00:57:12.700 --> 00:57:18.500]   array in the languages, it's a native language array, and then you say for be, and that's
[00:57:18.500 --> 00:57:23.820]   a user defined data structure, then you maybe don't want to mess with that one, but you
[00:57:23.820 --> 00:57:29.100]   do want to mess with that one, but it's hard for a preprocessor to know about this, because
[00:57:29.100 --> 00:57:33.260]   the difference between these two things is a semantic difference, and it requires understanding
[00:57:33.260 --> 00:57:34.260]   the program, right?
[00:57:34.260 --> 00:57:39.020]   And by the time you understand the program, you're way deeper into compilation than the
[00:57:39.020 --> 00:57:41.700]   preprocess step, traditionally, right?
[00:57:41.700 --> 00:57:48.700]   So that's why I've done those metaprogramming examples that use type inference information,
[00:57:48.700 --> 00:57:51.660]   and so that is the place that I would do it.
[00:57:51.660 --> 00:57:59.020]   If you remember the profiling demo, the inserted profiling code or the go bananas demo, that's
[00:57:59.020 --> 00:58:04.780]   sort of the level where I think my first attempt at doing this would happen.
[00:58:04.780 --> 00:58:20.460]   I don't want to keep talking about this question, but he says, well, if you have several nested
[00:58:20.460 --> 00:58:24.020]   for loops over the same array and you call and remove deep inside the loop, then you would
[00:58:24.020 --> 00:58:27.220]   potentially invalidate the outer iterators and with a special remove function, the part
[00:58:27.220 --> 00:58:28.220]   of it.
[00:58:28.220 --> 00:58:29.220]   Yeah, yeah, yeah.
[00:58:29.220 --> 00:58:33.460]   Character validation doesn't really solve problems.
[00:58:33.460 --> 00:58:41.580]   It just at best gives you a runtime error condition or something that you can then deal
[00:58:41.580 --> 00:58:49.900]   with, which I would rather just say, look, be aware of when you're going to invalidate
[00:58:49.900 --> 00:58:51.660]   your iterator and don't do that.
[00:58:51.660 --> 00:58:56.620]   Like I think if you just adopt that policy, you get actually cleaner code that's easier
[00:58:56.620 --> 00:59:00.260]   to understand and that's faster even.
[00:59:00.260 --> 00:59:03.860]   So, you know, iterator invalidation is not free, right?
[00:59:03.860 --> 00:59:10.800]   Somebody has to be tracking your iterator and that takes time.
[00:59:10.800 --> 00:59:13.740]   And it introduces aliasing, right, because now someone's got a pointer to your frickin
[00:59:13.740 --> 00:59:21.220]   iterator.
[00:59:21.220 --> 00:59:25.180]   When I iterate over normal arrays, my value is it equivalent to array, it index, hence
[00:59:25.180 --> 00:59:27.820]   you can change the elements in the array.
[00:59:27.820 --> 00:59:38.380]   Yes, except that by default, if you say like for a something of it, right, by default,
[00:59:38.380 --> 00:59:40.500]   this is a value copy.
[00:59:40.500 --> 00:59:45.500]   But if you put a star here, you mean to visit everything with a pointer to everything.
[00:59:45.500 --> 00:59:46.660]   And then you definitely could.
[00:59:46.660 --> 00:59:53.500]   You could say for every element of a equals five or whatever and that would set an array
[00:59:53.500 --> 00:59:54.740]   of integers to five.
[00:59:54.740 --> 01:00:01.740]   In fact, in some earlier array demos, I did stuff like this, right, which will initialize
[01:00:01.740 --> 01:00:08.820]   an array to zero, one, two, three, four, five, up to the number of things in the array, right.
[01:00:08.820 --> 01:00:14.820]   Or instead of this pointer thing, you could say for a, a sub it index equals the index,
[01:00:14.820 --> 01:00:23.860]   what does a number of ways to say the same thing?
[01:00:23.860 --> 01:00:26.880]   No one's saying take a look at Rust's macro system.
[01:00:26.880 --> 01:00:32.900]   If you want to link me to that email address, a good example of a page, because there's lots
[01:00:32.900 --> 01:00:33.900]   of stuff.
[01:00:33.900 --> 01:00:37.060]   Rust is one of the languages that lots of people are interested in right now and so
[01:00:37.060 --> 01:00:42.340]   there tends to be a lot of stuff on the web and I don't tend to know exactly which information
[01:00:42.340 --> 01:00:43.340]   is the highest quality.
[01:00:43.340 --> 01:00:47.580]   So if you just want to link that, that'd be good.
[01:00:47.580 --> 01:01:11.740]   I'm not going to be able to, well, here I'll copy the sentence device, link, easy size talking
[01:01:11.740 --> 01:01:26.100]   shit about my programming language, look dude, make a better language and we'll talk.
[01:01:26.100 --> 01:01:29.700]   Someone says when there's an operator overloading, will there be a copy assignment operator?
[01:01:29.700 --> 01:01:30.940]   I don't think so.
[01:01:30.940 --> 01:01:38.300]   So, you know, this being a data oriented language, one of the things that I want to preserve
[01:01:38.300 --> 01:01:49.340]   is extreme clarity about what is happening in the program, right?
[01:01:49.340 --> 01:01:57.820]   And I think by the time you get to copy constructors and you're doing something with those, that's
[01:01:57.820 --> 01:02:00.140]   just, I never enjoy that.
[01:02:00.140 --> 01:02:05.700]   When I see a program doing that or when I even consider, I almost never do it, in fact, basically
[01:02:05.700 --> 01:02:10.280]   I've never done it, but I guess in a few cases I was experimenting with it and it doesn't
[01:02:10.280 --> 01:02:11.280]   make me happy.
[01:02:11.280 --> 01:02:13.820]   It introduces confusion at what's going on.
[01:02:13.820 --> 01:02:19.100]   So for the same reason, you know, we don't have the tables that are like some hidden member
[01:02:19.100 --> 01:02:20.820]   of a struct, right?
[01:02:20.820 --> 01:02:26.820]   Like structs, what you see is what you get and you can copy them with memcopy and get
[01:02:26.820 --> 01:02:27.980]   a valid struct.
[01:02:27.980 --> 01:02:35.220]   Like that's just a fundamental line in the sand about this language.
[01:02:35.220 --> 01:02:47.580]   And so, yeah, I mean copy constructors land in that category for me, right?
[01:02:47.580 --> 01:02:53.860]   Like I think A equals B should be a memory to memory copy always.
[01:02:53.860 --> 01:03:00.900]   If you want something more complex than that, then you should put a function call there
[01:03:00.900 --> 01:03:04.260]   or something manually so that people know what's going on.
[01:03:04.260 --> 01:03:08.860]   Now, of course, the argument behind copy constructors is like, well, I have a pointer to some resource
[01:03:08.860 --> 01:03:13.780]   that has to get, you know, like I have a pointer to some memory, like a string name
[01:03:13.780 --> 01:03:15.780]   and that string has got to be allocated on the heap.
[01:03:15.780 --> 01:03:20.580]   So when I make a copy of the thing, that has to be allocated on the heap also, so I have
[01:03:20.580 --> 01:03:21.580]   a copy constructor.
[01:03:21.580 --> 01:03:26.300]   And the problem is that once your program starts getting complicated, that kind of thing
[01:03:26.300 --> 01:03:28.900]   just goes completely bananas.
[01:03:28.900 --> 01:03:32.500]   And your program gets really complicated and slow and there's all these things happening
[01:03:32.500 --> 01:03:37.100]   that you're not even aware are happening because they happen in the copy constructors and you
[01:03:37.100 --> 01:03:41.300]   get the famous Chrome 50,000 allocations or the press keystroke or whatever.
[01:03:41.300 --> 01:03:44.260]   It's just not a good neighborhood to be in.
[01:03:44.260 --> 01:03:45.820]   So I don't want to go there.
[01:03:45.820 --> 01:04:02.420]   I want to do as much as I can without stepping on that landline.
[01:04:02.420 --> 01:04:04.860]   Would a break inside a visit mean return from iterate?
[01:04:04.860 --> 01:04:06.820]   I don't want to visit anything else.
[01:04:06.820 --> 01:04:12.300]   Well, if you're talking about visit as an explicit external function, then no, it wouldn't.
[01:04:12.300 --> 01:04:16.340]   But in the sense that it's pasting code in, then it would, because visit isn't actually
[01:04:16.340 --> 01:04:20.660]   a procedure call.
[01:04:20.660 --> 01:04:23.660]   Does this mean we could get a return value from a for loop?
[01:04:23.660 --> 01:04:24.660]   Well, I don't know.
[01:04:24.660 --> 01:04:27.500]   I mean, that's the way Python does for loops and stuff, I think.
[01:04:27.500 --> 01:04:32.260]   Like for loops and Python are actually generator functions or something.
[01:04:32.260 --> 01:04:40.140]   Or if you put them in square brackets, they are, I don't know.
[01:04:40.140 --> 01:05:02.060]   But although I need misunderstanding the question, he meant garbage, not like garbage.
[01:05:02.060 --> 01:05:08.820]   In rust, a equals b does a mem copy and disallows us at b, whereas a equals b dot copy, you
[01:05:08.820 --> 01:05:12.020]   can do a copy function.
[01:05:12.020 --> 01:05:17.340]   Well, maybe, but you can make a copy function, right?
[01:05:17.340 --> 01:05:22.180]   So why don't you just make copy function?
[01:05:22.180 --> 01:05:28.660]   When it comes to disallowing b, I don't think we're going to do that.
[01:05:28.660 --> 01:05:30.900]   That's a very opinionated thing to do.
[01:05:30.900 --> 01:05:38.620]   And for rust, it makes sense because the idea behind rust is extreme safety, right?
[01:05:38.620 --> 01:05:50.260]   But I've explained before why I don't necessarily think that's the best priority.
[01:05:50.260 --> 01:05:54.860]   But I would like to make sure that everything is powerful enough that if you wanted to make
[01:05:54.860 --> 01:05:56.700]   that happen, you could.
[01:05:56.700 --> 01:06:04.540]   So I think in at least many cases, it would be pretty straightforward.
[01:06:04.540 --> 01:06:05.540]   Sorry about that.
[01:06:05.540 --> 01:06:12.660]   It would be very straightforward to make a compiled time meta program that scans for
[01:06:12.660 --> 01:06:16.900]   that kind of thing, and disallows it, like, hey, you assigned something to b, now you're
[01:06:16.900 --> 01:06:18.420]   not allowed to use b.
[01:06:18.420 --> 01:06:21.820]   The thing that gets harder is when there's aliasing and stuff, like your compile time
[01:06:21.820 --> 01:06:26.100]   meta program would have to do all the aliasing analysis, right?
[01:06:26.100 --> 01:06:32.260]   So you'd have to build up something as complex as rusts, borrow, checking system.
[01:06:32.260 --> 01:06:33.860]   But you could do that, right?
[01:06:33.860 --> 01:06:41.100]   I don't think there's anything preventing you from doing that at compile time.
[01:06:41.100 --> 01:06:47.100]   So if you were interested in going that way, my job is to make sure that you can do it as
[01:06:47.100 --> 01:06:50.820]   best as possible using the meta programming facilities.
[01:06:50.820 --> 01:06:54.700]   I don't really see it as my job to put that in the core language just because it's so
[01:06:54.700 --> 01:07:01.300]   opinionated and causes problems for people who don't share that opinion, which I don't
[01:07:01.300 --> 01:07:02.940]   necessarily share.
[01:07:02.940 --> 01:07:07.340]   I mean, maybe rust will work out great, and it'll be 10 years from now the language that
[01:07:07.340 --> 01:07:08.580]   everybody should be using.
[01:07:08.580 --> 01:07:09.580]   That's possible.
[01:07:09.580 --> 01:07:12.500]   And we'll see how that works out, and we'll learn from what they do.
[01:07:12.500 --> 01:07:25.300]   But right now, I think it's a little early to be doing things that are that forceful.
[01:07:25.300 --> 01:07:28.740]   Python uses an exception to exit the for loop while we ain't doing that.
[01:07:28.740 --> 01:07:30.700]   No exceptions here, man.
[01:07:30.700 --> 01:07:41.460]   So, someone's saying, "I guess if you want to break from a custom iteration, you want
[01:07:41.460 --> 01:07:44.260]   to break from out the iterate procedure most of the time.
[01:07:44.260 --> 01:07:47.420]   But if you want to do something else, shouldn't you just write the iteration at the use site
[01:07:47.420 --> 01:07:48.420]   for clarity?"
[01:07:48.420 --> 01:07:52.700]   Well, maybe, but part of the whole reason that we want to abstract away the iteration
[01:07:52.700 --> 01:07:57.420]   is you don't have to worry about how to properly iterate over the particular data structure.
[01:07:57.420 --> 01:08:02.720]   And sometimes it's hard to remember all the details.
[01:08:02.720 --> 01:08:06.900]   Sometimes the more you write custom iterations like that, that assume they understand the
[01:08:06.900 --> 01:08:10.000]   data structure, the more you calcify things, and the more you'll break if you change the
[01:08:10.000 --> 01:08:11.000]   data structure.
[01:08:11.000 --> 01:08:15.220]   This is standard good programming practice.
[01:08:15.220 --> 01:08:44.940]   So something like this, something like this being a pastable thing, as well as being just
[01:08:44.940 --> 01:08:48.460]   a regular procedure, remember, we wouldn't prevent this from being called as regular procedure.
[01:08:48.460 --> 01:08:51.660]   We just make it pastable.
[01:08:51.660 --> 01:08:54.660]   This would be an idiom that I think you get used to.
[01:08:54.660 --> 01:08:58.460]   It seems weird right now because, you know, you're not used to it, right?
[01:08:58.460 --> 01:09:04.220]   But I think if you're used to it, it may be much preferable to explicitly writing this
[01:09:04.220 --> 01:09:06.300]   at the site every time.
[01:09:06.300 --> 01:09:10.100]   I mean, keep in mind, this bucket array is a pretty simple data structure, which is why
[01:09:10.100 --> 01:09:12.140]   this is all I'm doing.
[01:09:12.140 --> 01:09:16.180]   The more complex data structure we'd have more code here, and you really want to type that
[01:09:16.180 --> 01:09:17.180]   every time.
[01:09:17.180 --> 01:09:21.180]   I don't think so.
[01:09:21.180 --> 01:09:22.620]   Someone says, what is the road map from here?
[01:09:22.620 --> 01:09:25.740]   I know you've announced the game demo, but do you have any language plans after that?
[01:09:25.740 --> 01:09:32.420]   I'm sort of working on the game and making sure that all the features that are needed
[01:09:32.420 --> 01:09:35.060]   to make the game good happen.
[01:09:35.060 --> 01:09:36.060]   And then I don't know.
[01:09:36.060 --> 01:09:41.500]   I mean, there's still compiler bugs that I have to fight with once in a while, and I'll
[01:09:41.500 --> 01:09:43.540]   continue to run into those and squash those.
[01:09:43.540 --> 01:09:49.620]   But at some point soon, the functionality will be kind of stabilizing.
[01:09:49.620 --> 01:09:54.140]   And after it's stable enough for a while, then I'll start letting a close circle of
[01:09:54.140 --> 01:09:58.060]   people use the language and provide it that that works out.
[01:09:58.060 --> 01:09:59.060]   It can expand.
[01:09:59.060 --> 01:10:03.620]   So we'll see.
[01:10:03.620 --> 01:10:06.620]   Is interoperability with C++ a goal?
[01:10:06.620 --> 01:10:11.540]   Ideally, I would like people to use the language with anything and have it be very convenient,
[01:10:11.540 --> 01:10:12.540]   right?
[01:10:12.540 --> 01:10:14.180]   Because it makes it easier to get adoption.
[01:10:14.180 --> 01:10:19.020]   I have no idea how hard it's going to be to talk to C++.
[01:10:19.020 --> 01:10:25.780]   It may be that if there's some low hanging fruit, like, hey, on Windows, we can integrate,
[01:10:25.780 --> 01:10:30.380]   like we happen to know, or we happen to know how the names get mangled by both Clang and
[01:10:30.380 --> 01:10:38.380]   GCC and MSDC, and we can do that and do something with the view tables, I don't know.
[01:10:38.380 --> 01:10:42.740]   There's probably something doable with a subset of C++ that lets you bridge.
[01:10:42.740 --> 01:10:49.300]   But we're never, for example, going to deal with C++ templates in a meaningful way.
[01:10:49.300 --> 01:10:56.300]   And modern C++ is all about using modern C++ is all about using templated crap everywhere,
[01:10:56.300 --> 01:10:58.380]   which makes it impossible to interface with anything.
[01:10:58.380 --> 01:11:02.700]   So good luck there, right?
[01:11:02.700 --> 01:11:03.780]   But we'll see.
[01:11:03.780 --> 01:11:12.960]   It's just been a little bit early for me to think about that in a serious way.
[01:11:12.960 --> 01:11:17.220]   How would the standard library do error types?
[01:11:17.220 --> 01:11:23.660]   You currently seem to use the same error handling as Go without the automatic airfaces.
[01:11:23.660 --> 01:11:27.500]   People complain about Go, but I don't share their complaints, so I don't see what the
[01:11:27.500 --> 01:11:28.500]   problem is.
[01:11:28.500 --> 01:11:36.020]   I think multiple return values are a perfectly fine way of handling errors.
[01:11:36.020 --> 01:11:40.540]   People who say that that's more bothersome than exceptions don't understand the impact
[01:11:40.540 --> 01:11:44.180]   of exceptions in my opinion.
[01:11:44.180 --> 01:11:48.940]   So, you know, it is what it is.
[01:11:48.940 --> 01:11:56.620]   I think, let me put it this way, I don't have any problems in my day-to-day programming
[01:11:56.620 --> 01:11:57.620]   returning errors.
[01:11:57.620 --> 01:12:01.180]   It's just not an issue that I feel needs to be solved.
[01:12:01.180 --> 01:12:05.820]   And that's even in C++ where returning errors is annoying, right?
[01:12:05.820 --> 01:12:14.100]   Once you have multiple return values and it's less annoying, then it's just not -- do we
[01:12:14.100 --> 01:12:16.100]   have any native multi-threading features now?
[01:12:16.100 --> 01:12:19.100]   No, something like that might happen later.
[01:12:19.100 --> 01:12:24.980]   You know, I have some vague ideas in that category, but honestly, high-performance threaded
[01:12:24.980 --> 01:12:28.220]   programming is not my area of expertise.
[01:12:28.220 --> 01:12:37.500]   I've done a bunch of threading stuff, but it's all the relatively regular -- you know,
[01:12:37.500 --> 01:12:41.660]   like, "Hey, I wrote the mixer for the witness in grade," right, and that tends to run in
[01:12:41.660 --> 01:12:48.700]   a separate thread and you pass data back and forth and whatever, but the synchronization
[01:12:48.700 --> 01:12:52.580]   between the two threads is not usually a bottleneck.
[01:12:52.580 --> 01:12:56.780]   Actually, it sort of became one in the witness, but there were some straightforward things
[01:12:56.780 --> 01:12:57.980]   to do to work around it.
[01:12:57.980 --> 01:13:03.020]   But when you start programming language features for threading, you really want to get very
[01:13:03.020 --> 01:13:04.220]   serious about that.
[01:13:04.220 --> 01:13:08.580]   So I think that I will be consulting with some friends when that time comes, but that time
[01:13:08.580 --> 01:13:13.900]   is not yet.
[01:13:13.900 --> 01:13:17.700]   Ignoring the cost of RAIAI and the mental overhead of debugging can't throw in an exception
[01:13:17.700 --> 01:13:23.340]   to yield better performance than returning and checking an error code.
[01:13:23.340 --> 01:13:24.340]   I don't know.
[01:13:24.340 --> 01:13:27.980]   I don't care because my problem with exceptions is not performance.
[01:13:27.980 --> 01:13:30.540]   My problem with exceptions is twofold.
[01:13:30.540 --> 01:13:35.460]   One is they make a program very hard to understand -- actually, threefold.
[01:13:35.460 --> 01:13:39.220]   Two, because the program is very hard to understand people make mistakes all the damn
[01:13:39.220 --> 01:13:47.660]   time, dude, the percentage of C++ code that's actually exception safe is very small.
[01:13:47.660 --> 01:13:51.260]   People have no idea how buggy their code actually is.
[01:13:51.260 --> 01:13:52.980]   There's a paper somewhere.
[01:13:52.980 --> 01:13:58.260]   I forget which one it was, so I can't tell you who wrote it, but they actually analyzed
[01:13:58.260 --> 01:14:01.900]   code for exception safety, and they were just like, "Everybody fails all the time.
[01:14:01.900 --> 01:14:04.620]   You guys are all bozers."
[01:14:04.620 --> 01:14:09.340]   And three, it makes people afraid of their code because it reduces your morale about
[01:14:09.340 --> 01:14:10.340]   what's happening.
[01:14:10.340 --> 01:14:11.340]   Like, is something going to throw?
[01:14:11.340 --> 01:14:12.340]   I don't know.
[01:14:12.340 --> 01:14:13.660]   Do I need to worry about something throwing?
[01:14:13.660 --> 01:14:14.660]   I don't know.
[01:14:14.660 --> 01:14:15.660]   Forget all that.
[01:14:15.660 --> 01:14:16.660]   It's junk.
[01:14:16.660 --> 01:14:22.940]   It brings so much complication, and the amount of benefit is very close to zero.
[01:14:22.940 --> 01:14:27.060]   So who gives a crap about whether it's faster or not?
[01:14:27.060 --> 01:14:32.340]   Like, why do you care about whether your exceptional error path is fast?
[01:14:32.340 --> 01:14:34.980]   That's not your regular thing, right?
[01:14:34.980 --> 01:14:40.020]   If you're throwing exceptions at 120 frames per second in the middle of a video game,
[01:14:40.020 --> 01:14:41.020]   something is broken.
[01:14:41.020 --> 01:14:42.900]   That shouldn't be regular behavior.
[01:14:42.900 --> 01:14:45.300]   So why are you optimizing that?
[01:14:45.300 --> 01:14:48.660]   It doesn't make any sense.
[01:14:48.660 --> 01:14:54.460]   How do I feel about ghost style switch statements?
[01:14:54.460 --> 01:15:08.540]   No opinion, I haven't really used them.
[01:15:08.540 --> 01:15:12.140]   People are saying error handling code gets bigger when you don't have exceptions.
[01:15:12.140 --> 01:15:21.260]   I don't believe that because either you care about some number of conditions or you don't.
[01:15:21.260 --> 01:15:28.300]   The reason exception handling results in smaller code a lot of the time is because people just
[01:15:28.300 --> 01:15:32.140]   catch exceptions and don't give a crap and drop them on the floor.
[01:15:32.140 --> 01:15:38.060]   Like, if you don't actually handle the different cases, then how is that any better, right?
[01:15:38.060 --> 01:15:45.800]   It's worse because you look like you're handling the error, but you're actually not, right?
[01:15:45.800 --> 01:15:48.820]   I'm fully in the multiple returns camp.
[01:15:48.820 --> 01:15:50.420]   I think exceptions are bananas.
[01:15:50.420 --> 01:15:56.980]   They're one of the worst things to happen in programming in the past several decades.
[01:15:56.980 --> 01:16:05.020]   All right, so people are linking me some cool links to research.
[01:16:05.020 --> 01:16:06.660]   I'm going to be done now.
[01:16:06.660 --> 01:16:08.700]   This went on longer than I thought it would go.
[01:16:08.700 --> 01:16:09.700]   Thanks for hanging out.
[01:16:09.700 --> 01:16:13.620]   It went over an hour.
[01:16:13.620 --> 01:16:19.260]   I will do another session next time that I have good stuff to show.
[01:16:19.260 --> 01:16:23.980]   In the meantime, I'm heading out, so goodbye.
[01:16:23.980 --> 01:16:25.020]   Thanks for stopping by.
[01:16:25.020 --> 01:16:35.020]   [BLANK_AUDIO]

