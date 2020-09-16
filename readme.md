# Introduction

[LanguageTool](https://www.languagetool.org/) is an Open Source proof­reading software for English, French,
German, Polish, and more than 20 other languages.

This is a Dockerfile to get the LanguageTool running on a Docker environment.

You can use the LanguageTool with a [FireFox](https://addons.mozilla.org/en-US/firefox/addon/languagetool/) or [Chrome](https://chrome.google.com/webstore/detail/grammar-and-spell-checker/oldceeleldhonbafppcapldpdifcinji?hl=en) plugin.

# Usage
## General
The server is running on port 8010, this port should exposed.

`docker run --rm -p 8010:8010 silviof/docker-languagetool`

Or you run it in background via `-d`-option.

## Additional
The default LanguageTool server port is 8081, so please run better this command:

`docker run -d --name languagetool -p 8010:8010 --restart unless-stopped silviof/docker-languagetool`

It will run the container in the background, set the name `languagetool` and restart it automatically when Docker Dekstop is running and the container was not stopped manually.


## Expert
Run with no capabilities and limited RAM:
```
docker run -d --name languagetool \
    -p 8081:8010 \
    --restart unless-stopped \
    --cap-drop=ALL \
    --read-only \
    --memory 300m --memory-swap 1g \
    silviof/docker-languagetool:latest
```

### ngram support
To support [ngrams](http://wiki.languagetool.org/finding-errors-using-n-gram-data) you need an additional volume or directory mounted to the
`/ngrams` directory. For that add a `-v` to the `docker run` command.

`docker run ... -v /path/to/ngrams:/ngrams ...`

Download English ngrams with the commands:
```
mkdir ngrams
wget https://languagetool.org/download/ngram-data/ngrams-en-20150817.zip
(cd ngrams && unzip ../ngrams-en-20150817.zip)
rm -f ngrams-en-20150817.zip
```

After run this command:
```
docker run -d --name languagetool \
    -p 8081:8010 \
    --restart unless-stopped \
    -v `pwd`/ngrams:/ngrams:ro \
    silviof/docker-languagetool
```
