# polyglot-base
An Alpine-Linux-based image with Polyglot installed. It is a "natural language
pipeline that supports massive multilingual applications".

# About Polyglot
- https://github.com/aboSamoor/polyglot
- http://polyglot.readthedocs.io/

# More about this Docker image
This is a small image intended as a platform for building applications that can
process natural languages. It uses Python 3 to take advantage of the unified
unicode string support. Why? It is because when using Python 2, Polyglot quickly
breaks down due to ASCII being the default encoding--interoperation with other
system processes does not work. Therefore, Python 3 to the rescue!

# See also
Docker image: polyglot-service

# Playing with the image
Launch a temporary (discarded when terminated) container, via command line:

    #!/bin/sh
    docker pull turbulent/polyglot-base
    docker run --rm -it turbulent/polyglot-base /bin/ash

Below are samples for language detection. The results should be:

- English
- French
- Chinese (Simplified)
- Chinese (Traditional)
- Russian
- Ukrainian

Notice that the `polyglot` app simply returns broad language name whereas the
Python approach returns a more specific locale code.

Detect languages using `polyglot` app:

    #!/bin/sh

    echo "Hello World!" | polyglot detect
    echo "Ceci n'est pas un pipe" | polyglot detect
    echo "我需要你的帮助。这是紧急情况。" | polyglot detect
    echo "我需要你的幫助。這是緊急情況。" | polyglot detect
    echo "Все люди рождаются свободными и равными в своем достоинстве и правах." | polyglot detect
    echo "Всі люди народжуються вільними і рівними у своїй гідності та правах." | polyglot detect

Detect languages using Python:

    #!/usr/bin/env python

    from polyglot.detect import Detector
    detector = Detector("Hello World!")
    print(detector.language.locale)
    detector = Detector("Ceci n'est pas un pipe")
    print(detector.language.locale)
    detector = Detector("我需要你的帮助。这是紧急情况。")
    print(detector.language.locale)
    detector = Detector("我需要你的幫助。這是緊急情況。")
    print(detector.language.locale)
    detector = Detector("Все люди рождаются свободными и равными в своем достоинстве и правах.")
    print(detector.language.locale)
    detector = Detector("Всі люди народжуються вільними і рівними у своїй гідності та правах.")
    print(detector.language.locale)

# "Some assembly required" demonstrations
Some Polyglot features require "models" to be downloaded. Embeddings, for
instance. http://polyglot.readthedocs.io/en/latest/Embeddings.html

Download "embeddings2.en"

    #!/bin/sh

    polyglot download embeddings2.en

Perform data mining using Python:

    #!/usr/bin/env python

    from polyglot.mapping import Embedding
    embeddings = Embedding.load("/root/polyglot_data/embeddings2/en/embeddings_pkl.tar.bz2")
    neighbors = embeddings.nearest_neighbors("green")
    print(neighbors)

# Copyright & License
Copyright (C) 2017  Turbulent Media inc.

GPLv3 - See LICENSE file
