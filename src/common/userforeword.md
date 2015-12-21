User Foreword
=============

<span id="cha:user-foreword"></span>

Machinekit is modular and flexible. These attributes lead many to see it as a confusing jumble of little things and wonder why it is the way it is. This page attempts to answer that question before you get into the thick of things.

Machinekit started at the National Institute of Standards and Technology in the USA. It grew up using Unix as its operating system. Unix made it different. Among early Unix developers there grew a set of code writing ideas that some call the Unix way. These early Machinekit authors followed those ways.

Eric S. Raymond, in his book The Art of Unix Programming, summarizes the Unix philosophy as the widely-used engineering philosophy, "Keep it Simple, Stupid" (KISS Principle). He then describes how he believes this overall philosophy is applied as a cultural Unix norm, although unsurprisingly it is not difficult to find severe violations of most of the following in actual Unix practice:

-   Rule of Modularity: Write simple parts connected by clean interfaces.

-   Rule of Clarity: Clarity is better than cleverness.

-   Rule of Composition: Design programs to be connected to other programs.

-   Rule of Separation: Separate policy from mechanism; separate interfaces from engines.<span class="footnote">
    \[Found at <http://en.wikipedia.org/wiki/Unix_philosophy>, 07/06/2008\]
    </span>

Mr. Raymond offered several more rules but these four describe essential characteristics of the Machinekit motion control system.

The **Modularity** rule is critical. Throughout these handbooks you will find talk of the interpreter or task planner or motion or HAL. Each of these is a module or collection of modules. It’s modularity that allows you to connect together just the parts you need to run your machine.

The **Clarity** rule is essential. Machinekit is a work in progress — it is not finished nor will it ever be. It is complete enough to run most of the machines we want it to run. Much of that progress is achieved because many users and code developers are able to look at the work of others and build on what they have done.

The **Composition** rule allows us to build a predictable control system from the many modules available by making them connectable. We achieve connectability by setting up standard interfaces to sets of modules and following those standards.

The **Separation** rule requires that we make distinct parts that do little things. By separating functions debugging is much easier and replacement modules can be dropped into the system and comparisons easily made.

What does the Unix way mean for you as a user of Machinekit. It means that you are able to make choices about how you will use the system. Many of these choices are a part of machine integration, but many also affect the way you will use your machine. As you read you will find many places where you will need to make comparisons. Eventually you will make choices, "I’ll use this interface rather than that” or, “I’ll write part offsets this way rather than that way." Throughout these handbooks we describe the range of abilities currently available.

As you begin your journey into using Machinekit we offer two cautionary notes:<span class="footnote">
\[Found at <http://en.wikipedia.org/wiki/Unix_philosophy>, 07/06/2008\]
</span>

-   Paraphrasing the words of Doug Gwyn on UNIX: "Machinekit was not designed to stop its users from doing stupid things, as that would also stop them from doing clever things."

-   Likewise the words of Steven King: "Machinekit is user-friendly. It just isn’t promiscuous about which users it’s friendly with."

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET

