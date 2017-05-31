---
title: Contributing
permalink: contributing.html
---
# Contact us

The best way to contact us is to join <a href="irc://irc.spotchat.org/linuxmint-dev">#linuxmint-dev</a> on irc.spotchat.org.

As a general rule of thumb, don't attempt to start a conversation with pointless statements. If you just say "hello" and shut up, you will get ignored. If you ask if you could speak/ask a question, you will get ignored. If you ask "anyone here?", you won't get ignored. Three different people will answer "no" in three different ways.

Also, wait for people to answer. It might take hours, but eventually someone will reply. Don't quit after 15 minutes. Or 1 hour. Or 3 hours. Wait as long as you can. It doesn't hurt to leave the chat window open. Remember everyone is in a different timezone and you might be coming at a time where everyone is asleep.

This is also why you should say something meaningful, not "hi". If what you say actually has some content, someone will see it and reply sooner or later, even if decades have past. If you said "hi" an hour ago, replying is very awkward.

I'll reiterate. Never, <em>ever</em>, say "I have a problem", or "Hello", and wait for someone to reply before continuing. It will never happen. Just keep speaking until you have delivered the full message you wanted to deliver. THEN someone will reply and help you.

Thanks for reading and (hopefully) making my life on the IRC less miserable.

## Translations
Translations are done on <a href="https://translations.launchpad.net/linuxmint/latest/+templates">Launchpad</a>. This includes both Cinnamon and Linux Mint translations. Help is particularly needed near release time since a lot of new words need to be translated for newer versions of Cinnamon and Mint tools.

You can also join a <a href="https://translations.launchpad.net/+groups/linux-mint">Linux Mint translation team</a> if you wish.

## Coding
Mint and Cinnamon projects are found on <a href="https://github.com/linuxmint/">GitHub</a>. The languages used are mainly C, Javascript and Python. It is a good idea to talk to us on the IRC before starting any coding work. Do not email developers directly. Everything shall be discussed on the IRC (see above for how not to get ignored on the IRC. If you are ignored, you are doing it wrong).

## Reporting an issue
So. You have a problem with Cinnamon. Something's not working. What should you do about it?

First have a look at the bug tracker of <a href="https://github.com/linuxmint/Cinnamon/issues">Cinnamon</a> or whatever project you find the bug in. There is a search button, and see if someone has reported the issue already. GitHub unhelpfully defaults to searching open issues only, but <strong>search in both open and closed issues</strong>. Issues are closed whenever the problem is fixed in the development version of Cinnamon, and it is likely that you are not running the development version. This remains true even if you are running a stable version released a few days ago. There is no point in reporting a bug that is already fixed.

If possible, try to install the latest development version of Cinnamon. Bugs come and go quickly, and might be fixed without someone opening an issue at all. If you are running Mint or Ubuntu, you may want to use the <a href="https://launchpad.net/~gwendal-lebihan-dev/+archive/ubuntu/cinnamon-nightly">nightly ppa</a> to run the latest version. Many other distros also have similar packages (eg. AUR for Arch users).

If the bug hasn't been reported, and it is reproducible on Cinnamon git, congratulations! You have found a new bug. Go ahead and create a new issue. Don't hijack existing issues if they are not the same issue, even if they are similar. If it is a separate issue, make a separate issue. They are free.

When opening your issue, be as descriptive as possible. If possible, tell us

- what linux distro you are running (plus version number if applicable)
- the version of Cinnamon you are running (output of `cinnamon --version` from the terminal, or git/nightlies)
- what theme/applets you were using (if relevant)
- how exactly we can reproduce the error
- what you expected to happen
- what <em>actually</em> happened
- whether the problem happens consistently or intermittently

In particular, don't just post an error log and expect us to know what was going on. Explain what you were trying to do, anything special you were trying out, what problems the errors caused (freezing, crashing, or nothing) etc. Contrary to popular belief, developers are not psychic.

Note that you <em>don't</em> have to include all these pieces of information all the time. For example, if your issue is about Cinnamon crashing under certain circumstances, you don't have to say "I expect Cinnamon to not crash" - we all know that and it sounds silly.

By opening a new issue, you will be automatically subscribed to that issue, and receive notifications of any updates to it by the development team or other users. Don't hesitate to add more information if you discover something else - every little bit can help towards solving a problem.

After creating the issue, hopefully, very shortly, your issue will be confirmed as a bug. Do not open more issues on it! All we can ask is that you remain patient while the issue is sorted out. Some issues may only take a few minutes to fix, while others can vex us for days or even weeks before finding a solution. Note that we usually don't reply unless we have found a fix/workaround or need more information. This does not mean that we are not looking into the issue. We just don't reply if we cannot add anything of value.