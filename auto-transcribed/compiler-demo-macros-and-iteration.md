# Talk: Compiler Demo: Macros and Iteration

Link: https://www.youtube.com/watch?v=QX46eLqq1ps

Auto-transcribed with Whisper

[00:00:00.000 --> 00:00:04.460]   Let me just talk about one of the motivations for this first before you know
[00:00:04.460 --> 00:00:09.820]   I might have a point in this program where we get to again, but let me just talk about motivation behind this
[00:00:09.820 --> 00:00:15.900]   You know when I started making the language I decided to stay away from macros as much as possible
[00:00:15.900 --> 00:00:18.860]   especially
[00:00:18.860 --> 00:00:24.400]   Sea style preprocessor macros because those don't play very well with debuggers
[00:00:24.400 --> 00:00:27.780]   They're they're messy. They tend to
[00:00:28.620 --> 00:00:33.560]   They tend to cause a lot of problems, and you have to use a lot of weird hacks to work around the problems
[00:00:33.560 --> 00:00:36.440]   and I wanted to
[00:00:36.440 --> 00:00:39.500]   Say okay, how much?
[00:00:39.500 --> 00:00:47.140]   How much power can we make sure that the language has without resorting to any kind of textual overlay right that like that?
[00:00:47.140 --> 00:00:52.020]   And then maybe later if we do a macro system, it'll be a macro system more
[00:00:52.460 --> 00:00:58.820]   Like a language like lisp has where the macros are more something that the programming language understands
[00:00:58.820 --> 00:01:04.200]   As opposed to something like layered on top, right?
[00:01:04.200 --> 00:01:11.180]   Layering is good because it lets you separate the things, but it's bad because the things don't know about each other and
[00:01:11.180 --> 00:01:17.300]   When you're programming you kind of want an integrated experience if you're stepping around through code
[00:01:17.300 --> 00:01:21.420]   You want to be able to step into a macro and then step back out and so forth
[00:01:21.620 --> 00:01:28.300]   So that had been my attitude from the beginning about maybe we'll do macros if we do them
[00:01:28.300 --> 00:01:30.300]   They'll be more like lisp than like see
[00:01:30.300 --> 00:01:35.220]   But we went for a long time without it because it was like let's just see how good we could make the language
[00:01:35.220 --> 00:01:39.740]   Without macros, but then at some point I guess it was a couple years ago now
[00:01:39.740 --> 00:01:44.220]   we have to look we started to do iterators for data structures and
[00:01:44.220 --> 00:01:49.380]   Let's just go ahead and look at this data structure at first
[00:01:49.380 --> 00:01:52.500]   We have whatever stream it was where I introduced iterators
[00:01:52.500 --> 00:01:55.620]   It's on YouTube and I go through this whole thing for like an hour
[00:01:55.620 --> 00:02:00.740]   We're gonna rush through it a little bit at the beginning, but like, you know, here's a data structure
[00:02:00.740 --> 00:02:02.980]   it's called a bucket array and
[00:02:02.980 --> 00:02:12.140]   The way it works is there's just one memory bucket that holds n of whatever particular type of thing you want to put in the array
[00:02:12.140 --> 00:02:17.340]   And we allocate one bucket at a time and we have like an occupancy mask that tells us
[00:02:18.420 --> 00:02:20.420]   which of those things
[00:02:20.420 --> 00:02:26.300]   Which of those slots are in use in which aren't and so you can add new things to the array without
[00:02:26.300 --> 00:02:28.100]   Reallocating the whole array, right?
[00:02:28.100 --> 00:02:34.660]   You can take a pointer to something in the array and you can pass that pointer around and it'll stay stable as the array is modified, right?
[00:02:34.660 --> 00:02:41.660]   But you also just want to iterate over the array and so I said, oh, this will be a good test for iterators. Let's
[00:02:43.220 --> 00:02:50.380]   Make some hooks so that our native for loop knows how to talk to arbitrary data structures like this one, right?
[00:02:50.380 --> 00:02:57.020]   So I started doing that and here's what we ended up with and this is cleaner than C++ iterators would be
[00:02:57.020 --> 00:02:59.300]   but like I
[00:02:59.300 --> 00:03:06.300]   Was not happy with how much code it was so we made a type that's like an iterator that goes over a bucket array, right?
[00:03:06.300 --> 00:03:10.660]   This is a polymorphic type or what you would call a generic and C++ because
[00:03:11.100 --> 00:03:17.500]   You know, it's got an argument. It's like the type of the thing and then we have some functions and the way it works is that the for loop
[00:03:17.500 --> 00:03:24.540]   Knows to look for these functions if you're if you're doing a for on a type. That's not a number, right?
[00:03:24.540 --> 00:03:27.140]   So they calls iterator make at the start
[00:03:27.140 --> 00:03:35.300]   Which sets an iterator to the beginning it calls iterator next and iterator get value and then if you call remove
[00:03:37.020 --> 00:03:43.260]   On the loop then this is how you define what should happen in the array and this all worked
[00:03:43.260 --> 00:03:49.900]   We use this in production on the game for like two years, but I was really unhappy with it because here's the thing
[00:03:49.900 --> 00:03:55.140]   All I was trying to do
[00:03:55.140 --> 00:04:01.500]   In reality anytime I wanted to iterate over a bucket array is actually this following thing
[00:04:02.580 --> 00:04:09.260]   It's a two-level iteration. There's buckets right on the array and then each bucket has some number of slots
[00:04:09.260 --> 00:04:13.020]   And if that slots not occupied then skip otherwise just put
[00:04:13.020 --> 00:04:16.460]   Just put whatever code you would do here, right?
[00:04:16.460 --> 00:04:22.980]   That is all I wanted to do every time and so in some sense
[00:04:22.980 --> 00:04:29.340]   The job of a custom iterator for this data type is just to encapsulate this knowledge
[00:04:29.340 --> 00:04:33.220]   So that we don't have to remember how it works and type it out all the time and maybe get it wrong
[00:04:33.220 --> 00:04:39.460]   The knowledge that it's actually a 2d iteration and you've got to check the occupied mask, right?
[00:04:39.460 --> 00:04:43.300]   Now how does this
[00:04:43.300 --> 00:04:48.580]   Turn into this or even more code if it was in C++
[00:04:48.580 --> 00:04:54.740]   So I was I was unhappy with that and so I've been thinking in the back of my mind. You know if we have a macro system
[00:04:56.140 --> 00:05:01.980]   One thing I want to do with the macro system is just write a macro that says here's how you iterate this thing it looks like this
[00:05:01.980 --> 00:05:10.020]   Right and then your code just goes in there and instead of having this giant thing with an extra data type
[00:05:10.020 --> 00:05:12.300]   By the way, it's in addition to being more code
[00:05:12.300 --> 00:05:18.620]   This is also slower because we have this extra data type representing an iterator and you have to hope that that gets optimized out
[00:05:18.620 --> 00:05:21.660]   Which it probably won't in many circumstances
[00:05:24.540 --> 00:05:26.540]   Yeah, it's a lot
[00:05:26.540 --> 00:05:30.780]   Okay, so
[00:05:30.780 --> 00:05:33.820]   You know, this is what we actually want to do
[00:05:33.820 --> 00:05:38.780]   That's all and so I I didn't I
[00:05:38.780 --> 00:05:44.220]   Decided some time ago that this scheme of iterators would be unacceptable in the long term
[00:05:44.220 --> 00:05:49.660]   But that we would just work with it for a while. So we did wow everybody's loud down there
[00:05:49.660 --> 00:05:52.740]   I might have to close the window
[00:05:53.740 --> 00:05:56.340]   This is what I get for living in San Francisco
[00:05:56.340 --> 00:06:01.420]   So
[00:06:01.420 --> 00:06:06.500]   So we lived with this this way for a while, but because this was such a headache
[00:06:06.500 --> 00:06:09.100]   I didn't even make iterators for several data structures
[00:06:09.100 --> 00:06:15.700]   So there's a hash table that I made which was just a one-dimensional iteration and where you would just check the hash
[00:06:15.700 --> 00:06:22.700]   and hash value of zero or one means unoccupied and we would just do that manually every time because I just didn't want to do all this
[00:06:22.700 --> 00:06:23.820]   stuff
[00:06:23.820 --> 00:06:28.100]   All right, so that's the motivation the motivation well among other things of course
[00:06:28.100 --> 00:06:31.300]   you want to be able to use macros for a lot of different purposes, but
[00:06:31.300 --> 00:06:34.140]   this was a
[00:06:34.140 --> 00:06:42.020]   A concrete application that helped me understand whether I was meeting the goals or not, which is can we just do this?
[00:06:42.020 --> 00:06:44.780]   That's all we want to do
[00:06:44.780 --> 00:06:50.780]   We'll talk about the subtleties. It's not as easy as you might think there are subtleties involved here, okay
[00:06:52.460 --> 00:06:58.500]   Now leading into talking about macro features, we're gonna actually not do a macro feature
[00:06:58.500 --> 00:07:02.660]   This is a for loop feature, but it helps us plug into iterators later, right?
[00:07:02.660 --> 00:07:08.300]   So as people have been following this programming language. No, we have a very
[00:07:08.300 --> 00:07:13.860]   syntactically terse for loop, right? So here you've got an array of three strings called fruits
[00:07:13.860 --> 00:07:19.420]   It's apple banana and cherry right ABC fruit and we iterate over that
[00:07:19.980 --> 00:07:26.300]   And we're just printing one and then the item right and so that's all you have to do to iterate over loop and do something
[00:07:26.300 --> 00:07:29.540]   It's much less verbose than it is in some other programming language
[00:07:29.540 --> 00:07:36.300]   Languages so if I run this let's just go back up to the beginning
[00:07:36.300 --> 00:07:44.140]   We're just going one apple one banana one cherry right and so that's what this is doing and
[00:07:44.900 --> 00:07:50.220]   We have for a very long time had a syntactic way to reverse iteration on the loop
[00:07:50.220 --> 00:07:55.020]   Right, which is just by putting this little less than in there and then you go the other way
[00:07:55.020 --> 00:07:57.580]   So for two we go cherry banana apple, right?
[00:07:57.580 --> 00:08:00.260]   the problem with that is
[00:08:00.260 --> 00:08:04.700]   It's only something that you can like type
[00:08:04.700 --> 00:08:11.700]   Which means it's a syntactic feature of your program, which means you can't like parameterize it
[00:08:11.780 --> 00:08:16.600]   You can't have a parameter to a function that says if you want to go backwards or not without
[00:08:16.600 --> 00:08:23.500]   Typing the loop twice and say if backwards do this one otherwise do this one and so there's always been a little bit of a disconnect there
[00:08:23.500 --> 00:08:26.580]   So we added this feature where you can say
[00:08:26.580 --> 00:08:35.780]   Well, we're reversing or not right you can say less than equal and then that parameter right
[00:08:35.780 --> 00:08:40.100]   As opposed to just the less than here and and so here
[00:08:40.820 --> 00:08:45.900]   Fruits is going to go forward again if I recompiled with this set to true it would go backward
[00:08:45.900 --> 00:08:50.820]   But we have a better example right here, which is here. We have a function called print animals
[00:08:50.820 --> 00:08:52.820]   We've changed animals instead of fruits
[00:08:52.820 --> 00:09:01.500]   We put a prefix in front and then a compile time parameter that tells us whether we reverse or not
[00:09:01.500 --> 00:09:05.660]   Right, so this dollar sign means again when you call this with a constant
[00:09:05.740 --> 00:09:11.900]   We bake the constant into the procedure and generate a separate procedure for each value of this parameter right?
[00:09:11.900 --> 00:09:16.100]   So here we're going to print animals not reversing
[00:09:16.100 --> 00:09:22.740]   So we're just going to go ABC with hello as the prefix and here we're going to put sailor is the prefix and
[00:09:22.740 --> 00:09:26.900]   We're going to do it in reverse order, right? So
[00:09:26.900 --> 00:09:31.220]   Hello ABC sailor CB antelope
[00:09:31.420 --> 00:09:38.700]   So that is an example of using this, right now. This isn't a macro yet. This is just a regular procedure
[00:09:38.700 --> 00:09:43.140]   Okay
[00:09:43.140 --> 00:09:51.060]   Now we also had this thing on for loop that's about whether you want to take the pointer to each element or not which is very useful
[00:09:51.060 --> 00:09:54.300]   You know for
[00:09:54.300 --> 00:10:00.300]   If you want to be data oriented about things you want to store things by value in memory as opposed to what a lot of
[00:10:00.860 --> 00:10:01.940]   modern
[00:10:01.940 --> 00:10:06.620]   Quote modern unquote languages encourage you to do which is to have them separately heap allocated
[00:10:06.620 --> 00:10:09.980]   And so you very often want to have a raise packed full of data
[00:10:09.980 --> 00:10:15.500]   And then you want to iterate over independent things and maybe you want to modify those things or maybe you want to pass them somewhere else
[00:10:15.500 --> 00:10:17.500]   It's convenient to have a pointer. So
[00:10:17.500 --> 00:10:21.820]   You can do the same thing, you know the syntax
[00:10:21.820 --> 00:10:25.700]   Before I didn't have an example of it, but you would you would say
[00:10:26.420 --> 00:10:32.500]   star to say by pointer and now you can say false or true. So we've got the numbers one two or three
[00:10:32.500 --> 00:10:38.460]   We're iterating them and here we're iterating the values and here we're iterating the pointers to those values
[00:10:38.460 --> 00:10:43.060]   All right, and that's controllable by a parameter in the same way
[00:10:43.060 --> 00:10:48.540]   And so then we've got this function that does both we could say it's a for loop
[00:10:48.540 --> 00:10:55.100]   Maybe it's by pointer or not. Maybe it's reversed or not iterating over these computers and printing
[00:10:56.100 --> 00:10:59.060]   The prefix and then
[00:10:59.060 --> 00:11:04.700]   Whatever the value is of the thing that we're iterating right and so
[00:11:04.700 --> 00:11:14.140]   Here I've made the prefix be the values of variables for F is for false in the first slot for not by pointer and the second slot
[00:11:14.140 --> 00:11:18.260]   It's like not reversed right and so here we're printing the computers
[00:11:18.260 --> 00:11:24.940]   Not by pointer not reversed right not by pointers in reversed by pointers not reversed
[00:11:25.420 --> 00:11:29.780]   by pointers reversed now these numbers are different from these numbers because we actually
[00:11:29.780 --> 00:11:38.260]   Generated this procedure twice and so we generated this array twice and whereas we deduplicate individual strings right now
[00:11:38.260 --> 00:11:40.260]   We don't deduplicate the actual
[00:11:40.260 --> 00:11:45.700]   Array pointing to those strings right now. I don't know if that would ever make sense to do
[00:11:45.700 --> 00:11:50.780]   But so those have different addresses because it's two instances of function
[00:11:53.300 --> 00:11:55.300]   Anyway, so we buffed up the for loop
[00:11:55.300 --> 00:12:01.300]   Now we're gonna start talking about macros and then how that iterates with how that
[00:12:01.300 --> 00:12:07.140]   Interacts with macros
[00:12:07.140 --> 00:12:09.460]   hmm, all right
[00:12:09.460 --> 00:12:13.540]   Someone is saying the syntax looks really awful look it's not final
[00:12:13.540 --> 00:12:16.660]   and it's one of these syntaxes that
[00:12:16.660 --> 00:12:21.900]   It's actually nice when it's like this. It's very nice when it's like this
[00:12:21.900 --> 00:12:26.940]   I agree that it looks a little messier when it's like this, but you don't really do this very often, right?
[00:12:26.940 --> 00:12:29.300]   You almost never do this actually so
[00:12:29.300 --> 00:12:34.660]   It's okay if things are less nice and you do them less often in my opinion
[00:12:34.660 --> 00:12:38.060]   But yeah, the whole syntax of the whole language may change at a later date
[00:12:38.060 --> 00:12:44.180]   Okay, so let's start talking about
[00:12:46.300 --> 00:12:55.020]   Another motivation for macros actually so the first motivation was to be able to iterate very cleanly another motivation was
[00:12:55.020 --> 00:13:01.700]   Because constructors and destructors were not working out as I mentioned in the previous demo. We took them out of the language
[00:13:01.700 --> 00:13:06.660]   The
[00:13:06.660 --> 00:13:13.260]   One reason why it felt okay to take them out was because I wasn't using them very often and the places where I was using them
[00:13:14.740 --> 00:13:17.820]   We were really using them for destructor functionality
[00:13:17.820 --> 00:13:22.220]   Where the destructor was just a way to call something at the end of the scope?
[00:13:22.220 --> 00:13:29.780]   And it's like well if you could just insert something into your scope that had a defer statement, which runs at the same time as a destructor
[00:13:29.780 --> 00:13:32.260]   then
[00:13:32.260 --> 00:13:33.660]   you could
[00:13:33.660 --> 00:13:39.340]   Get by without destructors and still have that clean way of doing things and so here's a second motivation
[00:13:41.540 --> 00:13:46.700]   For example, say you want to have a scoped lock that like grabs a mutex at the start of a scope
[00:13:46.700 --> 00:13:52.420]   And then you might exit out of that scope in many different places and no matter where you exit out it'll
[00:13:52.420 --> 00:13:58.700]   Release the mutex, right? So here's an example. We say hey
[00:13:58.700 --> 00:14:04.100]   I'm entering this example. We open up a little scope in the middle here. We say lock this mutex
[00:14:04.100 --> 00:14:10.180]   Do stuff do some more stuff and then at the end of the scope will release the mutex, right?
[00:14:10.980 --> 00:14:14.380]   So a scoped lock here is actually a macro. It's
[00:14:14.380 --> 00:14:21.180]   It looks like a regular function in every way, but it has this word expand here. That's the thing that's different
[00:14:21.180 --> 00:14:27.220]   Up here for a mutex. We're just put we've got a dummy struct. Just like pretend it's a mutex
[00:14:27.220 --> 00:14:32.100]   It's just when we lock we print this and when we unlock we print that
[00:14:32.100 --> 00:14:34.620]   so
[00:14:36.220 --> 00:14:42.300]   Well, what is this example print, right? So we say we're entering the example. We say locking we're doing our stuff
[00:14:42.300 --> 00:14:44.660]   We're doing more stuff. We're unlocking at the end of the block
[00:14:44.660 --> 00:14:47.460]   Actually, let me put this over here and over
[00:14:47.460 --> 00:14:56.180]   Here so you can see these right? So we're doing more stuff. We're doing more stuff and then we do unlock because the scope closes
[00:14:56.180 --> 00:14:58.820]   right and then we exit the example so
[00:14:58.820 --> 00:15:05.180]   This lets us do the same thing as a destructor would have now. Why does this work?
[00:15:05.180 --> 00:15:10.780]   Well, it's because when you put expand here it turns a regular function into a macro and
[00:15:10.780 --> 00:15:18.980]   It being a macro just means that these statements get inserted at the calling site
[00:15:18.980 --> 00:15:24.260]   Like you might imagine as opposed to being living in a separate body somewhere that you call
[00:15:24.260 --> 00:15:38.760]   Okay, that's a very straightforward example that doesn't really
[00:15:38.760 --> 00:15:47.540]   Tell you anything yet because there's many different things that this could mean like a sea style macro
[00:15:47.540 --> 00:15:51.060]   Well, see doesn't really have defer. I don't think still maybe it does now
[00:15:51.900 --> 00:15:56.540]   So we'll start getting into details of what's going on
[00:15:56.540 --> 00:15:59.940]   So firstly, these are hygienic macros
[00:15:59.940 --> 00:16:03.060]   just like in Lisp so
[00:16:03.060 --> 00:16:07.940]   Here we've got an example of a value
[00:16:07.940 --> 00:16:13.220]   What do let me just say what hygienic macro means hygienic macro means
[00:16:13.220 --> 00:16:16.820]   That just as if this was a function, right?
[00:16:16.900 --> 00:16:20.820]   It's got its own namespace with its own names for variables and stuff
[00:16:20.820 --> 00:16:25.540]   And those are lexically determined by the scope of this and when you use the macro
[00:16:25.540 --> 00:16:29.780]   You can't see these functions and you or see these names and you don't have to worry about the names
[00:16:29.780 --> 00:16:35.900]   You don't have to worry about using things of the same name accidentally as the other thing, right?
[00:16:35.900 --> 00:16:42.300]   So
[00:16:42.300 --> 00:16:44.300]   Yeah
[00:16:44.300 --> 00:16:51.260]   Um, yeah
[00:16:51.260 --> 00:16:57.100]   Yeah, one thing at a time, I don't want to get ahead of myself
[00:16:57.100 --> 00:16:59.820]   So here we've got a macro
[00:16:59.820 --> 00:17:04.380]   And it's there's not really a good reason for it to be a macro in this case
[00:17:04.380 --> 00:17:07.820]   We're just showcasing the fact that our identifiers don't collide
[00:17:08.060 --> 00:17:13.260]   So we take a string, we concatenate it three times with commas in between an exclamation point
[00:17:13.260 --> 00:17:15.260]   And then we print that
[00:17:15.260 --> 00:17:18.540]   You know, we could have done that all in one print
[00:17:18.540 --> 00:17:21.180]   But of course we wouldn't have had a temporary variable called value
[00:17:21.180 --> 00:17:26.700]   So we call macro on developers and of course we get developers developers developers, right?
[00:17:26.700 --> 00:17:31.900]   Um, but here in this scope, we've got a thing called value and
[00:17:34.620 --> 00:17:39.420]   It's integer, right? It's a sum of integers and we print the it's the sum of integers, right?
[00:17:39.420 --> 00:17:47.420]   So if this were behaving like a C macro and this was just directly inserted into the scope naively
[00:17:47.420 --> 00:17:51.260]   We get a redeclared identifier down here because this value
[00:17:51.260 --> 00:17:55.260]   You know is
[00:17:55.260 --> 00:18:00.620]   They would be a redecoration of this but that doesn't happen and you don't have to worry about it
[00:18:01.100 --> 00:18:06.060]   So you don't have to name the parameters of your macro funny names or you don't have to gen
[00:18:06.060 --> 00:18:08.060]   Same things or anything like that
[00:18:08.060 --> 00:18:11.340]   Okay
[00:18:11.340 --> 00:18:13.340]   Now implementation wise
[00:18:13.340 --> 00:18:19.820]   Macros work just like procedures actually because they are procedures, okay as far as the programming language is concerned
[00:18:19.820 --> 00:18:27.980]   Um, a macro and a procedure are the same thing until the time comes to generate the code for the procedure call
[00:18:27.980 --> 00:18:30.780]   um
[00:18:30.780 --> 00:18:32.220]   well
[00:18:32.220 --> 00:18:33.660]   Approximately
[00:18:33.660 --> 00:18:36.380]   There are some syntax tree things that happen that are a little different
[00:18:36.380 --> 00:18:39.660]   But in terms of the way that they interact with the rest of the compiler
[00:18:39.660 --> 00:18:41.180]   They're actually the same thing
[00:18:41.180 --> 00:18:43.020]   And that means that all of the
[00:18:43.020 --> 00:18:48.380]   Features that we have for functions also work for macros just straight out of the box, right?
[00:18:48.380 --> 00:18:50.940]   So you get type checking on the arguments
[00:18:50.940 --> 00:18:57.740]   You don't know need to know where the macros are in your program. Just like functions. There's no declaration order constraints, right?
[00:18:58.220 --> 00:19:03.340]   Um, you can overload macros with other macros or with functions again because they're just functions
[00:19:03.340 --> 00:19:05.740]   They could be locally scoped
[00:19:05.740 --> 00:19:08.140]   Um, we'll show a couple more features in a minute
[00:19:08.140 --> 00:19:12.540]   Anyway, so here's an example
[00:19:12.540 --> 00:19:16.380]   We have two versions of foo. These are both macros because they're marked expand
[00:19:16.380 --> 00:19:18.620]   This one is not a macro
[00:19:18.620 --> 00:19:22.060]   Uh, it can't be a macro actually because it's recursive it calls itself
[00:19:22.060 --> 00:19:24.620]   Right
[00:19:24.620 --> 00:19:27.660]   So then we have this thing we're going to say foo of high
[00:19:27.660 --> 00:19:30.460]   That's going to call this overload
[00:19:30.460 --> 00:19:36.060]   Right, we're going to say foo of 2.5 3.3, which is obviously going to call this overload
[00:19:36.060 --> 00:19:40.220]   And then we're going to say foo factorial which has to call this one
[00:19:40.220 --> 00:19:43.580]   And uh, oh wait
[00:19:43.580 --> 00:19:48.300]   Foo factorial is foo of six, right? So that foo is going to be this foo and we print that out
[00:19:48.300 --> 00:19:50.780]   So let's just get through that first of all, right?
[00:19:50.780 --> 00:19:53.020]   um
[00:19:54.060 --> 00:19:56.780]   Boom macro foo of string was given high
[00:19:56.780 --> 00:20:03.020]   Macro two floats was given these floats the sum is this right foo factorial is uh, 720
[00:20:03.020 --> 00:20:08.060]   Um, okay
[00:20:08.060 --> 00:20:12.460]   So now we have a fourth version of foo that's defined
[00:20:12.460 --> 00:20:17.580]   Let's even find where it is. It says the bottom of the file is actually not at the bottom of the file now
[00:20:17.580 --> 00:20:18.940]   Okay, it's down here
[00:20:18.940 --> 00:20:20.540]   It's way down here
[00:20:20.540 --> 00:20:26.780]   Again, just to show that there's no ordering scope and constraints. It's a variable integer argument version
[00:20:26.780 --> 00:20:29.580]   um
[00:20:29.580 --> 00:20:36.700]   That is a macro again, right and uh, it just sums the arguments, right?
[00:20:36.700 --> 00:20:43.020]   So it's just we're just going through the versatility of all these uh, function things. Here's a locally scoped one
[00:20:43.020 --> 00:20:45.500]   um
[00:20:45.500 --> 00:20:47.340]   We can do that, right? So where are we?
[00:20:47.820 --> 00:20:50.300]   uh, the Verargs one the locally scoped one
[00:20:50.300 --> 00:20:56.060]   And just like functions you can bake these because again, they're just functions. So here
[00:20:56.060 --> 00:21:00.460]   We're going to bake so for the functional language people in the audience
[00:21:00.460 --> 00:21:03.900]   Baking is a compile time currying
[00:21:03.900 --> 00:21:07.260]   Where you take the old version of this function
[00:21:07.260 --> 00:21:13.180]   And you generate a new version of the function with this value substituted and then all compile time
[00:21:13.580 --> 00:21:18.300]   Optimizations are applied because this entire function is computed at compile time, right?
[00:21:18.300 --> 00:21:24.620]   Not that it matters much for this function, but we're just going to show we're baking a version of locally scoped macro
[00:21:24.620 --> 00:21:29.900]   Uh, it gives us a new macro where 69 105 is hard-coded as this parameter
[00:21:29.900 --> 00:21:33.580]   We're actually going to use this later in this demo if this seems like a
[00:21:33.580 --> 00:21:37.340]   Like a weird feature and you're like what what's that for? You'll see
[00:21:37.340 --> 00:21:40.140]   um
[00:21:40.300 --> 00:21:46.700]   But anyway, so then we call baked with no parameters because it used to have one and we baked that parameter and now it has zero parameters
[00:21:46.700 --> 00:21:48.540]   um
[00:21:48.540 --> 00:21:49.980]   Okay
[00:21:49.980 --> 00:21:53.980]   Named arguments just like functions. So here's a macro a and b
[00:21:53.980 --> 00:21:56.540]   Uh, it takes a it takes b
[00:21:56.540 --> 00:21:57.740]   um
[00:21:57.740 --> 00:21:59.260]   Uh, oh
[00:21:59.260 --> 00:22:02.060]   I jumped ahead so we're going to have to explain this feature
[00:22:02.060 --> 00:22:06.860]   Um
[00:22:06.860 --> 00:22:11.260]   Oops, this is what happens when I rearrange the order in which I demo things a few times
[00:22:11.260 --> 00:22:13.900]   Let's ignore that feature for a second
[00:22:13.900 --> 00:22:15.740]   Um
[00:22:15.740 --> 00:22:23.260]   So we have a and b it takes two strings and we put them together with some join variable. Well, I have to explain so
[00:22:23.260 --> 00:22:26.540]   It's a hygienic macro. So it doesn't
[00:22:26.540 --> 00:22:32.060]   Have access to your variables or you don't have to worry about namespaces colliding
[00:22:32.060 --> 00:22:36.460]   Unless you want them to right so the back tick in front of join says
[00:22:37.260 --> 00:22:39.260]   actually
[00:22:39.260 --> 00:22:43.420]   Let's refer to this variable from the calling scope, right
[00:22:43.420 --> 00:22:51.580]   Of of where we're inserted. So here we're making a thing called join. It's got like a spaceship b or whatever
[00:22:51.580 --> 00:22:53.500]   Um
[00:22:53.500 --> 00:23:00.940]   And we're calling a and b with b is sailor and a is hello, right? We're using named arguments. So we're using
[00:23:00.940 --> 00:23:04.220]   just all this
[00:23:05.020 --> 00:23:06.460]   uh
[00:23:06.460 --> 00:23:08.460]   All this
[00:23:08.460 --> 00:23:14.380]   Tooling that we have available for calling functions and it works just fine on macros the same as functions, right?
[00:23:14.380 --> 00:23:15.820]   So the
[00:23:15.820 --> 00:23:22.380]   This wouldn't work on a function. You would have to pass join in somehow, but it works here, right?
[00:23:22.380 --> 00:23:25.420]   um
[00:23:25.420 --> 00:23:29.980]   Well, you could almost imagine it working because this is lexically scoped here
[00:23:30.460 --> 00:23:35.740]   But like if we put it outside this function, then it would surely have no way to see this
[00:23:35.740 --> 00:23:39.500]   Um, and yet it would still work because it's a macro that gets pasted in
[00:23:39.500 --> 00:23:47.500]   Uh, varroggs. We already did varroggs. I didn't notice that I have two varroggs demos. Anyway, so here's a varroggs
[00:23:47.500 --> 00:23:50.540]   That puts a defer
[00:23:50.540 --> 00:23:53.580]   Which is again gives it a reason to be a macro. It's
[00:23:53.580 --> 00:23:57.660]   Defers and for all the values prints them
[00:23:58.860 --> 00:24:05.020]   And so we're doing dimwit first and then flathead even though the fall the calls are in the reverse because defer
[00:24:05.020 --> 00:24:11.180]   Executes in stack order, which is what you want for destroying and closing things, right?
[00:24:11.180 --> 00:24:14.700]   So, uh, we do dimwit first and then we do flathead
[00:24:14.700 --> 00:24:21.820]   Okay, so this is where I was efficiently really going to explain backticks
[00:24:21.820 --> 00:24:24.380]   um
[00:24:24.940 --> 00:24:29.820]   So here I've got a value. I've got a local variable. It's called value. It's bound to the string local
[00:24:29.820 --> 00:24:32.940]   I've got a macro called back tick macro
[00:24:32.940 --> 00:24:37.820]   And back tick macro says I'm able to access value. It is whatever, right?
[00:24:37.820 --> 00:24:38.700]   So
[00:24:38.700 --> 00:24:42.620]   We have a back tick in front of this even though this doesn't have anything called value in its scope
[00:24:42.620 --> 00:24:48.540]   It's able to look up to the scope of the person applying it now. Notice this time. It's not in function scope either
[00:24:48.540 --> 00:24:51.020]   It's like at global scope. So
[00:24:51.660 --> 00:24:58.300]   You know really the only way that we're able to see this is because this is a macro that's being inserted directly into the syntax tree, right?
[00:24:58.300 --> 00:25:01.180]   um
[00:25:01.180 --> 00:25:03.980]   Okay, where where are we?
[00:25:03.980 --> 00:25:11.740]   I am able to access value. Here we go. I'm able to access value. It is local
[00:25:11.740 --> 00:25:14.540]   um
[00:25:19.420 --> 00:25:20.940]   So now
[00:25:20.940 --> 00:25:27.100]   We can use have this back tick macro and it'll look at both values. So here I have a value called value
[00:25:27.100 --> 00:25:30.140]   Bound to the macro meaning the macro scope
[00:25:30.140 --> 00:25:36.460]   I've got another version of value out here still called local, right? And so here we're going to enter this macro
[00:25:36.460 --> 00:25:41.020]   Um, we're going to look at
[00:25:41.020 --> 00:25:43.820]   um
[00:25:43.820 --> 00:25:47.980]   This is just a label, right? So we're calling this with the label first or second
[00:25:48.380 --> 00:25:52.780]   So we're going to say we entered first or entered second and we're going to say what the value is
[00:25:52.780 --> 00:25:55.580]   And this is just our value, right?
[00:25:55.580 --> 00:25:59.740]   But then we're going to say back ticked value and we're going to access the one from the enclosing scope
[00:25:59.740 --> 00:26:05.580]   And then to give this a reason to be a macro again. We're going to use a defer at the end. Um, so
[00:26:05.580 --> 00:26:08.300]   Uh, here we go. We entered first
[00:26:08.300 --> 00:26:10.460]   Value is macro
[00:26:10.460 --> 00:26:16.300]   Back ticked value is local entered second value is macro back ticked value is local and then we defer closed the second
[00:26:17.100 --> 00:26:19.100]   Right
[00:26:19.100 --> 00:26:21.180]   And to close the first
[00:26:21.180 --> 00:26:22.700]   Okay
[00:26:22.700 --> 00:26:27.660]   Um, are there any questions so far very on topic questions? I've been glancing at chat
[00:26:27.660 --> 00:26:31.660]   I've seen very off topic things go by but like questions about
[00:26:31.660 --> 00:26:34.700]   How this works before we go on?
[00:26:34.700 --> 00:26:39.500]   Because these are like basics and i'm speeding through it. I'm sort of assuming you know what a macro is
[00:26:39.500 --> 00:26:42.860]   I'm assuming you know the pitfalls of c macros and
[00:26:42.860 --> 00:26:46.300]   Why it might be good to do things this way and so forth
[00:26:47.180 --> 00:26:50.300]   Can you use macro and macro? Yes, we will have an example of that later
[00:26:50.300 --> 00:26:57.260]   Can you apply expand at the call sites? Um
[00:26:57.260 --> 00:27:04.940]   There's not really a reason to do that expand at a call site would be the same as in line
[00:27:04.940 --> 00:27:07.500]   because
[00:27:07.500 --> 00:27:13.260]   If the macro isn't if the body of the macro isn't written knowing that it's a macro, there's like no reason
[00:27:13.980 --> 00:27:17.340]   Can macros have polymorphic arguments? Yes, I believe we have an example of that later
[00:27:17.340 --> 00:27:22.140]   How does the macro back tick feature differ from a capturing lambda?
[00:27:22.140 --> 00:27:26.300]   Uh, it is different because we're not making a separate function
[00:27:26.300 --> 00:27:28.540]   and
[00:27:28.540 --> 00:27:30.620]   That has a number of
[00:27:30.620 --> 00:27:34.860]   uh ramifications one you don't have to worry about getting optimized out two
[00:27:34.860 --> 00:27:36.460]   um
[00:27:36.460 --> 00:27:37.900]   you can
[00:27:37.900 --> 00:27:40.780]   Use little snippets of code and pass them to macros
[00:27:41.260 --> 00:27:45.820]   Uh in ways that are less bulky than you would if you had to make a lambda. We'll show that later
[00:27:45.820 --> 00:27:49.020]   um
[00:27:49.020 --> 00:27:57.180]   If the macro has its own scope, how does it differ happen on the caller? Well, it's a little bit magical, right? So what happens is
[00:27:57.180 --> 00:28:00.460]   There's a scope
[00:28:00.460 --> 00:28:04.220]   The scope of the macro contains the macros own names
[00:28:04.220 --> 00:28:09.820]   but it's transparent to certain kinds of things defers is one of them and
[00:28:10.620 --> 00:28:13.420]   We're going to go into another one of them in a minute. So
[00:28:13.420 --> 00:28:18.380]   It's not a naive separate scope. Let's put it that way
[00:28:18.380 --> 00:28:32.700]   All right, there's a lot of questions
[00:28:32.700 --> 00:28:33.820]   um
[00:28:33.820 --> 00:28:39.580]   I'm just scanning really fast for anything that might like a lot of these questions are about things I haven't covered yet
[00:28:39.580 --> 00:28:41.580]   So I'm going to skip those
[00:28:41.580 --> 00:28:45.100]   Um
[00:28:45.100 --> 00:28:51.100]   Can you bake with a reference to a local any parameter to bake has to be a constant? That's the way it works
[00:28:51.100 --> 00:28:53.740]   So if it evaluates to a constant
[00:28:53.740 --> 00:28:56.300]   Sure, otherwise no
[00:29:07.900 --> 00:29:12.300]   Doesn't the back tick feature make it unnecessarily complex wouldn't argument make the code clearer
[00:29:12.300 --> 00:29:20.220]   Well, if you can write something as a function write it as a function sure this the reason the back tick exists is to do things
[00:29:20.220 --> 00:29:22.860]   Okay
[00:29:22.860 --> 00:29:25.820]   The point of a macro
[00:29:25.820 --> 00:29:32.860]   Well, let's go back one step further the point of a strongly structured programming language that gives you functions
[00:29:32.860 --> 00:29:36.300]   with very well defined lexical scope
[00:29:37.500 --> 00:29:44.540]   Is that that's a very good way to organize programs generally, right? It cuts down on mistakes
[00:29:44.540 --> 00:29:47.180]   If I use a variable
[00:29:47.180 --> 00:29:51.580]   We don't just pretend it's empty string. We say it's undefined. Why because that cuts down on mistakes
[00:29:51.580 --> 00:29:58.060]   Um, it helps you produce code that is not very error prone. Okay, however
[00:29:58.060 --> 00:30:01.900]   Sometimes you just need a way to get out of that structure
[00:30:01.900 --> 00:30:06.860]   Because it's much more efficient to do something outside that structure or it's much more
[00:30:07.420 --> 00:30:09.420]   Um
[00:30:09.420 --> 00:30:15.900]   Much more understandable even right that's why macros exist in c is because sometimes you just got to do stuff that's not like
[00:30:15.900 --> 00:30:21.740]   Functions right and that's why macros exist here. So if you're asking why all this back tick stuff
[00:30:21.740 --> 00:30:28.140]   It's because it lets you do things that are outside of that normal system of strict lexical
[00:30:28.140 --> 00:30:34.060]   Scoping and that's useful and we're going to show in the rest of this some of the ways in which that's useful, right?
[00:30:34.060 --> 00:30:41.500]   Okay, a lot of this is stuff that we're going to cover later
[00:30:41.500 --> 00:30:45.580]   Um
[00:30:45.580 --> 00:30:47.900]   Can a macro return another macro?
[00:30:47.900 --> 00:30:55.020]   Yes, although I hadn't thought to demo that but yes, they're just they're literally just functions
[00:30:55.020 --> 00:30:59.260]   Um, although you would have to apply it at compile time
[00:30:59.260 --> 00:31:02.860]   Otherwise, it's just a macro, right? Okay
[00:31:02.940 --> 00:31:04.940]   Okay
[00:31:04.940 --> 00:31:07.500]   Let's move on
[00:31:07.500 --> 00:31:10.860]   And we'll do some more questions in a little bit after we've covered some more material
[00:31:10.860 --> 00:31:16.380]   Okay, now you can also use the back tick to define things in the outer scope
[00:31:16.380 --> 00:31:20.540]   So just like those defers, we're able to affect the scope of the collar, right?
[00:31:20.540 --> 00:31:23.500]   You can declare variables in the scope of this collar as well
[00:31:23.500 --> 00:31:27.580]   So here we've got this thing called definer. It's a macro. It takes a value
[00:31:28.380 --> 00:31:33.100]   We define a variable called negation in the outer scope that is minus that value
[00:31:33.100 --> 00:31:36.700]   And then then we just do some other stuff. We're going to square it and print something, right?
[00:31:36.700 --> 00:31:39.260]   So we're calling definer on 42
[00:31:39.260 --> 00:31:46.620]   And then we're also just printing negation to show that we have a local variable defined called negation, all right?
[00:31:46.620 --> 00:31:49.340]   so, um
[00:31:49.340 --> 00:31:56.780]   Where are we in the output value is 42 at squares 1764 negation is minus 42, right?
[00:31:57.180 --> 00:32:00.540]   So by using this back tick, we're able to define something in this outer scope
[00:32:00.540 --> 00:32:05.260]   So what these are as I go through the things that you do with back tick and some of this other stuff
[00:32:05.260 --> 00:32:07.660]   These are more structured ways
[00:32:07.660 --> 00:32:11.820]   Of doing things that you would do with c macros
[00:32:11.820 --> 00:32:17.580]   Like very often you might make a macro that like defines a variable because you just need that to do something, right?
[00:32:17.580 --> 00:32:19.740]   This is a way to do that
[00:32:19.740 --> 00:32:20.860]   where
[00:32:20.860 --> 00:32:23.100]   It's not just a textual insertion
[00:32:23.420 --> 00:32:26.220]   So you could step through this in the debugger for example, right?
[00:32:26.220 --> 00:32:30.700]   If you step into definer in the debugger, you'll go here, right?
[00:32:30.700 --> 00:32:35.340]   And if you're here and you say where's the declaration of that variable? It'll know that it's here
[00:32:35.340 --> 00:32:38.940]   Right, that's the benefit of doing things this way
[00:32:38.940 --> 00:32:45.900]   As opposed to just like doing an arbitrary textual rewrite on your program that your other systems don't know about
[00:32:45.900 --> 00:32:50.140]   Are the back tick variable still on the stack? Yes
[00:32:50.780 --> 00:32:53.100]   It would be on the stack of definer tests here
[00:32:53.100 --> 00:32:55.740]   Okay
[00:32:55.740 --> 00:32:57.660]   So let's do some more stuff
[00:32:57.660 --> 00:32:59.100]   We have
[00:32:59.100 --> 00:33:00.300]   a
[00:33:00.300 --> 00:33:01.500]   special
[00:33:01.500 --> 00:33:05.580]   New data type called code that represents an arbitrary syntax tree
[00:33:05.580 --> 00:33:10.380]   In the programming language and macros can take these as parameters
[00:33:10.380 --> 00:33:16.060]   And using this thing called insert they can put them into the body of the macro
[00:33:19.420 --> 00:33:21.420]   So
[00:33:21.420 --> 00:33:30.380]   We'll go down here to insert or test
[00:33:30.380 --> 00:33:33.100]   We're saying tripler
[00:33:33.100 --> 00:33:37.740]   We have this directive called code that says make this syntax tree out of whatever's after
[00:33:37.740 --> 00:33:39.740]   We're just going to put print hello sailor
[00:33:39.740 --> 00:33:41.900]   We're going to pass that to the macro
[00:33:41.900 --> 00:33:43.100]   Right
[00:33:43.100 --> 00:33:45.740]   The macro takes some code it inserts it three times
[00:33:46.140 --> 00:33:49.980]   So this is just a macro that basically turns one statement into three
[00:33:49.980 --> 00:33:52.940]   and uh
[00:33:52.940 --> 00:33:58.380]   Then we call it, right? So we're calling tripler on hello sailor. We get three hello sailors, right?
[00:33:58.380 --> 00:34:00.300]   Now
[00:34:00.300 --> 00:34:02.700]   You don't actually need this code directive
[00:34:02.700 --> 00:34:07.980]   If whatever is inside here already would have resolved lexical lexically
[00:34:07.980 --> 00:34:10.940]   Like if the variables and all that are already well defined
[00:34:11.420 --> 00:34:17.020]   Then the caller doesn't even have to know that this is getting converted to a syntax tree
[00:34:17.020 --> 00:34:19.020]   He can just call so you can just say tripler
[00:34:19.020 --> 00:34:24.380]   And then this thing and it'll do goodbye sailor three times like you see here
[00:34:24.380 --> 00:34:29.500]   Now you can put arbitrary amounts of stuff in there
[00:34:29.500 --> 00:34:33.260]   It doesn't have to look like something that you might usually be able to pass
[00:34:33.260 --> 00:34:36.620]   So here we have a code directive. It's saying here's this whole block
[00:34:37.420 --> 00:34:42.940]   Within that block i'm defining some function called polynomial that takes a variable
[00:34:42.940 --> 00:34:45.180]   And computes x squared minus three x plus two
[00:34:45.180 --> 00:34:51.020]   Uh, and then makes a variable takes a random number between one and twenty
[00:34:51.020 --> 00:34:52.780]   and
[00:34:52.780 --> 00:34:53.740]   It
[00:34:53.740 --> 00:34:58.060]   Evaluates polynomial on that and prints that and the value and do that three times
[00:34:58.060 --> 00:35:03.180]   Okay, we're going to pick a random number every time because this is calling something that has global state
[00:35:03.980 --> 00:35:11.500]   Um, so here we go polynomial of 19 is 306 of 11. It's 90 and a 15. It's 182 right so
[00:35:11.500 --> 00:35:15.340]   This just shows you
[00:35:15.340 --> 00:35:22.780]   We're passing in a piece of code in another programming language
[00:35:22.780 --> 00:35:25.740]   This might be something that you would have to make a function out of right
[00:35:25.740 --> 00:35:31.420]   Uh, like a locally capturing lambda or whatever here. We are not
[00:35:32.140 --> 00:35:37.580]   doing that right this is uh, literally just being pasted in to tripler
[00:35:37.580 --> 00:35:42.940]   And then that the result of that is being pasted back in out here
[00:35:42.940 --> 00:35:45.340]   So you don't actually have to worry about
[00:35:45.340 --> 00:35:51.020]   Wrapping and unwrapping functions. You don't have to worry about whether something is going to get inlined whether if
[00:35:51.020 --> 00:35:53.900]   Constructed function is going to get optimized out
[00:35:53.900 --> 00:35:59.580]   Or whether the abstraction for it is going to decide to heap allocate like happens with standard function in c plus plus
[00:35:59.980 --> 00:36:01.980]   Uh, it just goes, right?
[00:36:01.980 --> 00:36:06.460]   All right, so
[00:36:06.460 --> 00:36:09.020]   Here is something just like tripler except it's slightly different
[00:36:09.020 --> 00:36:13.260]   It takes two parameters a and b and it does a and then b and then a
[00:36:13.260 --> 00:36:17.740]   So here we're calling that with it's pitch black you're likely to be eaten by a guru, right?
[00:36:17.740 --> 00:36:21.260]   So pitch black likely to be eaten by a guru. It's pitch black
[00:36:21.260 --> 00:36:24.620]   um
[00:36:24.620 --> 00:36:26.940]   Now
[00:36:27.740 --> 00:36:29.740]   I
[00:36:29.740 --> 00:36:32.940]   Just like you have back tick
[00:36:32.940 --> 00:36:35.820]   That says I want to reach outside the scope
[00:36:35.820 --> 00:36:40.940]   So notice that all this scope all this code out here actually we maybe could have had some better examples
[00:36:40.940 --> 00:36:45.580]   But all of this code lives lexically in the scope of the caller, right?
[00:36:45.580 --> 00:36:48.860]   All this is code that makes sense to the caller, but
[00:36:48.860 --> 00:36:54.860]   You can also have a system where you define a macro that takes parameters
[00:36:55.500 --> 00:37:01.260]   That are code meant to actually go in the body of the macro and live inside the body of the macro
[00:37:01.260 --> 00:37:05.100]   And when you want to do that, it's the same thing. It's code. We just insert it differently
[00:37:05.100 --> 00:37:08.780]   We just say insert this to be internal to my scope instead of
[00:37:08.780 --> 00:37:13.980]   In the caller scope, right? So then we can do tricky stuff like um
[00:37:13.980 --> 00:37:17.500]   I can refer to the variables in the macro
[00:37:17.500 --> 00:37:22.860]   Like this index variable, which I can't see out here and I don't know what it is, right?
[00:37:23.420 --> 00:37:26.060]   But this gets inserted in such a way
[00:37:26.060 --> 00:37:33.340]   It's like an inverse back tick. Maybe it could syntactically look like the opposite of a back tick, but um
[00:37:33.340 --> 00:37:38.780]   Not really because it's an insertion and not a variable reference. I don't know man
[00:37:38.780 --> 00:37:44.700]   But uh, so that's how this works. So I'm able to make a parameter, right?
[00:37:44.700 --> 00:37:46.540]   um
[00:37:46.540 --> 00:37:48.380]   That's just some code
[00:37:48.380 --> 00:37:53.020]   And pass it to this macro it uses variables in the macro and it evaluates just fine, right?
[00:37:53.500 --> 00:37:55.500]   So, um, where are we?
[00:37:55.500 --> 00:37:57.340]   Uh
[00:37:57.340 --> 00:37:59.820]   Square is 149625 right here
[00:37:59.820 --> 00:38:03.180]   Okay, um
[00:38:03.180 --> 00:38:04.460]   Why do that?
[00:38:04.460 --> 00:38:06.780]   What's the point? Well again instead of
[00:38:06.780 --> 00:38:14.300]   Getting all functional programming brain where you make a function that takes other functions as arguments every time you want to do something
[00:38:14.300 --> 00:38:17.260]   And so those pieces of code have to be all super encapsulated
[00:38:17.820 --> 00:38:23.500]   You can make macros that are just diff designed for you to fill in little pieces
[00:38:23.500 --> 00:38:30.700]   Almost like your copy and pasting code into the macro at the site, right? And so here
[00:38:30.700 --> 00:38:36.780]   We have one called visit files. It's again, maybe not the best example, but um
[00:38:36.780 --> 00:38:44.460]   We take three pieces of code a pre that we put at the beginning a post that we put at the end
[00:38:44.940 --> 00:38:49.740]   And then just pretend we're doing like a file search and we find these file names or something, right?
[00:38:49.740 --> 00:38:51.660]   Um
[00:38:51.660 --> 00:38:57.260]   Over for everything in those files, then we insert this but we insert it internally
[00:38:57.260 --> 00:39:02.540]   So the caller is like knows what this macro is and knows what the variables are
[00:39:02.540 --> 00:39:05.180]   and just can uh
[00:39:05.180 --> 00:39:10.380]   Can refer to file name and file index even though those are not exported from the macro
[00:39:10.380 --> 00:39:12.540]   but
[00:39:12.540 --> 00:39:14.540]   Can use a back tick?
[00:39:14.620 --> 00:39:20.060]   That refers back to this so like we know this is being inserted into the body of the macro
[00:39:20.060 --> 00:39:23.340]   But we can also use our own variable by back-ticking it, right?
[00:39:23.340 --> 00:39:26.140]   so
[00:39:26.140 --> 00:39:27.100]   Um
[00:39:27.100 --> 00:39:31.260]   So our pre is just starting our post is just ending and for every file
[00:39:31.260 --> 00:39:37.100]   Uh, this macro or no this insertion to the macro is printing the file index
[00:39:37.100 --> 00:39:41.660]   Um, which is just whatever this index was from the iteration
[00:39:42.220 --> 00:39:47.180]   Um, but then also our caller count which we start at 42 and then we add one every time, right?
[00:39:47.180 --> 00:39:48.060]   So
[00:39:48.060 --> 00:39:51.340]   The index in the iteration is 0 1 2 3 and our caller count is 42
[00:39:51.340 --> 00:39:53.260]   4 to 3 44 45
[00:39:53.260 --> 00:39:57.820]   Which again, this is just a way of with more hygienic name spaces
[00:39:57.820 --> 00:40:02.540]   Being able to control what is happening and also having good error reporting
[00:40:02.540 --> 00:40:05.340]   Over when you get things wrong
[00:40:10.220 --> 00:40:12.220]   Okay, um
[00:40:12.220 --> 00:40:17.900]   All right, so here's here's another example
[00:40:17.900 --> 00:40:20.220]   Um for convenience, let me
[00:40:20.220 --> 00:40:25.740]   Let me just pull a space here. So we're going to call a sorting routine
[00:40:25.740 --> 00:40:28.300]   Um, so we're going to make some numbers
[00:40:28.300 --> 00:40:32.700]   We randomly generate them and we say before sort here's the numbers, right?
[00:40:32.700 --> 00:40:35.340]   So before the sort here's the numbers
[00:40:35.340 --> 00:40:39.020]   They're just numbers between 1 and 100 or 0 and 99
[00:40:39.660 --> 00:40:41.260]   Um
[00:40:41.260 --> 00:40:43.580]   Not in any particular order, right?
[00:40:43.580 --> 00:40:46.620]   And this is the index in brackets and those are the actual numbers
[00:40:46.620 --> 00:40:49.100]   So we're going to call this bubble sort now
[00:40:49.100 --> 00:40:52.860]   You know sorting in c and c plus plus is a giant annoyance?
[00:40:52.860 --> 00:40:54.540]   because
[00:40:54.540 --> 00:41:00.460]   It's all based on like q sort from back in the c days where you make a comparator function
[00:41:00.460 --> 00:41:03.580]   That takes like two types
[00:41:03.580 --> 00:41:08.540]   And it outputs a number that's less than zero or equal to zero or greater than zero
[00:41:09.100 --> 00:41:14.140]   And then maybe in c plus plus you have templated versions of that that maybe don't exactly use a function
[00:41:14.140 --> 00:41:16.220]   But maybe probably they do
[00:41:16.220 --> 00:41:18.620]   It's all very lambda brainy
[00:41:18.620 --> 00:41:23.020]   And it generates relatively slow code and it's just a headache like look
[00:41:23.020 --> 00:41:27.180]   Sometimes I don't want to think that much sometimes I just want to like sort stuff
[00:41:27.180 --> 00:41:31.660]   So here i've got this bubble sort and I can just say bubble sort the numbers
[00:41:31.660 --> 00:41:35.340]   And the sorting criterion is just going to be a less than b
[00:41:35.980 --> 00:41:40.220]   Now I know in the macro that a and b or the two things we're comparing so i'm just saying
[00:41:40.220 --> 00:41:42.620]   You know
[00:41:42.620 --> 00:41:44.620]   Sort things so that
[00:41:44.620 --> 00:41:45.740]   You know
[00:41:45.740 --> 00:41:48.220]   In increasing order a is less than b, right?
[00:41:48.220 --> 00:41:51.980]   And then if I want to do it the other way I could do a is greater than b, right?
[00:41:51.980 --> 00:41:54.220]   And
[00:41:54.220 --> 00:41:56.220]   I'm just passing in this code
[00:41:56.220 --> 00:42:01.420]   This code is being inserted at this point in the macro and the rest of the macro is
[00:42:01.420 --> 00:42:04.140]   Is a bubble sort, right?
[00:42:04.140 --> 00:42:05.260]   So
[00:42:05.260 --> 00:42:11.100]   After the first sort the numbers are this they're increasing after the second sort the numbers are this, right? So
[00:42:11.100 --> 00:42:14.780]   We can do
[00:42:14.780 --> 00:42:17.020]   We can
[00:42:17.020 --> 00:42:20.540]   Pass code that is very concise and succinct
[00:42:20.540 --> 00:42:24.540]   To a macro because it's just getting pasted together anyway
[00:42:24.540 --> 00:42:29.340]   It does this doesn't have to make sense by itself as a function if you know what i'm saying
[00:42:29.820 --> 00:42:34.860]   We don't have to like lambda of parameter a of some type and parameter b of some other type and whatever
[00:42:34.860 --> 00:42:41.340]   And hope that that gets optimized we just say a less than b, right?
[00:42:41.340 --> 00:42:50.860]   Okay
[00:42:50.860 --> 00:42:53.580]   Um
[00:42:53.580 --> 00:42:59.340]   Let's get past our visit files in bubble sort. Oh, I did use this. It's delete that comment
[00:42:59.900 --> 00:43:01.900]   Okay, um
[00:43:01.900 --> 00:43:10.380]   This is maybe a little bit of a nuance, but of course
[00:43:10.380 --> 00:43:16.620]   Uh in the same way that we were able to use the back tick to decline very to define variables
[00:43:16.620 --> 00:43:18.460]   um
[00:43:18.460 --> 00:43:23.020]   We can have the type to that b parameters just like you would expect with a polymorphic function
[00:43:23.020 --> 00:43:28.540]   Except you don't even have to make this polymorphic because like macros are automatically polymorphic
[00:43:28.620 --> 00:43:31.420]   In their arguments because they get compiled
[00:43:31.420 --> 00:43:36.540]   They get expanded at compile time. I really can't speak today. I don't know what my problem is
[00:43:36.540 --> 00:43:41.500]   So here we've got a variable called t that's a type and hey, it's a macro
[00:43:41.500 --> 00:43:45.180]   So we don't have to say dollar sign t and even though this doesn't say dollar sign t
[00:43:45.180 --> 00:43:50.860]   We can declare two things of type t. These are hygienic in the local scope and then we make a thing baz
[00:43:50.860 --> 00:43:52.860]   That's the product of those, right?
[00:43:52.860 --> 00:44:00.620]   So we're going to make two floats three and five and say what baz is and then three signed eight bit numbers three and five and say what baz is, right?
[00:44:00.620 --> 00:44:03.260]   um
[00:44:03.260 --> 00:44:06.220]   All right, here we go 15.0 and then 15
[00:44:06.220 --> 00:44:08.700]   So again, it
[00:44:08.700 --> 00:44:12.780]   If you haven't written large volumes of code, you probably are thinking what
[00:44:12.780 --> 00:44:17.820]   What is all this about but like this is the kind of stuff that you want to do sometimes
[00:44:17.820 --> 00:44:21.740]   When you want to do a whole bunch of things over and over
[00:44:22.460 --> 00:44:26.780]   That are a little bit different, but not that different and you want to be able to control the differences
[00:44:26.780 --> 00:44:30.060]   But you don't want to have to be like calling functions
[00:44:30.060 --> 00:44:36.620]   Um, you just want to like declare some variables or whatever, right? This is how you do that
[00:44:36.620 --> 00:44:39.100]   Okay
[00:44:39.100 --> 00:44:39.900]   Now
[00:44:39.900 --> 00:44:44.220]   I started out uh by talking about for loops and now we're going to get to the for loop part
[00:44:44.220 --> 00:44:48.700]   So for loops now instead of having this protocol where they call
[00:44:49.420 --> 00:44:54.060]   Make iterator and then get value on the iterator and next on the iterator
[00:44:54.060 --> 00:44:57.980]   The for loops now when you for over a custom type
[00:44:57.980 --> 00:45:00.060]   it just
[00:45:00.060 --> 00:45:01.820]   calls one macro
[00:45:01.820 --> 00:45:06.540]   It's called uh for expansion is the default name. We'll see in a little bit that you can actually change the name
[00:45:06.540 --> 00:45:08.940]   um
[00:45:08.940 --> 00:45:13.980]   We're just gonna um, oh, I already did that. I have a comment here to look at the old iterator stuff, but we did that at the start
[00:45:13.980 --> 00:45:15.820]   so
[00:45:15.820 --> 00:45:17.660]   um, if you want
[00:45:17.660 --> 00:45:23.980]   Uh to iterate over any type you define a thing called for expansion that's got four parameters
[00:45:23.980 --> 00:45:25.500]   um
[00:45:25.500 --> 00:45:28.700]   It's got the thing that you want to iterate over, right?
[00:45:28.700 --> 00:45:31.580]   The body code, which is what the for loop
[00:45:31.580 --> 00:45:34.700]   Passes in that's the thing that you want to paste in order to iterate
[00:45:34.700 --> 00:45:40.060]   And then two control variables that says whether you want to take the pointer to each thing and whether you want to reverse
[00:45:40.060 --> 00:45:44.140]   Each thing you don't actually have to support those you're free to ignore them
[00:45:45.500 --> 00:45:48.300]   But part of the protocol is that those are passed to you, right?
[00:45:48.300 --> 00:45:52.540]   So here we're just ignoring this by pointer and reverse stuff
[00:45:52.540 --> 00:45:58.460]   And we're just going to say we're going to make an iterator for vector four and it prints kalabunga
[00:45:58.460 --> 00:46:03.100]   That's a really stupid iterator, but it does um, we're going to give you back
[00:46:03.100 --> 00:46:05.900]   Uh a variable called it
[00:46:05.900 --> 00:46:11.980]   Right, which is the standard remember if you do a for loop and you don't name the variables you get things called it and it index
[00:46:12.380 --> 00:46:15.820]   So this is saying hey, you have a thing called it that gets v.x
[00:46:15.820 --> 00:46:20.540]   And then we're going to insert the body of your iterator, which is this thing, right?
[00:46:20.540 --> 00:46:27.900]   And then we're going to assign it to v.y and insert the body in v.z and insert the body in v.w and insert the body
[00:46:27.900 --> 00:46:30.380]   This is a really dumb iterator
[00:46:30.380 --> 00:46:33.340]   Um
[00:46:33.340 --> 00:46:39.500]   But it's just showing you how this macro functionality that i've talked about so far plugs in to the for loop stuff in a very simple way
[00:46:39.500 --> 00:46:41.580]   So here we go
[00:46:41.820 --> 00:46:48.620]   We assign xyz and w here and then we for over this and we get those and there we go
[00:46:48.620 --> 00:46:51.740]   That's all we had to do to make a for loop was this
[00:46:51.740 --> 00:46:55.500]   Um, it would have been way more painful and annoying
[00:46:55.500 --> 00:47:00.140]   To do it using a iterator making scheme
[00:47:00.140 --> 00:47:03.660]   Um, okay, let's go back to the bucket array
[00:47:03.660 --> 00:47:05.660]   Um
[00:47:05.660 --> 00:47:11.260]   Remember i said at the start of the stream that this is all we really wanted to do when we for
[00:47:11.500 --> 00:47:13.500]   Over a bucket array, right?
[00:47:13.500 --> 00:47:16.460]   Um
[00:47:16.460 --> 00:47:19.100]   The actual iterator is a little more complicated
[00:47:19.100 --> 00:47:21.740]   Uh, but not that much more complicated
[00:47:21.740 --> 00:47:23.820]   Um, here it is
[00:47:23.820 --> 00:47:28.300]   And in part, it's more complicated because we're handling taking things by pointer and reversal
[00:47:28.300 --> 00:47:31.100]   But this is the amount of code that it is
[00:47:31.100 --> 00:47:34.140]   Actually, it's got a little bit extra because this is like a feature
[00:47:34.140 --> 00:47:37.020]   That we wanted to use in the socobon game that doesn't
[00:47:37.020 --> 00:47:40.540]   Really have to do with actually iterating so
[00:47:41.180 --> 00:47:44.860]   Um, really without our hard-toated thing
[00:47:44.860 --> 00:47:48.860]   It would be that
[00:47:48.860 --> 00:47:57.660]   Uh, but we'll start we'll start talking about some of these other features of integrating with for loops. Oh, actually
[00:47:57.660 --> 00:48:03.660]   We'll ignore this actually because this got some features we haven't talked about yet. Um
[00:48:03.660 --> 00:48:09.740]   So we get an array of strings for each string in this array. We add it to this bucket array, right?
[00:48:10.300 --> 00:48:13.420]   We're just going to show here that it works. So for every item
[00:48:13.420 --> 00:48:16.220]   in the loop, uh
[00:48:16.220 --> 00:48:18.460]   So note first of all that we're renaming
[00:48:18.460 --> 00:48:21.340]   This is not called it. This is called item
[00:48:21.340 --> 00:48:26.300]   Uh, and so part of the protocol of how four interfaces with a macro has to
[00:48:26.300 --> 00:48:31.580]   Negotiate that name. Um, but so every item is called item
[00:48:31.580 --> 00:48:36.460]   We're going to use a defer to say this should print at the end of each loop
[00:48:36.460 --> 00:48:39.180]   Then we're going to say we're about to say something about x
[00:48:39.900 --> 00:48:46.780]   And then we're going to go four times, uh, and just say x is whatever or three times
[00:48:46.780 --> 00:48:52.460]   We're just going to print x is one x is two x is three for each item
[00:48:52.460 --> 00:48:57.340]   Um, we're not printing this string at all
[00:48:57.340 --> 00:49:02.300]   Let's print the string
[00:49:02.300 --> 00:49:05.820]   Um
[00:49:06.300 --> 00:49:11.500]   Like we should use the item. Otherwise, why are we frickin iterating over the item, right?
[00:49:11.500 --> 00:49:17.420]   Okay
[00:49:17.420 --> 00:49:22.940]   Maybe that makes this less readable. I don't know but so we go there are 69 105 leaves in the pile
[00:49:22.940 --> 00:49:27.500]   Right. So we're iterating at each of those things. Um, we are about to say something about x
[00:49:27.500 --> 00:49:31.660]   We do it and then print at the end because it's a defer so it gets popped down there
[00:49:31.660 --> 00:49:34.060]   um
[00:49:34.060 --> 00:49:36.060]   so
[00:49:36.300 --> 00:49:38.300]   however
[00:49:38.300 --> 00:49:41.980]   Remember remember at the beginning we said there are some subtleties for example
[00:49:41.980 --> 00:49:45.740]   If you're going to iterate over this bucket array, um
[00:49:45.740 --> 00:49:49.420]   Well, uh
[00:49:49.420 --> 00:49:55.660]   You need to start thinking about what the loop controls actually mean because if you replace a one-dimensional iteration
[00:49:55.660 --> 00:49:58.860]   with a two-dimensional iteration
[00:49:58.860 --> 00:50:01.180]   um
[00:50:01.900 --> 00:50:05.580]   Break and continue need to start doing different things because it continue
[00:50:05.580 --> 00:50:10.780]   Maybe it's fine if it goes in the inner inner thing and a break maybe needs to go to the outer thing, right?
[00:50:10.780 --> 00:50:13.260]   Um, those sort of
[00:50:13.260 --> 00:50:15.420]   The iterator knows what's right
[00:50:15.420 --> 00:50:18.220]   Like your data. It's probably different for every data type
[00:50:18.220 --> 00:50:19.500]   But
[00:50:19.500 --> 00:50:22.780]   The for loop needs to let you interface and control with that stuff
[00:50:22.780 --> 00:50:27.740]   Um, let me just show for right now that that works in a very basic way. So
[00:50:28.860 --> 00:50:29.900]   here
[00:50:29.900 --> 00:50:31.900]   I've got my okay, so
[00:50:31.900 --> 00:50:34.380]   Let's clarify what's happening here, right?
[00:50:34.380 --> 00:50:38.380]   This a is our bucket array, which means this for loop is a custom for loop
[00:50:38.380 --> 00:50:41.580]   That calls our equivalent of this iterator
[00:50:41.580 --> 00:50:46.140]   This for loop is not a custom for loop. This is just over integers
[00:50:46.140 --> 00:50:51.740]   So down here, we don't want to continue over the integers. We want to continue out
[00:50:51.740 --> 00:50:58.140]   Of the main loop. So let's first of all show that that works and if it works, we won't print two or three
[00:50:58.700 --> 00:51:03.740]   Because by continuing out here, we're going to skip two and three after we do one, right? So
[00:51:03.740 --> 00:51:11.980]   Hopefully, I'm just not totally lost in the weeds and people get what's going on, but here you go x is one x is one x is one x is one, right?
[00:51:11.980 --> 00:51:14.620]   Okay
[00:51:14.620 --> 00:51:16.620]   um
[00:51:16.620 --> 00:51:21.980]   So we're just sort of showing
[00:51:22.940 --> 00:51:29.420]   Macro driven for loops interacting with non macro driven for loops and that sort of thing. Um, let's show
[00:51:29.420 --> 00:51:36.700]   Um, let's show it working, uh, with these, right?
[00:51:36.700 --> 00:51:38.940]   So
[00:51:38.940 --> 00:51:40.940]   First we're just iterating over
[00:51:40.940 --> 00:51:45.820]   Uh, the bucket array just printing out all the things with an index so we're not doing our inner loop, right?
[00:51:45.820 --> 00:51:48.780]   So we're saying there are 69 105 leaves in the pile
[00:51:48.780 --> 00:51:51.740]   Um, we're going to iterate in the reverse direction
[00:51:51.740 --> 00:51:56.700]   We get this handy very terse syntax, uh, that I like using
[00:51:56.700 --> 00:51:58.540]   Um
[00:51:58.540 --> 00:52:00.700]   And when we do this, what happens is
[00:52:00.700 --> 00:52:07.340]   Let's go back to our actual thing. What happens is true gets passed in this parameter, right? And then remember
[00:52:07.340 --> 00:52:12.780]   As we opened with we are able to do this thing very tersely
[00:52:12.780 --> 00:52:18.940]   To control the direction over which we iterate the loop and for this data structure, it happens to be correct
[00:52:19.580 --> 00:52:25.180]   That if we want to reverse, then you reverse the outer loop and you reverse the inner loop and you just go, right?
[00:52:25.180 --> 00:52:31.580]   Um, and if you want to take things by a pointer, uh similarly, um
[00:52:31.580 --> 00:52:36.780]   This thing right here and then this like I said is that's our own
[00:52:36.780 --> 00:52:39.740]   That's our own thing
[00:52:39.740 --> 00:52:43.500]   That we're using the game. So
[00:52:43.500 --> 00:52:48.380]   It's a data oriented thing. Somebody remind me to explain that at the end because I don't want to go into it right now
[00:52:48.380 --> 00:52:50.380]   But it's it has to do with
[00:52:50.380 --> 00:52:54.700]   Just being data oriented. Okay. So we go forward
[00:52:54.700 --> 00:53:01.660]   We go backward and we go by pointer to all these elements, right? And so we have a very nice terse syntax for doing all these things
[00:53:01.660 --> 00:53:04.860]   Um
[00:53:04.860 --> 00:53:08.140]   Now there's this name remapping that happens that have sort of been
[00:53:08.140 --> 00:53:13.740]   Tangentially referencing here, right? So, uh
[00:53:15.340 --> 00:53:20.220]   Here we have a two-dimensional for loop. These are both custom for loops that evaluate that macro
[00:53:20.220 --> 00:53:26.620]   So we're evaluating this macro here that gives us two dimensions of loop and then we're doing it again inside
[00:53:26.620 --> 00:53:28.940]   Which gives us another two dimensions of loop?
[00:53:28.940 --> 00:53:37.100]   So we've actually got a four-dimensional iteration here and in this version. We're going to break s2 which should break actually
[00:53:37.100 --> 00:53:40.380]   from here
[00:53:40.380 --> 00:53:47.260]   Uh and here we're going to break s1 which should actually break from there of the outer loop and not the inner loop
[00:53:47.260 --> 00:53:52.700]   And so first let's show that that works and then let's talk about how it works. So here we're going
[00:53:52.700 --> 00:53:57.340]   Over
[00:53:57.340 --> 00:54:01.020]   Every s2 or no, we're going over every s1
[00:54:01.020 --> 00:54:03.980]   Let's talk about what s1 and s2 are
[00:54:04.860 --> 00:54:10.060]   They're just uh the strings, right and then i1 and i2 are the indices
[00:54:10.060 --> 00:54:13.100]   and
[00:54:13.100 --> 00:54:18.140]   The i1 and s1 are the indices and strings for the outer loop. I should have maybe called them outer. I don't know
[00:54:18.140 --> 00:54:20.780]   um
[00:54:20.780 --> 00:54:23.500]   So here we're iterating over everything in the outer loop
[00:54:23.500 --> 00:54:28.300]   But we only get to element zero in the inner loop and the reason why is because we break
[00:54:28.300 --> 00:54:31.020]   from this loop
[00:54:31.020 --> 00:54:33.020]   Already, this is this break
[00:54:33.820 --> 00:54:36.060]   Named break is something that we already supported
[00:54:36.060 --> 00:54:41.900]   Way back for vanilla non-custom loops, but now it actually just works here
[00:54:41.900 --> 00:54:47.340]   So named break here. Let's just break from this or this so in this second example
[00:54:47.340 --> 00:54:51.900]   Um, we're breaking out from zero zero right here. All right
[00:54:51.900 --> 00:54:54.940]   Um
[00:54:54.940 --> 00:54:56.940]   So you might be saying
[00:54:56.940 --> 00:55:01.020]   Uh, oh well actually hold on. Let's let's talk about why this works
[00:55:01.580 --> 00:55:05.740]   Uh, the reason it works is because when we insert code for a loop control
[00:55:05.740 --> 00:55:08.620]   We're able to replace a few things
[00:55:08.620 --> 00:55:15.980]   Like this standard loop control primitives break continue and remove we can specify what they should be replaced by
[00:55:15.980 --> 00:55:20.860]   Right, so we're going to insert something. We're going to say all right. We're going to insert body, but
[00:55:20.860 --> 00:55:23.500]   If we ever see a break
[00:55:23.500 --> 00:55:27.180]   We're going to replace it by break bucket which breaks us out of this outer loop
[00:55:27.180 --> 00:55:30.460]   That's how the named for loops work as you say the name of the iterator
[00:55:31.020 --> 00:55:36.780]   Um, and if you say remove we're going to replace that with this piece of code that
[00:55:36.780 --> 00:55:38.860]   um
[00:55:38.860 --> 00:55:41.100]   Just sets the occupied flag by false and
[00:55:41.100 --> 00:55:45.580]   reduces the count on the number of elements in the thing
[00:55:45.580 --> 00:55:47.420]   right
[00:55:47.420 --> 00:55:49.340]   um
[00:55:49.340 --> 00:55:53.820]   And so you can think of this as being like a wildcard search and replace
[00:55:53.820 --> 00:55:58.300]   That operates on the piece of code that is inserted
[00:55:59.740 --> 00:56:04.620]   When this macro activates which again is just whatever's in the body of the thing
[00:56:04.620 --> 00:56:16.780]   Now why does the two-dimensional one work because like doesn't doesn't this
[00:56:16.780 --> 00:56:26.540]   When you expand the outer one doesn't this replace the break in the inner one? Um, and the answer is
[00:56:27.420 --> 00:56:29.420]   Uh,
[00:56:29.420 --> 00:56:33.820]   I think the answer is
[00:56:33.820 --> 00:56:37.580]   Oh, we only um
[00:56:37.580 --> 00:56:44.620]   The identification of this identifier with which loop
[00:56:44.620 --> 00:56:47.900]   Um controls
[00:56:47.900 --> 00:56:52.140]   Whether the expansion actually applies, right? So when you say break s1
[00:56:52.140 --> 00:56:55.180]   When we're expanding s2 we say
[00:56:56.460 --> 00:57:01.180]   Oh, that's actually not a break for our for loop. So it doesn't it doesn't match for the purposes of that replacement
[00:57:01.180 --> 00:57:03.740]   Same thing here when we do this
[00:57:03.740 --> 00:57:06.380]   Uh
[00:57:06.380 --> 00:57:11.820]   We see this break and we're like, oh, that's a break, but it's not for our loop. So we leave it
[00:57:11.820 --> 00:57:17.580]   Okay, um, so you might be saying wow that it sounds like a lot of
[00:57:17.580 --> 00:57:19.740]   very uh
[00:57:19.740 --> 00:57:23.420]   Custom things that only apply to this one case and it's like no actually it's been
[00:57:23.980 --> 00:57:26.700]   Very carefully designed to apply to as many things
[00:57:26.700 --> 00:57:29.500]   As we can make it apply to
[00:57:29.500 --> 00:57:34.780]   Um, so let's show some more examples and then maybe the maybe it'll make more sense
[00:57:34.780 --> 00:57:38.060]   Um, these are all examples that we use in production by the way
[00:57:38.060 --> 00:57:42.860]   So that bucket array this this new iterator down here is what we use
[00:57:42.860 --> 00:57:47.660]   In the game now we've been using it for a few weeks everywhere
[00:57:47.660 --> 00:57:50.220]   um
[00:57:50.780 --> 00:57:55.420]   We've got my hash table that I that I did that's a port of shawn Barrett's hash table
[00:57:55.420 --> 00:57:57.500]   um
[00:57:57.500 --> 00:58:02.540]   Here is our iterator for the hash table. It's actually simpler. It only needs a one-dimensional array
[00:58:02.540 --> 00:58:06.380]   It supports taking things by pointer or by reversing, right?
[00:58:06.380 --> 00:58:08.140]   Um
[00:58:08.140 --> 00:58:12.300]   It gives you this variable called it which again you can rename if you want
[00:58:12.300 --> 00:58:15.660]   uh
[00:58:15.660 --> 00:58:19.900]   The way the way it works is you're I didn't explain this but your iterator always
[00:58:20.540 --> 00:58:23.580]   Exports values called or names called it and it index
[00:58:23.580 --> 00:58:27.580]   And then the for loop itself when it goes to expand the for loop
[00:58:27.580 --> 00:58:32.460]   Knows if it needs different names and it changes the names, right?
[00:58:32.460 --> 00:58:39.020]   So this always says it and it index but the names that you get are for example value and key here, right?
[00:58:39.020 --> 00:58:45.580]   So this is actually very concise and very nice. Um, so
[00:58:47.020 --> 00:58:50.140]   you reverse or not you take by pointer or not and
[00:58:50.140 --> 00:58:54.940]   Remember what I was explaining that we would do this by hand because all you really need to know is
[00:58:54.940 --> 00:58:59.500]   If the hash value is less than a valid hash, then you don't iterate that, right?
[00:58:59.500 --> 00:59:06.460]   If it is greater than or equal to a valid hash, then we set the index to be the key, right?
[00:59:06.460 --> 00:59:13.580]   Uh, which is by the way of any type. This isn't necessarily an integer type. So you have a value called it. That's the value
[00:59:13.580 --> 00:59:15.900]   You have an it index. That's the key
[00:59:16.140 --> 00:59:17.420]   um
[00:59:17.420 --> 00:59:19.420]   And then, uh
[00:59:19.420 --> 00:59:22.620]   I don't know why I put these braces here, but um
[00:59:22.620 --> 00:59:31.020]   We insert the body, which is whatever your loop is, but if you say remove, then we replace remove by saying, oh
[00:59:31.020 --> 00:59:38.940]   Well, we just put a special hash in the table that says this item has been removed so that further iterations will skip it, right? So
[00:59:38.940 --> 00:59:45.900]   Here we're going to iterate the table. We're going to print before it's got a key and a value
[00:59:46.860 --> 00:59:51.260]   And then we're going to say if the key happens to be this thing 69 105 remove
[00:59:51.260 --> 00:59:55.820]   That value, right? And then we're going to iterate again and make sure it's removed
[00:59:55.820 --> 00:59:58.460]   So here before
[00:59:58.460 --> 01:00:03.740]   Leaves in the our 69 105 pile there. It's in a random order because it's a hash table, right?
[01:00:03.740 --> 01:00:08.220]   Um, and we remove the 69 105 and we get that, right?
[01:00:08.220 --> 01:00:12.060]   So we do all this custom for loop and removal
[01:00:12.620 --> 01:00:19.180]   And ability to reverse and go by pointer or not we get all of that in an iterator in that many lines of code, right?
[01:00:19.180 --> 01:00:22.060]   Which is kind of crazy
[01:00:22.060 --> 01:00:27.020]   But it's good and it works it works all over the place. So here. We've got a binary tree
[01:00:27.020 --> 01:00:33.420]   Um, say we want to iterate over a binary tree is remove a special word in the for loop context. Um
[01:00:33.420 --> 01:00:36.540]   Here
[01:00:36.540 --> 01:00:41.100]   It's a special word if you want to talk about arbitrary pattern matching and replacement
[01:00:41.980 --> 01:00:47.100]   There's a reason why I said we're going to have to break this demo into two parts and cover part of it later because it'll be too long otherwise
[01:00:47.100 --> 01:00:52.460]   Okay, um
[01:00:52.460 --> 01:00:57.260]   Iterating over a binary tree
[01:00:57.260 --> 01:00:59.740]   Um
[01:00:59.740 --> 01:01:02.940]   We're making this thing called a tree node. It's a polymorphic type that
[01:01:02.940 --> 01:01:09.420]   Takes any value type, right? So it's got a left element, which goes to a tree node a right element, right?
[01:01:09.420 --> 01:01:11.420]   This is a very textbook binary tree
[01:01:11.420 --> 01:01:16.300]   Because we're going to be data oriented. We're going to have an array of nodes instead of newing them
[01:01:16.300 --> 01:01:18.140]   So we've just got six
[01:01:18.140 --> 01:01:23.500]   We're going to make names for them abcdef and then we're going to set up the links in some way
[01:01:23.500 --> 01:01:25.900]   and we're going to give the values right so
[01:01:25.900 --> 01:01:31.340]   The values are all names of famous countries like belgium or albania or france
[01:01:31.340 --> 01:01:35.500]   And we're just going to iterate over these and print out the values
[01:01:35.500 --> 01:01:38.780]   Here we go
[01:01:39.420 --> 01:01:41.420]   What's the iterator for this?
[01:01:41.420 --> 01:01:43.740]   Well, we have it down here
[01:01:43.740 --> 01:01:47.820]   It's just a map. We don't bother expanding
[01:01:47.820 --> 01:01:53.500]   We don't bother supporting reversal right now because this isn't a particularly ordered
[01:01:53.500 --> 01:01:57.260]   traversal so we just assert that you didn't call it with reverse true
[01:01:57.260 --> 01:02:05.180]   We just pop a tree node on the stack and then we have a while loop where we pop a tree node off the stack
[01:02:06.140 --> 01:02:10.060]   If it's got a right neighbor put the right neighbor on the stack if it's got a left neighbor put the left neighbor
[01:02:10.060 --> 01:02:15.980]   And then well if we want it by pointer define it to be the pointer to the current value
[01:02:15.980 --> 01:02:22.140]   Otherwise define it to be the current value and then insert the body, right? So here's a here's a for loop expansion
[01:02:22.140 --> 01:02:25.100]   um for that for loop
[01:02:25.100 --> 01:02:32.700]   Uh, that's again very simple. This is pretty close to the minimal code that you would write for
[01:02:35.340 --> 01:02:37.340]   a pre-order. No, this is a
[01:02:37.340 --> 01:02:41.100]   Inorder traverse
[01:02:41.100 --> 01:02:48.220]   No pre-order. I don't remember. I think it's pre-order my textbook traversals are
[01:02:48.220 --> 01:02:58.860]   Are a little bit confused somebody's asking why not use insert internal in the for expansion macros
[01:02:58.860 --> 01:03:01.500]   The reason is because you don't want to think about
[01:03:02.940 --> 01:03:07.580]   The whole point that this is a macro that lives somewhere else is you don't want to have to care what the names are internally
[01:03:07.580 --> 01:03:10.860]   So you don't want to have to worry about colliding with the names, right?
[01:03:10.860 --> 01:03:14.940]   That's why it's that's why it's not internal although you could I mean
[01:03:14.940 --> 01:03:20.140]   This macro could decide to make it an internal insert. I just don't think that's a good idea
[01:03:20.140 --> 01:03:22.860]   right um
[01:03:22.860 --> 01:03:29.980]   Okay, so that's the thing that is then printing out uh, these countries in no particular order. All right
[01:03:30.620 --> 01:03:32.620]   Well, let's go to another
[01:03:32.620 --> 01:03:38.140]   Another data type just to show how good of an approach this is because I realized while I was explaining it
[01:03:38.140 --> 01:03:42.620]   It may be sounded like a bunch of weird features that were complicated
[01:03:42.620 --> 01:03:45.340]   But it's actually not that much
[01:03:45.340 --> 01:03:51.820]   Once you know how it works and it applies to a lot of different cases. So here's another thing that we use in production right now
[01:03:51.820 --> 01:03:57.580]   Uh bit array, uh, which is a data type where if you just want to have a whole bunch of booleans
[01:03:57.580 --> 01:03:59.180]   um
[01:03:59.180 --> 01:04:03.500]   Effectively booleans then you maybe want to pack them into an array of 64-bit numbers
[01:04:03.500 --> 01:04:07.180]   Right and then you can just ask if a particular bit is set
[01:04:07.180 --> 01:04:14.220]   Right and then you have these accessor functions that just set a bit or clear a bit or whatever, right? Great
[01:04:14.220 --> 01:04:19.660]   Uh, you can set or clear all the bits. So what does a for loop look like for that?
[01:04:19.660 --> 01:04:21.820]   um
[01:04:21.820 --> 01:04:25.580]   I wrote a version. Let's let's uh take out the comment for a second
[01:04:25.580 --> 01:04:27.260]   um
[01:04:27.260 --> 01:04:29.260]   So we get syntax highlighting. Um
[01:04:29.260 --> 01:04:38.620]   I wrote a version that wanted to support reverse by pointer doesn't make sense because they're bits
[01:04:38.620 --> 01:04:42.460]   You can't really have a pointer do a bit and if I gave you a pointer to a man
[01:04:42.460 --> 01:04:47.500]   Manufactured boolean like that wouldn't really make sense. So we're asserting that you don't call this with pointer
[01:04:47.500 --> 01:04:52.140]   um, and then you know sort of the cleanest way seemed like
[01:04:53.420 --> 01:04:57.980]   Doing two different loops for if it should be reversed or not, right? If it's not reversed, it's very simple
[01:04:57.980 --> 01:05:00.620]   um
[01:05:00.620 --> 01:05:07.340]   Go iterate over every slot. It's kind of not unlike the bucket array iterate over every slot
[01:05:07.340 --> 01:05:11.260]   Um start a bit at one iterate over every bit
[01:05:11.260 --> 01:05:14.060]   Uh make a boolean that's whether
[01:05:14.060 --> 01:05:20.940]   The bit is set or not right insert the body. We actually don't need these braces
[01:05:22.380 --> 01:05:28.220]   We used to need those syntactically, but now we don't so this was living in a comment, which is why that it hadn't been cleaned up
[01:05:28.220 --> 01:05:29.740]   I think um
[01:05:29.740 --> 01:05:35.340]   Anyway, so you insert the body and if somebody tries to remove a bit from a bit array that doesn't make any sense
[01:05:35.340 --> 01:05:37.340]   So we just say we don't support that right?
[01:05:37.340 --> 01:05:38.940]   Um
[01:05:38.940 --> 01:05:44.860]   But break needs to break from the outer loop again because it's 2d. It's a very common pattern that you need to turn
[01:05:44.860 --> 01:05:49.980]   What looks like a one-dimensional iteration to the user into a two or more dimensional thing
[01:05:50.380 --> 01:05:56.860]   To iterate over your data structure, right? And and that's why this replacement facility makes a lot of sense, right?
[01:05:56.860 --> 01:05:59.820]   um
[01:05:59.820 --> 01:06:01.820]   Okay, so
[01:06:01.820 --> 01:06:05.900]   Then we shift the bit one every time we add one to the index and we stop
[01:06:05.900 --> 01:06:12.540]   If we've reached the maximum number of bits in the array, which is not equal to the number of slots times 64 because
[01:06:12.540 --> 01:06:16.140]   The slots are like an over allocation, right? So if you have
[01:06:16.140 --> 01:06:18.700]   72 bits
[01:06:18.700 --> 01:06:23.100]   You're going to have 128 bits worth of storage, but you don't necessarily want to iterate over all of those
[01:06:23.100 --> 01:06:28.700]   Which is why we stop here. All right, so essentially this is the iterator, but supporting reversal
[01:06:28.700 --> 01:06:31.260]   Makes us do it twice
[01:06:31.260 --> 01:06:37.020]   Um, and then I was experimenting and I was like, well, let's make a one version that iterates once in his less code
[01:06:37.020 --> 01:06:40.780]   It's definitely less code and we do things in various places
[01:06:40.780 --> 01:06:43.340]   Uh, whether we want to reverse or not
[01:06:43.340 --> 01:06:46.220]   Um, but it's a little harder to understand
[01:06:46.940 --> 01:06:52.140]   Like this, I think you can look at and understand pretty easily. So I'm not sure which of these I favor
[01:06:52.140 --> 01:06:54.940]   um
[01:06:54.940 --> 01:07:00.620]   You could just say screw it. I don't want to support reversal at all, right? And so you could just do this one
[01:07:00.620 --> 01:07:03.580]   um
[01:07:03.580 --> 01:07:05.580]   All right, there we go
[01:07:05.580 --> 01:07:08.140]   um
[01:07:08.140 --> 01:07:16.700]   And maybe this is the right answer actually because do you really want to support reversal for a bit array?
[01:07:16.940 --> 01:07:20.220]   Well, I did in this demo because um
[01:07:20.220 --> 01:07:26.220]   If you're storing an n-bit number the way the bit array stores it is like left to right in the array
[01:07:26.220 --> 01:07:32.460]   But if the way that we write these is the least significant bit goes on the right. So, um
[01:07:32.460 --> 01:07:36.060]   Where are we?
[01:07:36.060 --> 01:07:38.300]   We're up here. Um
[01:07:41.580 --> 01:07:45.580]   So I'm iterating over all the bits and I'm casting the boolean back to an int
[01:07:45.580 --> 01:07:50.780]   Which is going to give me zero or one and just printing the ints without new lines, which is giving me this, right?
[01:07:50.780 --> 01:07:53.580]   So those are the bits you can see we set bits three
[01:07:53.580 --> 01:07:56.140]   Seven and eleven, right?
[01:07:56.140 --> 01:08:00.700]   This is three from left to right zero one two three four five six seven eight nine ten eleven
[01:08:00.700 --> 01:08:04.940]   But the if you wanted to read this as a binary number you kind of want to write it that way
[01:08:04.940 --> 01:08:07.420]   um
[01:08:07.420 --> 01:08:09.420]   So that's what the reversals for
[01:08:10.220 --> 01:08:13.420]   So bit zero one two three four five six seven
[01:08:13.420 --> 01:08:18.140]   This is the way that you would normally expect to read it if it's being treated as a number now
[01:08:18.140 --> 01:08:21.340]   And then here I just printed out his booleans as well
[01:08:21.340 --> 01:08:26.460]   Now do you really expect a bit array to be like um
[01:08:26.460 --> 01:08:29.980]   I don't know if you need reversal. So it's sort of
[01:08:29.980 --> 01:08:39.340]   The exercise that i've been engaging in in applying these macros to these different data structures is in trying to get down
[01:08:39.900 --> 01:08:42.220]   To like what's that minimal code, right?
[01:08:42.220 --> 01:08:48.860]   And the macro system helps us get down to something like the minimal code, right? So if you look at this one
[01:08:48.860 --> 01:08:54.940]   Um, this is approximately if you were to hard code and iteration what you would write
[01:08:54.940 --> 01:08:57.820]   And this one doesn't support
[01:08:57.820 --> 01:09:00.060]   um
[01:09:00.060 --> 01:09:04.060]   Doesn't support a reversal doesn't support a pointer
[01:09:06.460 --> 01:09:13.500]   It's pretty clean and pretty understandable and in this case then what adds the complexity is just if we want to do a more complex job
[01:09:13.500 --> 01:09:16.060]   Which I guess is good
[01:09:16.060 --> 01:09:18.060]   um
[01:09:18.060 --> 01:09:20.060]   But I have this sort of philosophical
[01:09:20.060 --> 01:09:22.540]   struggle that i'm having here about like
[01:09:22.540 --> 01:09:27.980]   Okay, if i'm implementing this data structure for bit array and I want a lot of people to use it
[01:09:27.980 --> 01:09:31.340]   Do I implement the version of the iterator that's
[01:09:31.340 --> 01:09:35.900]   Capable and understandable it could reverse or not and is easy to understand
[01:09:36.540 --> 01:09:42.380]   Or do I implement the one that's less code but is a little hard to understand or do I just say screw the reversal?
[01:09:42.380 --> 01:09:47.980]   This is just going to be a little bit of a gem of the iterator and we'll just assert if you try to reverse and i'm actually
[01:09:47.980 --> 01:09:50.700]   I'm leaning toward
[01:09:50.700 --> 01:09:56.300]   The version that doesn't support reversal because you know the more i've been programming
[01:09:56.300 --> 01:10:04.220]   The more i've come to the attitude that having large volumes of code that do anything you might ever want to do
[01:10:04.780 --> 01:10:10.300]   Is often part of the problem and it's better to have code that's understandable that does
[01:10:10.300 --> 01:10:13.420]   The thing that you want to do in a concise and clean way
[01:10:13.420 --> 01:10:18.220]   And then you'll understand what the data structure does and how it works and then if you need a version that's different
[01:10:18.220 --> 01:10:20.700]   You could maybe just copy and paste this
[01:10:20.700 --> 01:10:23.900]   Somewhere else right
[01:10:23.900 --> 01:10:25.180]   um
[01:10:25.180 --> 01:10:31.420]   So those are the things that i'm just throwing around and that are up in the air in terms of the implementations of these data structures
[01:10:31.820 --> 01:10:37.580]   But what i wish to show for this is just that um, oh well, there's one more
[01:10:37.580 --> 01:10:43.500]   The most concise one is the modern programmer version modern programmer where you you just
[01:10:43.500 --> 01:10:48.620]   It's a one-dimensional loop and may it even supports reversal
[01:10:48.620 --> 01:10:53.260]   And you just go over all the bits in the array and you use the subscript operator, right?
[01:10:53.260 --> 01:11:00.220]   And you assert on remove this is actually the shortest syntactic for loop
[01:11:01.180 --> 01:11:05.580]   A version that i found for these this bit array and it actually works
[01:11:05.580 --> 01:11:08.220]   Um, it's just if you do this one
[01:11:08.220 --> 01:11:13.580]   You're depending on the compiler optimizing the array subscripts
[01:11:13.580 --> 01:11:17.980]   Uh, which maybe it is so it would be interesting
[01:11:17.980 --> 01:11:21.100]   To use this version
[01:11:21.100 --> 01:11:24.700]   Actually, I don't know if we say in line on this we don't say in line on the subscript
[01:11:24.700 --> 01:11:27.660]   So you actually would probably want to say this
[01:11:28.300 --> 01:11:35.340]   um, it would be interesting to see, uh, what the generated code looks like for this in the end
[01:11:35.340 --> 01:11:37.980]   maybe
[01:11:37.980 --> 01:11:39.340]   This is totally fine
[01:11:39.340 --> 01:11:44.780]   And it might be that this is just as good as this or any of the other versions and it even supports reverse
[01:11:44.780 --> 01:11:49.260]   So there is some universe in which this is the iterator for this thing
[01:11:49.260 --> 01:11:52.700]   And that's just an amazing universe because that's like the cleanest
[01:11:52.700 --> 01:11:55.660]   possible iterator that you could think about
[01:11:56.860 --> 01:11:58.860]   Um
[01:11:58.860 --> 01:12:01.340]   Okay
[01:12:01.340 --> 01:12:04.700]   I got a little bit
[01:12:04.700 --> 01:12:06.700]   In the weeds on that one
[01:12:06.700 --> 01:12:07.820]   Um
[01:12:07.820 --> 01:12:09.500]   But the point is
[01:12:09.500 --> 01:12:11.820]   You can do in this number of lines of code
[01:12:11.820 --> 01:12:17.340]   What it would have taken many many lines of like c++ style stuff to do
[01:12:17.340 --> 01:12:19.980]   um
[01:12:19.980 --> 01:12:26.780]   And it's up to you whether you want to do this concise version or this longer version or this longer version or this longer version
[01:12:26.940 --> 01:12:29.980]   And in the past few weeks i've been playing around with all of those
[01:12:29.980 --> 01:12:36.620]   Um, this one is the one that we're currently using in production, although we don't actually use the reversal in production
[01:12:36.620 --> 01:12:39.020]   We're just using the reversal in the demo
[01:12:39.020 --> 01:12:43.260]   Okay
[01:12:43.260 --> 01:12:45.260]   Um, so now
[01:12:45.260 --> 01:12:50.460]   Like I said before though, you might not want to use this iterator called for expansion
[01:12:50.460 --> 01:12:53.820]   You might want to write your own iterator for somebody else's data structure
[01:12:54.300 --> 01:12:58.700]   That does it in a different way or you might want to provide several iterators for your data structure
[01:12:58.700 --> 01:13:03.820]   That do slightly different jobs, right? So let's make a much bigger bit array
[01:13:03.820 --> 01:13:06.860]   It's just going to have four bits set
[01:13:06.860 --> 01:13:10.140]   364 375 and 999 all right
[01:13:10.140 --> 01:13:13.260]   And we're going to use an iterator
[01:13:13.260 --> 01:13:17.340]   That iterates over only the bits that are set. We don't care about the zero bits, right?
[01:13:17.340 --> 01:13:20.380]   So we've got this iterator called only set
[01:13:20.380 --> 01:13:23.980]   Um
[01:13:23.980 --> 01:13:27.900]   What does it do? Well, we're not going to support reversal right now because whatever
[01:13:27.900 --> 01:13:31.020]   That's a complication. Um
[01:13:31.020 --> 01:13:34.860]   We're going to give you a value it and that's always going to be true because
[01:13:34.860 --> 01:13:38.620]   We're just sort of doing this for compatibility
[01:13:38.620 --> 01:13:41.100]   like
[01:13:41.100 --> 01:13:43.900]   You probably should know that the value is true because you're asking
[01:13:43.900 --> 01:13:47.260]   Iterate over only the set bits, but there it is. It's true
[01:13:47.260 --> 01:13:49.340]   um
[01:13:49.340 --> 01:13:50.540]   And then
[01:13:50.540 --> 01:13:53.740]   Uh, well, we iterate over all the slots in the bit array
[01:13:54.220 --> 01:13:56.220]   If the slot is zero
[01:13:56.220 --> 01:14:00.940]   Then we know that none of the bits can be the ones we care about so we just skip
[01:14:00.940 --> 01:14:06.620]   So actually in some sense the whole point of this macro is this line, right?
[01:14:06.620 --> 01:14:16.300]   It lets us uh skip past the slot very quickly, right? Otherwise, um, we start with an index that's
[01:14:18.300 --> 01:14:25.180]   The slot index times 64 and then our index is that plus whichever part of the loop we're in, right?
[01:14:25.180 --> 01:14:26.780]   Um
[01:14:26.780 --> 01:14:29.340]   We again just like before we start our bit out at one
[01:14:29.340 --> 01:14:34.700]   We shift it left one every time and we insert the body of your code, right?
[01:14:34.700 --> 01:14:42.060]   And so when we do that, uh, here we go iterating only the set bits three 64 375 989
[01:14:42.060 --> 01:14:44.620]   Now
[01:14:44.620 --> 01:14:46.700]   What if you want to only iterate the zero bits?
[01:14:47.500 --> 01:14:49.500]   Right, um
[01:14:49.500 --> 01:14:53.580]   What do you do there?
[01:14:53.580 --> 01:14:58.060]   Well, you could write a version of this that's only unset
[01:14:58.060 --> 01:15:01.420]   and um
[01:15:01.420 --> 01:15:04.700]   You know, you change a couple things in the function or
[01:15:04.700 --> 01:15:08.860]   You can write a more general version that's got an extra parameter
[01:15:08.860 --> 01:15:11.980]   So here i've made this thing called only set or unset, right?
[01:15:11.980 --> 01:15:16.220]   And it has a target value that's a bool and the target value says
[01:15:17.020 --> 01:15:21.180]   Are we searching for one bits if so, that'll be true
[01:15:21.180 --> 01:15:24.460]   Or are we searching for zero bits if so, that'll be false?
[01:15:24.460 --> 01:15:30.140]   And i was able to do this for this data structure with almost no additional code, right? Here's what we do
[01:15:30.140 --> 01:15:37.740]   Um, again, we set it to our target value now, which would be true or false and it's kind of stupid that we do that
[01:15:37.740 --> 01:15:42.060]   But again, it's just convention that we're giving you a value called it every time
[01:15:42.060 --> 01:15:44.140]   um
[01:15:44.140 --> 01:15:46.140]   And then the only change
[01:15:46.140 --> 01:15:51.420]   Between the previous version is uh, if target value is false
[01:15:51.420 --> 01:15:59.500]   Uh, we flip all the bits in the local value of the slot after we read it so we don't modify the original data structure
[01:15:59.500 --> 01:16:06.780]   Um, this is a static if that gets evaluated when the macro gets inserted so it has no runtime cost at all
[01:16:06.780 --> 01:16:08.780]   Uh
[01:16:08.780 --> 01:16:12.060]   If if this is false because it's not even there if it's true
[01:16:12.620 --> 01:16:15.740]   Um, then the only cost is this additional instruction
[01:16:15.740 --> 01:16:21.180]   Um
[01:16:21.180 --> 01:16:25.100]   So then if the slot's zero we continue, uh, blah, blah, blah, right so
[01:16:25.100 --> 01:16:31.580]   Down here I have these underscore versions because I wanted to um, I wanted to demo both of these
[01:16:31.580 --> 01:16:34.780]   So we have this version that called only a set without an underscore
[01:16:34.780 --> 01:16:41.580]   And then these with underscores that are defined as baking only set or unset for a given target value, right?
[01:16:41.980 --> 01:16:46.780]   So this has five arguments once we bake out the target value, it has four arguments
[01:16:46.780 --> 01:16:48.860]   Which is the right number for an iterator, right?
[01:16:48.860 --> 01:16:53.740]   I don't think we should I don't know if we should call these iterators, but it's the terminology I'm used to
[01:16:53.740 --> 01:16:55.660]   um
[01:16:55.660 --> 01:16:58.220]   So only set is only set or unset with true
[01:16:58.220 --> 01:17:04.540]   Only unset is only set or unset with false. These are both still macros just like before and they expand out
[01:17:04.540 --> 01:17:07.980]   Uh, as you use them. So here we're using our underscore
[01:17:08.460 --> 01:17:12.620]   underscore version of only set so it should produce the same output as this one
[01:17:12.620 --> 01:17:15.180]   um
[01:17:15.180 --> 01:17:20.540]   And then okay if we if we say only unset on this whole array, it's going to give us, you know
[01:17:20.540 --> 01:17:26.620]   996 numbers, which is too many so I decided to toggle all the bits in the thing and then
[01:17:26.620 --> 01:17:29.900]   Iterate over the only the unset bits
[01:17:29.900 --> 01:17:36.140]   And it should be the same numbers except now they're unset instead of set right just to prove that all this works
[01:17:36.700 --> 01:17:40.620]   And there we go set set unset
[01:17:40.620 --> 01:17:46.220]   Right, this is a very clean and elegant compile time
[01:17:46.220 --> 01:17:54.060]   provably not adding lambda closure style overhead way to
[01:17:54.060 --> 01:17:59.500]   Iterate over a data structure with the same efficiency
[01:17:59.500 --> 01:18:02.300]   As if you'd hard coded it by hand
[01:18:02.300 --> 01:18:04.300]   Right
[01:18:04.300 --> 01:18:06.300]   That's the point of all this
[01:18:07.100 --> 01:18:09.100]   Um
[01:18:09.100 --> 01:18:12.620]   Um, okay
[01:18:12.620 --> 01:18:21.420]   These actually by the way when I when I do these assertions, um, these probably should be compile time asserts
[01:18:21.420 --> 01:18:24.380]   Uh, because then you know
[01:18:24.380 --> 01:18:29.900]   Then you know when you make the mistake. So here we compile but
[01:18:29.900 --> 01:18:33.020]   Uh, if I decide to try to reverse
[01:18:33.020 --> 01:18:35.900]   Um, my only set
[01:18:36.380 --> 01:18:41.340]   This should give me a compile time error. Oh, it didn't. Oh wait, did that one?
[01:18:41.340 --> 01:18:49.980]   Sorry, this is the other version. I got confused by the naming. This one is this one which still only has the runtime asserts
[01:18:49.980 --> 01:18:54.860]   I had a little demo panic there. All right, so
[01:18:54.860 --> 01:19:00.140]   There we go compile time assertion fail here
[01:19:00.140 --> 01:19:05.420]   We're asserting that it's not reverse, right? And so then if I do that
[01:19:06.300 --> 01:19:11.260]   But I do this on only unset again, we'll get a compile time assertion
[01:19:11.260 --> 01:19:14.540]   Line 232 this time instead of 207, right?
[01:19:14.540 --> 01:19:18.700]   Okay
[01:19:18.700 --> 01:19:22.380]   This is great
[01:19:22.380 --> 01:19:24.540]   Um
[01:19:24.540 --> 01:19:29.500]   Now
[01:19:29.500 --> 01:19:33.820]   Well, let's keep going. So in the same way that we could define different iterators now
[01:19:34.540 --> 01:19:37.020]   Um, oh, I forgot to mention, right? Of course
[01:19:37.020 --> 01:19:42.540]   I sort of glided over it, but the way that you specify the name is just with this colon in the four
[01:19:42.540 --> 01:19:46.780]   Right, you just say colon and then the name of the iterator you want to use
[01:19:46.780 --> 01:19:52.380]   And that works syntactically with these things, right? It's just another one of those things
[01:19:52.380 --> 01:19:55.420]   Okay
[01:19:55.420 --> 01:19:58.860]   So remember that back when we iterated the binary tree
[01:19:58.860 --> 01:20:03.820]   Um, it didn't go in any particular order, right? Which one was the?
[01:20:04.780 --> 01:20:07.180]   It was all the countries was our binary tree
[01:20:07.180 --> 01:20:11.580]   How frickin far back was that? No, that's
[01:20:11.580 --> 01:20:18.140]   Okay, it was here
[01:20:18.140 --> 01:20:21.900]   So we were iterating these but our iterator wasn't trying that hard to be ordered
[01:20:21.900 --> 01:20:25.100]   So and in fact, it was like a
[01:20:25.100 --> 01:20:29.500]   Pre-order traversal, I guess which doesn't really mean that much
[01:20:29.500 --> 01:20:33.500]   Uh, in terms of treating the tree as a sorted thing, right?
[01:20:34.060 --> 01:20:36.060]   But back when I did my hands
[01:20:36.060 --> 01:20:40.380]   concocted tree assembly, I assembled it so that the
[01:20:40.380 --> 01:20:45.580]   The countries would be in alphabetical order if you did an in-order traversal left or right
[01:20:45.580 --> 01:20:49.980]   Which is also what the output of a binary tree constructor might do, right?
[01:20:49.980 --> 01:20:52.940]   So, um here instead of doing
[01:20:52.940 --> 01:20:58.780]   The pre-order traversal we can do the in-order traversal on our tree root and print in-order
[01:20:58.780 --> 01:21:03.260]   And you note that we get the countries in alphabetical order now, right?
[01:21:03.980 --> 01:21:05.980]   Um
[01:21:05.980 --> 01:21:10.540]   Let's look at that for a second. It's very close to the same thing
[01:21:10.540 --> 01:21:15.820]   It's just a different iteration
[01:21:15.820 --> 01:21:19.900]   Uh, it's a few more lines because it's just a little
[01:21:19.900 --> 01:21:28.220]   So when you take computer science class in college and you learn about binary trees
[01:21:28.700 --> 01:21:33.580]   You learn about pre-order and in-order and post-order and they're usually defined recursively
[01:21:33.580 --> 01:21:36.220]   um
[01:21:36.220 --> 01:21:41.420]   The problem with doing them recursively is it's kind of inefficient and again it forces you
[01:21:41.420 --> 01:21:46.540]   If you want to pass an operation to that, you have to define it as a function
[01:21:46.540 --> 01:21:48.700]   to pass in
[01:21:48.700 --> 01:21:53.420]   Um here, we don't have to define it as a function, right? Again, it's a macro. We paste it in
[01:21:53.420 --> 01:21:56.540]   So this is our pre-order expansion
[01:21:58.060 --> 01:22:00.460]   This is our in-order expansion and the main difference
[01:22:00.460 --> 01:22:03.660]   aside from slightly juggling
[01:22:03.660 --> 01:22:10.140]   Things around is that you basically chase down the left side of the tree first whenever you can
[01:22:10.140 --> 01:22:15.340]   And then you go to the right when you can't go left and you rehearse back up
[01:22:15.340 --> 01:22:22.700]   Anyway, so this is an iterative version of a pre-order traversal iterative version of an in-order traversal, all right?
[01:22:22.700 --> 01:22:25.340]   so
[01:22:25.340 --> 01:22:28.380]   What if we want to be able to reverse that? Uh, well
[01:22:28.380 --> 01:22:31.900]   Uh, sometimes it's very easy to implement, right?
[01:22:31.900 --> 01:22:34.220]   um
[01:22:34.220 --> 01:22:38.460]   Like like with that bit array version where reversal was hard and annoying
[01:22:38.460 --> 01:22:43.740]   Because it was so many lines until we decided to use the subscript operator and then it was like very short
[01:22:43.740 --> 01:22:46.460]   um here
[01:22:46.460 --> 01:22:50.140]   Uh, we have this thing called reverse, right?
[01:22:50.700 --> 01:22:54.380]   and we're gonna basically just say instead of saying
[01:22:54.380 --> 01:22:57.900]   node equals no dot left and node equals no dot right, we're gonna say
[01:22:57.900 --> 01:23:03.820]   Go left or right depending on the value of this and we knot it here and we don't knot it here
[01:23:03.820 --> 01:23:05.740]   So we go in opposite directions
[01:23:05.740 --> 01:23:09.420]   So if we're not reversing node equals no dot left, otherwise node equals no dot right
[01:23:09.420 --> 01:23:13.340]   And if we're not reversing node equals no dot right, otherwise node equals no dot left
[01:23:13.340 --> 01:23:17.740]   People were asking can you call a macro from a macro and the answer is yes
[01:23:17.820 --> 01:23:21.740]   We made this macro called left or right. It's in fact locally scoped helper
[01:23:21.740 --> 01:23:29.020]   And it just it's just got a static if so that we don't incur compile time cost or runtime cost
[01:23:29.020 --> 01:23:32.620]   And we say did I have been saying compile time cost the whole time?
[01:23:32.620 --> 01:23:37.100]   Sorry, uh runtime cost there's no runtime cost to this
[01:23:37.100 --> 01:23:41.900]   Uh, if left we pick this statement, otherwise this statement, right?
[01:23:41.900 --> 01:23:47.020]   This is a very much more structured than see it's easy to know when you make a mistake
[01:23:47.500 --> 01:23:51.580]   It's hygienic. Um, and yet it does all the right thing so
[01:23:51.580 --> 01:23:59.980]   So this iterator supports both reversal and taking by a pointer or not that part's easy. That's right there
[01:23:59.980 --> 01:24:02.460]   just like before so
[01:24:02.460 --> 01:24:08.380]   Here's our in order with the countries in order and here's our reverse with the countries reversed, right?
[01:24:08.380 --> 01:24:11.420]   um
[01:24:14.860 --> 01:24:17.180]   Whoops, that's done right there
[01:24:17.180 --> 01:24:23.820]   And I think oh that's almost everything I wanted to demo
[01:24:23.820 --> 01:24:27.180]   There are some questions like what happens if you concoct
[01:24:27.180 --> 01:24:30.060]   intranate insertions
[01:24:30.060 --> 01:24:33.660]   Uh, so for example here. I've got a and b
[01:24:33.660 --> 01:24:43.660]   Um, I don't actually use a in here or I don't actually use b. Sorry. I just insert a I don't know why I put b
[01:24:44.460 --> 01:24:46.460]   Um
[01:24:46.460 --> 01:24:50.060]   Oh, right
[01:24:50.060 --> 01:24:54.540]   Yeah, it's it's uh
[01:24:54.540 --> 01:24:58.700]   There
[01:24:58.700 --> 01:25:04.140]   Do this
[01:25:04.140 --> 01:25:10.220]   Yeah, so basically
[01:25:11.820 --> 01:25:15.500]   So a inserts b and b inserts a and we insert a right?
[01:25:15.500 --> 01:25:18.540]   So, uh
[01:25:18.540 --> 01:25:25.420]   Well, that inserts b which inserts a which inserts a inserts a so the compiler is kind of in an infinite
[01:25:25.420 --> 01:25:27.820]   regression, right and it's just going to say like look
[01:25:27.820 --> 01:25:30.380]   You just can't
[01:25:30.380 --> 01:25:37.020]   You just can't do that. I think I set the limit right now at like a hundred deep, but it's going to be configurable at compile time
[01:25:37.020 --> 01:25:40.140]   Um, so you the answer is you get reasonable
[01:25:41.100 --> 01:25:42.780]   messages
[01:25:42.780 --> 01:25:44.940]   For some definition of reasonable when this happens
[01:25:44.940 --> 01:25:51.660]   Uh, I think that's everything that I wanted to say so i'm going to take a voice break for a couple minutes
[01:25:51.660 --> 01:25:54.940]   I'm glad I didn't try to do everything. Okay, so to recap
[01:25:54.940 --> 01:26:02.700]   This is the start of the macro functionality. We haven't covered today how this integrates with the meta programming
[01:26:02.700 --> 01:26:05.340]   features from
[01:26:07.180 --> 01:26:11.580]   Like the the user level extra workspace meta programming stuff
[01:26:11.580 --> 01:26:14.620]   Um, that'll be next time
[01:26:14.620 --> 01:26:20.700]   And we haven't talked about uh more more generic
[01:26:20.700 --> 01:26:26.700]   Um
[01:26:26.700 --> 01:26:33.020]   Like search and replaces on user code the only search and replaces we've demoed today or these that's also going to be next time
[01:26:33.100 --> 01:26:36.780]   So don't really ask about those. Um, if you have other questions
[01:26:36.780 --> 01:26:46.700]   Question and answer time address questions to me
[01:26:46.700 --> 01:26:50.780]   Uh for max visibility
[01:26:50.780 --> 01:26:56.780]   And uh, I'm going to take a four minute break and just make some new tea and stuff and get my voice back
[01:26:56.780 --> 01:26:59.580]   um
[01:26:59.580 --> 01:27:01.660]   And then we'll continue

