# Git R Done

You may or may not be familiar with [`git`](https://en.wikipedia.org/wiki/Git) or you may use it everyday.  Whether that is or isn't true, `git` is a piece of distributed `scm` (Source Control Management) software.

The purpose of this article isn't to make you an expert, but to enforce the best practices for using `git`.  If you ever run into issues with branches, remotes, or merge/pull requests it's probably because you're doing something wrong at a pretty basic level.  Basically, you should know enough to never have to say:

> Help, my git is borked! [sic]

Source control is nothing new.  One of the first notable systems was just called [`Source Code Control System`](https://en.wikipedia.org/wiki/Source_Code_Control_System) and was developed at Bell Labs in 1972.  There's much more to that story, but the only point here is that: after nearly fifty years, people kind of have it figured out.

That said, people seem to get into trouble trying to second guess the system.  Then they either get overwhelmed with the technical under pinnings and give up or they end up in some weird, detached branch limbo and someone else has to come over and help them.

Let's start with the basics:

__Use a terminal!__

Yes, many IDE (Integrated Development Environments) and third party UI tools exist, but to do it right those often just get in the way.  Use `bash`, `zsh`, `fsh`,  or (for the Windows folks) PowerShell.  They all have `git`.  

Now, before I talk about _how_ or _why_ I use `git` the way I do, I'm just going to start with an example scenario and work back from there.

Here's a problem statement to start with:

_You're a new developer on a software team.  The company is using [GitHub](https://github.com/) and you have just been sent a URL to a story and told that they expect a Pull Request (PR) from you that encompasses your work._

Again, __I am skipping a bunch of background__ (we will talk on things like setting up your ssh key and such).  For users of [GitLab](https://about.gitlab.com/) the process is the same (they both use `git`, I will explain the differences later) only with a difference in nomenclature: "pull request" versus "merge request."  They're basically the same thing and the difference in naming is really a sort of "glass half full" versus "glass half empty" argument.

More to the point: I first need to clone the repo.

__Step 1: Clone Repo.__

```bash
git clone git@github.com:ezweave/gitexample.git
```

Now I have a _local_ version of this repository. 

__Step 2: Go Into Repo__

Easy peasy:

```bash
cd gitexample
```

__Step 3: Create Local Branch__

```bash
git checkout -b example_issue
```

I named this one "`example_issue`" but you can use whatever would work to describe the work you are doing.

__Step 4: Start Coding__

This isn't a git thing.  It's just a usage thing.  But what if you find yourself at the end of your first day and you didn't quite finish it.  You have a local copy, but what if your laptop is stolen by some angry neighborhood raccoons or you spill a liter of Slushie on it?

Well, it's a good idea to __commit and push often__.  I will explain _roughly_ what is going on, but here is what I am going to do today.

__Step 5: Commit Often__

If I have new files, I do two things:

```bash
git add .
git commit -am 'Stopping for today'
```

The first command adds any new files to the local repository (that are not in `.gitignore`, but we will tackle this in a bit).  The second command is a handy one line way of committing all changes (`-a`) with a one line message (`-m 'message goes here'`).

__Step 6: Push It To The Server__

Pushing to the server/cloud is easy and costs nothing.  No one else cares that you pushed your branch, as long as you named it uniquely.

```bash
git push origin example_issue
```

Now, `git` gets a response from the server/cloud (either GitHub or GitLab, in most cases) and will even prompt you to make a "Pull Request":

```bash
mweaver@Matts-MacBook-Pro ~/code/gitexample (example_issue)$ git push origin example_issue  
Total 0 (delta 0), reused 0 (delta 0)
remote:
remote: Create a pull request for 'example_issue' on GitHub by visiting:
remote:      https://github.com/ezweave/gitexample/pull/new/example_issue
remote:
To github.com:ezweave/gitexample.git
 * [new branch]      example_issue -> example_issue
 ```
 
 Clicking that URL will bring up a web page where I can _automagically_ create my "Pull Request":

 
