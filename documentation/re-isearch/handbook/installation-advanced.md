# Installation (Advanced)

```
The basic steps are:

1) clone the repository.
   $ git clone https://github.com/re-Isearch/re-Isearch.git

   Prerequisites you need:
   - libdl    This is used for dynamic loading. It should be part of your core OS
   - libz     In package zlib. It should be part of your core OS 


   Some prerequisites you may want/need:
   - libmagic (in Package: libmagic-dev )
   - Berkeley DB (in Package: libdb-dev)

  Use of libdb is currently limied to some caching code that is NOT required by the engine kernel. 

  If you do not have libmagic (and don't want to install or build it). Turn it off. It is controlled by
  USE_LIBMAGIC 
  See doctype/autodetect.hxx.


2) Go into the build/ directory
   $ cd re-Isearch/build/

   if you have g++/gcc-10 installed and are running Ubuntu x86 64-bit you could just type Make and it should just build. 

But generally:
3) Edit the Makefile to confirm that the correct version of GCC is specified as well as the desired paths and command line args.
   The standard we used to build was g++/gcc-10. You may also use whatever gcc/g++ is installed (we don't use any of the newer language features much less C++20) but since we all have typically multiple g++/gcc versions (due to some software requirements) you set it here.

   In the file you will see there are also provisions for other compilers such as the typical CC and cc.

   It should work on any of the BSD variants, Solaris, AIX etc. with any of a number of processors from ARM to x86.

   One sets here a number of compile options and flags for producing debug or production among other things.

   See the Makefile up until
   After the ############### End Differences ###################################### 
   there are a few options that may need tweaking on different platforms.

   $make -j4
   (or any -j option depending upon hardware)

4) To build the optional plugins
   $ make plugins

    WARNING: The default install directory for plugins in the Makefile is: 
    /opt/nonmonotonic/ib/lib/plugins
    this directory needs to be writable by the user doing the make plugins
    if you want to have the plugins installed elsewhere please redefine the path
    and consult the handbook about configuration details.

    NOTES about ODT2: plugin
    The ODT2 plugin is currently experimental. It will eventually replace the ODT doctype (wwhich uses Pandoc).
    for it to work on needs to 
    a) install the odf-search shell script in some directory findable by the ODT2 plugin.
    b) modify the odf-search shell script so that it uses the odf-search.jar (e.g. fully specified path where it is installed)


5) Test to see everything went well.
   $ ../bin/Iindex

it should produce a long list of command line options-- as well as options to get the embdeded help.
If it returns a message on of the order of "  error while loading shared libraries: " you need to either define the library load path:
   LD_LIBRARY_PATH
   or ldconfig
   or define an appropriate user account for the installation (see the handbook Admin section).

   Now that the libraries resolve let's index something.

   In the test/data directory are a number of files for testing. We'll index the XMLified collected works of Shakespeare.

  $ ../bin/Iindex -d /tmp/BART ../test/data/shakespeare.xml
  Here we read the sample XML collection of the collected works of Shakespeare and write an index to /tmp/BART
  We let the AUTODETECT handler do its magic and recognize the document both as a kind of XML and a play.
  Notice that in the conf/ directory we have a "play.ini" configuration file.  We read it and do what it suggests-- namely to ignore P and FM tags.

   We might want to know what fields/paths have been parsed:

    $ ../bin/Iutil -d /tmp/BART -vf
Iutil [Info]: Iutil 4.0a (x86_64 GNU/Linux)
The following fields are defined in this database: 'ACT' 'EPILOGUE' 'FM' 'GRPDESCR' 'INDUCT' 'LINE' 'P' 'PERSONA' 'PERSONAE' 'PGROUP' 'PLAY' 'PLAYSUBT' 'PLAY\ACT' 'PLAY\ACT\EPILOGUE' 'PLAY\ACT\EPILOGUE\SPEECH' 'PLAY\ACT\EPILOGUE\SPEECH\LINE' 'PLAY\ACT\EPILOGUE\SPEECH\SPEAKER' 'PLAY\ACT\EPILOGUE\STAGEDIR' 'PLAY\ACT\EPILOGUE\SUBTITLE' 'PLAY\ACT\EPILOGUE\TITLE' 'PLAY\ACT\PROLOGUE' 'PLAY\ACT\PROLOGUE\SPEECH' 'PLAY\ACT\PROLOGUE\SPEECH\LINE' 'PLAY\ACT\PROLOGUE\SPEECH\SPEAKER' 'PLAY\ACT\PROLOGUE\SPEECH\STAGEDIR' 'PLAY\ACT\PROLOGUE\SPEECH\SUBHEAD' 'PLAY\ACT\PROLOGUE\STAGEDIR' 'PLAY\ACT\PROLOGUE\TITLE' 'PLAY\ACT\SCENE' 'PLAY\ACT\SCENE\SPEECH' 'PLAY\ACT\SCENE\SPEECH\LINE' 'PLAY\ACT\SCENE\SPEECH\LINE\STAGEDIR' 'PLAY\ACT\SCENE\SPEECH\SPEAKER' 'PLAY\ACT\SCENE\SPEECH\STAGEDIR' 'PLAY\ACT\SCENE\SPEECH\SUBHEAD' 'PLAY\ACT\SCENE\STAGEDIR' 'PLAY\ACT\SCENE\SUBHEAD' 'PLAY\ACT\SCENE\TITLE' 'PLAY\ACT\TITLE' 'PLAY\FM' 'PLAY\FM\P' 'PLAY\INDUCT' 'PLAY\INDUCT\SCENE' 'PLAY\INDUCT\SCENE\SPEECH' 'PLAY\INDUCT\SCENE\SPEECH\LINE' 'PLAY\INDUCT\SCENE\SPEECH\SPEAKER' 'PLAY\INDUCT\SCENE\SPEECH\STAGEDIR' 'PLAY\INDUCT\SCENE\STAGEDIR' 'PLAY\INDUCT\SCENE\TITLE' 'PLAY\INDUCT\SPEECH' 'PLAY\INDUCT\SPEECH\LINE' 'PLAY\INDUCT\SPEECH\SPEAKER' 'PLAY\INDUCT\STAGEDIR' 'PLAY\INDUCT\TITLE' 'PLAY\PERSONAE' 'PLAY\PERSONAE\PERSONA' 'PLAY\PERSONAE\PGROUP' 'PLAY\PERSONAE\PGROUP\GRPDESCR' 'PLAY\PERSONAE\PGROUP\PERSONA' 'PLAY\PERSONAE\TITLE' 'PLAY\PLAYSUBT' 'PLAY\PROLOGUE' 'PLAY\PROLOGUE\SPEECH' 'PLAY\PROLOGUE\SPEECH\LINE' 'PLAY\PROLOGUE\TITLE' 'PLAY\SCNDESCR' 'PLAY\TITLE' 'PLAY\' 'PROLOGUE' 'SCENE' 'SCNDESCR' 'SPEAKER' 'SPEECH' 'STAGEDIR' 'SUBHEAD' 'SUBTITLE' 'TITLE'



   Now search
   Search has a lot of options. We'll use -show to have the tool show us the hit.
  $../bin/Isearch -d /tmp/BART -show to be or not to be

Process time: 19 ms. (1) Search: 17 ms.
Query: "to be or not to be"

1 record of 37 matched your query, 1 record displayed.

       Score  File
   1.   100  /home/edz/ib/ib2/build/data/shakespeare.xml
`The Tragedy of Hamlet, Prince of Denmark'
Hit: To be, or not to be: that is the question:

   There are a bunch of options and many tools. Please consult the handbook.

Example:
   $ ../bin/Isearch -d /tmp/BART -show -P speech/speaker to be or not to be

Process time: 18 ms. (2) Search: 16 ms.
Query: "to be or not to be"

1 record of 37 matched your query, 1 record displayed.

       Score  File
   1.   100  /home/edz/ib/ib2/build/data/shakespeare.xml
`The Tragedy of Hamlet, Prince of Denmark'
** 'speech/speaker' Fragment: 
HAMLET
Hit: To be, or not to be: that is the question:


   $ ../bin/Isearch -d /tmp/BART -P SPEECH/SPEAKER -rpn out spot PEER

Process time: 4 ms. (1) Search: 3 ms. (1.3 ms/term)
Query: "out" "spot" PEER

1 record of 37 matched your query, 1 record displayed.

       Score  File
   1.   100  /home/edz/ib/ib2/build/data/shakespeare.xml
`The Tragedy of Macbeth'
** 'SPEECH/SPEAKER' Fragment: 
LADY MACBETH
Hit: Out, damned spot! out, I say!--One: two: why,

The above search is looking for the words "out" and "spot" in the same node. It found them in the same line. We also asked for the value of the child SPEAKER of the parent SPEECH of the hit, e.g. who expressed the line.  It was, of course, Lady Macbeth

For a short usage summary:

   $ ../bin/Isearch


6) To make language bindings:
   $ cd ../swig


Windows Platform note:
   We don't officilly support the Windows platform at this time.
   That said, everything is there for Windows but have not maintained any testing so may have broken something at one point-- fortunately it should be releative easy to patch is needed and would welcome the contribution.
```

\
