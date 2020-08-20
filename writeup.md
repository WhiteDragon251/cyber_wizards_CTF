# <Challenge Name>

---
---

## Description

> mastardet, I am giving this just randomly right now, please let me know if you want me to edit this.
> Find the flag in the binary.

* **Category:** Basic Binary and Linux
* **Creator:** [mastardet](https://www.youtube.com/channel/UC66idcoWzeZ_AsTnYr3L37g/featured)
* **Challenge File:** [Link to the tar.gz file goes here]()

Make sure you try the challenge once on your own beofre reading the writeup. You can find **hints** to the challenges at the bottom of the page.

---
---

## Writeup

---

#### Finding the password

The challenge begins with a binary, that's all we are given. So the first step is obviously to figure out what the binary does.

```
./challenge_linux
```

We are given a prompt saying `root@cyberwizards password:`. Well, looks like a machine and we definitely need a password to get in. I tried sending **payloads** thinking I could overflow it, but that didn't work! :( Ok, so let's try some other stuff out.

Let's try reversing the binay and see what we get.

```
r2 -d ./challenge_linux
```

Ok, so this is insane, when we try to print the functions, there are more than 1000 functions and there is no main function. If you all wanna go ahead and try to reverse it then please carry on, but I really don't think you will get any success. And well, when you try going through the binary in `vim` you will see that it is written in `go` which must be why its hard to reverse it. Or you can simply use the file command to see that its written in `go`.

Alright then, let's try out the simpler stuff. Let's just use `strings` and `grep` to get useful information.

```
strings ./challenge_linux | grep -i --color flag
```

This looks interesting, but doesn't seem we can use it at the moment to our advangtage in anyway. So let's move on.

```
strings ./challenge_linux | grep -i --color root
```
Can't see anything useful in this one.

```
strings ./challenge_linux | grep -i --color cyberwizards
```

Alright, so this one took me a while (actually it took me hours to figure out, haha...) but when you look at it closely you can see it!! This must be the password we were looking for. Let's see if it works...

[Machine_promt]()

Ok great!! The password we found is correct. 

---

#### Reading the flag in the restricted shell environment

And now we are given a shell and when we run `ls -la`, we can see that the `flag.txt` is right there. 

[ls la]()

Alright, let's grab it using `cat`.

[cat_error]()

So here comes the twist in the story! I am sure you must have noticed by now that commands like `cat`, `less`, `more`, etc. are not even there. In fact, you can't run most of the commands. Some of you might try to `reboot` or `shutdown` and will get this-

[shut_reboot_flag]()

Sorry mates, but that is not the flag. We need to dig more to find it.

Now, when you will run the `help` command, you will see all the commands available to you. So we have `ls`, `pwd`, `export`, `exit`, `whoami`, `history`, `logout` and `clear`. Among all these, all that seems useful is the `export` command.

[help_command]()

So I tried running the `export` command and even tried to modify the values of the environment variables such as `$PATH` and `$SHELL` but all that ended up giving me an error.

[export_error]()

So, when I took a look at the **HINT 4** and **HINT 5**, you will immediately understand that you need to work on the `history` and the `export` command. So let's get started.

And obviously, in these situations Google is our best friend!! So I did a lot of googling and then I came across this

[Googling]()

So, `HISTFILE` is an **environment variable** that specifies the file for `history` command to read from. This looks interesting... So what happens when we set the `HISTFILE` to `/root/flag.txt` :)

[HISTFILE]()

Alright, great, the command worked!! But still, if we run `history`, that prints the same old stuff.

[history_fail]()

So, I looked up a little more on Google and I finally found this on StackOverflow

[History-r](https://unix.stackexchange.com/questions/549450/history-command-does-not-show-all-lines-in-histfile)

Ok, so now we know that we need to refresh the history using the `history -r`. Alright, so let's grab that flag hackers!!

[Flag]()

Voila!! We have the flag!!

---
---

## Hints

* **HINT 1:** try searching strings with the hostname of the machine, in the binary
* **HINT 2:** flag.txt has the flag in it. Don't worry if you see it's 0 bytes
* **HINT 3:** you could ask the server for `help`
* **HINT 4:** We are nothing without **history**
* **HINT 5:** And linux is nothing without environment variables

---
---

## More about the creator of the challenge

This challenge was created by **mastardet** who is a great friend and even mentor to me. He is great person with around 15 years of experience in the field and currently, works as an **ethical hacker** and a **software developer**. He has helped me learn a lot of stuff in the field of penetration testing. Make sure you guys check out his [youtube channel](https://www.youtube.com/channel/UC66idcoWzeZ_AsTnYr3L37g/featured), definitely you will learn lot of good and useful stuff from his videos!
