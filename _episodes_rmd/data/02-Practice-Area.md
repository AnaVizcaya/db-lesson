---
title: Practice area
teaching: 30
exercises: 1
questions:
- Try making various examples yourself
objectives:
- Learn how to format different use cases
keypoints:
- Using Markdown to make note/warnings/quizzes
---



---

1. Make a Note

    ~~~
     > ## Note check out the Markdown CheatSheet for standard syntax 
    > check out the Markdown CheatSheet for standard syntax 
    > [Cheat Sheet for Markdown](https://www.markdownguide.org/cheat-sheet/)
    {: .callout}
    ~~~
    {: .source}

    Produces: 

    > ## Note check out the Markdown CheatSheet for standard syntax 
    > check out the Markdown CheatSheet for standard syntax 
    > [Cheat Sheet for Markdown](https://www.markdownguide.org/cheat-sheet/)
    {: .callout}

   

    These tutorials use some addons.  See [Formatting episode]({{ page.root }}{% link _episodes/04-formatting.md %}) for details.


    ---

    
    > ## I have to indent the text within a particular numbered section or the number resets
    {: .callout}

2. Refer to a different episode 

    ~~~
    [Formatting episode]({{ page.root }}{% link _episodes/04-formatting.md %})
    ~~~
    {: .source}

    Produces: 

    [Formatting episode]({{ page.root }}{% link _episodes/04-formatting.md %})

    ---

3. Show me some source code

    ~~~
    for thing in collection:
        do_something
    ~~~
    {: .source}

    or you can use specific language specifiers like `{: .language-python}`

    Please write code so that people can copy directly over to their terminal to run it.  No terminal prompts at the beginning of lines.

    ---

4. Ask me a tricky question 

    ~~~
    > ## Challenge Title
    >
    >  What happens if I type?
    >
    > ~~~
    > rm -rf *
    > ~~~
    > {: .source}
    >
    > > ## Solution
    > >
    > > This is a bad thing to do
    > >
    > > ~~~
    > > You have deleted all your files
    > > ~~~
    > > {: .output}
    > {: .solution}
    {: .challenge}
    ~~~
    {: .source}

    Produces: 

    > ## Challenge Title
    >
    >  What happens if I type
    >
    > ~~~
    > rm -rf *
    > ~~~
    > {: .source}
    >
    > > ## Solution
    > >
    > > This is a bad thing to do
    > >
    > > ~~~
    > > You have deleted all your files
    > > ~~~
    > > {: .output}
    > {: .solution}
    {: .challenge}

    ---

5. Make certain you can version things so the lesson is easy to keep up to date.

    ~~~
    export CODE_VERSION=v20
    ~~~
    {: .language-bash}

    Allows you to do 

    ~~~
    setup $CODE_VERSION
    ~~~
    {: .language-bash}

    throughout your lesson. 

    (Visual code and other editors also allow you to do global changes if you have to. )
