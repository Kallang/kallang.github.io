---
layout: post
title:  "Use ffmpeg to Extract MP3 from Video or Audio in Other Formats"
date:   2014-06-08 16:28:02
categories: unix
---

# 1. Install ffmpeg

{% highlight bash %}
brew install ffmpeg # for MAC users
sudo apt-get install ffmpeg # for linux users
{% endhighlight %}

# 2. Extrat MP3 from video

{% highlight bash %}
ffmpeg -i 01.flv -f mp3 -vn 01.mp3
ffmpeg -i 01.flv -acodec libmp3lame -vn 01.mp3 # use specified mp3 encoder.
{% endhighlight %}

* `-vn` disable the video output.
*  `-acodec` specify the mp3 encoder ffmpeg should uses.

# 3. Convert flac audio to MP3

{% highlight bash %}
ffmpeg -i flac.flac -ar 44100 -ab 320k -ac 2 1.mp3
{% endhighlight %}

* `-ar` set audio sampling rate (in Hz)
* `-ac` set number of audio channels
* `-ab` set the audio bitrate
