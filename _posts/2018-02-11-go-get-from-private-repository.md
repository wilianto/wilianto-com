--- 
title: "[TIL] Go Get From Private Repository"
date: 2018-02-10T09:52:00
categories:
- golang
- til
layout: post
meta_keywords: go get private repository, enable terminal prompt, git terminal authentication, git ssh instead of https
meta_description: How to get golang package from a private repository
comments: true
---
This week I'm working on a golang project. This project needs depedencies to other golang libraries. Unfortunatelly, one of them are a private repository.

When trying to <b>go get</b> it, I got this error on my Terminal.
```
# cd .; git clone https://github.com/radionbot/bot /Users/wilianto/Workspace/golang/src/github.com/radionbot/bot
Cloning into '/Users/wilianto/Workspace/golang/src/github.com/radionbot/bot'...
fatal: could not read Username for 'https://github.com': terminal prompts disabled
package github.com/radionbot/bot: exit status 128
```

It was caused by git by default disabled terminal prompt. The repository was expected an authentication process on it.
After search for reference I got a reference how to enable git terminal prompt.

```
GIT_TERMINAL_PROMPT=1 go get xxx
```

It worked and I could enter my username and password to download the repository.

Then...

I pushed my code and run it on CI. It failed because I can't do authentication on CI. 

So, I need to use different approach by using <b>ssh</b> to clone it instead of <b>https</b>. These are the steps:
- Adding my CI ssh public key to repository
- Adding one more step on my CI script to set git config to use <b>ssh</b> by default
```
git config --global url."git@github.com:".insteadof "https://github.com/"
```

It works for me. Does it work for you as well?

Ref:
- [stackoverflow.com/questions/32232655](https://stackoverflow.com/questions/32232655/go-get-results-in-terminal-prompts-disabled-error-for-github-private-repo)
- [stackoverflow.com/questions/27500861](https://stackoverflow.com/questions/27500861/whats-the-proper-way-to-go-get-a-private-repository)
