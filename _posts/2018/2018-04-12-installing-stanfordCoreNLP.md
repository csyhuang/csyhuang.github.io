---
layout: post
title: Installing Stanford Core NLP package on Mac OS X
tags: ['setup']
comments: true
---

I am following instructions on the [GitHub page of Stanford Core NLP](https://github.com/stanfordnlp/CoreNLP) 
under **Build with Ant**. To install `ant`, you can use homebrew:
<!--more-->
```
$ brew install ant
```

In Step 5, you have to include the .jar files in the directory `CoreNLP/lib` and 
`CoreNLP/liblocal` in your `CLASSPATH`. To do this, first, I install *coreutils*:

```
brew install coreutils
```

such that I can use the utility `realpath` there. Then, I include the following in my `~/.bashrc`:

```
for file in `find /Users/clare.huang/CoreNLP/lib/ -name "*.jar"`;
  do export CLASSPATH="$CLASSPATH:`realpath $file`";
done

for file in `find /Users/clare.huang/CoreNLP/liblocal/ -name "*.jar"`;
  do export CLASSPATH="$CLASSPATH:`realpath $file`";
done
```

(I guess there are better ways to combine the commands above. Let me know if there are.)

To run CoreNLP, I have to download the latest version of it, and place it in the directory 
`CoreNLP/`:

```
wget http://nlp.stanford.edu/software/stanford-corenlp-full-2018-01-31.zip
```

The latest version is available on 
their [official website](https://stanfordnlp.github.io/CoreNLP/download.html#steps). Unzip 
it, and add all the .jar there to the $CLASSPATH.

Afterwards, you shall be able to run CoreNLP with the commands provided 
in [the blogpost of Khalid Alnajjar](https://www.khalidalnajjar.com/setup-use-stanford-corenlp-server-python/) 
(under **Running Stanford CoreNLP Server**). If you have no problem starting the server, 
you shall be able to see the interface on your browser at http://localhost:9000/:


```
java -mx4g -cp "*" edu.stanford.nlp.pipeline.StanfordCoreNLPServer -annotators "tokenize,ssplit,pos,lemma,parse,sentiment" -port 9000 -timeout 30000
```

Yay. Next, I will try setting up the python interface.
