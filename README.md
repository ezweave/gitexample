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

## Example

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
 
 Clicking that URL will bring up a web page where I can _automagically_ create my "Pull Request."  I always go ahead and create a new request even if I'm not ready for a code review yet.  I just make the title say 'WIP'.  GitHub will recognize this as a "Work In Progress" and that tells other members of your team to ignore it for now.  

<p align="center">
 <img src="/static/images/pull_request.png"/>
</p

And because I am not creating the request manually in the GitHub UI, I don't make any mistakes about what branch I'm creating.

__That's It!__

Now, I've hand waved over a bunch of details that should be discussed.

But let's just list the steps (from scratch):

1. Clone repo
1. Go into repo directory
1. Make local branch
1. Add files and commit
1. Push to origin
1. Make a Pull/Merge Request from the magic URL

## SSH Keys

I didn't go into this before, but I always clone and push updates via `ssh` keys.  The process to create a key and link it to your account is simple.  You can find instructhions [here](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)(for GitHub) or [here](https://docs.gitlab.com/ee/ssh/)(for GitLab).

I have multiple keys setup for different work (whether it's for my day job or personal projects) and add them to my box under the: `~/.ssh` directory.

Sometimes the keys need to be re-added, due to a restart, and I simply tell `ssh-agent` to add a specific key:

```bash
ssh-add ~/.ssh/id_rsa_work  
```

This prompts me for the password for that key (which I keep in LastPass or Keychain) and then lets me push/pull/clone with impunity.  _Some_ repositories are set up to allow `username`/`password` combinations, but not only does this require you to type it _all the time_ (and thus hampers your impetus to "commit early and often"), but it is also less secure.

__Set up your SSH keys!__

## What Is Git?

The first thing we should establish is _roughly_ what `git` is.  It's just a piece of software that runs _locally_.  It is _not_ a server.  That's where GitHub or GitLab come into play.  

`git` is really just a command line tool that acts like a "file system on top of your file system" ("I heard you like filesystems...").  It's actually an index (built from caches) _and_ a database (I'm hand waving).  When you `clone` a repo from GitHub or GitLab, you are just getting a copy of that index and database.  It's yours now.  The `clone` command also sets up a remote, and by default `origin` is the remote's name.

You don't see any of that, but unless you tell `git` to do something with the remote, it is just local to the box you cloned the repo on.  You can also make a new repo and name it something else, run `git init` and manually set up the remotes... __don't do that__.  99% of the time you will never need to do that.

### Branches

The `git` mantra is "commit early, commit often".  Your local `git` repo and even your remote branch, can be chock full of all kinds of commits.  These can be "squashed" in your Pull/Merge Request to make them more sensible.  I commit after making a major change or getting some tests to work and `push` it immediately.  We will get into this more in a bit, but if you made a branch, __that branch is yours__.  There is _zero_ penalty for doing 50 commits.  In fact, there is much more _risk_ in doing one giant commit.  

I will say it again:

__Commit early, commit often.__

### Staging Commits

There are a few ways to stage commits.  You can do it by file (don't) and you can do it with your default shell editor.

There are two things to know:

* Adding a file: this tells `git` that some new file(s) needs to be tracked.
* Committing changes: this tells `git` that you have made changes to files it already knew about.

You _might_ __add a file specifically__ (because `git` doesn't know that it needs to track something you just made):

```bash
git add src/newFile.ts
```

But assuming you don't have a bunch of junk that isn't part of your actual work, it's much easier to do:

```bash
git add .
```

And for staging, if you just run:

```bash
git commit -a
```

It will pick up all files you _did_ change.  Adding the `-m` flag as I did just means you don't need to hop into an editor to add a comment.

You _can_ also stage commits separately, but this can also lead to a mess and forgetting to include some changes.

### .gitignore

At the base of every repository (e.g. the directory at the root of a project) managed by `git` is a `.gitignore` file.  This is a list of files or directories that you don't want `git` to track or to remind you that it isn't tracking.  Use it sparingly.

### The Power of Reset

At times, you might have some test data or test code you don't want to add to a commit and you've already committed what you did care about.  Assuming you have added what you care about prior, you can simply run:

```bash
git reset --hard
```

__Do not do this lightly!__

It will obliterate from your local `git` database any files that were not tracked by `git`.  It is a hammer, but it can be a useful hammer.

### Remotes

By default, by running `git clone` brings down the index and database and sets you up with a default `remote`, defaulted to `origin`.

With my example above, that looks like this:

```bash
mweaver@Matts-MacBook-Pro ~/code/gitexample (example_issue●●)$ git remote                                                    origin
```

It shows I have one remote and it is called `origin`.  Now I can ask for more info on that remote:

```bash
mweaver@Matts-MacBook-Pro ~/code/gitexample (example_issue●●)$ git remote show origin 
* remote origin
  Fetch URL: git@github.com:ezweave/gitexample.git
  Push  URL: git@github.com:ezweave/gitexample.git
```

And some other _stuff_, but you don't need to know about that now.  You just now that if you're going to `push` a branch it will go to `git@github.com:ezweave/gitexample.git`.

### Pushing

Now your local `git` could be wildly different than `origin`.  When you run `git push origin example_issue` you are telling the remote server "here are some changes, this is the branch name".  __This has no effect on origin's master branch__.  This goes back to talking about branches.  What you do need to know is that when you run a command like:

```bash
git push origin example_issue
```

GitHub or GitLab are assuming (per contract, I'm not anthropomorphizing software) that you want to push your local branch named "`example_issue`" to a branch on `origin` called "`example_issue`".  You _can_ tell GitHub or GitLab that you want to push it to a differently named branch because you picked a name that is still a live branch on `origin` by just appending it to the end of that command:

```bash
git push origin example_issue matts_example_issue
```

Don't get in the habit of doing this.  Branches are usually deleted on `origin` after they are merged.  This can just cause you headaches down the line.

You _can_, however, see all branch names on origin by running:

```bash
git fetch --all
```

But barring that, your branch exists on master as soon as you push it there.  And if you didn't have to tell the remote to change the name of it, you can simply type:

```bash
git push origin example_issue
```

### Tracking Branches

Now, you might be in a position to look at someone elses branch.  Perhaps they got sick and couldn't finish it or, more likely, you are reviewing their Pull/Merge Request and you want to run their code.  There are many ways to do this, but the most straight forward is:

```bash
git checkout -t origin/their_branch_name
```

This will update your local `git` with a new branch that has the same name.  There are _other_ ways of doing this, but don't muck about with them.  Keep it simple.

Now all you have to do is run:

```bash
git pull
```

And all is updated locally.

### Setting Upstream

Now, if you made the branch locally, `git` doesn't know that you might be getting changes from someone else.  So if Linus Torvalds jumps into my branch, and runs `git checkout -t origin/example_issue` and pushes some updates to `origin`, I can't just type:

```bash
git pull
```

`git` will complain that it doesn't know where to pull from.

So I have to tell it:

```bash
git branch --set-upstream-to=origin/example_issue
```

Now this won't work correctly if I am not _on that branch_ already (don't do this from master) as it assumes I'm talking about the branch I am currently on (otherwise you have to add it to the end of that command).

## DO NOTS

There are tons of ways to use `git` and newer or novice users get into trouble by doing the following:
* changing origin
* not opening Pull/Merge Requests via the handy dandy first push link
* stage commits separately
* trying to revert or merge branches on their machine

## Conclusion

`git` is not complex.  It's basically just a database that manages file changes.  Trying to _overthink_ how it works is not going to help you.  All you're ever doing is telling your local database or the remote/server database that it needs to update or you need updates from it.  

Here's the take aways:

* __Use a terminal__
* __Setup SSH__
* __Commit early, commit often__
* __Push to origin frequently__
* __Create your pull/merge request right away__