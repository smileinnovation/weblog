---
layout: post
title: From multilingual speech to subtitles 
subtitle: "How to build a multilingual video automatic subtitles system"
slug: from-multilingual-speech-to-subtitles 
description:
tags:
- ai
- artificial-intelligence
- machine-learning
- openai
- llm
- multilingual
- speech2text
- voice
author: alrou
featured: true
image: assets/images/posts/transcribovox/subtitles.png
---

---
# From multilingual speech to subtitles

## How to build a multilingual video automatic subtitles system

Recently we have participated in a public tender related to the need of an innovative automatic video transcription and subtitles generation.

A toolchain to automatically create speech transcription is nowaday pretty common, and many solutions exist.

But in the context of this project a special request adds an important challenge. The need is to transcript multilingual videos, including Luxembourgish language, to generate subtitles.

Luxembourgish is spoken mainly in Luxembourg. [About 400,000](https://en.wikipedia.org/wiki/Luxembourgish) people speak Luxembourgish worldwide. Luxembourgish is quite close to German, but it is characterised by a high degree of [multilingualism](https://orbilu.uni.lu/bitstream/10993/55105/1/0001147.pdf): words and phrases coming mainly from French and German are part of the regular lexicon and occur quite frequently.

This is a challenge because all the common tools available are not always made to support multilingual audio, but also mainly because Luxembourgish is definitely not a language as common as English or French.

And last but not least, we had to do it with a constraint in terms of budget, for this proof of concept phase, which was approximately 10 days to work on a concept to validate our ideas.


## State-of-the-art speech to text

For this project we looked at open source “speech to text” models capable of handling such tasks. As of today, two candidates are clearly leading the game :

* [Meta XLS-R (wav2vec)](https://ai.meta.com/blog/xls-r-self-supervised-speech-processing-for-128-languages/)
* [OpenAI Whisper](https://openai.com/research/whisper)

Both models are using an [encoder/decoder model](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model)) architecture, and so are comparable. Their training data and training methods are different, which definitely impact the accuracy and the supported languages

They both propose different model sizes to choose the accuracy / throughput ratio adapted to the need. They also both support many languages, with interesting accuracy for broadly spoken languages. Luxembourgish is part of the supported languages, but with a poor accuracy, due to a very small amount of training data for this particular language.

Regarding the optimisation of these models to better support the Luxembourgish, we can mention several local initiatives. These initiatives all come from the academic world, as it requires collecting and organising specific training data (Luxembourgish speech records with textual ground truth). To name two of them, both using model from Meta (wav2vec) :



* Luxembourg University work to build training data and fine-tuning of model : [summary](https://infolux.uni.lu/lux-asr/) and [detailed work ](https://orbilu.uni.lu/handle/10993/55105)
* A thesis work from [Le Minh Nguyen](https://campus-fryslan.studenttheses.ub.rug.nl/223/1/MA_s4923723_LM_Nguyen.pdf)

Their results are interesting, as all of them have proven to significantly decrease the word error rate on Luxembourg audio transcription. Therefore, for some unknown reasons, the available fine-tunes models completely lack support for other languages.

As we can easily understand that the success of such a project will require this kind model fine-tuning to get a proper accuracy, it is also important to take into account the subtitles constraints: the transcription needs to be in sync with the video stream, and should be organised as sentences, not just as a list of words without any punctuation.

All these criterias must be taken into account to get a proper result. In that context it is important to look at what the models are capable of.

And during our tests we found that the Meta model was only capable of giving the words timestamp, no sentence structure nor their timestamp. In comparison the Whisper model is able to transcribe precisely the sentences with their timestamp.


## Which model for this proof of concept ?

To summarise, we need a model with multi-language transcription capabilities, with Luxembourgish support, and with a transcription that contains sentence structures and timestamps.

At the time of writing this article, there is no model that matches all these criterias. As we had to deal with a short period of time to build this proof of concept, and as the Luxembourgish training data set is somehow already existing, we choose to focus on the model that is the more suitable to generate the subtitle rather than having the best Luxembourgish transcription accuracy.

Fine-tuning a model takes time and can be costly, so we choose to build a full toolchain to validate each step rather than focusing on early accuracy. Fine-tuning could be done later on, with a validated toolchain architecture.

Also, Luxembourgish is not the only language to be supported by this proof of concept. English, German and French are expected to be frequent. And these languages have very good accuracy without any fine-tuning.

So the Whisper model approach is interesting :



* We can get a good sentence transcription
* Main languages have very good word error rate
* Fine-tuning step for better support of Luxembourgish will be possible

One alternative solution would have been to keep using the fine-tuned Meta XLS-R, and build some algorithm to rebuild sentences from word timestamps. We tried this method, but the results were not satisfactory, or would have involved other AI models.

And on top of that, you also need to take into consideration the multilingual content aspect.

That’s why we had chosen to go with the Whisper model.


## Challenge and our idea

Beyond the compromise made on the choice of a model, the main challenge for this project is to address multilingual video/audio content.

The whisper model is capable of understanding different languages, but its internal logic is not capable of switching from one language to another within the same audio transcription.

With a multilingual content, this model will transcribe the content using the first language it will detect at the very beginning of the audio stream. Its multilingual capabilities could catch the language differences and produce a good enough transcription, but we will lose the context information (current language) and the word error rate will definitely increase.

An idea we came up with was to process the transcription of each speaker individually: the Whisper model would process each of them as individual audio, repeating then language detection and transcription.

The detection and division of an audio stream by speaker is what is called “speaker diarization”.

Several solutions exist to apply this “speaker diarization”, in our case we choose to go with an open-source solution, based on an AI model : [Pyannote audio](https://github.com/pyannote/pyannote-audio).

This python library is a toolkit specially designed for diarization tasks:



* speech activity detection,
* speaker change detection,
* overlapped speech detection,
* speaker embedding

In our case we have included this toolkit to be able to split one audio stream into several, each of them representing a talk from an identified speaker, with timestamp relative to the original audio file.

![alt_text](/assets/images/posts/transcribovox/step1.png "Step 1")


After this step we have then a list of audio files, representing a track of a specific speaker.

![alt_text](/assets/images/posts/transcribovox/step2.png "Step 2")

For each of them we can then use the Whisper model that will return a textual transcript of the audio, as sentences, with the timestamp.

The last step will be to apply the offset of each track on each transcription timestamp to get a full transcription synchronised with the original audio file.

**Transcription to subtitles**

The final step is to use all this information to produce a subtitle file, according to the expected syntax (i.e: [SRT](https://fr.wikipedia.org/wiki/SubRip), [VTT](https://fr.wikipedia.org/wiki/WebVTT)).

As we get structured information from Pyannote and Whisper (JSON or CSV), it is quite easy to use regular programming language to transform this information into a subtitle file.

But here we wanted to go a bit further to introduce automation and transcription adjustments, which are essential parts of the subtitle creation job, often forgot:



* To fix any possible transcription typos or to rephrase a sentence in a more concise way
* To rearrange the sentences
* To automatically format the transcription into the desired format
* To apply any automatic translation

One perfect tool for such a task is a LLM. For example we can build a prompt to ask OpenAI GPT to transform our transcription (structured information as JSON or CSV) into a subtitle file, with text review/adjustment, and if required to translate them in any target language.

![alt_text](/assets/images/posts/transcribovox/step3.png "Step 3")

A prompt sample we have been using:


```
You are an AI assistant that automates CSV to ${format} conversion.

To remember you some languages codes:
- lb = Luxembourgish
- en = English
- fr = French
- de = German

For the conversion from the CSV content :
- For each line transcribe the text in "Text" column as a whole text, using the language code available in the language column of the same line
- Convert time code in the Start and End columns into dubbing time code.
- Use the speaker column to add the speaker identification in the output format (${format})

CSV content:
Speaker;language;Start;End;Text
${content}

${format} content:
```

## What we have built

To demonstrate our idea we had to build a minimal toolchain that will rely on the technologies we just described.


![alt_text](/assets/images/posts/transcribovox/architecture.png "Architecture")


Our prototype was based on a simple web application able to:

* Upload or record an audio file
* Send the audio file to a remote API to run the diarization, the transcription and subtitle format steps
* Display the result of each step


![alt_text](/assets/images/posts/transcribovox/app1.png "Web application screen")

The remote API was running on a server equipped with a GPU (Nvidia T4/16Gb) to accelerate the AI steps.


## Does it work ?

We ran many tests to understand the performance of this prototype, especially to compare the accuracy of transcription with and without the diarization. Of course we knew that the word error rate for Luxembourgish would not be good, as our Whisper model was not trained enough for this language.

But we saw a clear improvement with diarization, on both multilingual and monolingual contents.


### Audio with Luxembourgish only

For this test we were using the audio track of this [video : GovJobs - Présentation métiers : Ministère de la Justice](https://www.youtube.com/watch?v=9JJ4UORMhPU).

<span style="text-decoration:underline;">Without diarization</span>


![alt_text](/assets/images/posts/transcribovox/app2.png "Results without diarization, luxembourgish only")


The detected language is German, and so the full audio is transcribed into German, with an average accuracy.

<span style="text-decoration:underline;">With diarization </span>

The diarization allows Whisper to produce better results, both in language detection accuracy. The language detection is not perfect : even if all speakers are Luxembourgish speakers, not all of their dialogs are identified as Luxembourgish, but German or Dutch.

One interesting point is that the language detection is consistent with the speaker (for example the speaker “03” is alway detected as Luxembourgish).

We certainly reach the limit of the Whisper model regarding the Luxembourgish language: its training is certainly insufficient and is not sufficiently representative of the varieties of accents or pronunciations.

<span style="text-decoration:underline;">Subtitles creation</span>

Here no surprise, with the prompt we described previously, the LLM is producing the SRT file content without any issue


![alt_text](/assets/images/posts/transcribovox/app3.png "Results with diarization, luxembourgish only")


### Multilingual content

We tested several multilingual audio streams, and in this example we read the transcription of a live recording made by our client during the demo of our prototype.

Their record was made by 4 people, speaking respectively in German, English, French and Luxembourgish.

<span style="text-decoration:underline;">Without diarization</span>


![alt_text](/assets/images/posts/transcribovox/app3.png "Results without diarization, multilingual")


Without the diarization, of course we have the same issue: everything is transcribed into German as this is the first language used during the recording. Also the word error rate is what you can expect from this behaviour.

<span style="text-decoration:underline;">With diarization</span>


![alt_text](/assets/images/posts/transcribovox/app4.png "Results with diarization, multilingual")


If we enable diarization, we have the 4 speakers identified, with their language. The word error rate is better for each of them, but still pretty low for Luxembourgish language.

<span style="text-decoration:underline;">Subtitle creation</span>

The subtitle creation is also good, either we let the original language or force a specific translation.


![alt_text](/assets/images/posts/transcribovox/app5.png "Final subtitles #1")
![alt_text](/assets/images/posts/transcribovox/app6.png "Final subtitles #2")


## Some numbers

### Word error rate

In accordance with our sample video, the ground truth is determined by the SRT file provided by the relevant ministry responsible for the video.

| Generated prompt (reduced for readability) | Without Diarization | With Diarization |
|--------------------------------------------|---------------------|------------------|
| Transcribe auto (DE detected, wrongly)     |        93,86%       | 88,60%           |
| With Diarization                           |        92,60%       | 82,41%           |
{:.post_table}

### Real time factor

Our solution involves multiple computation steps, so the transcription speed is also to be taken into account. This workload requires intensive computation, and specific hardware for their optimization.

In our case to host the API (pyannote and whisper) we have been using an AWS EC2 instance equipped with a dedicated GPU. This is an entry level GPU, but still not the cheapest hardware. The API can be hosted on a regular CPU instance but its performance will be really impacted (between x6 and x10 slower).

With this instance (EC2 g4dn.xlarge - 1x Nvidia T4 GPU 16Gb), we have been able to reach a real time factor of 3 : this means that for a 3 minutes audio, this solution will require 1 minute of total processing time.

This performance factor can be indeed increased by using a better GPU, but of course this will also increase the total cost of the solution.

## Conclusion

In conclusion, the integration of diarization and transcription tracking has significantly improved the quality of subtitle generation. Our chosen Whisper model excels in sentence timestamp accuracy, which is pivotal for creating synchronised subtitles. Despite its efficacy, the model's performance in language detection and word error rate for Luxembourgish is inadequate when compared to more widely spoken languages. This was an expected trade-off, considering the model's current training data.

Moving forward, our refinement strategy involves the accumulation of a substantial corpus of Luxembourgish audio samples. Our aim is to collect data that not only exceeds the volume obtained in prior initiatives, like the Luxembourg University usage of Meta model, but also encompasses a diverse spectrum of dialects and pronunciations. This diversity is crucial to mitigate the model’s language confusion with similar languages, such as Dutch and German. By doing so, we can enhance the model's robustness and ensure that Luxembourgish is represented with the same accuracy as other languages.

In addition to improving technical metrics, our project holds the potential to significantly impact Luxembourg's multilingual community by providing more accessible and accurate information. Furthermore, it will contribute to the growing field of computational linguistics and automatic subtitle generation, paving the way for more inclusive technology that bridges language barriers. As we embark on this journey, we welcome collaboration with linguistic experts and the tech community to create a model that serves as a benchmark for smaller language groups globally.
