# Setting up a development environment


[Note that this won't be the *real* first chapter. This should be under something like "advanced". I just want to record the notes I have, in case they're useful to others, and to get feedback.]

The DataDicer source-code ecosystem comprises several Git repositories that are interdependent. This chapter describes how you can do development on them, setting up a complete development environment, with minimal dependencies on outside code or services. 

We'll start by cloning the lowest-level library, singularjs, and work your way up to the "boilerplate" app. Info about setting up ngram (the `servlet` repo) will be forthcoming.

## Initial setup
You can do development on any type of platform (Windows, Linux, or Mac). You'll need to have these tools installed on your system:

* Node.js - *[FIXME: what is the minimum required version? We should try to ensure, if possible that everything works with the latest LTS, 4.4.3, and doesn't require any version 5 features.]*
* npm - *[FIXME: ditto]*

For Linux or Mac, the easiest and best way to get these is to use nvm.

Next, install these Node.js-based command-line tools:

```
npm install -g bower grunt-cli jshint karma-cli
```

## singularjs

<div style='background: #EEE; padding: 0.5em; border: 2px solid #833; border-radius: 0.7em; margin: 0.5em 1.5em; font-style: italic; font-size: 90%; color: #833;'>We are working to get this up on GitHub (see WT-690), so you can clone it from anywhere. At the time of this writing [4/23/2016], clone it from Stash, ~maloneyc/singularjs-github.</div>

Next, follow the instructions in the README.