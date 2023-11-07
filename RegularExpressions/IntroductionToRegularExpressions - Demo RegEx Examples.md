# Demonstration Examples for Regular Expressions Brown Bag Session

## Table of Contents

- [Demonstration Examples for Regular Expressions Brown Bag Session](#demonstration-examples-for-regular-expressions-brown-bag-session)
  - [Table of Contents](#table-of-contents)
  - [Notepad++](#notepad)
    - [Lorem](#lorem)
    - [Log Sample](#log-sample)
  - [Linux - Fedora](#linux---fedora)
    - [Notes](#notes)
    - [Lorem Ipsum Sample](#lorem-ipsum-sample)
      - [Examples](#examples)
      - [Recommendations for further exploration](#recommendations-for-further-exploration)
    - [Log Sample](#log-sample-1)
      - [Examples](#examples-1)
      - [Recommendations for further exploration](#recommendations-for-further-exploration-1)
  - [PowerShell - Windows](#powershell---windows)
    - [Notes](#notes-1)
    - [Lorem Ipsum Sample](#lorem-ipsum-sample-1)
      - [Examples](#examples-2)
      - [Recommendations for further exploration](#recommendations-for-further-exploration-2)
    - [Log Sample](#log-sample-2)
      - [Recommendations for further exploration](#recommendations-for-further-exploration-3)
  - [CloudWatch](#cloudwatch)
    - [Notes](#notes-2)
    - [Log Group](#log-group)
    - [Logs Insights](#logs-insights)
    - [Metrics Filters](#metrics-filters)
  - [Closing Notes](#closing-notes)


## Notepad++

Open:

- IntroductionToRegularExpressions - Lorem Ipsum.txt
- IntroductionToRegularExpressions - LogSample.log
- **Find All in Current Document**

### Lorem

- Search for all instances of Lorem or lorem in the document
  - Upper case or lower case 'L' in Lorem
  - `[lL]orem`
- Search for 'Version' followed by a space and a number
  - `Version\s[0-9]`
  - e.g. Version 1 or Version 2
- Same again but using \d notation
  - `Version\s\d`

### Log Sample

- Look for all instances of 4xx or 5xx HTTP status codes
  - 4xx of 5xx with a space before and after
  - `\s(4[0-9]{2}|5[0-9]{2})\s`

## Linux - Fedora

- `cd /home/gwatts/RegExDemo` - path may vary depending on your setup
- `ls -alh` - show all files in the directory

### Notes

- These examples were made and tested on Fedora Linux; however, they should work on any Linux distribution.
- The examples were put together on Windows Subsystem for Linux (WSL) and used symbolic links to the files in the Windows file system.  This is why the paths are a little different to the Windows examples.
  - Replace the paths with the correct paths for your system or use the `ln -s` command to create symbolic links to the files in the Windows file system
- Examples use the `bat` command to display the contents of the file. This is a replacement for `cat` which adds syntax highlighting and line numbers.  If you don't have `bat` installed you can use `cat` instead, or install `bat` using `sudo dnf install bat`

### Lorem Ipsum Sample

#### Examples

- Show contents of the file using the `bat` command
  - `bat RegEx-LoremIpsum.txt`
- Use `grep` to search for all instances of Lorem or lorem in the document
  - `grep -nE "Version\s[0-9]{1,}" RegEx-LoremIpsum.txt`

  ``` shell
  # Example result

  5:Finally, as we conclude our discussion on Version 1, it's important to note the impact it had on subsequent developments. Curabitur arcu erat, accumsan id imperdiet et, porttitor at sem. Vivamus magna justo, lacinia eget consectetur sed, convallis at tellus. Donec sollicitudin molestie malesuada. Pellentesque in ipsum id orci porta dapibus.
  7:Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed in gravida purus, eget pulvinar sem. In in nisl non nulla placerat pharetra at et libero. Proin eget nisi sit amet lorem tincidunt vehicula non id ante. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Version 2 of the project was a great success, leading to much anticipation for the next iteration.
  ```
- Demonstrate that `grep` doesn't always match with the `\d` notation
  - `grep -E "Version\s\d" RegEx-LoremIpsum.txt`

  ``` shell
  # Example result

  grep: warning: stray \ before d
  ```
- Demonstrate that `grep`  match with the `\d` notation when using `-P`
  - `grep -P "Version\s\d" RegEx-LoremIpsum.txt`
 
  ``` shell
  # Example result

  5:Finally, as we conclude our discussion on Version 1, it's important to note the impact it had on subsequent developments. Curabitur arcu erat, accumsan id imperdiet et, porttitor at sem. Vivamus magna justo, lacinia eget consectetur sed, convallis at tellus. Donec sollicitudin molestie malesuada. Pellentesque in ipsum id orci porta dapibus.
  7:Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed in gravida purus, eget pulvinar sem. In in nisl non nulla placerat pharetra at et libero. Proin eget nisi sit amet lorem tincidunt vehicula non id ante. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Version 2 of the project was a great success, leading to much anticipation for the next iteration.
  ```

#### Recommendations for further exploration

1. Find examples of Version 1 or Version 2 in the document **only** when the next character is a `.` (full stop)

### Log Sample

#### Examples

- Show contents of the file using the `bat` command
  - `bat RegEx-LogSample.log`
- Use `grep` to search for all instances of 4xx or 5xx HTTP status codes
  - `grep -nE "\s(4[0-9]{2}|5[0-9]{2})\s" RegEx-LogSample.log`

  ``` shell
  # Example result

  11:127.0.0.1 - - [12/Oct/2023:10:25:30 +0000] "GET /blog/post1.html HTTP/1.1" 404 0
  13:127.0.0.1 - - [12/Oct/2023:10:30:10 +0000] "GET /images/missing.jpg HTTP/1.1" 404 0
  15:127.0.0.1 - - [12/Oct/2023:10:35:00 +0000] "POST /login HTTP/1.1" 500 0
  16:127.0.0.1 - - [12/Oct/2023:10:37:25 +0000] "GET /downloads/file.zip HTTP/1.1" 404 0
  22:127.0.0.1 - - [12/Oct/2023:10:53:15 +0000] "GET /videos/video2.mp4 HTTP/1.1" 404 0
  26:127.0.0.1 - - [12/Oct/2023:11:02:35 +0000] "GET /videos/video4.mp4 HTTP/1.1" 404 0
  30:127.0.0.1 - - [12/Oct/2023:11:13:05 +0000] "GET /images/missing.jpg HTTP/1.1" 404 0
  ```

#### Recommendations for further exploration

1. Find only the video files that result in a 404 error
2. Find all the requests for blog posts regardless of the HTTP status code

## PowerShell - Windows

- `Set-Location -Path "C:\Users\WattsG\OneDrive - Version 1\Documents\Presentations"` - path may vary depending on your setup
- `Get-ChildItem -Exclude *.pptx` - show all files in the directory excluding PowerPoint files

### Notes

- PowerShell makes working with Regular Expressions, particularly in demos like this, easy by allowing visible access to the inner workings of the objects it returns

### Lorem Ipsum Sample

#### Examples

- Basic search using Select-String for all instances of Lorem or lorem in the document
  - Does include extra prefix text about the file and line match that may not be desired
    - e.g. `IntroductionToRegularExpressions - Lorem Ipsum.txt:1:`
  - `Select-String -Pattern "[lL]orem" -Path '.\IntroductionToRegularExpressions - Lorem Ipsum.txt'`

  ``` powershell
  # Example result

  IntroductionToRegularExpressions - Lorem Ipsum.txt:1:Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed in gravida purus, eget pulvinar sem. In in nisl non nulla placerat pharetra at et libero.  Proin eget nisi sit amet lorem tincidunt vehicula non id ante. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Version 1 of the project was a great success,  leading to much anticipation for the next iteration.
  IntroductionToRegularExpressions - Lorem Ipsum.txt:7:Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed in gravida purus, eget pulvinar sem. In in nisl non nulla placerat pharetra at et libero.  Proin eget nisi sit amet lorem tincidunt vehicula non id ante. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Version 2 of the project was a great success,  leading to much anticipation for the next iteration.
  ```

- Use Select-String to search for all instances Version followed by a space and a number
  - Expanding the `Line` property of the `Select-String` object returns just the line that matched and none of the extra prefix text
  - `Select-String -Pattern "Version\s[0-9]{1,}" -Path '.\IntroductionToRegularExpressions - Lorem Ipsum.txt' | Select-Object -ExpandProperty Line`
  
  ``` powershell
  # Example result

  Finally, as we conclude our discussion on Version 1, it's important to note the impact it had on subsequent developments. Curabitur arcu erat, accumsan id imperdiet et, porttitor at sem. Vivamus magna justo, lacinia eget consectetur sed, convallis at tellus. Donec sollicitudin molestie malesuada. Pellentesque in ipsum id orci porta dapibus.
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed in gravida purus, eget pulvinar sem. In in nisl non nulla placerat pharetra at et libero. Proin eget nisi sit amet lorem tincidunt vehicula non id ante. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Version 2 of the project was a great success, leading to much anticipation for the next iteration.
  ```
- Explore the `Matches` property of the `Select-String` object
  - Will return the `Matches` property of the `Select-String` object
    - All of the information about the match is returned such as Value, length, index, etc.
  - `Select-String -Pattern "Version\s[0-9]{1,}" -Path '.\IntroductionToRegularExpressions - Lorem Ipsum.txt' | Select-Object -ExpandProperty Matches`

  ``` powershell
  # Example result

  Groups    : {0}
  Success   : True
  Name      : 0
  Captures  : {0}
  Index     : 42
  Length    : 9
  Value     : Version 1
  ValueSpan :

  Groups    : {0}
  Success   : True
  Name      : 0
  Captures  : {0}
  Index     : 308
  Length    : 9
  Value     : Version 2
  ValueSpan :
  ```

- Find just the value of the `Matches` property by expanding the `Matches` property and then selecting the `Value` property
  - `Select-String -Pattern "Version\s[0-9]{1,}" -Path '.\IntroductionToRegularExpressions - Lorem Ipsum.txt' | Select-Object -ExpandProperty Matches | Select-Object Value`

  ``` powershell
  # Example result

  Value
  -----
  Version 1
  Version 1
  Version 1
  Version 2
  Version 2
  Version 2
  ```

#### Recommendations for further exploration

1. Find all the instances of Version 1 or Version 2 in the document **only** when the next character is a `,` (comma)
2. Print just the match values for the previous example

### Log Sample

- Let's explore that really complicated looking example from the beginning
- **Example RegEx: `\b((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\b`**
- PowerShell Example `Select-String -Pattern "\b((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\b" -Path '.\IntroductionToRegularExpressions - LogSample.log' | Select-Object -ExpandProperty Matches | Select-Object Value`
  - What is `\b((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\b` looking for?
    - IP address
    - `\b` - word boundary
    - `((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}` - 3 octets of an IP address followed by a `.`
      - `25[0-5]` - 250-255
      - `2[0-4][0-9]` - 200-249
      - `[01]?[0-9][0-9]?` - 0-199
      - `\.` - literal `.`
      - `{3}` - 3 times
        - 192.168.1. or 10.0.0.
    - `(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)` - final octet of an IP address
      - `25[0-5]` - 250-255
      - `2[0-4][0-9]` - 200-249
      - `[01]?[0-9][0-9]?` - 0-199
    - `\b` - word boundary
  - What is the `Select-Object -ExpandProperty Matches | Select-Object Value` doing?
    - `Select-Object -ExpandProperty Matches` - returns the `Matches` property of the `Select-String` object
    - `Select-Object Value` - returns the `Value` property of the `Matches` property of the `Select-String` object
    - This is a way of getting just the IP addresses from the log file

  ``` powershell
  # Example result

  Value
  -----
  127.0.0.1
  127.0.0.1
  127.0.0.1
  ```

- **REALLY** complicated example using named capture groups
  - PowerShell and Python are some of the languages that support naming capture groups
    - This can help when back referencing capture groups or when using the `Select-Object -ExpandProperty Matches` method to reform the string
  - Maybe just showing off but it's a good example of what can be done
  - `Select-String -Pattern "(?<IP>^\b((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\b)(\s{1,}-.+-\s{1,})(\[(?<DAY>\d{1,2})\/(?<MONTH>\w+)\/(?<YEAR>\d{4})\:(?<TIME>\d{2}\:\d{2}\:\d{2}).+\]).+?(?<ACTION>[a-zA-Z]{1,})\s(?<OBJECT>.+?)\s.+(?<ERROR>4[0-9]{2}|5[0-9]{2})\s\d+$" -Path '.\IntroductionToRegularExpressions - LogSample.log' | Select-Object -ExpandProperty Matches | ForEach-Object {[string]::Join(" ",($_.Groups["YEAR","MONTH","DAY","TIME","IP","ACTION","OBJECT","ERROR"]))}`

  ``` powershell
  # Example result

  2023 Oct 12 10:25:30 127.0.0.1 GET /blog/post1.html 404
  2023 Oct 12 10:30:10 127.0.0.1 GET /images/missing.jpg 404
  2023 Oct 12 10:35:00 127.0.0.1 POST /login 500
  ```
- Let's break this complicated example down
  - We already know `( )` denotes a capture group
  - Starting a capture group with `?<word>` names the capture group
    - e.g. `(?<IP>^\b((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\b)` - named capture group for IP address
  - We've already seen how to capture an IP address and the above example shows naming it, so we understand the first group in the example
  - The next group is not named and is `\s{1,}-.+-\s{1,}`
    - `\s{1,}` - 1 or more spaces
    - `-.+-` - `-` followed by 1 or more of any character followed by `-`
    - `\s{1,}` - 1 or more spaces
    - In our example log we have each data element separated by `-` and surrounded by spaces
    - Also in our short example the next element after the IP address is empty, so this capture group will work, it may not do in the real world
    - We're not assigning a name as we don't intend to use it, however it could still be referencesd by it's index number, e.g. `$_.Groups[2]`
    - Alternatively it does not need to be a capture group at all as we're not using it, it's left in for demonstration purposes
  - The next group is named `DAY` and is captured with the group `(?<DAY>\d{1,2})`
    - `\d{1,2}` - 1 or 2 digits
    - This is the day of the month
  - Then we have a forward slash `/`
    - This is a literal forward slash and is delimited by the `\` characters, e.g. `\/`
  - Next we have a group to capture the month named `MONTH` and is captured with the group `(?<MONTH>\w+)`
    - `\w+` - 1 or more word characters
    - This is the month of the year
  - Again we have a forward slash `/` separating the month and year this time
  - The next group is named `YEAR` and is captured with the group `(?<YEAR>\d{4})`
    - `\d{4}` - 4 digits
    - This is the year
  - After the year there is some data we don't care about, so we use `.+` to match 1 or more of any character but we don't want to be **greedy** so we use `.+?` to match 1 or more of any character but as few as possible up until the next match group
    - This is details such as the time, time zone offset, etc.
  - Our next group of interest is named `ACTION` and is captured with the group `(?<ACTION>[a-zA-Z]{1,})`
    - `[a-zA-Z]{1,}` - 1 or more upper or lower case letters
    - This is the HTTP action, e.g. GET, POST, PUT, etc.
  - After a space (`\s`) the next group is named `OBJECT` and is captured with the group `(?<OBJECT>.+?)`
    - `.+?` - 1 or more of any character but as few as possible until we reach the next space (`\s`)
    - This is the object being requested, e.g. `/blog/post1.html`
  - We're then looking explicitly for 400 or 500 HTTP status codes with the group named `ERROR` and matched with `(?<ERROR>4[0-9]{2}|5[0-9]{2})`
    - `4[0-9]{2}` - 400-499
    - `5[0-9]{2}` - 500-599
  - Finally we have a space followed by 1 or more digits `\s\d+` which we don't care about
    - This is the size of the object being requested, e.g. `404 0` or `200 12345`
  - After the path to the file in question we then expand the `Matches` property of the `Select-String` object as we've seen above
  - Finally we use `ForEach-Object` to join the capture groups together with a space between them
    - `[string]::Join(" ",($_.Groups["YEAR","MONTH","DAY","TIME","IP","ACTION","OBJECT","ERROR"]))`
    - This gives us a way of formatting the output of the `Select-String` object to be more readable
      - We could alternatively join with a comma, tab, etc. or even a new line and/or output the data to a file such as a CSV file

#### Recommendations for further exploration

1. Experiment with aspects of the RegEx to see what happens
   1. Can you remove one of the capture groups and still get a result?
   2. Can you also capture the time of the event?

## CloudWatch

### Notes

- This example is based on having access to the UKDDC AWS Sandbox account
  - If you need access see <https://version1.sharepoint.com/sites/UKDCSPlatformTeam/SitePages/AWS-Sandbox.aspx>
- The use of the sandbox is variable and so you may need to change to a different log group and or modify the RegEx to match the log entries in the log group you choose and/or otherwise experiment
- The details below work as of October 2023

### Log Group

- Group(s): rds-custom-instance-agent-log
- Time/Date range: last 3 months

### Logs Insights

- Find all the commands executed which didn't result in an exit code of 0 (successful)

  ```
  fields @timestamp, @message, @logStream, @log
  | filter @message like /executing command .*? exit code \[[1-9]{1,}\]/
  | sort @timestamp desc
  | limit 20
  ```

### Metrics Filters

- `%exit\scode\s%`

## Closing Notes

The following websites can be really useful when learning and experimenting with Regular Expressions.  Caution should be used if testing with customer data, **do not export customer data to a public website**.

- <https://www.regex101.com/>
- <http://www.rubular.com/>
- <https://www.regular-expressions.info/tutorial.html>

Also consider using a secured, and approved AI tool such as [Bing Enterprise](https://www.bing.com/search?q=Bing+AI&showconv=1&FORM=hpcodx) to help develop and test Regular Expressions.
