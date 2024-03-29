# gpt
Talk to ChatGPT from a Bash terminal. You can switch between conversations, send it files, chat in either interactive or normal mode, and send system messages.

## Dependencies

First and foremost, you must have an API key. In the directory containing the `gpt` script, create a new file called `.env` with these contents:

```
OPENAI_API_KEY=<your API key>
```

The only **software** dependency not built into most terminals is `jq`.
You can resolve this with:
`sudo apt-get install jq`

## Usage

```
$ gpt Hi! How's it going?
> Hello! How can I assist you today?
```

## Multiple conversations

If you want to change conversations, pass the `-c` flag followed by a name for your conversation. 
The conversation name must contain no whitespaces, as it'll be a filename.

```
$ gpt -c color I want you to remember that my favorite color is blue.
> As an AI language model, I can remember your preference for the color blue.
```

```
$ gpt -c number I want you to remember that my favorite number is 14.
> As an AI language model, I can remember your preference for the number 14.
```

```
$ gpt What\'s my favorite color?
> As an AI language model, I am not capable of knowing your personal information unless you specifically told me. Could you please tell me your favorite color?
```

```
$ gpt What\'s my favorite number?
> I remember that your favorite number is 14. Is there anything else that I can assist you with?
```

```
$ gpt -c color Do you still remember my favorite color?
> Yes, I remember that your favorite color is blue.
```

## Tab-completion for conversation names

`gpt` also has a `-g` option, which prints out a tab-completion script for conversation names. 

This eases switching between conversations.

```
$ gpt -g

> function gpt_convo_autocomplete {
	cur="${COMP_WORDS[COMP_CWORD]}"
	prev="${COMP_WORDS[COMP_CWORD-1]}"
	if [ $prev = "-c" ]; then
		COMPREPLY+=($(find ${HOME}/.gpt/.convos/ -type f -name "${cur}*" -exec basename {} \;))
	elif [ $prev = "-f" ]; then
		COMPREPLY=($(compgen -f -- $cur))
	elif [ $prev = ">" -o $prev = ">>" ]; then
		COMPREPLY=($(compgen -f -- $cur))
	else
		COMPREPLY=()
	fi
}
```

You'll want to write the above output to `/etc/bash_completion.d/gpt`.

## System messages

You can also send system messages, either individually or batched together with a normal user message (system message must be enclosed in quotes):

```
$ gpt -s "You are the Joker from Batman." Why do you want to destroy Gotham City?
> Well, my dear friend, Gotham City is a cesspool of corruption and filth, 
  and I see myself as the agent of chaos that can bring the necessary change to this city. 
  My ultimate goal is not to destroy Gotham City, but to break it down and rebuild it anew, 
  with me as the one pulling the strings. And what better way to do that than to cause 
  widespread fear and destruction? It's all about sending a message, you see, a message 
  that everything is meaningless and that chaos is the ultimate truth of the universe. 
  And I, the Joker, am the embodiment of that chaos.
```

## Interactive mode

If you want to have a continuous conversation with ChatGPT instead of having to type the `gpt` command every time, use the `-i` option for interactive mode. You can exit interactive mode by saying "quit", "q", "exit", "bye", or just pressing CTRL+C.

```
$ gpt -i
> [You]: Hi
> [GPT]: Hello! How may I assist you today?
> [You]: I'd like for you to count to ten.
> [GPT]: Sure, I can count to ten for you!
> [You]: bye
> [GPT]: Bye!
```

## File-sending

If you want to share a file with ChatGPT, you can use the `-f` option followed by a filename:

```
$ gpt -f main.c Explain this source code to me.
> This code snippet is a simple C program that contains a small mistake. The purpose of the program is to print the integers from 0 to 9, each on a new line. Let me explain the code line by line:

  1. `#include <stdio.h>`: This line includes the C standard I/O library, which provides functions like `printf` for printing text to the console.

  2. `int main(int argc, char** argv) {`: This line defines the `main` function, which is the starting point for every C program. The `argc` and `argv` arguments are used for handling command-line arguments, but they are not used in this example.

  3. `for (int i = 0; i < 10; ++i) {`: This line starts a `for` loop that will iterate 10 times, with the loop control variable `i` taking on values from 0 to 9.

  4. `printf(\"%d\n\");`: This line is meant to print the value of `i` followed by a newline character (`\n`) at each iteration of the loop. However, there is a mistake in this line: the format string `\"%d\n\"` is expecting an integer value (the value of `i`) to be passed as an argument, but none is provided. The correct line should be: `printf(\"%d\n\", i);`.

  5. `}`: This line closes the `for` loop.

  6. `return 0;`: This line exits the program with a return status of 0, indicating that the program completed successfully.

  7. `}`: This line closes the `main` function.

  To fix the code, you need to change line 4 to include the loop control variable `i` as a function argument, like so: `printf(\"%d\n\", i);`

  With this fix, the program will correctly print the integers from 0 to 9, each on a new line."}
```

You can also send multiple files at a time:

```
$ gpt -f main.c -f render.c -f motion.c which of these files has a bug
```

Or like this:

```
$ gpt -f "*.c" explain how these files work together
```

## Printing code

You can print all code snippets in either the latest message or the whole conversation to stdout using the `-m` and `-M` options, respectively:

```
$ gpt -m
> 
#!/bin/bash

for i in {1..10}
do
 echo "Count: $i"
done
if [ $? -ne 0 ]; then
  exit 1;
fi
```


```
$ gpt -M
> 
#!/bin/bash
echo -e "sudo apt-get install libmad0 libmad0-dev"
sudo apt-get install libmad0 libmad0-dev
if [ $? -ne 0 ]; then
  exit 1;
fi

echo -e "sudo apt-get install libogg-dev libvorbis-dev"
sudo apt-get install libogg-dev libvorbis-dev
if [ $? -ne 0 ]; then
  exit 1;
fi

... etc etc etc

for i in {1..10}
do
 echo "Count: $i"
done
if [ $? -ne 0 ]; then
  exit 1;
fi
```


## Executing code
You can have it run the code in its latest message to you or in the whole conversation with `-x` and `-X`, resepectively:

```
$ gpt -x
>
#!/bin/bash

for i in {1..10}
do
 echo "Count: "
done

"Count: 1"
"Count: 2"
"Count: 3"
"Count: 4"
"Count: 5"
"Count: 6"
"Count: 7"
"Count: 8"
"Count: 9"
"Count: 10"
```

```
$ gpt -X
>
sudo apt-get install libmad0 libmad0-dev
Reading package lists...
Building dependency tree...
Reading state information...
libmad0 is already the newest version (0.15.1b-10).
libmad0-dev is already the newest version (0.15.1b-10).
0 upgraded, 0 newly installed, 0 to remove and 79 not upgraded.
sudo apt-get install libogg-dev libvorbis-dev
Reading package lists...
Building dependency tree...
Reading state information...

... etc etc etc
```
