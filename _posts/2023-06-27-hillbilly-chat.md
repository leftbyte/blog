---
title: "Hillbilly Chat"
date: 2023-06-27
---

Joining the crowd of folks interested in the current advances in
machine learning and specifically the LLVMs being produced by OpenAI,
I thought I'd create a simple chatbot. I have always loved how people
say certain things in regional areas (like the south in the United
States), so I thought a translator to "hillbilly speak" would be fun.

# Initial stab

I looked at some examples and cobbled together something in a few
minutes that used OpenAI's gpt-3.5-turbo model. The
[API](https://platform.openai.com/docs/api-reference/introduction) is
fairly straight forward and it's interesting that all the state is
saved on the client, but it seems like the future bottleneck will be
the memory limit of the messages context. After configuring the OpenAI
API key and dealing with the pricing plan and billing, I was able to
share the demo quickly to my friends using
[gradio](https://gradio.app/quickstart).

# Publishing to the Internet

After seeing that my friends had so much fun with it, I then wanted to
push this out to the Internet to let others enjoy the banter. My first
thought was to review the security issues. There's not much exposed
with this app but I did worry about Internet-scale and how the app
would run up my OpenAI API bill, so I implemented some rate limiting
to throttle the requests rate.

The actual publishing to a hosting service using gradio and
HuggingFace was very simple. Within the terminal I simply used 'gradio
deploy' and that workflow walked me through pushing it to a
HuggingFace space. I ran into two issues:

1. README.md Configuration error: The HuggingFace Space build failed
and said that there was a README.md configuration error. With some
quick Googling about the issue I saw that I needed some frontmatter as
described in the [HuggingFace Spaces Configuration
reference](https://huggingface.co/docs/hub/spaces-config-reference).

```
---
title: hillbilly-chat
app_file: billy.py
sdk: gradio
sdk_version: 3.35.2
emoji: 🤠
colorFrom: orange
colorTo: brown
pinned: true
---
```

2. The build failed again due to "RuntimeError: Share is not supported
   when you are in Spaces". Looking at my app, I saw that I had
   `gr.Interface(...).launch(share=True)`, which I simply changed to
   false.

The app was then deployed and you can now play around with the
[hillbilly-chat](https://huggingface.co/spaces/leftbyte/hillbilly-chat)!

To try it out, hit the link and enter a phrase into the input box and
see what a hillbilly would say.

# Talking about it

I'm in the process of taking Jeremy Howard's excellent fast.ai course
in which he espouses the value of talking about your work. This is why
I'm blogging about this now. My last step in this little project was
to create a blogging site and post to talk about my fun hillbilly-chat
project. I don't expect to get anybody reading this, but it's useful
to shed that fear of showing the world what you've done.

# Next steps

I tried changing the prompts so that more regional dialects and
colloquialisms would be returned, but it seems like the only responses
are from a "United States Southern" region, which starts to get stale
quickly. My assumption is that the reason for this regionality is
because the corpus of data sucked into GPT-3.5 is very US centric and
doesn't discern between regional differences. The ideas to try to fix this
are to 1. create my own vector database with a documents from different regions
(books, articles, cartoons, etc) and feed that as context prompts into ChatGPT or
2. try to fine tune a smaller language model with that same corpus of
data. Seems like access to good data is at the core of the issue.