The purpose of this repo is  to plan, iterate, and develop a system for backing up my desktop computer.

Here are some basic details:

My desktop runs Ubuntu. I have been using Linux for a long time (going on 20 years!). 

Linux has gotten better over time. So have my skills as a human operator of it. 

This means that my systems also become more "valuable" over time: the changes that I make to define the system as I want to use it accure. Which means that a system failure would be progressively more inconvenient. 

One approach is backup. However, I have a pretty excellent BTRFS snapshotting system that has proven its worth time and time again. 

I don't want to solely depend on this, however, as snapshotting is not the same as true backup. I would like to have one "ultimate" layer of protection: if the system were truly bricked (think hardware failure) I can get a new system back up and running.

My computing philosophy is this: I try to keep all my important data in the cloud. My desktop is a workspace where I work on repositories, videos, photos, docs, audio, browse the internet. 

What kind of data is it annoying to lose? 

I'll give a mundane example:

I just installed an Open In VS Code addition to KDE Plasma (found thanks to [this](https://store.kde.org/p/1413799/)) nice project. 

Let's imagine that my computer catches fire tonight and I buy a new one tomorrow. 

I would:

- Install the OS 
- Try to remember all the programs I installed and where they came from (Snap? Apt sources? Flatpaks?) 
- Have to reinstall some of the custom utils I've built - like a home IP cam monitor 
- Reinstall npm packages; reinstall pipx stuff 
- Hope that VS code backed up to the cloud properly 
- Reinstall Gitkraken. Pull down repos. Recreate my project manager favorites folder 
- Reinstall dot files from YADM 

Then I have packages like Claude Code which were installed using the official method on Anthropic's website. And projects that were build from source from Github.

The last time I went through a full OS reinstallation was about a year ago when I moved back from Fedora to Ubuntu. It took the best part of a week and swallowed up basicaly all of my free time. I can't afford such lengthy processes!

# My Backup History!

I've been thinking about backup for a while. I would rather not think about backup at all!

But I've explored a few approaches for this "top level" system backup and each have their pros and cons:

## Clonezilla

As close as you can get to "reinstall my OS exactly as I had it".

The deficiencies are that: 

1) It's a bit tedious to use (manual method) 
2) For that reason, when I used it, I tended to do it relatively infrequently 

Ideally backups are automated and done regularly so that you could roll back on a fresh env without too much loss. 

## Traditional Incremental Backup Approaches

There are more of these than I can remember from rsync to the rest of them. These are fairly straightforward: you incrementallyl back up to say an NAS.

However, over time, they've proven less useful to me: for rollback (update broke a driver) I'll go to my BTRFS snapshots which are taken daily, weekly, monthly, and more. BTRFS snapshotting is one of the most impressive technologies I have ever seen and I have used it for recovery enough times that I have full trust in it for this kind of rollback. 

For the "get my OS back" ... I've never felt confident that these tools could really handle that well. 

# My Idea: Treat OS As Ephemeral Data (Mostly)

Over time, my thinking has changed a bit.

Backing up globs of local data doesn't make much sense if you back it all up to the cloud anyway. 

The only data that I can't get from the internet are those little configuration changes (say my F13 keymapping for my dictation button!). These multiply over time and are what makes recreating the environment such work. But these are very lightweight files.

Although Ansible (etc) are usually used for other things, I've thought that they might actually make more sense for what I'm trying to achieve.

The list of packages I have installed (the stack) and configs are what constitute my environment. That's the data that I want to protect. 

So rather than backing up files, version control the full system posture, if possible - and ideally in such a way that it's versatile enough to be recreatable, as far as possible, if the underlying hardware changed slightly.

Automate this backup at a regular basis (daily, weekly). Do so locally and to the cloud. And then create some recovery system that - were it ever called into action - could be used to recreate the environment easily from the remote. 

# Recovery Scenario

Here's what I envision as an ideal recovery scenario:

- My desktop catches fire! The whole computer is fried!

Well - I have my data in the cloud so that's still there. But setting this up again is so much work!

I go out and buy a new computer with as much space as I had before. 

I can reinstall the old environment (KDE 25.10 or whatever I'm using at the time). But that will get me back to a blank slate.

From here, either:

1) The system reprovisioning is a script that includes the package and config installation or 
2) I reprovision onto a blank OS and then run something like a playbook

Either option would be fine. Even if a full system restore were not 100%, 80% success would be "good enough."

The ultimate backup posture I'd like to achieve and know I have reliably is this:

- In the first instance, BTRFS snapshots to recover from minus hiccups  
- If a full restore is ever needed, an installer will handle most of the work and is regularly updated with the system config
































