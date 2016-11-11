# An Introduction to Shell Scripts

## Objectives

By the end of this 25 minute lesson you should be able to:

- Write an executable and globally available shell script in both node and bash

## Motivations

Shell scripts are particularly helpful for automating work on team and personal projects, or for common operations specific to your own daily use cases. Consider writing them at times when you need some support from a program, but the specifics of your needs aren't well suited to finding already existing programs. Perhaps you don't have time to make something production ready, but do have time to write a quick and dirty tool to do some heavy lifting for you or your team: this is a great time to whip up a shell script.

Here are some possible scripts to get your imagination working:
- a `mkcd` script to both make a directory and change into it at the same time.
- a `commit-and-push` script that you pass a commit message and git remote to and that commits and pushes your work in one command.
- a `props` script that gives you a compliment with a fun emoji anytime you need it.
- a `pamela` script lets you email your coworker Pamela from the command line.
- a `lock-doors` script that sends commands to your IOT door lock system.

:question: Can you think of a time when you wished you had some greater automation at your disposal for a task that was rather individualized, or team specific?

## How to Write a Script

In order to write an executable and globally available script, in any scripting language of your choice, there are **three** things which must be true about your script:
1) It must indicate which interpreter will be used to run it by containing a special comment called an **interpreter directive**.
1) It must have **permissions** set on it so that you can execute it.
1) It must be located somewhere in your file system where your shell can find it, by placing it somewhere on **the PATH**.

### The Interpreter Directive

Unlike programs in some languages like C, C++, C# and Go, which are *compiled* down to machine code and executed directly by the computer's CPU, programs in *interpreted* languages like JavaScript, Python, Bash, and Ruby, are given to an interpreter program to be run.

Here is an example, passing the `howdy.py` file to the *python3* interpreter:

```python
# howdy.py
print('howdy')
```

```bash
$ python3 howdy.py
howdy
```

:question: Assuming you had a file called `index.js`, what would the command be to give it to the `node` interpreter to run?

When we create an executable script, we want to be able to call it directly from the command line, without giving it to an interpreter:

```bash
$ top-hn-story
Writing Your Own Programming Language
```

In order to do this we start our script with a special kind of comment called an *interpreter directive.* An interpreter directive is the first line of a file that starts with `#!` (aka *shebang* or *hashbang*) followed by the path to the location of the interpreter that you'd like to run your script.

If you don't know the path to where the interpreter you need is, use `which <name-of-interpreter>`. For example I can find the `ruby` interpreter's location on my computer as follows:

```bash
$ which ruby
/usr/bin/ruby
```

:question: How I would find the location of the `node` interpreter on my computer?

:star: Locate the bash interpreter located on your computer.

Now you can add, to the first line of a script, the hashbang, followed by the path to the interpreter. Here's an example `howdy.js` file with an interpreter directive for the `node` interpreter, which may not be in the same location on your computer (read more about how to address this issue in the additional reading below):

```javascript
#! /usr/local/bin/node

console.log('howdy');
```

### Setting Execute Permissions on a Script

Once your script has an interpreter directive, you still need to set permissions on the file before you are able to execute it as a standalone script. To give a file permissions to be executed by the current user, simply use the

`chmod u+x <filename>`

command. The `u` indicates the current user, the `+` part of this command means to *add* permissions, whereas a `-` would mean to remove them. So assuming we were in the same directory as the `howdy.js` file, we could issue:

```bash
$ chmod u+x howdy.js
```

Now we are ready to execute our script simply by entering the path to it into the command line. Again assuming `howdy.js` is located in the current directory:

```bash
$ ./howdy.js
howdy
```

:star: Create your own `howdy.js` script, that prints `howdy <your-name>` to the terminal and set permissions on it so that you can execute it.

:star: Run your `howdy.js` script.

:star: Remove execute permissions and try to run it, what happens? Add back execute permissions.

### Placing your Script on the PATH to Execute it From Anywhere

Once your script has an interpreter directive, and permissions to be executed, the last step is to place it somewhere in the file system so that you don't always have to give the entire path to its location everytime you want to call it. Let's investigate a couple of things before learning how to do this.

:star: Given that you know how to locate where an interpreter is in the filesystem, and that an interpreter is just a program you can run from the shell like `cd`, or `pwd`, find the location of the `ls` program.

:star: Now that you know the entire path to `ls`, run `ls` from the command line by passing in the entire path to it.

:star: Try to run `howdy.js` directly, without giving the path to its location.

So even though we can execute programs like `ls` by giving the command line their entire path, or by simply issuing the command directly, we cannot call our `howdy.js` script directly, even though it has an interpreter directive and execute permissions.

Here's the reason why. Your shell has access to a variable called `PATH` that contains a list of directories, and whenever you issue a command from the command line, without giving a location in the filesystem, your shell automatically goes looking for a file by that name in each of the directories in the `PATH` variable until either it finds the file and tries to execute it, or reports that it cannot be found.

`PATH` is one of the *environmental variables*, meaning, that it is available to you in any running shell.

We can use `echo` to look at the value of `PATH`, but need to place a `$` in front of `PATH` or else the shell won't know to treat it like a variable, and will instead simply print out the word `PATH`:

```bash
$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/go/bin
```

As you can see, the `PATH` variable's value is just a colon separated list of directories. Let's pipe this output into the `tr` command to replace those colons with newlines for easier reading:

```bash
$ echo $PATH | tr ":", "\n"
/usr/local/sbin
/usr/local/bin
/usr/bin
/bin
/usr/sbin
/sbin
/usr/local/go/bin
```

:question: Which directory is the `tr` command located in?

:question: Is that directory on the PATH?

:question: What do you think we need to do to make our `howdy.js` file available to us anywhere in the filesystem?

:star: Move `howdy.js` somewhere onto the path (`/usr/local/bin` is a conventional location for user created scripts). Rename the file to `howdy` instead of `howdy.js`.

:star: Open a new terminal tab or window and go to a different directory and run `howdy`.

:star: If you haven't already guessed it, `which` also uses the `PATH` variable to locate commands. Use `which` to locate your `howdy` command.

:star: Tab completion also uses the `PATH` variable, type in `how` and then hit `TAB`.

## Assessment

:star2: Create a node script called `how-to-script` that when run from anywhere, prints the 3 things you need to do in order to make a globally available executable shell script.

:star2: Create a shell script in bash called `path` that prints each directory on the `PATH` on its own line, using the command above.

## Further Reading

- Learn more about [the `env` utility](http://stackoverflow.com/questions/33509816/what-exactly-does-usr-bin-env-node-do-at-the-beginning-of-node-files) for a more portable interpreter directory.
- Learn more about [the distinctions between the shell, terminal, command line and Console](http://askubuntu.com/questions/506510/what-is-the-difference-between-terminal-console-shell-and-command-line).
- Learn more about [reading file permissions with `ls -l`](https://en.wikipedia.org/wiki/File_system_permissions#Notation_of_traditional_Unix_permissions).
