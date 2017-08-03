# `cl-conllu` quickstart

[ ](a biblioteca funciona em outros compiladores além de sbcl?)

`cl-conllu` is a Common Lisp library to work
with [CoNLL-U](http://universaldependencies.org/format.html) files,
licensed under
the [Apache license](http://www.apache.org/licenses/LICENSE-2.0).

## quicklisp

If don't have quicklisp installed already,
follow [these steps](https://www.quicklisp.org/beta/#installation).

The `cl-conllu` library is not available from quicklisp's servers, so
you must clone this project to your `local-projects` quicklisp
directory (usually at `~/quicklisp/local-projects/`). Navigate there
and

- if you'll be developing the library, you should clone your fork of
  the library. So fork the library, and do
  
		git clone https://github.com/your-username/cl-conllu.git

- if you just want to use it, you can download
  the [.zip](https://github.com/own-pt/cl-conllu/archive/master.zip)
  of the repository, and unpack it.

After downloading the project to your `local-projects` directory, you
just have to load the library. Do

	(ql:quickload "cl-conllu")

and quicklisp will download the library's dependencies and then load
it.

## reading CoNLL-U files

First off, you need some CoNLL-U files to start with. If you have none
in mind, you may get some from
[here](https://github.com/own-pt/bosque-UD/tree/master/documents).

The simplest way to read a file with `cl-conllu` is with `read-file`:

``` common-lisp
CL-USER> (cl-conllu:read-file #p"/path/to/my/file/CF1.conllu")
(#<CL-CONLLU:SENTENCE {1003CFD9C3}> #<CL-CONLLU:SENTENCE {1003D164C3}>
 #<CL-CONLLU:SENTENCE {1003D1BB13}> #<CL-CONLLU:SENTENCE {1003D2C013}>
 #<CL-CONLLU:SENTENCE {1003D348A3}> #<CL-CONLLU:SENTENCE {1003D3E383}>
 #<CL-CONLLU:SENTENCE {1003D49C23}>)
```

Each object returned is a `sentence` object, made up of `token`
objects, which we will describe in the next section.

Other convinient functions are `read-directory` and
`read-stream`. There's also an umbrella function, `read-conllu`, which
will try to guess the input you passed and call the appropriate
function to read it.

All read functions accept a `fn-meta` function as argument. This
function collects the metadata from each CoNLL-U sentence, which
usually includes the raw sentence (see
the [reference](http://universaldependencies.org/format.html)). The
default metadata collector function, `collect-meta`, is defined for
the metadata scheme used by
the [OpenWordnet-PT](http://wnpt.brlcloud.com/wn/).

## cl-conllu objects

[ ](see data.lisp)

### tokens

[ ](what's the difference between tokens and mtokens?)

### sentences

## visualizing CoNLL-U files

## querying CoNLL-U files

