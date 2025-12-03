---
tags:
  - todo
---
## `cat` (concatenate)
Simple overview:
```BASH
cat file.txt # view a single file
cat file1.txt file2.txt # view vultiple files sequentially
cat > newfile.txt # creating and interactive input to a new file
cat >> existing.txt # appending in intreactive mode to a existing file
cat 1.txt 2.txt 3.txt > full.txt # merge multiple files into one
cat additional.txt >> main.txt # append file to another file
```
Useful flags:

| Flag | Description                                 |
| ---- | ------------------------------------------- |
| `-n` | Show line numbers *(including empty lines)* |
| `-b` | Show line numbers *(only non-empty lines)*  |
| `-s` | Squeeze multiple blank lines                |
| `-E` | Show `$` at end of each line                |
| `-T` | Show tabs as `^I`                           |
| `-v` | Show non-printing characters                |
| `-A` | Equivalent to `-vET`                        |
When NOT to use:
- **For binary files** - use `file`, `hexdump`, or specific tools
- **For huge files** - use `less`, `head`, `tail` instead
- **When you just need file info** - use `wc`, `file`, `stat`
- **For editing** - use `nano`, `vim`, `sed -i`

## `tac` (literally `cat` backwards)
Display and concatenate files with reversed lines.
The basic usage the same as `cat`, but different options.
The flags give us advanced usage with *separators*.

| Flag | Description                                          | Example                    |
| ---- | ---------------------------------------------------- | -------------------------- |
| `-s` | Reverse only blocks by the specified separator       | `tac -s "---" file.txt`    |
| `-b` | When the separator is BEFORE the block               | `tac -b -s "---" file.txt` |
| `-r` | When the separator is [[Regular Expressions\|RegEx]] | `tac -r -s "^d" file.txt`  |

## `grep` (Global Regular Expression Print)
It searches for patterns in files and prints lines that match those patterns.
Simple overview:
```BASH
grep "word" text.txt # print all the lines that contain word
grep -i "wOrD" text.txt # case-insensitive search
grep "error" *.log # search in multiple files
grep -r "pattern" folder/ # search in all files recursively
```
Useful flags:

| Flag      | Description                         | Example                              |
| --------- | ----------------------------------- | ------------------------------------ |
| `-i`      | ignore case                         | `grep -i "error" file.txt`           |
| `-v`      | invert match (lines NOT containing) | `grep -v "debug" file.txt`           |
| `-n`      | show line numbers                   | `grep -n "pattern" file.txt`         |
| `-c`      | count matches                       | `grep -c "pattern" file.txt`         |
| `-l`      | show only filenames                 | `grep -l "pattern" *.txt`            |
| `-L`      | show files WITHOUT pattern          | `grep -L "pattern" *.txt`            |
| `-r`/`-R` | recursive search                    | `grep -r "function" .`               |
| `-w`      | whole word only                     | `grep -w "the" file.txt`             |
| `-A N`    | show N lines after match            | `grep -A 2 "error" log.txt`          |
| `-B N`    | show N lines before match           | `grep -B 2 "error" log.txt`          |
| `-C N`    | `-AB N`                             | `grep -C 2 "error" log.txt`          |
| `-e`      | multiple patterns                   | `grep -e "1" -e "2" file.txt`        |
| `-E`      | ERE, use `egrep` command            | `grep -E "(error\|warning)" log.txt` |
| `-P`      | PCRE                                | `grep -P "(?<!foo)bar" file.txt`     |
| `-F`      | fixed strings (no regex at all)     | `grep -F "a.b" log.txt`              |
Important notice: by default grep use BRE, about this [[Regular Expressions|HERE]].

## `egrep` (Extended grep)
`egrep` = `grep -E`

## Pipes `|`
Pipes let you combine the commands, allow you to use the output of the previous command like an input for the next.
Example:
```BASH
ls -l | egrep "\S+.txt"
```

## `awk` (a.k.a. python+sql)
It's not just a command - this is a complete programming language designed for text processing. Named after its creators: Aho, Weinberger and Kernighan.

Basic syntax:
```BASH
awk 'pattern { action }' filename # basic call
awk -F separator 'pattern { action }' filename # using specifiec separator
awk -f script.awk filename # execute the programm from another file
```

`awk` automatically splits input into:
- **Records**: usually lines *(default: newline separated)*
- **Fields**: columns with each record *(default: whitespace separated)*

Program structure:
```BASH
BEGIN {
	# Executes ONCE before processing any input
	# Initialize variables, print headers
}
pattern {
	# Executes for EACH line matching pattern
	# Main processing logic
}
END {
	# Executes ONCE after all input is processed
	# Print summaries, totals, etc.
}
```

Buit-in variables:

| Variable         | Value                                     |
| ---------------- | ----------------------------------------- |
| `$0`             | entire line                               |
| `$1, $2, .., $N` | first field, second field, ..., N field   |
| `NR`             | Number of Records (line number)           |
| `NF`             | Number of Fields in current record        |
| `FS`             | Field Separator (input)                   |
| `OFS`            | Output Field Separator                    |
| `RS`             | Record Separator (input)                  |
| `ORS`            | Output Record Separator                   |
| `FILENAME`       | current filename                          |
| `FNR`            | File Record Number (resets for each file) |
Patterns:
```BASH
# Line Number Patterns
awk 'NR >= 2 && NR <= 3 {print $0}' data.txt # process lines 2-4

# Field Value Patterns
awk '$4 == 4123 || $4 == 1000 {print $0}' data.txt

# String matching (RegEx)
awk '/regex/ {print $0}' data.txt # lines containing regex
awk '$1 ~ /regex/ {print $0}' data.txt # first field == regex

# Range Patterns
awk '/regex1/,/regex2/ {print $0}' data.txt # process from line matching regex1 to line matching regex2
```

Built-in functions:

| Function                       | Description                                                                            |
| ------------------------------ | -------------------------------------------------------------------------------------- |
| `print "a", variable`          | `python << print("a", variable)`                                                       |
| `length(var)`                  | length of a string                                                                     |
| `substr(a, b, c)`              | making a new string from string `a`, from character number `b` to character number `c` |
| `index(string, text)`          | position of text in a string                                                           |
| `split(string, arr, " ")`      | split string by spaces in an array called `arr`                                        |
| `gsub(/regex/, "text")`        | global substitution, replacing matches to the text                                     |
| `match(var, /regex/)`          | match and extract using regex                                                          |
| `sqrt(num)`                    | square root from num                                                                   |
| `int(num)`                     | remove float (rounding)                                                                |
| `rand()`                       | random value between 0 and 1                                                           |
| `printf("%.2f\n", value)`      | formatted print of value                                                               |
| `systime()`                    | timestamp of now                                                                       |
| `strftime("%d.%m", timestamp)` | [[Timestamp Formatting]]                                                               |

Arrays:
Arrays in `awk` are not like traditional arrays, they are like associative arrays (dictionaries, hash maps).
Quick reference:
```BASH
if ("key" in arr) # check existance
delete arr["key"] # delete element
for (key in arr) {print key, arr[key]} # iteration (UNORDERED!)
length(arr) # array size
arr["row", "col"] = "value" # multi-dimensional array (actually arr["row\034col"])
arr[key]++ # initialize and incrementing array from zero
```

If-Else:
```bash
awk '{
	if ($2 > 30)
		category = 1
	else if ($2 > 25)
		category = 2
	else
		category = 3
	print $1, "is", category
}' data.txt
```

Loops:
```bash
awk '{
	for (i=1; i<=NF; i++) {}
		print "Field", i, ": ", $i
	}
	i = 1
	while (i <= NF) {
		print $i
		i++
	}
	i = 1	
	do {
		print $i
		i++
	} while (i <= NF)
}'
```

## `sed` (Stream EDitor)
It's a non-interactive text editor that transforms text via scripts or commands.
Basic syntax:
```BASH
sed [OPTIONS] 'COMMAND' file
sed [OPTIONS] -f script.sed file
sed [OPTIONS] -e 'COMMAND1' -e 'COMMAND2' file
```
Overview:
```BASH
# SUBSTITUTION
sed 's/old/new/' file # subtitute first occurrence
sed 's/old/new/g' file # substitute all occurrences
sed 's/old/new/3' file # substitute 3rd occurence
sed 's/old/new/ig' file # case-insensitive

# DELETION
sed '3d' file # delete line 3
sed '/pattern/d' file # delete matching lines
sed '/start/,/end/d' file # delete range

# PRINTING
sed -n '3p' file # print line 3
sed -n '/pattern/p' file # print matching lines
sed -n '2,4p' file # print range

# INSERT / APPEND / CHANGE
sed '3i\text' file # insert before line 3
sed '3a\text' file # append after line 3
sed '3c\new text' file # replace line 3

# IN-PLACE EDIT (CHANGES THE ORIGINAL FILE)
sed -i 's/old/new/g' file # GNU without backup
sed -i.bak 's/old/new/g' file # GNU with backup
sed -i '' 's/old/new/g' file # BSD/macOS without backup
sed -i '.bak' 's/old/new/g' file # BSD/macOS with backup

# MULTIPLE FILES
sed -i 's/old/new/g' *.text

# REGEX
sed 's/BRE/new/g' file # you can use BRE
sed -r 's/ERE/new/g' file # ERE mode
```
Scripting:
```BASH
cat > script.sed
/condition/ {
	s/old/new/
	s/another/other/
	p # to print
}
# usage
sed -n -f script.sed file.txt
```

## `cut` (slicer)
Slice the character or field from data.
Overview:
```BASH
# by characters
cut -c 3 file # 3rd character
cut -c 1-5 file # characters 1-5
cut -c 1,3,5 file # characters 1,3,5
cut -c 3- file # from 3rd character to the end
cut -c -5 file # from 1st charater to the 5th

# by fields (TAB-delimited by default)
cut -f 2 file # 2nd field
cut -f 1,3 file # fields 1,3
cut -f 2- file # from 2nd field to the last
cut -f -3 file # from start to the 3rd field
cut -d ',' -f 2 # custom delimiter

# by bytes (similar to -c for ASCII)
cut -b 1-10 file

# complement (exclude)
cut --complement -c 3 file # everything except 3rd character
cut --complenent -f 2 file # everything except 2nd field

# output delimiter (GNU only)
cut -d ',' -f 1,3 --output-delimiter='/' file
```

## `sort` (literally data sorting)
Overview:
```BASH
# sorting types
sort file # alphabetical
sort -r file # reverse
sort -n file # numerical
sort -h file # human-redeable (K,B,M,G)
sort -V file # versions
sort -M file # month names
sort -d file # dictionary order (ignore non-alphanumeric)
sort -f file # case-insensitive mode

# columns sorting
sort -k2 file # by 2nd field (whitespace default delimiter)
sort -t ',' -k3 # by 3d field, comma-separated
# -k syntax: -kSTART[.COLUMN][,END.[COLUMN]]
sort -k2 -k3 file # sort by 2nd column, then by 3rd
sort -k2,3 file # sort by 2 column through 3 column
sort -k2,2n file # by 2nd field, numerical
sort -k2.3,2.5 file # by chars 3-5 of 2nd field

# duplicates
sort -u file # remove duplicates

# output control
sort -o output input # output to file
sort -c file && echo "Sorted" || echo "Not sorted"# check if sorted
sort -m file1 file2 # merge sorted files

# performance
sort --parallel=4 file # use 4 CPU cores
sort -S 2G file # use 2GB buffer
sort -T /tmp file # use temp directory

# special
sort -R file # random order
sort -s file # stable sort (preserves equal items from rearranging)
sort -b file # ignore leading blanks
```

## `uniq` (delete duplicates)
The main rule - data MUST be sorted before using.
```BASH
sort file | uniq # basic usage - delete duplicates
sort file | uniq -c # count occurrences
sort file | uniq -d # show only duplicates
sort file | uniq -D # show ALL duplicates
sort file | uniq -u # show only unique lines
sort file | uniq -i # case-insensitive
sort file | uniq -f 2 # skip 2 fields before comparing (use only whitespace as separator)
sort file | uniq -s 3 # skip 3 characters before comparing
sort file | uniq -w 9 # compare only first 9 characters
# specify output file
uniq input output
sort input | uniq > output
```

## `head` (first N lines)
Overview:

| Command               | Description                              |
| --------------------- | ---------------------------------------- |
| `head file`           | first 10 lines                           |
| `head -n 20 file`     | first 20 lines                           |
| `head -20 file`       | first 20 lines (short form)              |
| `head -c 100 file`    | first 100 bytes                          |
| `head -c 1M file`     | first 1 megabyte (human-readable format) |
| `head -n -5 file`     | all except last 5 lines                  |
| `head -q file1 file2` | quiet (no headers, just the content)     |
| `head -v file`        | verbose (show filenames)                 |

## `tail` (last N lines)
Overview:

| Command                   | Description                                      |
| ------------------------- | ------------------------------------------------ |
| `tail file`               | last 10 lines                                    |
| `tail -n 20 file`         | last 20 lines                                    |
| `tail -20 file`           | last 20 lines (short form)                       |
| `tail -c 100 file`        | last 100 bytes (human-readable format)           |
| `tail -n +5 file`         | from 5 line to the end                           |
| `tail -f log`             | follow (live updates, if file is deleted - stop) |
| `tail -F log`             | the same, but continue after recreation of file  |
| `tail -f -s 2 log`        | follow, check every 2 seconds                    |
| `tail --pid=1234 -f file` | stops when PID 1234 dies                         |
| `tail -q file1 file2`     | quiet mode (no headers)                          |
| `tail -v file`            | verbose (show filename)                          |
## `shuf` (shuffle)
`shuf` = `sort -R`

## `tr` (translate)
An utility for translating, deleting or squeezing **characters**.
*Important! It works only with stdin/stdout, no file arguments.*
Overview:
```BASH
# basic translation
echo "rat" | tr 'a' 'o' # rat > rot
echo "hello" | tr 'hlo' 'HLO' # hello > HeLLO
echo "car" | tr 'a-z' 'A-Z' # car > CAR
tr '[:lower:]' '[:upper:]' # same with char classes

# delete characters
echo "car" | tr -d 'a' # car > cr
echo "wc26" | tr -d '[:digit:]' # wc26 > wc
tr -d '\n' # delete newlines

# squeeze repeats
echo "a    b" | tr -s ' ' # a    b > a b
echo "aaaaa" | tr -s 'a' # aaaaa > a
tr -s '[:space:]' # squeeze all whitespace

# complement operation (apply on everything EXCEPT)
echo "ab%%12" | tr -cd '[:alnum:]' # ab%%12 > ab12
echo "ab%%12" | tr -c '[:alnum:]' 'X' # ab%%12 > abXX12

# combined operations
tr -ds 'aeiou' ' ' # delete vowels, squeeze spaces

# ranges and sets
tr '0-9' '9876543210' # digit mapping
tr 'A-Za-z' 'N-ZA-Mn-za-m' # rot13 
 ```
Used character classes are listed [[Regular Expressions#BRE character classes|here]].

## `wc` (word count)
Overview:
```BASH
# basic counts
wc file # output: lines, words, bytes, filename
wc -l file # lines only
wc -w file # words only
wc -c file # bytes only
wc -m file # characters only
wc -L file # max line length

# multiple files
wc *.txt # for each + total
wc -l file1 file2 # for each + total
```

## `nl` (number lines)
Overview:
```BASH
# basic numering
nl file # number non-blank lines
nl -b a file # number ALL lines
nl -b n file # no numbering

# formatting
nl -n ln file # left justify (default)
nl -n rn file # right justify
nl -n lz file # left justify with leading zeros
nl -w 6 file # 6-char wide numbers
nl -s " --> " file # custom separator between number and data

# starting/increment
nl -v 100 file # start at 100
nl -i 5 file # increment by 5

# RegEx selection
nl -b p^[A-Z] file # number lines starting with caps
nl -b p\.$ file # number lines ending with period

# section (numbering from start for each section)
nl -d '---' file # reset at delimiter
```

## `rg` (recursive grep)
*Not installed by default.*
Smart, 5-50x faster and recursive by default.
It ignores `.gitignore` files and directories, use case-insensitive mode if your pattern is lowercase etc.
Overview:
```BASH
# basic search
rg pattern # recursive search
rg -i pattern # case-insensitive
rg -w pattern # whole word
rg -n pattern # show line numbers (default)

# file handling
rg -t py pattern # only python files
rg -T txt pattern # exclude .txt files
rg -g "*.js" pattern # global pattern
rg --hidden pattern # search hidden files

# output control
rg -l pattern # only filenames
rg -c pattern # count per file
rg -C 3 pattern # 3 lines context
rg -A 2 pattern # 3 lines after
rg -B 1 pattern # 1 line before

# advanced
rg -P pattern # PCRE2 regex
rg -U pattern # multiline search
rg -o pattern # only matching part
rg -r replacement pattern # replace in output

# performance
rg --threads 4 pattern # use 4 CPU cores
rg --nmap pattern # memory map for large files

# special
rg --json pattern # json output
rg --type-list # show file types
rg --files # list all searchable files
```

## `tee` (T-shaped pipe splitter)
stdout < stdin > file
Overview:
```BASH
# basic usage
command | tee file # save and display
command | tee -a file # append and display
command | tee file1 file2 # multiple files

# advanced usage
command | sudo tee /etc/file # write as root
command | tee >(grep A) >(grep B) # multiple processes
command 2>&1 | tee log.txt # capture stderr too
cmd1 | tee step1.txt | cmd2 | tee step2.txt # debugging
```

## `find` (file search)
Search recursively.

Find by name:
```BASH
find . -name "*.txt" # exact case
find folder -iname "*.TXT" # case-insensitive
find . -name "file[0-9].txt" # file1.txt, file2.txt, ...
```

Find by type:
```BASH
find . -type f # regular file
find . -type d # directories
find . -type l # symbolic links
find . -type b # block devices
find . -type c # character devices
find . -type p # named pipes (FIFOs)
find . -type s # sockets
```

Combine conditions:
```BASH
find . -name "*.py" -type f # AND (default), python files only
find . -name "*.py" -o -name "*.sh" # OR operator
find . ! -name "*.tmp" # NOT (everything except)
```

Time-based search:
```BASH
# modification time
find . -mtime 0 # today
find . -mtime 1 # yesterday
find . -mtime -1 # less than 1 day ago (last 24h)
find . -mtime +1 # more than 1 day ago
find . -nmin -30 # modified in last 30 minutes (minutes instead of days)

# access time
find . -atime -7 # accessed in last 7 days
find . -amin -60 # accessed in last hour

# change time
find . -ctime +30 # status changed more than 30 days ago

# newer than reference
find . -newer reference.txt # find files newer than reference.txt
find . -newer timestamp # newer than timestamp
```

Size-based search:

| Size Unit | Meaning                   |
| --------- | ------------------------- |
| `c`       | bytes                     |
| `k`       | kilobytes (1024 bytes)    |
| `M`       | megabytes                 |
| `G`       | gigabytes                 |
| `b`       | 512-byte blocks (default) |
```BASH
find . -size +1M # larger than 1MB
find . -size -100k # smaller than 100KB
find . -size 0 # empty files
find . -size +100M -size -1G # between 100MB and 1GB
```

Permission & Ownership:
```BASH
# permissions
find . -perm 644 # exactly rw-r--r--
find . -perm /222 # any of these bits set (writeable by someone)
find . -perm -644 # at least rw-r--r--
find . -perm /u=w # writeable by owner (symbolic mode)

# ownership
find . -user $(whoami) # owned by current user
find . -user root # owned by root
find . -group users # belongs to users group
find . -group $(id -gn) # belongs to currents user's group
find . -uid 1000 # user ID 1000
find . -gid 100 # group ID 100
```

Depth & Directory control:
==todo==

- xargs
- find
- paste
- join
- split
- expand
- unexpand