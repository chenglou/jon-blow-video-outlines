# Talk: Demo: Iteration and arrays, uninitialized values, enums

Link: https://www.youtube.com/watch?v=-UPFH0eWHEI

Auto-transcribed with Whisper

[00:00:00.000 --> 00:00:02.100]   what this is.
[00:00:02.100 --> 00:00:06.540]   Occasionally, recently, I've been doing more informal live
[00:00:06.540 --> 00:00:09.940]   coding types of things where I worked on compiler features.
[00:00:09.940 --> 00:00:10.940]   That's not what this is.
[00:00:10.940 --> 00:00:13.460]   This is a more formal presentation of work that's been
[00:00:13.460 --> 00:00:16.180]   happening for the last five or six weeks.
[00:00:16.180 --> 00:00:23.260]   I'm going to try to keep it short, if only because
[00:00:23.260 --> 00:00:25.940]   Handmade Hero is coming on at 8 p.m.
[00:00:25.940 --> 00:00:28.140]   But who knows how long it's going to go?
[00:00:28.140 --> 00:00:30.260]   I'll probably stay and do questions afterwards.
[00:00:30.260 --> 00:00:40.700]   Last time, I'd sort of implied that I was going to work toward
[00:00:40.700 --> 00:00:48.420]   doing an object system demo, which I think by the time we
[00:00:48.420 --> 00:00:51.380]   do that, it'll start being really evident, like what the
[00:00:51.380 --> 00:00:54.980]   flavor of this language is or what the spirit of it is and
[00:00:54.980 --> 00:00:56.980]   what makes it so different from many other languages.
[00:00:56.980 --> 00:00:57.980]   Well, that'll be one thing.
[00:00:57.980 --> 00:01:01.420]   I mean, the compile time program execution already
[00:01:01.420 --> 00:01:04.220]   goes pretty far that way.
[00:01:04.220 --> 00:01:08.180]   But I decided, instead of leaping forward and trying to
[00:01:08.180 --> 00:01:12.780]   reach for grand new features, I wanted to make sure that the
[00:01:12.780 --> 00:01:17.620]   base of the language was stable and robust and had a lot of
[00:01:17.620 --> 00:01:20.500]   the stuff that it needs.
[00:01:20.500 --> 00:01:23.580]   Because the problem with a lot of programming languages is
[00:01:23.580 --> 00:01:27.300]   they end up like toy languages, and they don't really do all
[00:01:27.300 --> 00:01:29.980]   the stuff that you want, but they do this one thing that the
[00:01:29.980 --> 00:01:31.700]   author thought was really cool.
[00:01:31.700 --> 00:01:34.260]   And I wanted to make sure that this doesn't end up in the
[00:01:34.260 --> 00:01:36.500]   domain of toy languages.
[00:01:36.500 --> 00:01:42.460]   So I looked at what kind of stuff do I want to do when I'm
[00:01:42.460 --> 00:01:46.020]   programming all the time, and what kinds of things does
[00:01:46.020 --> 00:01:50.460]   this language system need in order to be more robust, and I
[00:01:50.460 --> 00:01:53.020]   worked on that, and just solidifying the core of the
[00:01:53.020 --> 00:01:54.820]   language.
[00:01:54.820 --> 00:01:57.460]   Now, part of that is new features, which I'm going to
[00:01:57.460 --> 00:01:58.420]   demo now.
[00:01:58.420 --> 00:02:02.100]   But part of it is just testing a bunch of different cases,
[00:02:02.100 --> 00:02:05.100]   making sure that things that I demoed in the last demo
[00:02:05.100 --> 00:02:07.940]   really work generally across all sorts of situations that
[00:02:07.940 --> 00:02:10.020]   you would counter and counter in the language.
[00:02:10.020 --> 00:02:12.100]   And of course, as soon as you start testing that stuff, you
[00:02:12.100 --> 00:02:14.540]   realize, oh, it doesn't always.
[00:02:14.540 --> 00:02:19.300]   So like last time, I demoed a dynamic array, and it was not
[00:02:19.300 --> 00:02:20.460]   evident from the demo.
[00:02:20.460 --> 00:02:22.660]   But if you look closely, you'll see that I only ever use it
[00:02:22.660 --> 00:02:25.340]   on things that are of pointer size, and that's because the
[00:02:25.340 --> 00:02:27.860]   code for the dynamic array wasn't generalized to large
[00:02:27.860 --> 00:02:29.380]   objects or things like that.
[00:02:29.380 --> 00:02:30.300]   Now it is.
[00:02:30.300 --> 00:02:33.220]   So I did a bunch of that kind of thing.
[00:02:33.220 --> 00:02:37.820]   But let me show you what I did feature-wise.
[00:02:37.820 --> 00:02:43.260]   The first thing-- oh, yes.
[00:02:43.260 --> 00:02:46.860]   So if you'll recall, the biggest program in this
[00:02:46.860 --> 00:02:52.060]   language so far is this.
[00:02:52.060 --> 00:02:58.460]   So I'm not running it at compile time anymore, because I
[00:02:58.460 --> 00:03:00.780]   don't want to totally show off like that.
[00:03:00.780 --> 00:03:04.860]   But this is the little space invader program, or
[00:03:04.860 --> 00:03:08.260]   Gallagher program, where you can fly around.
[00:03:08.260 --> 00:03:10.420]   It's got a little more functionality than I demoed
[00:03:10.420 --> 00:03:12.540]   before, like these guys have slightly different movement
[00:03:12.540 --> 00:03:14.820]   patterns, and they actually shoot back and stuff like that,
[00:03:14.820 --> 00:03:17.420]   and I can explode.
[00:03:17.420 --> 00:03:20.940]   But it's basically the same scale of program as it was
[00:03:20.940 --> 00:03:21.780]   before.
[00:03:21.780 --> 00:03:27.940]   And last time, I demoed using that program some ways that
[00:03:27.940 --> 00:03:30.780]   compile time program execution could help build your
[00:03:30.780 --> 00:03:32.180]   program.
[00:03:32.180 --> 00:03:35.500]   Let me go into there.
[00:03:35.500 --> 00:03:42.140]   I demoed some stuff like running a function that includes
[00:03:42.140 --> 00:03:47.340]   files dynamically instead of using a directive, and I demoed
[00:03:47.340 --> 00:03:49.700]   writing a preprocessor that processes those.
[00:03:49.700 --> 00:03:52.260]   But I didn't quite get to the level of really how you would
[00:03:52.260 --> 00:03:56.380]   interface with that with the command line on a day-to-day
[00:03:56.380 --> 00:04:00.220]   fashion, and how you would really build a program on a
[00:04:00.220 --> 00:04:01.140]   team of other people.
[00:04:01.140 --> 00:04:03.420]   And so I wanted to continue the build option stuff a little
[00:04:03.420 --> 00:04:07.340]   bit further and show you exactly how that would work
[00:04:07.340 --> 00:04:08.820]   in the real world.
[00:04:08.820 --> 00:04:14.820]   So here, I have a situation that's much like what we did
[00:04:14.820 --> 00:04:15.820]   last time.
[00:04:15.820 --> 00:04:19.180]   This run directive down here says that at compile time,
[00:04:19.180 --> 00:04:22.620]   when this run directive is hit, we're going to run this
[00:04:22.620 --> 00:04:25.100]   function and anything else that needs to get compiled in
[00:04:25.100 --> 00:04:26.820]   order to make that function work.
[00:04:26.820 --> 00:04:30.860]   Now, we'll open this so you can see what we printed over
[00:04:30.860 --> 00:04:31.940]   here.
[00:04:31.940 --> 00:04:34.820]   So we've got a few print Fs as soon as we start building.
[00:04:34.820 --> 00:04:38.260]   This file path thing is just a directive that tells you what
[00:04:38.260 --> 00:04:40.500]   the path is of the current file that's being compiled.
[00:04:40.500 --> 00:04:43.340]   And I assigned it to a variable just to make it clear that it
[00:04:43.340 --> 00:04:45.940]   wasn't some weird print F thing like it might look like if I
[00:04:45.940 --> 00:04:46.940]   inserted it in here.
[00:04:46.940 --> 00:04:48.620]   So we're just printing out the file path of the current
[00:04:48.620 --> 00:04:49.300]   file.
[00:04:49.300 --> 00:04:51.300]   We see that.
[00:04:51.300 --> 00:04:53.460]   And then we've got this build option stuff, which are
[00:04:53.460 --> 00:04:57.060]   variables that I can set to tell the compiler how to behave.
[00:04:57.060 --> 00:04:59.500]   But I'm doing that from my own program.
[00:04:59.500 --> 00:05:02.660]   I'm not doing it from an external program like Make or
[00:05:02.660 --> 00:05:06.100]   Visual Studio or anything like that.
[00:05:06.100 --> 00:05:09.700]   By the time you write a complex video game or web
[00:05:09.700 --> 00:05:13.540]   server or anything that's complicated, right, you're
[00:05:13.540 --> 00:05:16.460]   specifying a lot of very careful actions that that
[00:05:16.460 --> 00:05:18.940]   program has to perform perfectly, right?
[00:05:18.940 --> 00:05:21.220]   Compared to what it actually does when it's running,
[00:05:21.220 --> 00:05:23.060]   building the program ought to be really simple.
[00:05:23.060 --> 00:05:26.780]   So the idea that you need some totally separate system to
[00:05:26.780 --> 00:05:29.300]   specify the building of the program is a little bit
[00:05:29.300 --> 00:05:29.780]   absurd.
[00:05:29.780 --> 00:05:30.860]   It's silly, right?
[00:05:30.860 --> 00:05:32.540]   Because you're already able to specify things that are
[00:05:32.540 --> 00:05:33.940]   much more complicated.
[00:05:33.940 --> 00:05:36.940]   And you're just making the situation a lot more
[00:05:36.940 --> 00:05:40.060]   needlessly complicated as soon as you involve a Make
[00:05:40.060 --> 00:05:43.660]   language or an IDE with special menus that make it
[00:05:43.660 --> 00:05:46.060]   really hard to change all the options and all the
[00:05:46.060 --> 00:05:47.180]   variants of your build, right?
[00:05:47.180 --> 00:05:49.180]   So I'm going to show in a more practical way how you would
[00:05:49.180 --> 00:05:50.100]   approach this.
[00:05:50.100 --> 00:05:53.180]   So right now, say I'm just messing around as a single
[00:05:53.180 --> 00:05:53.580]   person.
[00:05:53.580 --> 00:05:55.500]   I'm getting my project up and running.
[00:05:55.500 --> 00:05:57.060]   So I'm doing things like this.
[00:05:57.060 --> 00:05:58.260]   I'm saying, run this build.
[00:05:58.260 --> 00:06:00.380]   I've only got one kind of build that I'm doing.
[00:06:00.380 --> 00:06:01.300]   And here's what I want.
[00:06:01.300 --> 00:06:02.940]   I want it to be a debug build, right?
[00:06:02.940 --> 00:06:06.980]   So I'm setting build options, optimization level to debug.
[00:06:06.980 --> 00:06:10.620]   Emit line directives is a thing that's for me.
[00:06:10.620 --> 00:06:12.100]   We're generating C code right now.
[00:06:12.100 --> 00:06:15.060]   And line directives will make, if there's an error in the C
[00:06:15.060 --> 00:06:20.300]   code, point back to this, right, so that I can debug.
[00:06:20.300 --> 00:06:23.180]   But if you're actually working on the compiler, you may
[00:06:23.180 --> 00:06:25.700]   just want to jump into that program and see which line of
[00:06:25.700 --> 00:06:28.780]   the output is messed up in the C file.
[00:06:28.780 --> 00:06:31.380]   So this is not really a user level feature, but it's one
[00:06:31.380 --> 00:06:33.700]   that I've put in for my own convenience for a while.
[00:06:33.700 --> 00:06:35.820]   I can set the executable name, right?
[00:06:35.820 --> 00:06:37.820]   And then I call update build options just to tell the
[00:06:37.820 --> 00:06:40.940]   compiler, hey, some of these settings have changed.
[00:06:40.940 --> 00:06:43.380]   And it'll change its behavior accordingly, right?
[00:06:43.380 --> 00:06:45.900]   Then I'm going to set my path and say, look for any other
[00:06:45.900 --> 00:06:49.460]   files that I add in the same folder as the current file
[00:06:49.460 --> 00:06:50.620]   that's being compiled.
[00:06:50.620 --> 00:06:52.540]   And then we add all this stuff, right?
[00:06:52.540 --> 00:07:00.020]   And so now what happens is you can see here we print our
[00:07:00.020 --> 00:07:01.140]   debug information.
[00:07:01.140 --> 00:07:02.580]   We've added all these files.
[00:07:02.580 --> 00:07:06.180]   And you can see here if you know CL.exe's options in
[00:07:06.180 --> 00:07:08.020]   Windows, this is a debug build, right?
[00:07:08.020 --> 00:07:11.140]   It's optimization level debug.
[00:07:11.140 --> 00:07:12.420]   Is there any other settings?
[00:07:12.420 --> 00:07:17.100]   No, but now I'll set this to release, right?
[00:07:17.100 --> 00:07:20.540]   And I'll change it, right?
[00:07:20.540 --> 00:07:21.460]   So now it's a release build.
[00:07:21.460 --> 00:07:24.420]   It's slash O2, which sets the optimization level.
[00:07:24.420 --> 00:07:29.140]   And Z7, which includes debug information, I suppose that
[00:07:29.140 --> 00:07:31.380]   should have been in the other build, too.
[00:07:31.380 --> 00:07:37.700]   Anyway, the point being you can toggle those settings
[00:07:37.700 --> 00:07:38.620]   from here, right?
[00:07:38.620 --> 00:07:39.660]   And that's pretty easy.
[00:07:39.660 --> 00:07:41.780]   Now that's what you might want to do when you're informal,
[00:07:41.780 --> 00:07:44.060]   right, and messing around with a project on your own.
[00:07:44.060 --> 00:07:46.460]   But then what do you do when you're on a team of people?
[00:07:46.460 --> 00:07:50.580]   And you've got eight people using source control.
[00:07:50.580 --> 00:07:57.140]   And if you edit your source file like this to change your
[00:07:57.140 --> 00:07:58.940]   build settings, you're going to be conflicting with other
[00:07:58.940 --> 00:08:02.220]   people's build settings, you need a canonical thing that's
[00:08:02.220 --> 00:08:04.780]   not going to change that everyone can refer to, right?
[00:08:04.780 --> 00:08:06.860]   So what you do is you factor your code a little bit.
[00:08:06.860 --> 00:08:10.020]   I mean, you could have some totally external file that's
[00:08:10.020 --> 00:08:12.420]   also part of this program that's not in source control that
[00:08:12.420 --> 00:08:13.500]   you personally use.
[00:08:13.500 --> 00:08:15.220]   And you can do that.
[00:08:15.220 --> 00:08:17.660]   But the cleanest way to do it is probably this.
[00:08:17.660 --> 00:08:20.220]   So instead of having one function called build, I'm
[00:08:20.220 --> 00:08:21.420]   going to factor it.
[00:08:21.420 --> 00:08:24.620]   There's going to be a function called build common that it's
[00:08:24.620 --> 00:08:26.260]   going to print our debugging info, and then it's going to
[00:08:26.260 --> 00:08:28.620]   add all these build files.
[00:08:28.620 --> 00:08:30.380]   Because we want to do that no matter what.
[00:08:30.380 --> 00:08:33.100]   But then we're going to have these functions build debug
[00:08:33.100 --> 00:08:36.980]   and build release that call build common and then do
[00:08:36.980 --> 00:08:40.020]   whatever else we want to that's in particular, right?
[00:08:40.020 --> 00:08:42.060]   So for this, it's just setting the optimization level.
[00:08:42.060 --> 00:08:44.260]   This was redundant in both of these.
[00:08:44.260 --> 00:08:46.940]   But the point is anything in build debug can be anything
[00:08:46.940 --> 00:08:47.820]   that I want.
[00:08:47.820 --> 00:08:50.580]   Anything in build release can be anything that I want, right?
[00:08:50.580 --> 00:08:53.860]   Now I still would have to, right, if I was doing this the
[00:08:53.860 --> 00:08:58.420]   way I was doing before, I'd say run, build debug, right?
[00:08:58.420 --> 00:09:01.140]   Or run, build release.
[00:09:01.140 --> 00:09:03.340]   And maybe I comment or uncomment these, but that's still
[00:09:03.340 --> 00:09:05.900]   annoying to other people trying to work on the program.
[00:09:05.900 --> 00:09:08.660]   So instead, you can specify that from the command line.
[00:09:08.660 --> 00:09:12.860]   So right now, if I try to compile this right now, no
[00:09:12.860 --> 00:09:15.460]   compile time program execution is going to happen, because I
[00:09:15.460 --> 00:09:18.660]   took out that build directive or that run directive.
[00:09:18.660 --> 00:09:20.820]   So it's not going to be able to compile anything, right?
[00:09:20.820 --> 00:09:25.380]   It's like you're trying to call some function invaders, which
[00:09:25.380 --> 00:09:26.820]   I guess is in main.
[00:09:26.820 --> 00:09:28.620]   You're trying to call invaders, and I don't even know what
[00:09:28.620 --> 00:09:31.620]   that is, because we never even added any files, right?
[00:09:31.620 --> 00:09:36.820]   But you specify on the command line dash run, build debug, right?
[00:09:36.820 --> 00:09:39.140]   This can actually be any expression in the whole
[00:09:39.140 --> 00:09:40.620]   programming language.
[00:09:40.620 --> 00:09:42.460]   You'd probably put it in quotes if it got really
[00:09:42.460 --> 00:09:45.020]   complicated, but there we go.
[00:09:45.020 --> 00:09:46.860]   So we've got a debug build, right?
[00:09:46.860 --> 00:09:50.780]   Or I can see on the command line dash run, build release, right?
[00:09:50.780 --> 00:09:53.820]   And all this says is, call the function build release at
[00:09:53.820 --> 00:09:58.980]   compile time, and then continue building after that, right?
[00:09:58.980 --> 00:10:01.180]   So that's cool.
[00:10:01.180 --> 00:10:02.580]   Here we've got our release options.
[00:10:02.580 --> 00:10:04.220]   Here we've got our debug options.
[00:10:04.220 --> 00:10:09.500]   So at that point, you've got a pretty nice situation.
[00:10:09.500 --> 00:10:12.180]   You can have many people working on the same program.
[00:10:12.180 --> 00:10:15.100]   All you need to do is just have this command to compile,
[00:10:15.100 --> 00:10:18.580]   bound to something, or in your history, or whatever.
[00:10:18.580 --> 00:10:21.060]   And you don't need an IDE to build.
[00:10:21.060 --> 00:10:23.220]   You don't need a make system to build, right?
[00:10:23.220 --> 00:10:26.260]   It's all right here.
[00:10:26.260 --> 00:10:29.420]   So that's very exciting.
[00:10:29.420 --> 00:10:34.460]   All right, I guess I'll take questions on all of these
[00:10:34.460 --> 00:10:35.660]   things at the end.
[00:10:35.660 --> 00:10:38.180]   Because this is broken into specific sections, it would
[00:10:38.180 --> 00:10:42.620]   probably be better for questions if I took them now, but
[00:10:42.620 --> 00:10:45.020]   we've got a schedule to stick on.
[00:10:45.020 --> 00:10:50.940]   All right, so as part two, let me put this in C++ mode, so
[00:10:50.940 --> 00:10:52.140]   we get a little syntax highlighting.
[00:10:52.140 --> 00:10:54.700]   Syntax highlighting, again, is going to be a little wrong
[00:10:54.700 --> 00:10:57.700]   because eMax thinks this is C++, but what can you do?
[00:10:57.700 --> 00:11:00.460]   All right, since last time there have been syntactic
[00:11:00.460 --> 00:11:02.700]   changes to the language, and I want to explain what those are
[00:11:02.700 --> 00:11:09.180]   and why they were done, the main motivation really is to
[00:11:09.180 --> 00:11:11.860]   clean things up and get rid of some inconsistencies that are
[00:11:11.860 --> 00:11:18.660]   in C and C++ syntax, mostly to do with the way arrays
[00:11:18.660 --> 00:11:20.620]   and pointers are declared.
[00:11:20.620 --> 00:11:29.620]   So I shifted to a syntax where arrays and pointers get
[00:11:29.620 --> 00:11:32.540]   introduced on the left of a type definition, much the way
[00:11:32.540 --> 00:11:34.460]   that Go does.
[00:11:34.460 --> 00:11:37.540]   Whereas for C, like part of the type, if it's a float or
[00:11:37.540 --> 00:11:40.020]   whatever, goes on the left, and then the array part goes
[00:11:40.020 --> 00:11:41.940]   on the right and the name goes in the middle.
[00:11:41.940 --> 00:11:43.580]   So we're getting rid of all that, right?
[00:11:43.580 --> 00:11:48.060]   When you're doing that, when you're trying to introduce
[00:11:48.060 --> 00:11:50.660]   this consistency and either put things on one side or the
[00:11:50.660 --> 00:11:52.180]   other, you kind of have to pick which side.
[00:11:52.180 --> 00:11:53.900]   Is it on the left or is it on the right?
[00:11:53.900 --> 00:11:58.140]   I went on the left because that gets rid of a couple of
[00:11:58.140 --> 00:12:01.980]   annoying syntactic ambiguities, but I could do it on the
[00:12:01.980 --> 00:12:04.740]   right as well, maybe it could change.
[00:12:04.740 --> 00:12:06.740]   But let me just explain what this syntax is.
[00:12:06.740 --> 00:12:09.980]   I don't think syntax is still super important for now,
[00:12:09.980 --> 00:12:11.580]   simply because it's so malleable, right?
[00:12:11.580 --> 00:12:13.540]   We're not deciding on a final thing.
[00:12:13.540 --> 00:12:16.780]   But I want to explain to you what it looks like so that the
[00:12:16.780 --> 00:12:18.620]   rest of these examples make sense.
[00:12:18.620 --> 00:12:23.540]   Now the first thing is the way pointers are declared, right?
[00:12:23.540 --> 00:12:27.220]   C in C and C++, if you declare a pointer, you use a star.
[00:12:27.220 --> 00:12:29.420]   And a star is also what you use to dereference.
[00:12:29.420 --> 00:12:32.580]   And the philosophy in C is that you're like writing a
[00:12:32.580 --> 00:12:35.580]   declaration of the way that you would use it.
[00:12:35.580 --> 00:12:39.300]   And that's really confusing, actually, and I don't think
[00:12:39.300 --> 00:12:40.780]   it's helpful at all.
[00:12:40.780 --> 00:12:46.140]   So what I've switched to is writing the declaration as
[00:12:46.140 --> 00:12:49.020]   what it is, and then writing usage as what it is, right?
[00:12:49.020 --> 00:12:52.420]   So here I've got some struct called entity, and I've
[00:12:52.420 --> 00:12:55.060]   got an E that's an entity, and now I'm going to declare the
[00:12:55.060 --> 00:12:57.260]   type of something called pointer, right?
[00:12:57.260 --> 00:13:01.460]   Pointer is of the type pointer to an entity, right?
[00:13:01.460 --> 00:13:07.860]   So this ampersand, right, in C, this would be the equivalent
[00:13:07.860 --> 00:13:11.580]   of an ampersand, or at least down here it would be, right?
[00:13:11.580 --> 00:13:13.100]   In C you would say that.
[00:13:13.100 --> 00:13:15.100]   Here I'm saying that.
[00:13:15.100 --> 00:13:17.620]   I didn't want to use ampersands everywhere, because I
[00:13:17.620 --> 00:13:19.900]   think that's kind of ugly looking if you're going to put
[00:13:19.900 --> 00:13:21.740]   them in all your declarations all the time.
[00:13:21.740 --> 00:13:25.420]   So I just did this, which is the way in Pascal that you write
[00:13:25.420 --> 00:13:28.460]   a pointer, and it's pointy, and it's symmetric, so it looks
[00:13:28.460 --> 00:13:29.580]   nice.
[00:13:29.580 --> 00:13:32.940]   But here's the point of this pointer declaration, which
[00:13:32.940 --> 00:13:39.100]   is just I'm declaring a pointer to a type, and this is a
[00:13:39.100 --> 00:13:40.540]   pointer to a value.
[00:13:40.540 --> 00:13:42.420]   It's the same thing, right?
[00:13:42.420 --> 00:13:46.500]   Like the up arrow means I'm going in this way of abstracting
[00:13:46.500 --> 00:13:49.020]   myself away from the type of value, right?
[00:13:49.020 --> 00:13:51.860]   And then we still dereference stuff with star down here.
[00:13:51.860 --> 00:13:55.220]   So star brings us toward the value, and hat brings us away
[00:13:55.220 --> 00:13:58.620]   from the value, and it's very consistent, right?
[00:13:58.620 --> 00:14:01.660]   So I like that a lot.
[00:14:01.660 --> 00:14:07.260]   It only takes five minutes to get used to, and it's cool.
[00:14:07.260 --> 00:14:09.980]   So now, because we have type inference and stuff here, I
[00:14:09.980 --> 00:14:13.380]   don't have to, again, declare the type of this, and then
[00:14:13.380 --> 00:14:13.900]   assign it.
[00:14:13.900 --> 00:14:18.540]   I can just do all that in one step, right?
[00:14:18.540 --> 00:14:21.940]   But for purposes of showing this parallelism, I wanted to do
[00:14:21.940 --> 00:14:23.060]   this in two lines.
[00:14:23.060 --> 00:14:25.220]   OK, arrays.
[00:14:25.220 --> 00:14:27.940]   I wanted to put everything on the left, and it seems to
[00:14:27.940 --> 00:14:30.580]   make sense to me anyway, like if you want to say something is
[00:14:30.580 --> 00:14:35.100]   an array of some type, the array part is probably pretty
[00:14:35.100 --> 00:14:35.940]   important, right?
[00:14:35.940 --> 00:14:39.580]   So you want to see that first.
[00:14:39.580 --> 00:14:43.780]   So, you know, this is an array, an n long, right?
[00:14:43.780 --> 00:14:48.100]   We declared n up here as a constant that's 10, right?
[00:14:48.100 --> 00:14:51.420]   This is an n long array of floats.
[00:14:51.420 --> 00:14:54.780]   You know, this is a pointer to an entity, or an address of
[00:14:54.780 --> 00:14:56.020]   an entity, right?
[00:14:56.020 --> 00:15:00.540]   And this is an n long array of addresses of entities.
[00:15:00.540 --> 00:15:03.740]   And you can write these things very neatly, and you can
[00:15:03.740 --> 00:15:05.660]   read them off in the order in which they're written.
[00:15:05.660 --> 00:15:14.380]   So, like I said, de-referencing is the same as in C right now,
[00:15:14.380 --> 00:15:17.300]   but for the purposes of conserving operators and maybe
[00:15:17.300 --> 00:15:21.540]   being clearer anyway, we might decide to go full Pascal and
[00:15:21.540 --> 00:15:23.620]   de-reference, I think this is what you do in Pascal, to
[00:15:23.620 --> 00:15:25.860]   de-reference as you put the pointer operator on the other
[00:15:25.860 --> 00:15:27.700]   side.
[00:15:27.700 --> 00:15:29.620]   I don't know, I haven't felt the need to do that yet, but
[00:15:29.620 --> 00:15:31.420]   it's a possibility, right?
[00:15:31.420 --> 00:15:33.700]   So then down here, you know, I declared these pointers, so
[00:15:33.700 --> 00:15:35.500]   I might as well use them, you know, I'm setting them to new
[00:15:35.500 --> 00:15:37.180]   entity or whatever.
[00:15:37.180 --> 00:15:45.620]   Okay, we can run that part of the demo, but it's not going
[00:15:45.620 --> 00:15:48.100]   to do anything, because it didn't print anything out.
[00:15:48.100 --> 00:15:51.260]   You'll note down here that we're getting some uninitialized
[00:15:51.260 --> 00:15:53.980]   value complaints, those are actually from the C compiler,
[00:15:53.980 --> 00:15:56.300]   and they're not a mistake, they're there for good reason.
[00:15:56.300 --> 00:15:58.940]   And we'll get to that in the next section, part three.
[00:15:58.940 --> 00:16:05.980]   So I want to talk about default values.
[00:16:05.980 --> 00:16:12.820]   C and C++, someone's saying that hat is in the middle of
[00:16:12.820 --> 00:16:15.340]   the keyboard and is harder to press, it might be a little
[00:16:15.340 --> 00:16:19.500]   bit, although it's actually, it's about as close to the left
[00:16:19.500 --> 00:16:23.020]   hand as ampersand is to the right hand.
[00:16:23.020 --> 00:16:24.620]   So I think it's just a matter of what you're used to.
[00:16:24.620 --> 00:16:26.540]   Like I thought it was harder to press for the first few
[00:16:26.540 --> 00:16:28.900]   minutes, and now I don't think that.
[00:16:28.900 --> 00:16:30.940]   Although that is an important consideration, and you know,
[00:16:30.940 --> 00:16:33.660]   like I said, that particular syntax doesn't have to stay,
[00:16:33.660 --> 00:16:37.660]   it's just what I'm using right now, because it looks nice.
[00:16:37.660 --> 00:16:47.380]   Anyway, C and C++ don't initialize plain old data values,
[00:16:47.380 --> 00:16:50.460]   unless you initialize them manually, and this is a
[00:16:50.460 --> 00:16:52.020]   performance concern, right?
[00:16:52.020 --> 00:16:54.500]   A lot of higher level languages will automatically set your
[00:16:54.500 --> 00:16:59.100]   values to zero or something like that.
[00:16:59.100 --> 00:17:04.980]   In this language, that's, we try to find a new compromise
[00:17:04.980 --> 00:17:08.320]   that works better for everybody, right?
[00:17:08.320 --> 00:17:12.140]   The thing about C or C++ is it's very easy to make a
[00:17:12.140 --> 00:17:15.780]   mistake and forget to initialize your variable and then go
[00:17:15.780 --> 00:17:18.140]   use it, and sometimes the compiler can detect that and
[00:17:18.140 --> 00:17:20.140]   tell you that you made a mistake, but sometimes the
[00:17:20.140 --> 00:17:23.060]   compiler won't detect that, and then you've got a little bit
[00:17:23.060 --> 00:17:27.580]   of a debugging session for you, and you know, but that's
[00:17:27.580 --> 00:17:28.380]   important, right?
[00:17:28.380 --> 00:17:30.380]   Like those of us who try to build things that are
[00:17:30.380 --> 00:17:34.420]   performant, we want that ability to not have things be
[00:17:34.420 --> 00:17:36.540]   initialized, because if you're going to instantiate a giant
[00:17:36.540 --> 00:17:39.700]   array of stuff and fill it, you don't want to have to fill it
[00:17:39.700 --> 00:17:43.100]   like twice, you know, first with zero, and then with the
[00:17:43.100 --> 00:17:45.820]   actual stuff that you want in it, and take all those cash
[00:17:45.820 --> 00:17:46.500]   measures and stuff.
[00:17:46.500 --> 00:17:49.900]   So the question was, what can we do that gives us the best
[00:17:49.900 --> 00:17:51.500]   of both worlds?
[00:17:51.500 --> 00:17:53.700]   So some of you guys may have seen me implement this the
[00:17:53.700 --> 00:17:56.020]   other week.
[00:17:56.020 --> 00:17:59.380]   The way it works now is every single thing in the language
[00:17:59.380 --> 00:18:02.260]   always gets initialized to its default value, which is
[00:18:02.260 --> 00:18:06.020]   usually zero unless you specify a different default value.
[00:18:06.020 --> 00:18:12.860]   But for performance reasons, you have the ability to say,
[00:18:12.860 --> 00:18:16.660]   well, actually, let's not initialize this thing, and you
[00:18:16.660 --> 00:18:18.380]   can do that in a couple of different ways, depending on
[00:18:18.380 --> 00:18:22.380]   what you need to do, and I'll show you how that works now.
[00:18:22.380 --> 00:18:25.620]   So I'm going to repeat the same sort of situation four times
[00:18:25.620 --> 00:18:26.420]   just a demo thing.
[00:18:26.420 --> 00:18:29.780]   So first, we're going to show default values, which, again,
[00:18:29.780 --> 00:18:30.740]   I demoed last time.
[00:18:30.740 --> 00:18:33.900]   So I can say F is a float that's equal to three.
[00:18:33.900 --> 00:18:36.260]   When I print F that, it'll tell me it's three.
[00:18:36.260 --> 00:18:38.740]   I can declare a vector three with three floating point
[00:18:38.740 --> 00:18:42.060]   values, and they each have default values of one, four,
[00:18:42.060 --> 00:18:43.500]   and nine, which is a little bit silly.
[00:18:43.500 --> 00:18:45.860]   I don't think I would ever make a vector three that way in
[00:18:45.860 --> 00:18:46.860]   real life.
[00:18:46.860 --> 00:18:50.780]   But it's a good test.
[00:18:50.780 --> 00:18:52.260]   So we're going to instantiate a vector three.
[00:18:52.260 --> 00:18:55.140]   We're going to print the values.
[00:18:55.140 --> 00:18:56.540]   We're going to make 100 vector threes.
[00:18:56.540 --> 00:18:59.340]   We're going to pull one out of the middle, or approximately
[00:18:59.340 --> 00:19:01.260]   the middle, the 50th element.
[00:19:01.260 --> 00:19:02.820]   And we're going to print the value of that.
[00:19:02.820 --> 00:19:07.580]   And so did I compile that this time?
[00:19:07.580 --> 00:19:09.180]   Yeah.
[00:19:09.180 --> 00:19:12.220]   So here's the first thing, and you can see that that worked.
[00:19:12.220 --> 00:19:17.460]   So F is three, the first vector, V1, is 1, 4, and 9.
[00:19:17.460 --> 00:19:18.780]   So it got the default values.
[00:19:18.780 --> 00:19:22.660]   And the second vector here is also 1, 4, and 9.
[00:19:22.660 --> 00:19:25.700]   So all of these 100 vector threes in this array
[00:19:25.700 --> 00:19:28.660]   got initialized, which might be what you want, if you want
[00:19:28.660 --> 00:19:29.380]   that for convenience.
[00:19:29.380 --> 00:19:32.780]   But you definitely don't always want that.
[00:19:32.780 --> 00:19:34.100]   But we'll get to that.
[00:19:34.100 --> 00:19:37.100]   So the second thing that happens is, suppose we don't want
[00:19:37.100 --> 00:19:39.340]   to specify all these default values, because maybe a lot
[00:19:39.340 --> 00:19:43.500]   of time in our program, we want things to be zero anyway.
[00:19:43.500 --> 00:19:48.100]   Well, in this implicit default value section,
[00:19:48.100 --> 00:19:49.460]   we're doing the same thing, but we're
[00:19:49.460 --> 00:19:50.900]   omitting those non-zero values.
[00:19:50.900 --> 00:19:52.260]   So F is a float.
[00:19:52.260 --> 00:19:54.340]   So now it's going to be zero when we print it out.
[00:19:54.340 --> 00:19:55.380]   X, Y, and Z are floats.
[00:19:55.380 --> 00:19:56.820]   We haven't given them default values.
[00:19:56.820 --> 00:19:58.420]   Oh, by the way, this is a different vector three
[00:19:58.420 --> 00:19:59.780]   than this one.
[00:19:59.780 --> 00:20:03.900]   So we have local structs, and that works just fine,
[00:20:03.900 --> 00:20:08.380]   unlike some other compilers I could mention.
[00:20:08.380 --> 00:20:11.940]   So we're doing exactly the same thing after this.
[00:20:11.940 --> 00:20:14.380]   We're instantiating a vector three, it should be zero.
[00:20:14.380 --> 00:20:16.380]   We're making 100 of them, we're pulling the 50th out,
[00:20:16.380 --> 00:20:17.140]   it should be zero.
[00:20:17.140 --> 00:20:20.020]   And we can look at our run, and in fact, all this stuff was zero.
[00:20:20.020 --> 00:20:27.060]   But now we're going to do a third thing.
[00:20:27.060 --> 00:20:31.020]   We're going to say, we don't want default values.
[00:20:31.020 --> 00:20:33.220]   We want this code to run fast, and so we're
[00:20:33.220 --> 00:20:35.340]   going to explicitly uninitialized everything.
[00:20:35.340 --> 00:20:36.940]   So I have a local float.
[00:20:36.940 --> 00:20:39.700]   Its value is this dash, dash, dash, which
[00:20:39.700 --> 00:20:41.580]   means don't initialize this.
[00:20:41.580 --> 00:20:44.820]   Now I picked that just because it's easy to type,
[00:20:44.820 --> 00:20:46.580]   and it's easy to search for.
[00:20:46.580 --> 00:20:49.300]   When you do this, if you have funny behavior in your program,
[00:20:49.300 --> 00:20:51.820]   you want to be able to search for all the things causing
[00:20:51.820 --> 00:20:54.460]   funny behavior, whether they're this thing,
[00:20:54.460 --> 00:20:55.980]   or a weird cast, or whatever.
[00:20:55.980 --> 00:21:01.180]   So part of my goal is to make all the mildly dangerous things
[00:21:01.180 --> 00:21:04.620]   searchable, but to let you do them easily at any time.
[00:21:04.620 --> 00:21:05.820]   So here's a vector three.
[00:21:05.820 --> 00:21:08.180]   I'm saying don't make x, y, and z zero by default,
[00:21:08.180 --> 00:21:12.740]   make them uninitialized, because we want to go fast.
[00:21:12.740 --> 00:21:13.980]   And again, you might think, well,
[00:21:13.980 --> 00:21:15.980]   isn't that a little bit of work to specify that?
[00:21:15.980 --> 00:21:18.700]   And it's like, well, you have to do the work either way.
[00:21:18.700 --> 00:21:21.940]   In C and C++, you have to do this constant work
[00:21:21.940 --> 00:21:25.700]   of having this mental load of have I
[00:21:25.700 --> 00:21:28.220]   initialized this thing yet or not.
[00:21:28.220 --> 00:21:32.340]   Here the idea is to shift the balance to something
[00:21:32.340 --> 00:21:36.300]   that gives you fewer bugs, and the ability
[00:21:36.300 --> 00:21:38.940]   to say that I want performance at the risk of a little bit
[00:21:38.940 --> 00:21:40.580]   of bugs, right?
[00:21:40.580 --> 00:21:42.300]   Although that risk has greatly decreased,
[00:21:42.300 --> 00:21:44.140]   because you're saying it's uninitialized.
[00:21:44.140 --> 00:21:49.540]   So at the time when you say that, you hopefully then go handle it.
[00:21:49.540 --> 00:21:54.300]   So here we're doing the same steps.
[00:21:54.300 --> 00:21:56.020]   This vector is going to be uninitialized,
[00:21:56.020 --> 00:21:59.020]   and this one is going to be uninitialized.
[00:21:59.020 --> 00:22:00.700]   This whole array of 100 vector threes
[00:22:00.700 --> 00:22:03.140]   is going to be uninitialized, so when we pull one out of the middle,
[00:22:03.140 --> 00:22:04.340]   we should get arbitrary values.
[00:22:04.340 --> 00:22:08.340]   Now, a lot of the time, we're going to get zeros anyway,
[00:22:08.340 --> 00:22:11.220]   but I think if I run this program a bunch of times,
[00:22:11.220 --> 00:22:14.820]   we might start to get funny values.
[00:22:14.820 --> 00:22:17.340]   We're not really getting funny values here,
[00:22:17.340 --> 00:22:19.700]   but note that we're getting one down here, right?
[00:22:19.700 --> 00:22:22.660]   So this is why uninitialized values are dangerous,
[00:22:22.660 --> 00:22:26.260]   because I might think, like, oh, they're being correctly set to zero,
[00:22:26.260 --> 00:22:26.940]   but they're not.
[00:22:26.940 --> 00:22:29.820]   At some point, there's going to be some nonzero stuff on the heap,
[00:22:29.820 --> 00:22:31.460]   and these are going to come out funny.
[00:22:31.460 --> 00:22:36.820]   Just like this value down here is like randomly changing every run.
[00:22:36.820 --> 00:22:39.700]   On my laptop, a lot more of these values came out random,
[00:22:39.700 --> 00:22:44.380]   but for whatever reason, on this machine, it's not happening so much.
[00:22:44.380 --> 00:22:47.820]   And that's the danger of uninitialized values, right?
[00:22:47.820 --> 00:22:52.220]   You get bugs that are inconsistent.
[00:22:52.220 --> 00:22:54.860]   But your program goes a lot faster.
[00:22:54.860 --> 00:23:00.220]   So now, this is when I declare the thing to say that they're uninitialized.
[00:23:00.220 --> 00:23:04.540]   So this is basically what c does by default, right?
[00:23:04.540 --> 00:23:07.380]   I can explicitly say I want that behavior.
[00:23:07.380 --> 00:23:13.820]   But down here, I have a fourth behavior, right, which is
[00:23:13.820 --> 00:23:17.580]   I'm going to give default values to my vector.
[00:23:17.580 --> 00:23:20.740]   And I could to the float, but the float doesn't make as much sense in this case,
[00:23:20.740 --> 00:23:21.780]   so I'm skipping it.
[00:23:21.780 --> 00:23:25.860]   But here we have our vector, it's got values of 1, 4, and 9.
[00:23:25.860 --> 00:23:28.860]   So if I just instantiate it normally, I would get those initialized.
[00:23:28.860 --> 00:23:32.060]   But here, I'm going to say V1 is a vector 3, but you know what?
[00:23:32.060 --> 00:23:33.140]   Don't initialize it.
[00:23:33.140 --> 00:23:34.540]   I don't want that, right?
[00:23:34.540 --> 00:23:37.140]   Or this array is 100 vector 3s, but you know what?
[00:23:37.140 --> 00:23:39.540]   Don't initialize it.
[00:23:39.540 --> 00:23:43.100]   I just want memory for 100 vector 3s, and I'm going to fill it in, right?
[00:23:43.100 --> 00:23:47.220]   So this time, V1 is uninitialized, V2 is uninitialized.
[00:23:47.220 --> 00:23:51.180]   Here, we're declaring it normally without the triple dash.
[00:23:51.180 --> 00:23:54.940]   So that's going to get these values of 1, 4, and 9.
[00:23:54.940 --> 00:23:56.220]   We can also use this with new.
[00:23:56.220 --> 00:23:59.380]   So we're going to make a new vector 3.
[00:23:59.380 --> 00:24:02.220]   And it's not going to be properly initialized.
[00:24:02.220 --> 00:24:03.420]   You won't see 1, 4, and 9.
[00:24:03.420 --> 00:24:07.500]   So note that we're seeing 0 is a lot of the time in here instead of 1, 4, and 9,
[00:24:07.500 --> 00:24:10.740]   except this number is kind of random.
[00:24:10.740 --> 00:24:18.980]   But down here, V3 is getting 1, 4, and 9 as it should.
[00:24:18.980 --> 00:24:23.420]   I wonder, am I making a debug build?
[00:24:23.420 --> 00:24:26.380]   Maybe that has to do with why a lot of these are 0.
[00:24:26.380 --> 00:24:26.820]   I don't know.
[00:24:26.820 --> 00:24:29.620]   Doesn't matter.
[00:24:29.620 --> 00:24:35.220]   OK, so that is explicit uninitialized, and is hopefully a safer way
[00:24:35.220 --> 00:24:36.580]   to make your program fast.
[00:24:36.580 --> 00:24:40.020]   Now, there is a downside to having default values all the time, which
[00:24:40.020 --> 00:24:42.700]   is that if you forget to initialize something, you know,
[00:24:42.700 --> 00:24:45.460]   in C, at least in some cases, the compiler will warn you
[00:24:45.460 --> 00:24:47.380]   that you didn't initialize that thing.
[00:24:47.380 --> 00:24:49.980]   Whereas if everything's initialized, then that's not an error anymore.
[00:24:49.980 --> 00:24:54.020]   So it remains to be seen whether this is entirely good, right?
[00:24:54.020 --> 00:24:56.620]   I don't want to come and try to sell you this language feature
[00:24:56.620 --> 00:24:58.060]   and say, oh, my god, it's amazing.
[00:24:58.060 --> 00:25:04.740]   But it should be at least be pretty easy to tell that your program's wrong
[00:25:04.740 --> 00:25:06.700]   when things are explicitly initialized to 0.
[00:25:06.700 --> 00:25:11.060]   Because if you didn't want them to be 0, then the behavior should be obviously
[00:25:11.060 --> 00:25:11.380]   wrong.
[00:25:11.380 --> 00:25:14.100]   And if you did want them to be 0, which you do want for most things most
[00:25:14.100 --> 00:25:16.180]   of the time, the behavior should be fine.
[00:25:16.180 --> 00:25:20.140]   It should be a great, notational convenience not to have to remember
[00:25:20.140 --> 00:25:20.740]   to initialize.
[00:25:20.740 --> 00:25:22.980]   And reduction in cognitive load not to have
[00:25:22.980 --> 00:25:25.220]   to initialize things all the time.
[00:25:25.220 --> 00:25:26.820]   All right, so part 4.
[00:25:26.820 --> 00:25:27.820]   Whoops.
[00:25:27.820 --> 00:25:36.780]   Part 4, I'm going to talk about the array types that are built into the language.
[00:25:36.780 --> 00:25:40.180]   I demoed dynamic arrays in the last demo.
[00:25:40.180 --> 00:25:44.060]   But I'm going to talk about how you use these practically now.
[00:25:44.060 --> 00:25:46.620]   And then I'm going to lead into part 5 with this, which
[00:25:46.620 --> 00:25:52.100]   is iterators, which have been greatly expanded since last time.
[00:25:52.100 --> 00:25:56.620]   To motivate why I would put a lot of work into building arrays
[00:25:56.620 --> 00:26:00.020]   as a low-level type in the language, like implemented by the compiler,
[00:26:00.020 --> 00:26:09.660]   well, first of all, I don't think you should build every data type into the compiler.
[00:26:09.660 --> 00:26:13.300]   Most data types should be defined by your program to be what they want.
[00:26:13.300 --> 00:26:15.340]   But some things are very common.
[00:26:15.340 --> 00:26:17.540]   And so you want them to be very fast.
[00:26:17.540 --> 00:26:20.420]   And you want them to compile very quickly because you're
[00:26:20.420 --> 00:26:22.380]   going to have them all over your program.
[00:26:22.380 --> 00:26:29.460]   And you want the debug visualize them, primitive types, as clearly as possible.
[00:26:29.460 --> 00:26:31.940]   And so when your array is built into a language,
[00:26:31.940 --> 00:26:35.620]   it lets all those things happen, if you don't screw it up.
[00:26:35.620 --> 00:26:42.460]   So as before, we have a basic static array here, which in C,
[00:26:42.460 --> 00:26:44.660]   this would be the same as saying int, static array of n.
[00:26:44.660 --> 00:26:46.980]   I'm just saying n int, right?
[00:26:46.980 --> 00:26:50.140]   Dynamic array, like last time, I believe in the previous demo,
[00:26:50.140 --> 00:26:52.460]   dynamic arrays were empty brackets, right?
[00:26:52.460 --> 00:26:53.740]   But this time, that's not true.
[00:26:53.740 --> 00:26:58.580]   They've got double dots in the brackets.
[00:26:58.580 --> 00:27:01.140]   And then up here, I've got empty brackets.
[00:27:01.140 --> 00:27:04.260]   But what empty brackets here means is this is an array.
[00:27:04.260 --> 00:27:07.540]   And I don't know or care what type it is.
[00:27:07.540 --> 00:27:09.860]   But I just have a pointer to the data, right?
[00:27:09.860 --> 00:27:14.700]   In this case, that's a pointer to a bunch of ints, and a length value, right?
[00:27:14.700 --> 00:27:16.860]   So this is a pointer and a length.
[00:27:16.860 --> 00:27:20.860]   It's the same as in C if you pass two parameters, a pointer and a length, right?
[00:27:20.860 --> 00:27:23.540]   But now the compiler knows that those two values are associated.
[00:27:23.540 --> 00:27:26.260]   And the debugger knows that those two values are associated.
[00:27:26.260 --> 00:27:31.820]   And this is so common that I think it's a good boon to both program correctness
[00:27:31.820 --> 00:27:35.220]   and your ability to just see what your program is doing to have them put together
[00:27:35.220 --> 00:27:36.460]   in one thing.
[00:27:36.460 --> 00:27:40.420]   Walter Bright of Defame wrote an essay called See's Biggest Mistake.
[00:27:40.420 --> 00:27:42.500]   It's on the Dr. Dobbs blog here.
[00:27:42.500 --> 00:27:49.020]   And he talks about how a lot of the type and safety or the memory and safety of the C runtime
[00:27:49.020 --> 00:27:57.860]   and inherited by C++ comes from this original idea that arrays get turned into pointers
[00:27:57.860 --> 00:28:00.020]   as soon as you pass them as a parameter to a procedure
[00:28:00.020 --> 00:28:01.940]   and they lose their length information.
[00:28:01.940 --> 00:28:07.220]   If you build your whole library with expecting length information in the form of this,
[00:28:07.220 --> 00:28:10.860]   then it's a lot easier to have fewer bugs, right?
[00:28:10.860 --> 00:28:14.780]   So Walter-- whoops, not water bright.
[00:28:14.780 --> 00:28:20.460]   Walter calls this See's Biggest Mistake, and I tend to agree.
[00:28:20.460 --> 00:28:24.340]   There might be some other ones that are almost as big, but I think that's the biggest one.
[00:28:24.340 --> 00:28:31.460]   Anyway, so that's why I'm putting this effort into building these things
[00:28:31.460 --> 00:28:32.460]   into the compiler.
[00:28:32.460 --> 00:28:35.140]   I don't want to build everything into the compiler, so for example, like a dictionary
[00:28:35.140 --> 00:28:38.300]   or a hash map, I'm not so sure that needs to be in the compiler.
[00:28:38.300 --> 00:28:43.980]   I think maybe the approach of providing that in the standard library might be better for
[00:28:43.980 --> 00:28:45.460]   that kind of thing.
[00:28:45.460 --> 00:28:51.660]   But when I write gameplay code, for example, all I can do is iterate over arrays all the
[00:28:51.660 --> 00:28:52.660]   time.
[00:28:52.660 --> 00:28:54.900]   I'll get into that in a bit.
[00:28:54.900 --> 00:28:59.040]   So I feel like arrays should be in the language with very solid support.
[00:28:59.040 --> 00:29:04.100]   So here, I'm doing-- this is an iteration over integers.
[00:29:04.100 --> 00:29:08.560]   I demoed that last time, and I'm just dropping every integer from 0 to n minus 1 into this
[00:29:08.560 --> 00:29:09.560]   static array.
[00:29:09.560 --> 00:29:12.600]   And I'm also adding those to the dynamic array.
[00:29:12.600 --> 00:29:17.400]   Because we don't have parameterized types yet, there's no concept of a function that
[00:29:17.400 --> 00:29:22.480]   is variadic or is polymorphic based on the type of its arguments.
[00:29:22.480 --> 00:29:25.980]   So I have to treat these as void pointers for now, but eventually dynamic arrays will
[00:29:25.980 --> 00:29:29.880]   have nice syntax when you add things.
[00:29:29.880 --> 00:29:33.060]   But the point is, these two types of things, they're different array types.
[00:29:33.060 --> 00:29:36.620]   The way they store things in memory is different.
[00:29:36.620 --> 00:29:40.840]   This one doesn't have a count that's stored anywhere.
[00:29:40.840 --> 00:29:44.760]   The number of elements in this array is known only by the compiler at compile time.
[00:29:44.760 --> 00:29:47.560]   It's not anywhere in memory.
[00:29:47.560 --> 00:29:51.580]   This one has that length stored in memory, as well as a maximum length, as well as like
[00:29:51.580 --> 00:29:55.840]   a pointer to the elements that are stored disjointly on the heap or whatever, whereas
[00:29:55.840 --> 00:29:57.840]   this one is only the elements.
[00:29:57.840 --> 00:30:03.800]   So these two are kind of different in the way that they work in memory.
[00:30:03.800 --> 00:30:09.440]   But we can pass either of them to this print in array function, and they get implicitly
[00:30:09.440 --> 00:30:13.800]   converted to this thing that's a pointer to the data and an integer that tells you the
[00:30:13.800 --> 00:30:14.800]   length.
[00:30:14.800 --> 00:30:17.800]   And that's very, very convenient.
[00:30:17.800 --> 00:30:19.360]   So let's run that.
[00:30:19.360 --> 00:30:22.240]   I believe I compiled it.
[00:30:22.240 --> 00:30:23.740]   Nope.
[00:30:23.740 --> 00:30:27.740]   All right, so this is part four.
[00:30:27.740 --> 00:30:31.300]   So our function here tells us how many items the array has.
[00:30:31.300 --> 00:30:36.100]   Note that it has this .count just as if it were a struct that had a count in it, because
[00:30:36.100 --> 00:30:42.660]   that's how I wrap the pointer with the integer instead of making them two parameters, they're
[00:30:42.660 --> 00:30:48.820]   crammed together into one struct behind the scenes, and then I'm iterating over them and
[00:30:48.820 --> 00:30:50.420]   printing out the value of each item.
[00:30:50.420 --> 00:30:51.820]   So here we go.
[00:30:51.820 --> 00:30:55.900]   And we're doing it once to each array, and the arrays and the contents of the arrays
[00:30:55.900 --> 00:31:00.860]   appear identical to this function, even though they're coming from different places.
[00:31:00.860 --> 00:31:05.260]   But you never have to explicitly grab a pointer and lose the length information in order to
[00:31:05.260 --> 00:31:06.260]   do this, right?
[00:31:06.260 --> 00:31:08.180]   So that's pretty cool.
[00:31:08.180 --> 00:31:12.020]   Now in the future, this is not implemented yet, but in the future you'll be able to set
[00:31:12.020 --> 00:31:14.540]   the size of your index in these arrays, right?
[00:31:14.540 --> 00:31:19.980]   So if you have some dynamic array, or even like an unknown length array on a struct somewhere,
[00:31:19.980 --> 00:31:22.620]   you might care about the struct packing and how big that thing is.
[00:31:22.620 --> 00:31:28.620]   And right now, by default, in a 64-bit architecture, these are all 64-bit integers, and you might
[00:31:28.620 --> 00:31:32.260]   get nervous about like, oh my god, I only -- I know there's never less than -- or never
[00:31:32.260 --> 00:31:36.780]   more than a thousand things in this array, so why would I put -- why would I waste all
[00:31:36.780 --> 00:31:37.780]   those bits?
[00:31:37.780 --> 00:31:45.140]   Well, eventually, you'll be able to say this, it's a float that's n-wide, but the index
[00:31:45.140 --> 00:31:46.620]   is only a U16, right?
[00:31:46.620 --> 00:31:52.220]   Or it's a dynamic array, but the count and the reserve size are only U32.
[00:31:52.220 --> 00:31:55.140]   I started implementing that in the parser, but it's just -- it's a complication that
[00:31:55.140 --> 00:32:00.820]   I don't want in the back end yet, but it'll come soon.
[00:32:00.820 --> 00:32:01.820]   Maybe later than sooner.
[00:32:01.820 --> 00:32:03.340]   It's not that important, honestly.
[00:32:03.340 --> 00:32:07.580]   Yet, it's important for serious users.
[00:32:07.580 --> 00:32:14.620]   Okay, iteration, let me compile this for part five.
[00:32:14.620 --> 00:32:19.820]   So like I said, when I build gameplay code, I'm just iterating over arrays all the damn
[00:32:19.820 --> 00:32:22.340]   time.
[00:32:22.340 --> 00:32:27.740]   And what, you know, in C and C++, I have macros that help me iterate over arrays.
[00:32:27.740 --> 00:32:31.060]   One of the first serious languages I learned in college was Lisp, okay?
[00:32:31.060 --> 00:32:39.220]   And the Lisp guys and scheme guys and people like that have this philosophy that iterating
[00:32:39.220 --> 00:32:44.260]   over arrays of stuff is a very powerful way to program, and that transforming one array
[00:32:44.260 --> 00:32:48.060]   into another is a very powerful way to program, and that's what I learned in college, and
[00:32:48.060 --> 00:32:50.020]   it's true, but it's false, right?
[00:32:50.020 --> 00:32:55.380]   It's very true in that you can express a lot of work in a very simple and encapsulated
[00:32:55.380 --> 00:32:59.020]   way that's easy to think about.
[00:32:59.020 --> 00:33:02.820]   It's false in those languages because the way they do it is so darn memory inefficient
[00:33:02.820 --> 00:33:08.240]   and icky and the dynamic typing makes things really confusing, and it's really easy to
[00:33:08.240 --> 00:33:11.940]   get lost in like what's in the array.
[00:33:11.940 --> 00:33:16.380]   I believe in a language, a curly brace language, you can do a much better job than you can
[00:33:16.380 --> 00:33:24.380]   in Lisp, actually, if you factor in all elements like how hard it is to debug your program.
[00:33:24.380 --> 00:33:29.000]   But anyway, inevitably what happens when I'm programming, and this is an example that
[00:33:29.000 --> 00:33:34.940]   actually happened in that little Galaga game, Space Invaders game, which is why I put this
[00:33:34.940 --> 00:33:37.460]   into the next list of features because I really wanted it.
[00:33:37.460 --> 00:33:43.420]   So I had this for, and like I demoed last time, I had this array of particles, right,
[00:33:43.420 --> 00:33:46.720]   it's stored on a particle emitter, there's a bunch of particles, and I want to simulate
[00:33:46.720 --> 00:33:50.360]   each particle to make it fly through space, and each particle is, right, I'm visiting
[00:33:50.360 --> 00:33:54.440]   every particle, and I'm calling it P, and then I'm calling this function on P. But eventually,
[00:33:54.440 --> 00:34:00.400]   I want to say, okay, well, once P has flamed out, I want to destroy P and remove it from
[00:34:00.400 --> 00:34:07.380]   that array of particles, right, and the problem is just, if your for loop is too simplistic,
[00:34:07.380 --> 00:34:11.020]   as soon as I want to do that, I have to rewrite it into some other loop, right, I have to
[00:34:11.020 --> 00:34:16.040]   write it as a for that doesn't iterate over the array anymore, but iterates over the count
[00:34:16.040 --> 00:34:24.060]   of the array and then explicitly indexes, right, or I need to, you know, make it a while loop,
[00:34:24.060 --> 00:34:28.340]   like, this is what I actually ended up doing is I said, okay, I have zero, and then while
[00:34:28.340 --> 00:34:33.300]   it's less than the count on the array, visit each particle, okay, and then simulate that
[00:34:33.300 --> 00:34:38.940]   particle, and if it's elapse is bigger than lifetime, then call some removal function,
[00:34:38.940 --> 00:34:43.580]   but leave I the same because we replace the current thing with the new guy, right, otherwise
[00:34:43.580 --> 00:34:47.240]   increment I like you normally would want in the while loop. This is a bug-prone way to
[00:34:47.240 --> 00:34:51.240]   do things, and it's annoying in the very first lecture, one of the things that I said was
[00:34:51.240 --> 00:34:56.560]   that small increments and functionality shouldn't result in discontinuities in how you write
[00:34:56.560 --> 00:35:00.680]   the program, right, like, just because I want to remove something from this for loop, I shouldn't
[00:35:00.680 --> 00:35:04.800]   have to rewrite the whole for loop into something else, right. So one of the things I'm going
[00:35:04.800 --> 00:35:12.000]   to talk about now is this remove primitive that lets us turn this into this, right, without
[00:35:12.000 --> 00:35:15.620]   having to go through all this kind of mess of explicitly talking about the index. So
[00:35:15.620 --> 00:35:20.820]   I say for each p in this particle's array, simulate it, and then if it's elapsed time
[00:35:20.820 --> 00:35:28.300]   is greater than its lifetime, then remove p, right. What is p? Well, p is that iterator,
[00:35:28.300 --> 00:35:35.220]   right, and the for loop knows what array p is attached to because it's right there, and
[00:35:35.220 --> 00:35:41.540]   it's very easy. It's very simple. So you can think of remove as being inherent in this
[00:35:41.540 --> 00:35:46.360]   loop like the way that break and continue are, right. It might be weird to see a new one
[00:35:46.360 --> 00:35:50.840]   of those if you've never seen it before, but this is like break or continue, but it's
[00:35:50.840 --> 00:35:54.900]   not about flow control. It's about data control. It's about removing the current element from
[00:35:54.900 --> 00:36:01.620]   the array. Anyway, so onto the example. Here we've got our static and dynamic array again,
[00:36:01.620 --> 00:36:05.980]   and we're filling them up. And then I'm going to have this unknown sized array just to show
[00:36:05.980 --> 00:36:11.140]   that, you know, you don't just have this when you're passing a parameter, you can assign
[00:36:11.140 --> 00:36:16.860]   to it. So now I've got an unknown sized array, and it's the same as the static array, but
[00:36:16.860 --> 00:36:22.700]   it's got the count stored explicitly with it. And then I'm going to for loop over all
[00:36:22.700 --> 00:36:25.940]   of these and do this simple print F, right. So for everything in the static array, I'm
[00:36:25.940 --> 00:36:30.420]   going to print that item and everything in a dynamic array and everything in the unknown
[00:36:30.420 --> 00:36:37.420]   size array. So let's run that. This is a longer example in terms of output, but here we go.
[00:36:37.420 --> 00:36:41.340]   So there's no surprise. All these arrays are working. They go zero to nine. They all behave
[00:36:41.340 --> 00:36:48.740]   the same in terms of iterating. Okay. Now, a new thing that I added that didn't exist
[00:36:48.740 --> 00:36:54.900]   last time is that, you know, sometimes you want the index into the array as well as the
[00:36:54.900 --> 00:36:59.940]   element, right. And you could do that by just always iterating over the index, but that's
[00:36:59.940 --> 00:37:03.740]   inconvenient because usually I care about the element, right. So now you can say two things.
[00:37:03.740 --> 00:37:11.660]   I can say four value comma index, right, in this array, print F that, right. So now I've
[00:37:11.660 --> 00:37:16.540]   got index is which element in the array we are and value is the actual value at that
[00:37:16.540 --> 00:37:23.060]   position. Here they're the same because we just made them the same up here, right. That's
[00:37:23.060 --> 00:37:28.060]   the point. Those will change in a minute. So that's a nice thing you can always get
[00:37:28.060 --> 00:37:35.980]   at the index. I'll get more specifically into how remove works in a little bit. Okay, another
[00:37:35.980 --> 00:37:40.860]   thing you might want to do is change the array while you're iterating over it, right. By default,
[00:37:40.860 --> 00:37:45.660]   if you iterate over an array, you're going to visit the elements by value, which means
[00:37:45.660 --> 00:37:49.380]   if you try to change one of those elements, it's only going to change in the body of your
[00:37:49.380 --> 00:37:55.540]   loop, right. But I can use this little pointer sigil here and just say, I'm going to visit
[00:37:55.540 --> 00:38:02.300]   P by pointer, right, or visit every element of static array by pointer. And now P is a
[00:38:02.300 --> 00:38:06.420]   pointer to each element instead of actually being the value of the element, right. So
[00:38:06.420 --> 00:38:10.340]   now I can say, it's a little confusing because of all the stars, right. But I'm going to say
[00:38:10.340 --> 00:38:15.460]   the contents of P are going to be multiplied by the contents of P, right. So I'm squaring
[00:38:15.460 --> 00:38:19.260]   every element of the array. I'm not printing it here though, but then I'm calling this
[00:38:19.260 --> 00:38:26.220]   print_in_array function, which I guess is this one from the previous example, right.
[00:38:26.220 --> 00:38:30.380]   So just to say that we're going to modify the value and it's going to stay modified
[00:38:30.380 --> 00:38:34.940]   outside the loop, right. So here's our print_in_array and all these values are squared, right. Instead
[00:38:34.940 --> 00:38:48.620]   of 0 to 9, now, you know, 981, 864, et cetera. So that's great. Alright, so let's talk about
[00:38:48.620 --> 00:38:54.020]   removal a little more specifically and how that then motivated me to make non-local break and
[00:38:54.020 --> 00:39:04.300]   continue, which some languages have. I felt like we should have it. I've got another print
[00:39:04.300 --> 00:39:07.780]   integers function. I guess I could have called that previous one that I just called, but
[00:39:07.780 --> 00:39:13.060]   whatever. We've got another one in here locally. Maybe I just like local functions. It's a
[00:39:13.060 --> 00:39:16.820]   little bit different. It prints a little bit different strings. Maybe that's why I did
[00:39:16.820 --> 00:39:21.660]   it. I just wanted different strings so I could know where I was in the list. So this tells
[00:39:21.660 --> 00:39:25.740]   us how many values and it prints them one by one, right. Oh, but this time it is written
[00:39:25.740 --> 00:39:30.540]   a little bit differently. So note that I'm not specifying my iterators at all, right.
[00:39:30.540 --> 00:39:35.100]   So as I mentioned last time, I'm just saying 4 over the array. So we get this implicit
[00:39:35.100 --> 00:39:40.740]   variable it that says, you know, that becomes every value in the array, like P was in the
[00:39:40.740 --> 00:39:47.100]   earlier example. But now you also get it index if you want it, which is the index of it.
[00:39:47.100 --> 00:39:53.560]   I could have called this something like, you know, I or something like that, but that, whoops,
[00:39:53.560 --> 00:40:01.100]   that felt wrong. It felt wrong because, you know, I as a variable that you use a lot and
[00:40:01.100 --> 00:40:05.460]   it's easy having it be too much like it, it means you can typo it. So since you maybe
[00:40:05.460 --> 00:40:09.740]   want the index less often, I felt like it was better to have it be a slightly longer
[00:40:09.740 --> 00:40:15.540]   name. But as always, these things are changeable. So this is two implicit variable names and
[00:40:15.540 --> 00:40:22.060]   we're using them both. And you can see that that works just great here. Now you've got
[00:40:22.060 --> 00:40:29.580]   14 items instead of 10 because I want it to be different. Okay. So I'm going to iterate
[00:40:29.580 --> 00:40:36.060]   over this and I'm going to do a removal, right. So we're starting with again, zero
[00:40:36.060 --> 00:40:40.940]   map to zero, one map to one, et cetera. When I hit six, I'm going to remove that element.
[00:40:40.940 --> 00:40:44.780]   So six should no longer be in the array. And when I hit 11, I'm going to remove that element.
[00:40:44.780 --> 00:40:48.620]   So 11 should no longer be in the array. And then I'm also going to break. So here I say
[00:40:48.620 --> 00:40:52.780]   if I hit 12, I should remove the element, but that shouldn't actually happen because
[00:40:52.780 --> 00:40:56.660]   we're going to break before that. So 12 should not ever be in our array, right? So now we'll
[00:40:56.660 --> 00:41:01.060]   go here and you'll see that that's true. We go one, two, three, four, five. And once
[00:41:01.060 --> 00:41:06.020]   we hit six, we have 13 actually. The reason we have 13 is because that was pulled off
[00:41:06.020 --> 00:41:12.380]   the end of the array, right? This remove by default is a non-ordered remove. I toyed
[00:41:12.380 --> 00:41:17.820]   around with adding a keyword like remove ordered. That's a much slower operation and
[00:41:17.820 --> 00:41:22.260]   I'm not sure we really want it. Like I sort of feel like if you want that, maybe you want
[00:41:22.260 --> 00:41:26.660]   to call your own function that takes this more seriously. But if there's demand for
[00:41:26.660 --> 00:41:31.300]   it, it's not too hard to put an ordered remove in. But I find that most of the cases in which
[00:41:31.300 --> 00:41:37.300]   I want this, I'm treating my array as a bag of stuff like here's all the items in your
[00:41:37.300 --> 00:41:42.220]   inventory, right? And I don't necessarily care what order that those happen in because
[00:41:42.220 --> 00:41:45.580]   the variables that say what inventory slot they are and how much space they take up are
[00:41:45.580 --> 00:41:52.620]   probably in a struct somewhere, usually. Anyway, that's a simple case. So that scrambles
[00:41:52.620 --> 00:41:55.780]   our array a little bit, but now there's only 12 items because we remove those other two
[00:41:55.780 --> 00:42:08.380]   items. So now, oh, by the way, I said remove I here. I guess I took out this example, but
[00:42:08.380 --> 00:42:12.940]   you could say remove J for example and remove from an outer loop. I'm going to demo non-local
[00:42:12.940 --> 00:42:18.580]   break and continue here and remove actually works non-locally just like break and continue.
[00:42:18.580 --> 00:42:22.420]   So let me demo that. So down here, as the last thing in this example, you can see this
[00:42:22.420 --> 00:42:29.300]   sort of grid of numbers and, you know, it's counting an I and J. So we've got this loop.
[00:42:29.300 --> 00:42:34.380]   Go from J zero to nine. Each J makes a new line. I goes from zero to nine and we're doing
[00:42:34.380 --> 00:42:39.220]   these defers so we can just make sure that these little punctuations happen at the right
[00:42:39.220 --> 00:42:46.100]   time. It's just sort of a language debugging exercise. So here's what we're saying. If
[00:42:46.100 --> 00:42:50.900]   I is equal to eight, then don't print this, right? Just continue through I. So as you
[00:42:50.900 --> 00:42:56.620]   can see here, we go I 1, 2, 3, 4, 6, 7, where I equals eight, we've got no numbers in between
[00:42:56.620 --> 00:43:01.140]   the two dots, right? So that's what that continue does, right? But if I equals five
[00:43:01.140 --> 00:43:05.300]   and J equals five, then continue J. Well, what does continue J means? Well, it means
[00:43:05.300 --> 00:43:09.960]   continue back at this loop where J is the iterator, right? Not our local loop, but the
[00:43:09.960 --> 00:43:15.060]   outside loop. So you can see when we get to I is five and J is five. Suddenly, we don't
[00:43:15.060 --> 00:43:19.900]   just break off here, but we break off the whole line and go on to the next line, right?
[00:43:19.900 --> 00:43:24.460]   And then here we're saying, well, if I is five and J is eight, then break out of J, right?
[00:43:24.460 --> 00:43:28.900]   So this is just like the C and C++ break, but instead of breaking out of our local loop,
[00:43:28.900 --> 00:43:33.380]   we break out of the outer loop, right? So here, if we didn't break, there would be a line
[00:43:33.380 --> 00:43:42.340]   nine here, right? But notice there's no line nine. That's because we stopped, right?
[00:43:42.340 --> 00:43:48.700]   So that's pretty nice. When you use a for loop with these iterators like this, especially
[00:43:48.700 --> 00:43:56.580]   with explicit names, then you have a very nice label to use to refer to that loop. With
[00:43:56.580 --> 00:44:01.980]   a while loop, there's no syntax in the language to attach a label to it. Maybe we'll add that
[00:44:01.980 --> 00:44:07.060]   later if we need to, but it would feel a little weird. Maybe while can sort of figure
[00:44:07.060 --> 00:44:11.700]   it out, like if you have a while and then a Boolean comparison, it can sort of decide
[00:44:11.700 --> 00:44:22.820]   that the variable in the comparison is the iterator. I don't know. Anyway, this takes
[00:44:22.820 --> 00:44:26.380]   care of almost everything that I ever want to do when looping, and it puts it into a
[00:44:26.380 --> 00:44:31.340]   style that's very terse, and it's implemented by the compiler again. So when you step through
[00:44:31.340 --> 00:44:34.940]   your program in the debugger, you don't have to do all this maddening, like stepping through
[00:44:34.940 --> 00:44:40.020]   iterators and glue functions like you do in C++, the compiler will treat your right, and
[00:44:40.020 --> 00:44:45.940]   the debugger will treat your right. Okay. Let's talk about enums. I don't feel like I'm going
[00:44:45.940 --> 00:44:49.540]   to finish before, well, there's only seven parts, so maybe we'll finish before Handmade
[00:44:49.540 --> 00:45:00.100]   Hero. Okay. Let's go to part six. So I demoed enums last time, but now I wanted to add a
[00:45:00.100 --> 00:45:04.580]   lot of introspection information to enums, because that's one of the things that I tend
[00:45:04.580 --> 00:45:10.820]   to really want. This is just a sort of a sneak peek at what's going to happen a lot later
[00:45:10.820 --> 00:45:15.340]   in the language, because really we're going to want this kind of introspection on everything,
[00:45:15.340 --> 00:45:21.300]   right? But for now, I said, okay, an enum, when you do an enum like this, that's actually
[00:45:21.300 --> 00:45:26.180]   syntactic sugar for a struct that has some elements that tells you things about the enum,
[00:45:26.180 --> 00:45:31.540]   right? So here we're going to say this enum hello has some number of members, right? That's
[00:45:31.540 --> 00:45:36.580]   count number of members, which is just an item that gets declared in this struct. It'll tell
[00:45:36.580 --> 00:45:42.900]   me the lowest value and the highest value, you know, it'll, and of course I can use the
[00:45:42.900 --> 00:45:48.740]   members. So the members get namespaced on this enum right now. If you want to make them global,
[00:45:48.740 --> 00:45:55.940]   you can use using, which I'm not going to demo now, using will be next time. Anyway, so let's just,
[00:45:55.940 --> 00:46:03.380]   let's start this. So here we go. Hello has four members. It ranges from 0 to 81, right? So this
[00:46:03.380 --> 00:46:08.180]   first member is going to be 0, just like it would in C or C++. But when you get to third,
[00:46:08.180 --> 00:46:13.380]   it gets set to this arbitrary value, which just to show off I define later, right, which is 80,
[00:46:13.380 --> 00:46:19.380]   and then fourth increments that to be 81, right? So we have that hello ranges from 0 to 81,
[00:46:19.380 --> 00:46:23.220]   and then I'm manually printing the name of every single one down here. So first, second, third,
[00:46:23.220 --> 00:46:31.380]   and fourth are 0, 1, 80, and 81. But we also get compile time information about what the names and
[00:46:31.380 --> 00:46:37.380]   the values of all the enum identifiers are. They get packed into an array called names and an array
[00:46:37.380 --> 00:46:42.100]   called values that are stored on the stock struct created by this enum. That's called type hello,
[00:46:42.100 --> 00:46:49.300]   right? So I can iterate over hello's names and print the name of every enum and the value of that,
[00:46:49.300 --> 00:46:55.060]   right? So the values is a parallel array. So I'm using this it index, right, to index the values
[00:46:55.060 --> 00:47:03.300]   array that's parallel to the it that I'm visiting, right? So I could, I could say hello.names,
[00:47:03.300 --> 00:47:07.860]   it index, right? So let me, let me compile that and show you that that does the same thing,
[00:47:07.860 --> 00:47:14.900]   right? So I'm iterating over this list and printing out the names of the enums, right? This
[00:47:14.900 --> 00:47:19.860]   isn't hard-coded. This one's hard-coded. Let me get rid of the hard-coded one just so you can
[00:47:19.860 --> 00:47:32.020]   know for sure that that's working, right? So there we go. I don't know that there's much more to say
[00:47:32.020 --> 00:47:36.020]   about that. But you, you know, how many times have you wanted to like print out an enum for
[00:47:36.020 --> 00:47:40.100]   debugging or in an error message and you had to like define all these freaking macros that like
[00:47:40.100 --> 00:47:44.900]   whatever. It's terrible. So the compiler just does that for you. Eventually it'll do that for you for
[00:47:44.900 --> 00:47:50.340]   everything. Okay. And again, this is, you know, these are starting to look like features like you
[00:47:50.340 --> 00:47:54.820]   see in other high-level languages, but this doesn't affect the level of control that you have. This
[00:47:54.820 --> 00:48:04.660]   is just as low level as C or C++, actually as C, really. Forget C++. But it's giving you this
[00:48:04.660 --> 00:48:08.980]   additional information that you should have been given many, many years ago, right? And you
[00:48:08.980 --> 00:48:13.140]   probably do. You get at least a lot of this in D, for example. I know D gives you the range,
[00:48:13.140 --> 00:48:19.460]   the value range. I don't know if it gives you the names. It probably does. You know, the D people
[00:48:19.460 --> 00:48:26.740]   are smart. All right. So there's a couple other members that are type members, right? So enums
[00:48:26.740 --> 00:48:31.220]   are strict types. You can't assign an enum to the wrong enum and make that mistake. That's a
[00:48:31.220 --> 00:48:37.540]   compile time error. So hello.strict is the strict type of this enum. It'll only match to other
[00:48:37.540 --> 00:48:43.460]   things in this array, right? So I can say x is hello.first and hello.second, but if I try to say x equals
[00:48:43.460 --> 00:48:49.860]   10, which is a valid U16, right, unsigned 16-bit number, that's actually going to give me a type
[00:48:49.860 --> 00:48:57.460]   mismatch, right, in line 421, long demo program, line 421, right? So I can't do that.
[00:48:57.460 --> 00:49:07.060]   Okay. So we fixed our program again. But there's another type called loose, which is, you know,
[00:49:07.060 --> 00:49:12.180]   this type, basically. Whatever the values of the enum happen to be representationally on the CPU.
[00:49:12.180 --> 00:49:19.060]   So I can set this to 10. Or if I want to do weird math on my enums or something, I can cast
[00:49:19.060 --> 00:49:24.260]   one of those values to the loose type. And now that's a U16, but I don't have to hard code the
[00:49:24.260 --> 00:49:29.060]   fact that it's a U16, right? It can be, this is like saying, whatever that representational
[00:49:29.060 --> 00:49:34.020]   type of that enum is, cast this number to it. So you can have very strictly typed enums.
[00:49:34.020 --> 00:49:38.980]   You can have loosely typed enums. You win either way, right? So now, you know, I can
[00:49:38.980 --> 00:49:45.060]   assign y to casting this. And it is, it's one or whatever that I print here.
[00:49:45.060 --> 00:49:48.260]   All right. Last part, inlining.
[00:49:55.860 --> 00:50:05.140]   Okay. C++ mode. So C has this thing that's really bad for people trying to make fast code,
[00:50:05.140 --> 00:50:10.820]   where inlining is a hint. It's like register in C. Like, register was a hint that, hey,
[00:50:10.820 --> 00:50:13.700]   maybe you should put this parameter or local variable in a register.
[00:50:16.900 --> 00:50:26.660]   And that way of handling things, I'll just say, causes a lot of problems,
[00:50:26.660 --> 00:50:30.980]   especially people who try to make ambitious video games. But I think for everybody,
[00:50:30.980 --> 00:50:34.740]   I think people don't realize it's as much of a problem as it does. So, for example,
[00:50:34.740 --> 00:50:42.020]   Chandler Caruth has this keynote at BoostCon, optimizing the emergent structures of C++
[00:50:42.020 --> 00:50:46.500]   from 2013. You can watch it on YouTube right here. And one of the things he talked about is
[00:50:46.500 --> 00:50:49.700]   how hard it is to inline and how hard it is for the compiler to make good,
[00:50:49.700 --> 00:50:54.500]   inlining decisions. And I'm watching that video and I'm just thinking, yeah, that's really hard,
[00:50:54.500 --> 00:51:00.180]   but instead of people putting so much energy into this, I wish that they would put energy into
[00:51:00.180 --> 00:51:06.900]   other parts of the language, right? Because the kinds of, when he says emergent structures of C++,
[00:51:06.900 --> 00:51:12.420]   he's talking about when you have a lot of glue code and a lot of so-called zero-cost abstractions,
[00:51:12.420 --> 00:51:17.140]   you get layer upon layer of all this stuff that needs to get inlined in order to be fast,
[00:51:17.140 --> 00:51:20.580]   right? In order for the compiler to see everything in one place and optimize it better.
[00:51:20.580 --> 00:51:27.460]   Now, first of all, I think when you get too much glue code like that, your code is just bad,
[00:51:27.460 --> 00:51:31.460]   because it's hard to understand code with that many layers of abstraction anyway.
[00:51:31.460 --> 00:51:35.380]   You shouldn't be putting that kind of abstraction into your program if you can help it, right?
[00:51:35.380 --> 00:51:39.540]   Sometimes it's the right thing, but not always. And it should be minimized, right?
[00:51:39.540 --> 00:51:45.140]   But secondly, this idea that the compiler is magically going to make their right in-line
[00:51:45.140 --> 00:51:50.420]   decisions or better in-line decisions than I can is not correct, I don't believe, right?
[00:51:50.420 --> 00:51:58.180]   At least in many specific cases. There's this idea that has taken roots among compiler people,
[00:51:58.180 --> 00:52:03.140]   right? And the idea is you shouldn't try to be smarter than the compiler. And I regard that idea
[00:52:03.140 --> 00:52:10.100]   as woefully wrong. Just look at the output of a compiler sometime on complicated code and you'll
[00:52:10.100 --> 00:52:15.620]   just see how not smart that compiler is, right? It misses all kinds of things that it should be
[00:52:15.620 --> 00:52:23.300]   able to optimize, right? Chandler's talk specifically about inlining though should be convincing enough
[00:52:23.300 --> 00:52:30.340]   for this specific case that I'm going to talk about. But anyway, this idea you can't or you
[00:52:30.340 --> 00:52:33.940]   shouldn't try to be smarter than the compiler or don't think you're smarter than the compiler.
[00:52:33.940 --> 00:52:40.900]   You know, it comes from this philosophy that I'm writing a program. I have no idea on what
[00:52:40.900 --> 00:52:46.020]   CPU or operating system this program is going to run. So how can I intelligently say register or
[00:52:46.020 --> 00:52:49.780]   in-line or anything like that? The compiler is going to know a lot more about the target platform
[00:52:49.780 --> 00:52:54.740]   than I do. And so it's going to make those decisions. And that's fine if you honestly don't
[00:52:54.740 --> 00:52:59.380]   know anything about your target platform. But actually, in my industry and in many industries,
[00:52:59.380 --> 00:53:04.660]   we actually do, right? And we're not just like throwing some code against the wall and hoping
[00:53:04.660 --> 00:53:09.860]   it's fast. We're compiling our program. We're looking at the resulting assembly language.
[00:53:09.860 --> 00:53:15.780]   We're saying, yes, that's fast. Or, no, that's not fast. Or this, you know, we're profiling it.
[00:53:15.780 --> 00:53:19.540]   We're saying this has too many instructions. Look, I can see right here, why are these
[00:53:19.540 --> 00:53:24.580]   instructions in there? And we're doing our best to fix it, right? So the compiler is a better
[00:53:24.580 --> 00:53:28.900]   tool if it helps us fix these problems. And so the way that you specify inlining here
[00:53:28.900 --> 00:53:35.700]   is for serious programmers, right? Again, this is a language for good programmers and to help them
[00:53:35.700 --> 00:53:39.780]   do what they want to do, not to prevent bad programmers from shooting cells in the foot.
[00:53:39.780 --> 00:53:44.500]   So if you say inline on a function declaration, for example, similarly to the way you would in
[00:53:44.500 --> 00:53:51.300]   C or C++, that means that function will always be inlined unless that inlining is impossible.
[00:53:51.300 --> 00:53:57.700]   How could it be impossible? Well, it's a recursive function, right? It's trying to inline itself.
[00:53:57.700 --> 00:54:04.100]   Compiler's got to stop eventually, right? But in all normal cases, when you say that something
[00:54:04.100 --> 00:54:09.940]   should be inlined, it will be. So here, I'm defining some local functions. The first is just plain
[00:54:09.940 --> 00:54:15.540]   old regular function. The compiler could decide to inline this or not, depending on what it
[00:54:15.540 --> 00:54:23.060]   feels is right for the situation. For test local B here, I have said this is inline. The compiler
[00:54:23.060 --> 00:54:29.380]   will always inline this every single time, again, unless it's impossible. I am guaranteed that,
[00:54:29.380 --> 00:54:34.740]   right? That's part of the programming language's definition, and that means you have to specify
[00:54:34.740 --> 00:54:41.220]   what impossible means, but, you know, that's, we'll deal with that later. Part C, I'm saying
[00:54:41.220 --> 00:54:46.100]   don't inline this, ever, right? No inline is sort of the opposite of inline. Do not ever
[00:54:46.100 --> 00:54:50.500]   inline this. You might use that more rarely, but, for example, you might want to make sure
[00:54:50.500 --> 00:54:55.780]   that a function exists in the executable, so you can hook it, right? Or something like that,
[00:54:55.780 --> 00:55:01.620]   right? When you're messing around at a low level, there's sometimes reasons why you want things to
[00:55:01.620 --> 00:55:07.220]   stay there, right? Or you might decide, well, it's not profitable to inline this, and I don't even
[00:55:07.220 --> 00:55:13.060]   want the compiler thinking about it, so let me just turn that off, right? So you can say inline,
[00:55:13.060 --> 00:55:16.980]   you can say no inline, right? And so let me compile and run this demo.
[00:55:16.980 --> 00:55:27.140]   Whoops, I didn't. Oh, right, this is a different program. I broke it off because it's big.
[00:55:27.140 --> 00:55:35.380]   All right, so here, I've printed out some, these aren't real warnings. These are just me
[00:55:35.380 --> 00:55:40.900]   demonstrating that the compiler is knowing when things are inlined or not inlined, right? So,
[00:55:40.900 --> 00:55:48.660]   you know, we have a yes in line, a no in line, blah, blah, blah, right? So here, line
[00:55:48.660 --> 00:55:58.100]   line 26 should be, I don't know, line 27 should be a yes in line, and line 28 should be a no in line,
[00:55:58.100 --> 00:56:03.940]   and we see that, yeah, 27 is yes in line, and 28 is no in line. Okay, now regardless of what these
[00:56:03.940 --> 00:56:12.500]   specifiers say, you can choose at the calling site to override them, right? So I can say, even
[00:56:12.500 --> 00:56:18.500]   though test local C is defined as no in line, in this case, I'm calling it, I want to call it in line.
[00:56:18.500 --> 00:56:25.460]   This means, again, always in line. The compiler is not following a recommendation, it's following a
[00:56:25.460 --> 00:56:31.300]   command, right? So these will always be inlined. These will never be inlined, even though this one
[00:56:31.300 --> 00:56:37.220]   is declared in line, right? So what this really means is, unless I say otherwise in line,
[00:56:37.220 --> 00:56:43.860]   okay. And then I have a little directive down here, but we'll see what that is in a second.
[00:56:43.860 --> 00:56:49.700]   So here I've got, so that's sort of the first two ways, right? There's actually three ways
[00:56:49.700 --> 00:56:56.020]   to determine whether something's inlined or not, right? The first one is in the function definition,
[00:56:56.020 --> 00:57:00.580]   the second one is at the calling site, and the third one is by directive elsewhere in the program.
[00:57:01.460 --> 00:57:10.020]   Why would you want to do that? Well, for example, maybe you're targeting a few specific platforms,
[00:57:10.020 --> 00:57:16.340]   and you want to change who gets inlined or not based on which platform it is, right? Well, one easy
[00:57:16.340 --> 00:57:22.740]   way to do that is to include a platform-specific file full of directives, right? So here we've got
[00:57:22.740 --> 00:57:28.900]   test D, E, F, and G, and none of them have inlined specifiers on the function, but here we've got a
[00:57:28.900 --> 00:57:33.140]   directive for E and a directive for F, and then way down here, as though it were in a different
[00:57:33.140 --> 00:57:37.140]   file, it could be in a different file, but I just talked it down here in inlined for G,
[00:57:37.140 --> 00:57:41.700]   and so those will all, that's as if you had written this in the function header, but you don't have
[00:57:41.700 --> 00:57:47.140]   to mess around with like weird if-defs or whatever inside the function, right? You can include them
[00:57:47.140 --> 00:57:52.020]   on a per-platform basis in like a performance template or whatever you want to think about it as
[00:57:52.020 --> 00:57:58.660]   that goes alongside your code. But again, as before, I can override these by saying, oh,
[00:57:58.660 --> 00:58:01.780]   always inlined, I don't care what the directive says and always don't inlined.
[00:58:01.780 --> 00:58:10.820]   And so if anyone wants to freeze frame and check all these, I'm pretty sure that they will all
[00:58:10.820 --> 00:58:14.980]   be in accordance, right? Like line 74 to 6 is no inlined, which is what we have here.
[00:58:14.980 --> 00:58:21.060]   Now, the one thing to say about this is we don't have a real back end yet, so this
[00:58:21.060 --> 00:58:28.180]   doesn't make it all the way to the back end because you can't tell C this. There's no way to say this
[00:58:28.180 --> 00:58:34.340]   and C. Once we have LLVM or some other real back end, then this will go all the way to machine
[00:58:34.340 --> 00:58:38.820]   code. Right now, this is just built all the way from the front to the middle of the compiler.
[00:58:38.820 --> 00:58:47.620]   But that's it. That is all I have in terms of feature rundown for this time.
[00:58:47.620 --> 00:58:53.540]   For next time, I think I want to do more introspection and maybe the object model because I think,
[00:58:53.540 --> 00:58:58.020]   you know, like every once in a while, people say, well, why don't you use D or why don't you use
[00:58:58.020 --> 00:59:04.180]   Nimrod or something like that? And the more I work on this language, the more I find ideas that
[00:59:04.180 --> 00:59:10.900]   are going to go further and further from what those languages do. And I hope that's starting to
[00:59:10.900 --> 00:59:17.780]   become apparent already. And I think it'll become ever more apparent by next time or the time after
[00:59:17.780 --> 00:59:24.180]   that. So yeah, I'm going to take a short voice saving break. And I'll be back in a few minutes
[00:59:24.180 --> 00:59:30.020]   to take questions. Handmade Hero started three minutes ago. So you can also go see that. Sorry
[00:59:30.020 --> 00:59:33.620]   if I raced through this a little bit quick. I was trying to fit it all into an hour. So
[00:59:33.620 --> 00:59:37.860]   we'll have a couple minute break and I will be right back.

