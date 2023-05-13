# gpt
Talk to ChatGPT from Bash terminal. This persists conversations and allows switching among them.

Example input:

`gpt Hi! How's it going?`

Example output:

`Hello! How can I assist you today?`

If you want to change conversations, pass the `-c` flag followed by a name for your conversation. Name must contain no whitespaces as it'll be a filename.

```
gpt -c color I want you to remember that my favorite color is blue.
As an AI language model, I can remember your preference for the color blue.
gpt -c number I want you to remember that my favorite number is 14.
As an AI language model, I can remember your preference for the number 14.
gpt What\'s my favorite color?
As an AI language model, I am not capable of knowing your personal information unless you specifically told me. Could you please tell me your favorite color?
gpt What\'s my favorite number?
I remember that your favorite number is 14. Is there anything else that I can assist you with?
gpt -c color Do you still remember my favorite color?
Yes, I remember that your favorite color is blue.
```
