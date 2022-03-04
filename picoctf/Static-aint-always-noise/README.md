# picoCTF, picoGym - Static ain't always noise

This problem gives us two things.
1. A 'static' file
2. A bash script that is called 'ltdis.sh'


This problem gives the hint
 `"Can you look at the data in this binary: static? This BASH script might help!"`

First thing I did was download both files.
After downloading both files I didnt want to blindly run the script and load up 'static' so I opened it with a text editor. 
It brought back to me a .ELF file. [Learn more about .ELFs here](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format)

Then I proceeded to open the script in Code and started dissescting it.

```bash
#!/bin/bash



echo "Attempting disassembly of $1 ..."


#This usage of "objdump" disassembles all (-D) of the first file given by 
#invoker, but only prints out the ".text" section (-j .text) (only section
#that matters in almost any compiled program...

objdump -Dj .text $1 > $1.ltdis.x86_64.txt


#Check that $1.ltdis.x86_64.txt is non-empty
#Continue if it is, otherwise print error and eject

if [ -s "$1.ltdis.x86_64.txt" ]
then
	echo "Disassembly successful! Available at: $1.ltdis.x86_64.txt"

	echo "Ripping strings from binary with file offsets..."
	strings -a -t x $1 > $1.ltdis.strings.txt
	echo "Any strings found in $1 have been written to $1.ltdis.strings.txt with file offset"



else
	echo "Disassembly failed!"
	echo "Usage: ltdis.sh <program-file>"
	echo "Bye!"
fi

```
It looks like it just goes went through the .elf to find all the strings, essentially automating the job of me using `ctrl+f` in the hex editor.

It then outputs a file as `"filename.ltdis.x86_64.txt"`

Use command `cat static.ltdis.x86_64.txt`

This produces all the scripts output into my terminal. Now I have a couple options at this point...use the `strings` tool, scroll through my hex editor or scroll through the terminal for the pico flag.

Any of those options would produce the flag. (posting the flag here is useless as I believe flags are different for users.)



