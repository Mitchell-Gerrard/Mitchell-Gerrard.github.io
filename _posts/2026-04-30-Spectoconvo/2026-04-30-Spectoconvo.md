---
layout: post
title: "Spectoconvo"
date: 2026-04-30
tagline: Hiding a conversation in sound
image: https://raw.githubusercontent.com/Mitchell-Gerrard/spectoconvo/main/output_with_message_spectrogram.png
category: Python
---
<h3 class="major">Why Put A Message In Music?</h3>

Spectrograms turn sound into pictures. Time runs from left to right, frequency runs from bottom to top, and the brightness of each point represents the energy in the audio. Once I learned that an ordinary piece of music could contain a second visual layer, I wanted to try putting a conversation there.

The idea started with *Doom (2016)*. Its soundtrack is famous for imagery that appears when audio is viewed as a spectrogram. That strange collision of music, code, and hidden pictures made me wonder whether a spectrogram could carry something more practical than an easter egg: a message for a friend.

<h3 class="major">What Is Spectoconvo?</h3>

<a href="https://github.com/Mitchell-Gerrard/spectoconvo">Spectoconvo</a> is a Python package for embedding text into audio and recovering it later. It uses a Fourier transform to inspect the sound, renders the message as an image, and writes that image into the higher-frequency part of the spectrum. The result still behaves like an audio file, but its spectrogram contains a readable message.

<figure>
	<img src="https://raw.githubusercontent.com/Mitchell-Gerrard/spectoconvo/main/output_spectrogram.png" alt="Spectrogram of the original audio" />
	<figcaption>The original audio, represented as a spectrogram.</figcaption>
</figure>

The package exposes two main steps:

<ol>
	<li><code>add_message_to_spectrogram</code> embeds a message and writes a new audio file.</li>
	<li><code>decode</code> reads the processed audio and reconstructs the message image.</li>
</ol>

The encoder can work with WAV and MP3 examples, and the text renderer was designed with multiple languages in mind. That made the project more interesting than drawing an English sentence into a fixed image: the message has to become a visual signal that can survive the trip through an audio format.

<h3 class="major">The Encoding Experiment</h3>

For a first test, I used a short audio sample and a message such as “Hello, Spectrogram!”. Spectoconvo creates an image from the text, transforms the source audio into the frequency domain, and combines the two. The `start_freq` and `intensity` settings control where the message appears and how strongly it is drawn.

<figure>
	<img src="https://raw.githubusercontent.com/Mitchell-Gerrard/spectoconvo/main/message_image.png" alt="The text message rendered as an image" />
	<figcaption>The message before it is placed into the audio spectrum.</figcaption>
</figure>

<figure>
	<img src="https://raw.githubusercontent.com/Mitchell-Gerrard/spectoconvo/main/output_with_message_spectrogram.png" alt="Spectrogram containing an encoded message" />
	<figcaption>The same audio after the message has been embedded.</figcaption>
</figure>

The high-frequency placement is a useful compromise. It gives the message room to exist without competing too much with the main musical content, although it is not invisible and it is not guaranteed to survive every conversion. Re-encoding, compression, aggressive filtering, or a low sample rate can all damage the hidden layer.

<h3 class="major">Decoding The Conversation</h3>

Decoding is deliberately simple from the user’s perspective: point the package at the modified audio file and provide a destination for the reconstructed image. The output is a visual record of the message rather than a decoded text string, which keeps the project closely tied to the spectrogram idea.

<figure>
	<img src="https://raw.githubusercontent.com/Mitchell-Gerrard/spectoconvo/main/output_image_decode.png" alt="Recovered message image from the audio file" />
	<figcaption>The message recovered from the processed audio.</figcaption>
</figure>

<h3 class="major">What I Learned</h3>

This project taught me that audio is not just something to listen to. It is also data that can be measured, transformed, drawn on, and inspected from another angle. The Fourier transform provided the bridge between those views: it changed the problem from “how do I edit a sound wave?” into “where can a visual signal live inside this sound?”

I also learned that the boundary between a demo and a reusable package is made of small details. Handling different audio formats, choosing sensible frequency and intensity defaults, rendering non-English text, and making the encode/decode workflow understandable all mattered as much as the central experiment.

Most importantly, Spectoconvo clarified the limits of the idea. This is a playful form of audio steganography, not encryption. Anyone who inspects the spectrum can discover that something is there, and the message can be degraded by ordinary audio processing. Its value is in the experiment: making a conversation visible inside a piece of sound.

<h3 class="major">Try It Yourself</h3>

The package is available on <a href="https://pypi.org/project/spectoconvo/">PyPI</a>, and the source, examples, and audio assets are on <a href="https://github.com/Mitchell-Gerrard/spectoconvo">GitHub</a>. Start with a short WAV file, encode a sentence, open the resulting spectrogram, and then decode it again. The best part is seeing the message appear in a place where you normally only expect music.