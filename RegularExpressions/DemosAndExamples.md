# Regular Expression Demo and Practice Files

The purpose of the files in this directory is to provide a set of files that can be used to practice regular expressions. The files are intended to be used in conjunction with my [Regular Expressions](https://www.grahamwatts.co.uk/regex) blog post. In the sections belows we'll explore some example regular expressions targetting both a regular text file and a common format log file. Examples of the output will be provided to help you check your work. At the end of each section there will be some suggested stretch goals for further practice.

For the best experience I recommend viewing this file in a text editor that supports Markdown and Markdown preview such as [VS Code](https://code.visualstudio.com/), or in a specialised Markdown editor such as [Typora](https://typora.io/).

## Table of Contents

- [Regular Expression Demo and Practice Files](#regular-expression-demo-and-practice-files)
  - [Table of Contents](#table-of-contents)
  - [Tools](#tools)
    - [grep](#grep)
    - [Select-String](#select-string)
  - [Text File: SampleText.txt](#text-file-sampletexttxt)
    - [Simple Example](#simple-example)
    - [Capitalised First Letter Example](#capitalised-first-letter-example)
    - [Or Search Example](#or-search-example)
    - [Optional Characters Example](#optional-characters-example)
    - [Stretch Goals For Text File](#stretch-goals-for-text-file)
  - [Linux Log File: Linux\_2k.log](#linux-log-file-linux_2klog)
    - [Stretch Goals For Linux Log File](#stretch-goals-for-linux-log-file)
  - [Stretch Goal Answers](#stretch-goal-answers)
    - [Text File](#text-file)
    - [Linux Log File](#linux-log-file)

## Tools

These examples are intended to be completed with common, command line regular expression tools such as `grep` for bash (and similar shells) on Linux and MacOS and/or `Select-String` for PowerShell on Windows. If you are using Windows 10 or 11 and want to experiment with the Linux examples, you can use the [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install/) to run the Linux examples. Alternatively, if you're on Linux or MacOS and want to try out the PowerShell you can install [PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell) on your system.

### grep

`grep` is a command line tool found on most Linux and MacOS systems and can be run from the terminal in shells such as bash and zsh.

To use regular expressions with `grep` you need to use the `-E` flag to enable extended regular expressions. For example:

```bash
grep -E 'regex' file.txt
```

In some cases the `-E` flag may not work, particularly with regex characters such as `\d` so you may wish to experiment with the `-P` flag to enable Perl regular expressions. For example:

```bash
grep -P 'regex' file.txt
```

### Select-String

`Select-String` is a PowerShell cmdlet that can be used to search for text in files and/or strings. It can be used in the PowerShell terminal or in PowerShell scripts.

To use regular expressions with `Select-String` you need to use the `-Pattern` parameter. For example:

```powershell
Select-String -Pattern 'regex' -Path file.txt
```

## Text File: SampleText.txt

### Simple Example

We'll start with a simple search using `grep` for the word `Word` in the file `SampleText.txt`:

```bash
grep -E 'Word' SampleText.txt
```

The output should look like this:

```text
> grep -E "Word" SampleText.txt

Video provides a powerful way to help you prove your point. When you click Online Video, you can paste in the embed code for the video you want to add. You can also type a keyword to search online for the video that best fits your document. To make your document look professionally produced, Word provides header, footer, cover page and text box designs that complement each other. For example, you can add a matching cover page, header and sidebar. Click Insert, then choose the elements you want from the different galleries. Regular expressions, often abbreviated as regex, are a powerful tool used in computing for pattern matching and manipulation of text.
Themes and styles also help to keep your document coordinated. When you click Design and choose a new Theme, the pictures, charts and SmartArt graphics change to match your new theme. When you apply styles, your headings change to match the new theme. Save time in Word with new buttons that show up where you need them. To change the way a picture fits in your document, click it and a button for layout options appears next to it. When you work on a table, click where you want to add a row or a column, then click the plus sign. Regular expressions are sequences of characters that form a search pattern, mainly for use in pattern matching with strings, or string matching, i.e., “find and replace”-like operations.
```

Note that most terminals will highlight or colour the matched text. In the example above the word `Word` would highlighted in red, as below.

![Terminal output from zsh with oh-my-zsh highlighting the word Word in red](./assets/grep-simple-Word.png "Output from grep with the word Word highlighted in red")

We can make the same search using `Select-String` in PowerShell:

```powershell
Select-String -Pattern 'Word' -Path SampleText.txt
```

The output should look like this:

```text
> Select-String -Pattern 'Word' -Path SampleText.txt

SampleText.txt:1:Video provides a powerful way to help you prove your point. When you click Online Video, you can paste in the embed code for the video you want to add. You can also type a keyword to search 
online for the video that best fits your document. To make your document look professionally produced, Word provides header, footer, cover page and text box designs that complement each other. For example, you 
can add a matching cover page, header and sidebar. Click Insert, then choose the elements you want from the different galleries. Regular expressions, often abbreviated as regex, are a powerful tool used in 
computing for pattern matching and manipulation of text.
SampleText.txt:3:Themes and styles also help to keep your document coordinated. When you click Design and choose a new Theme, the pictures, charts and SmartArt graphics change to match your new theme. When you 
apply styles, your headings change to match the new theme. Save time in Word with new buttons that show up where you need them. To change the way a picture fits in your document, click it and a button for 
layout options appears next to it. When you work on a table, click where you want to add a row or a column, then click the plus sign. Regular expressions are sequences of characters that form a search pattern, 
mainly for use in pattern matching with strings, or string matching, i.e., “find and replace”-like operations.
```

Again, the output will be marked by the terminal such as the image below:

![Terminal output from PowerShell with the word Word highlighted in red](./assets/pwsh-simple-Word.png "Output from Select-String with the word Word highlighted in red")

This doesn't really show the use and power of regular expressions, so let's try something a little more interesting.

### Capitalised First Letter Example

If you've looked at SampleText.txt you may have spotted that the phrase "regular expression" has appeared a number of times, sometimes as "Regular Expression" and sometimes as "regular expression". Let's see if we can find all the instances in the file, regardless of capitalisation. Now, there are several ways to go about this with grep, such as using the `-i` flag, however we're going to focus specifically on using regular expressions.

We could try something like this:

```bash
grep -E 'regular expression' SampleText.txt
```

Or, in PowerShell:

```powershell
Select-String -Pattern 'regular expression' -Path SampleText.txt
```

However, this will only find the lines with the exact phrase "regular expression" in lower case like this:

```text
> grep -E "regular expression" SampleText.txt

Save time in Word with new buttons that show up where you need them. To change the way a picture fits in your document, click it and a button for layout options appears next to it. When you work on a table, click where you want to add a row or a column, then click the plus sign. Reading is easier, too, in the new Reading view. You can collapse parts of the document and focus on the text you want. If you need to stop reading before you reach the end, Word remembers where you finished – even on another device. The concept of regular expressions arose in the 1950s, when the American mathematician Stephen Kleene formalized the description of a regular language, and came into common use with the Unix text processing utilities ed, an editor, and grep, a filter.
Themes and styles also help to keep your document coordinated. When you click Design and choose a new Theme, the pictures, charts and SmartArt graphics change to match your new theme. When you apply styles, your headings change to match the new theme. Save time in Word with new buttons that show up where you need them. To change the way a picture fits in your document, click it and a button for layout options appears next to it. When you work on a table, click where you want to add a row or a column, then click the plus sign. In modern usage, “regular expressions” are often distinguished from the derived, but fundamentally distinct concepts of regex or regexp, which no longer describe a regular language.
To make your document look professionally produced, Word provides header, footer, cover page and text box designs that complement each other. For example, you can add a matching cover page, header and sidebar. Click Insert, then choose the elements you want from the different galleries. Themes and styles also help to keep your document coordinated. When you click Design and choose a new Theme, the pictures, charts and SmartArt graphics change to match your new theme. When you apply styles, your headings change to match the new theme. Today, different syntaxes for writing regular expressions exist, one being the POSIX standard and another, widely used, being the Perl syntax.
```

Note that we have fewer lines returned by this search than we did with the simple search for the word "Word". This is because we are only matching the exact phrase "regular expression" in lower case. The same thing would happen if we searched for "Regular Expression" in upper case. Give that a try yourself with this command and see:

```bash
grep -E 'Regular Expression' SampleText.txt
```

Or, in PowerShell:

```powershell
Select-String -Pattern 'Regular Expression' -Path SampleText.txt
```

So, what we need is a way to tell the search engine that we want the "r" in "regular" to either be "R" or "r". We also need to achieve the same thing with "e" in "expression". Fortunately, regular expressions allow us to express allow character sets using the `[ ]` syntax. So, we can try something like this:

```bash
grep -E '[Rr]egular [Ee]xpression' SampleText.txt
```

Or, in PowerShell:

```powershell
Select-String -Pattern '[Rr]egular [Ee]xpression' -Path SampleText.txt
```

The output should look like this:

```text
> grep -E "[Rr]egular [Ee]xpression" SampleText.txt

Video provides a powerful way to help you prove your point. When you click Online Video, you can paste in the embed code for the video you want to add. You can also type a keyword to search online for the video that best fits your document. To make your document look professionally produced, Word provides header, footer, cover page and text box designs that complement each other. For example, you can add a matching cover page, header and sidebar. Click Insert, then choose the elements you want from the different galleries. Regular expressions, often abbreviated as regex, are a powerful tool used in computing for pattern matching and manipulation of text.
Themes and styles also help to keep your document coordinated. When you click Design and choose a new Theme, the pictures, charts and SmartArt graphics change to match your new theme. When you apply styles, your headings change to match the new theme. Save time in Word with new buttons that show up where you need them. To change the way a picture fits in your document, click it and a button for layout options appears next to it. When you work on a table, click where you want to add a row or a column, then click the plus sign. Regular expressions are sequences of characters that form a search pattern, mainly for use in pattern matching with strings, or string matching, i.e., “find and replace”-like operations.
Reading is easier, too, in the new Reading view. You can collapse parts of the document and focus on the text you want. If you need to stop reading before you reach the end, Word remembers where you finished – even on another device. Video provides a powerful way to help you prove your point. When you click Online Video, you can paste in the embed code for the video you want to add. You can also type a keyword to search online for the video that best fits your document. Regular expressions are used in search algorithms, search and replace dialogs of text editors, and in lexical analysis.
Save time in Word with new buttons that show up where you need them. To change the way a picture fits in your document, click it and a button for layout options appears next to it. When you work on a table, click where you want to add a row or a column, then click the plus sign. Reading is easier, too, in the new Reading view. You can collapse parts of the document and focus on the text you want. If you need to stop reading before you reach the end, Word remembers where you finished – even on another device. The concept of regular expressions arose in the 1950s, when the American mathematician Stephen Kleene formalized the description of a regular language, and came into common use with the Unix text processing utilities ed, an editor, and grep, a filter.
Themes and styles also help to keep your document coordinated. When you click Design and choose a new Theme, the pictures, charts and SmartArt graphics change to match your new theme. When you apply styles, your headings change to match the new theme. Save time in Word with new buttons that show up where you need them. To change the way a picture fits in your document, click it and a button for layout options appears next to it. When you work on a table, click where you want to add a row or a column, then click the plus sign. In modern usage, “regular expressions” are often distinguished from the derived, but fundamentally distinct concepts of regex or regexp, which no longer describe a regular language.
To make your document look professionally produced, Word provides header, footer, cover page and text box designs that complement each other. For example, you can add a matching cover page, header and sidebar. Click Insert, then choose the elements you want from the different galleries. Themes and styles also help to keep your document coordinated. When you click Design and choose a new Theme, the pictures, charts and SmartArt graphics change to match your new theme. When you apply styles, your headings change to match the new theme. Today, different syntaxes for writing regular expressions exist, one being the POSIX standard and another, widely used, being the Perl syntax.
```

When highlighted in the terminal the output should look like this:

![Terminal output from zsh with oh-my-zsh highlighting the phrase Regular Expression in red](./assets/grep-upperlower-regex-01.png "Output from grep with the phrase Regular Expression highlighted in red")

or this:

![Terminal output from PowerShell with the phrase Regular Expression highlighted in red](./assets/pwsh-upperlower-regex-01.png "Output from Select-String with the phrase Regular Expression highlighted in red")

### Or Search Example

OK, so we have found all examples of the phrase "regular expression" in the file, regardless of capitalisation. But what if we wanted to find all instances of either "regular expression" or "regex"? Well, we can use the `|` character to indicate an "or" search. Let's try it:

```bash
grep -E '[Rr]egular [Ee]xpression|[Rr]egex' SampleText.txt
```

Or, in PowerShell:

```powershell
Select-String -Pattern '[Rr]egular [Ee]xpression|[Rr]egex' -Path SampleText.txt -AllMatches
```

**Note:** In PowerShell we need to use the `-AllMatches` parameter to get all the matches. Otherwise, only the first match on a given line will be indicated/returned.

Notice that we have added `|[Rr]egex` to the end of the search string. This will match either "regular expression" or "regex". The output should look like this:

The text output will be the same as for previous examples as in our sample text we only ever seen "regex" and "regular expressions" in the same lines. However, the highlighted output should look like this:

![Terminal output from zsh with oh-my-zsh highlighting the phrase Regular Expression or Regex in red](./assets/grep-or-regex.png "Output from grep with the phrase Regular Expression or Regex highlighted in red")

or this:

![Terminal output from PowerShell with the phrase Regular Expression or Regex highlighted in red](./assets/pwsh-or-regex.png "Output from Select-String with the phrase Regular Expression or Regex highlighted in red")

### Optional Characters Example

The eagle-eyed among you may have noticed that for some matches we're actually matching "regular expression" and for others we're matching "regular expressions", but we're not matching the "s" at the end of "expressions". Similarly we're not matching the "p" in "regexp". So, let's look at how we could match the whole word whether it's "expression" or "expressions", or "regex" or "regexp". We can do this using the `?` character to indicate that the previous character is optional. Let's try it:

```bash
grep -E '[Rr]egular [Ee]xpressions?|[Rr]egexp?' SampleText.txt
```

Or, in PowerShell:

```powershell
Select-String -Pattern '[Rr]egular [Ee]xpressions?|[Rr]egexp?' -Path SampleText.txt -AllMatches
```

Again, the text match will be the same as before, but the highlighted output should look like this:

![Terminal output from zsh with oh-my-zsh highlighting the phrase Regular Expression or Expressions or Regex or Regexp in red](./assets/grep-simple-conditional.png "Output from grep with the phrase Regular Expression or Expressions or Regex or Regexp highlighted in red")

or this:

![Terminal output from PowerShell with the phrase Regular Expression or Expressions or Regex or Regexp highlighted in red](./assets/pwsh-simple-conditional.png "Output from Select-String with the phrase Regular Expression or Expressions or Regex or Regexp highlighted in red")

Notice now that the red text in grep, or the highlighted text in PowerShell, includes the "s" in "expressions" and the "p" in "regexp" whilst also matching the "regular expression" and "regex" phrases.

### Stretch Goals For Text File

1. Return only the matches for "regular expression(s)" or "regex(p)" (with or without the extra character) and not all of the text on the line. (Hint: this is a `grep` skill and not a regex skill.)
2. Extend the answer to (1) by now returning the line number of the match as well

## Linux Log File: Linux_2k.log

In the `Linux_2k.log` we have entries from `/var/messages` on a Linux system. There have been a number of authentication failures for users trying to connect to SSH and we want to explore them using regular expressions. We'll look to try and find out which users have been trying to connect and from which IP addresses, and how often to see what information and patterns we can find.

To start with we know from reading the first part of the log that the log lines of interest are in this format:

```bash
Jun 21 08:56:36 combo sshd(pam_unix)[14281]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=217.60.212.66  user=guest
```

Initially we're not interested in when the attempts happened or the service and process information, so we can ignore the first part of the line and focus on the following section:

```bash
authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=217.60.212.66  user=guest
```

So maybe we can start with a simple search for the words "authentication failure" in the file:

```bash
grep -E 'authentication failure' Linux_2k.log
```

Or, in PowerShell:

```powershell
Select-String -Pattern 'authentication failure' -Path Linux_2k.log
```

The output should look like this:

```text
Jul 11 03:46:15 combo sshd(pam_unix)[31854]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=82.77.200.128  user=root
Jul 11 03:46:15 combo sshd(pam_unix)[31853]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=82.77.200.128  user=root
Jul 11 03:46:16 combo sshd(pam_unix)[31862]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=82.77.200.128  user=root
Jul 11 03:46:17 combo sshd(pam_unix)[31855]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=82.77.200.128  user=root
Jul 11 03:46:17 combo sshd(pam_unix)[31852]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=82.77.200.128  user=root
Jul 11 03:46:19 combo sshd(pam_unix)[31860]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=82.77.200.128  user=root
Jul 11 11:33:13 combo gdm(pam_unix)[2803]: authentication failure; logname= uid=0 euid=0 tty=:0 ruser= rhost= 
```

If we look closely though we can see that actually there are some entries here for services other than SSH, so we should adjust our search to be more specific. We can do this by adding the word "sshd" to our search string:

```bash
grep -E 'sshd.+?authentication failure' Linux_2k.log
```

Or, in PowerShell:

```powershell
Select-String -Pattern 'sshd.+?authentication failure' -Path Linux_2k.log
```

The output should look like this:

```text
Jul  1 10:56:43 combo sshd(pam_unix)[22268]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=195.129.24.210  user=root
Jul  1 10:56:43 combo sshd(pam_unix)[22274]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=195.129.24.210  user=root
Jul  1 10:56:44 combo sshd(pam_unix)[22276]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=195.129.24.210  user=root
Jul  1 10:56:44 combo sshd(pam_unix)[22275]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=195.129.24.210  user=root
Jul  2 04:15:33 combo sshd(pam_unix)[24588]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=zummit.com 
Jul  2 04:15:33 combo sshd(pam_unix)[24587]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=zummit.com 
Jul  2 04:15:33 combo sshd(pam_unix)[24590]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=zummit.com 
```

We can now see that some attempts fail without even presenting a username, we're explicitly interested in those that do present a username, so let's add that to our search string:

```bash
grep -E 'sshd.+?authentication failure;.+?user=.+\b' Linux_2k.log
```

Or, in PowerShell:

```powershell
Select-String -Pattern 'sshd.+?authentication failure;.+?user=.+\b' -Path Linux_2k.log
```

The output should look like this:

```text
Jul 21 01:30:49 combo sshd(pam_unix)[3493]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=210.76.59.29  user=root
Jul 21 01:30:50 combo sshd(pam_unix)[3488]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=210.76.59.29  user=root
Jul 21 01:30:50 combo sshd(pam_unix)[3487]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=210.76.59.29  user=root
Jul 21 15:18:30 combo sshd(pam_unix)[5587]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=193.110.106.11  user=root
Jul 21 15:18:30 combo sshd(pam_unix)[5586]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=193.110.106.11  user=root
Jul 23 11:46:41 combo sshd(pam_unix)[15385]: authentication failure; logname= uid=0 euid=0 tty=NODEVssh ruser= rhost=85.44.47.166  user=root
```

Now, let's look at how many times each user has been tried in these failed attempts. As we're focusing on `grep` there are some limitations on what we can do, other tools may be better, but let's stick with grep for now. With `grep` this will mean piping the output of one `grep` command into another `grep` command which we can use with the `-o` flag to only return the matched text to trim down to just the `user=<username>` portion of the line. We will then use some other bash tools such as `cut`, `sort` and `uniq` to count the number of times each user appears. Let's try it:

```bash
grep -E "sshd.+?authentication failure;.+?user=.+\b" Linux_2k.log | grep -oE "\buser=(.+?)\b" | cut -d "=" -f 2 | sort | uniq -c
```

Here we're taking the output from our first `grep` and we're piping it into a second `grep` which is adding the `-o` flag to only return the matched text. Here we're using `grep` to match the word "user" followed by an equals sign and then any number of characters until we hit a word boundary (`\b`). We're then piping that output into `cut` which is using the `-d` flag to specify the delimiter as the equals sign and the `-f` flag to specify that we want the second field. We're then piping that output into `sort` to sort the output and then piping that into `uniq` with the `-c` flag to count the number of times each line appears.

Which would give us the output:

```text
> grep -E "sshd.+?authentication failure;.+?user=.+\b" Linux_2k.log | grep -oE "\buser=(.+?)\b" | cut -d "=" -f 2 | sort | uniq -c

     17 guest
    351 root
      4 test
```

In PowerShell we can be a bit more direct thanks to the ability of PowerShell to quickly access capture groups. Let's try it:

```powershell
Select-String -Pattern 'sshd.+?authentication failure;.+?\buser=(.+)\b' -Path Linux_2k.log | ForEach-Object { $_.Matches.Groups[1].Value } | Group-Object | Sort-Object -Property Count -Descending | Format-Table -Property Count, Name
```

Which would gives us the output:

```text
> Select-String -Pattern 'sshd.+?authentication failure;.+?\buser=(.+)\b' -Path Linux_2k.log | ForEach-Object { $_.Matches.Groups[1].Value } | Group-Object | Sort-Object -Property Count -Descending | Format-Table -Property Count, Name                             

Count Name
----- ----
  351 root
   17 guest
    4 test
```

Here we have used the ForEach-Oject cmdlet to extract the capture group that we want, in this case `[1]` as `[0]` is always the full match. We then pipe that into the Group-Object cmdlet to group the results and then pipe that into the Sort-Object cmdlet to sort the results by the Count property in descending order.

### Stretch Goals For Linux Log File

1. Capture the remote system (`rhost`) as well as the user and see how many times each remote host tried each user.

## Stretch Goal Answers

The answers supplied here are one way of achieving the result, you may find other ways of achieving the same result.

While PowerShell is very powerful, it can be tricky for a new learner to get to grips with manipulating the objects returned from each cmdlet. You'll notice that the PowerShell examples may look more complicated. Once you're familiar with PowerShell you'll find that it's often easier to achieve the same result in PowerShell than it is in bash, but the learning curve is steeper to get there.

### Text File

1. Return only the matches for "regular expression(s)" or "regex(p)" (with or without the extra character) and not all of the text on the line. (Hint: this is a `grep` skill and not a regex skill.)
   - Linux: `grep -oE "[Rr]egular [Ee]xpressions?|[Rr]egexp?" SampleText.txt`
   - PowerShell: `Select-String -Pattern "[Rr]egular [Ee]xpressions?|[Rr]egexp?" -Path SampleText.txt -AllMatches | Select-Object -ExpandProperty Matches | Select-Object Value`
2. Extend the answer to (1) by now returning the line number of the match as well
   - Linux: `grep -noE "[Rr]egular [Ee]xpressions?|[Rr]egexp?" SampleText.txt`
   - PowerShell: `Select-String -Pattern "[Rr]egular [Ee]xpressions?|[Rr]egexp?" -Path SampleText.txt -AllMatches | ForEach-Object { $lineNumber = $_.LineNumber; $_.Matches | ForEach-Object { [PSCustomObject]@{ LineNumber = $lineNumber; Value = $_.Value } } }`

### Linux Log File

1. Capture the remote system (`rhost`) as well as the user and see how many times each remote host tried each user.
   - Linux: `grep -E "sshd.+?authentication failure;.+?user=.+\b" Linux_2k.log | grep -oE "\brhost=(.+?)\s\buser=(.+?)\b" | sort | uniq -c | sort `
   - PowerShell: `Select-String -Pattern 'sshd.+?authentication failure;.+?\brhost=(.+?)\s+?\buser=(.+)\b' -Path Linux_2k.log | ForEach-Object {[string]::Join(" ",($_.Matches.Groups[1,2].Value)) } | Group-Object | Sort-Object -Property Name -Descending | Format-Table -Property Count,Name`
