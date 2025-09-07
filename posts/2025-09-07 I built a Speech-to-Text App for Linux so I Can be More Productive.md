# Long Story

I couldn't find a Speech-to-Text solution for Linux that works similarly to the STT feature on iOS, macOS, and Windows, among others.

I wanted to press a button or shortcut, have it record my voice, convert it to text, and automatically write it in any text box my cursor is focused on. There are a few apps that let you transcribe speech, but you have to use their UI to get your transcription. I could have just continued using one of those other apps and called it a day. It would have saved me a lot of trouble 🤣.

But where is the fun in that? I wanted to have state-of-the-art (SOTA) transcriptions on my Linux computer, making it superior to those on macOS and Windows computers. So a few weeks ago, when I read that Mistral AI introduced Voxtral, a new open-source SOTA transcription model, I got to work.

# First Attempt - Python

Since the Hugging Face Python library already had Voxtral implemented in it, and Python developers often implement AI models first, I decided to build the proof-of-concept app using Python.

It worked well; however, Python is notorious for being hard to package. And let's not even talk about asking someone to install a 5GB PyTorch library and other dependencies just to get things running. So I had to rethink my strategy.

If I am going to make something useful for myself, I want to make it as useful to others as well. 2026 will be the year of the Linux desktop 😉, and so I wanted new users to be able to install SuperSTT as easily as possible.

# Second Attempt - Rust

Rust excels at packaging and shipping things, and, of course, the COSMIC DE, my daily driver, is already built on Rust, making it easier for me to build a native UI later.

However, Rust is not known for supporting AI models (at least not yet). So, although STT models like Whisper were already implemented in Candle (Rust's version of Pytorch + Hugging Face), there was no support for Voxtral yet.

That meant that I had to implement the AI model myself 😵‍💫, which was intimidating, but super exciting, because I had never dealt with AI Rust code before. I will save you some time and skip this part, even though I really wanted to share it. It took 2 weeks to implement Voxtral in Candle and another 2 weeks to get it approved for production.

The rest of the app is much simpler, so once Voxtral had been implemented in SuperSTT, we were ready to use it to transcribe with a shortcut. However, there were no visual indicators, no easy way to change the transcription model, or update other settings.

# SuperSTT Service and UI Clients

I built SuperSTT as a service that runs on systemd. It loads the AI model once when you log in to your system. It exposes a protocol so that any client apps can communicate with the service to update its settings or receive raw audio data (for visualizations).

I built two UI apps:

1. A settings app using libcosmic that should work on any desktop environment.
2. An applet especially made for the COSMIC panel that displays visualizations.

Anyone can build a client for their DE if they really want to (e.g., a Gnome extension, a KDE Widget) and continue to use SuperSTT.

[https://github.com/jorge-menjivar/super-stt](https://github.com/jorge-menjivar/super-stt)
