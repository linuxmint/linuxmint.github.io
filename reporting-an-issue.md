---
title: Reporting an issue
permalink: reporting-an-issue.html
---

# Reporting an issue in Cinnamon or Linux Mint

So. You have a problem with Cinnamon. Something's not working. What should you do about it?

First have a look at the bug tracker of [Cinnamon](https://github.com/linuxmint/Cinnamon/issues) or whatever project you find the bug in.

There is a search button, and see if someone has reported the issue already. GitHub unhelpfully defaults to searching open issues only, but **search in both open and closed issues**.

Issues are closed whenever the problem is fixed in the development version of Cinnamon, and it is likely that you are not running the development version. This remains true even if you are running a stable version released a few days ago. There is no point in reporting a bug that is already fixed.

If possible, try to install the latest development version of Cinnamon. Bugs come and go quickly, and might be fixed without someone opening an issue at all. If you are running Mint or Ubuntu, you may want to use the [nightly ppa](https://launchpad.net/~gwendal-lebihan-dev/+archive/ubuntu/cinnamon-nightly) to run the latest version. Many other distros also have similar packages (eg. AUR for Arch users).

If the bug hasn't been reported, and it is reproducible on Cinnamon git, congratulations! You have found a new bug. Go ahead and create a new issue. Don't hijack existing issues if they are not the same issue, even if they are similar. If it is a separate issue, make a separate issue. They are free.

When opening your issue, be as descriptive as possible. If possible, tell us

- what linux distro you are running (plus version number if applicable)
- the version of Cinnamon you are running (output of `cinnamon --version` from the terminal, or git/nightlies)
- what theme/applets you were using (if relevant)
- how exactly we can reproduce the error
- what you expected to happen
- what *actually* happened
- whether the problem happens consistently or intermittently

If the program actually 'crashed' (The window disappeared, or, in the case of Cinnamon, the desktop crashed and you were presented with the restart dialog), you should also attempt to locate and attach low-level crash information. See [this guide](/reference/git/bugs/mintreport-crash-info.html) for more information on how to accomplish this.

In particular, don't just post an error log and expect us to know what was going on. Explain what you were trying to do, anything special you were trying out, what problems the errors caused (freezing, crashing, or nothing) etc. Contrary to popular belief, developers are not psychic.

Note that you *don't* have to include all these pieces of information all the time. For example, if your issue is about Cinnamon crashing under certain circumstances, you don't have to say "I expect Cinnamon to not crash" - we all know that and it sounds silly.

By opening a new issue, you will be automatically subscribed to that issue, and receive notifications of any updates to it by the development team or other users. Don't hesitate to add more information if you discover something else - every little bit can help towards solving a problem.

After creating the issue, hopefully, very shortly, your issue will be confirmed as a bug. Do not open more issues on it! All we can ask is that you remain patient while the issue is sorted out. Some issues may only take a few minutes to fix, while others can vex us for days or even weeks before finding a solution. Note that we usually don't reply unless we have found a fix/workaround or need more information. This does not mean that we are not looking into the issue. We just don't reply if we cannot add anything of value.
