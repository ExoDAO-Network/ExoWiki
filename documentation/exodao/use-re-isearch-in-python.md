---
description: A Short Introduction
---

# Use re-Isearch in Python

re-Isearch is implemented mainly in C / C++. We can create a Python Binding using [SWIG](https://www.swig.org) (_Simplified Wrapper and Interface Generator_).

<details>

<summary>Install SWIG</summary>

You may need to install the PCRE library:

```
 sudo apt-get install libpcre3 libpcre3-dev
```

Now just run the following commands:

```
wget https://downloads.sourceforge.net/project/swig/swig/swig-2.0.12/swig-2.0.12.tar.gz
```

```
tar -xzvf swig-2.0.12.tar.gz
```

```
cd swig-2.0.12
```

```
./configure
```

```
make
```

```
sudo make install
```

</details>

Simply go to the _swig_ folder in your local copy.

```
cd /re-Isearch/swig
```

There, you can edit  `Makefile.py` to specify the Python version you are using in `PYVERSION`.&#x20;

{% hint style="warning" %}
Make sure you have the appropiate `python-dev` package installed, which provides the header files needed to run C / C++ code.
{% endhint %}

Now simply run

```
make Python
```

This creates `PyIB.so`, which is a _shared object file_ that can be loaded into your Python script.

### Example

Run this in a folder with _shakespeare.xml_ (can be found in `/re-Isearch/test/data`) to create an Index

```python
import sys
import string
from IB import *

junk="/tmp/JUNK";
pdb = IDB(junk);
print "This is PyIB version %s/%s" % (string.split(sys.version)[0], pdb.GetVersionID());
if not pdb.IsDbCompatible():
  raise ValueError, "The specified database '%s' is not compatible with this version. Re-index!" % `junk`

pdb.AddRecord ("shakespeare.xml");

pdb.SetMergeStatus(iMerge);

pdb.BeforeIndexing();

if not pdb.Index() :
  print "Indexing error encountered";

pdb.AfterIndexing();
```

To do an example search, now run the following script

```python
import sys
import string
from IB import *

junk="/tmp/JUNK";
pdb = IDB(junk);
print "This is PyIB version %s/%s" % (string.split(sys.version)[0], pdb.GetVersionID());
if not pdb.IsDbCompatible():
  raise ValueError, "The specified database '%s' is not compatible with this version. Re-index!" % `junk`

sentence =  "to be or not to be";
#sentence = "Hate Christian OR";
#sentence = "Hate Jew OR";

squery = SQUERY(sentence);
print squery;
query = QUERY();
query.SetSQUERY(squery);

elements = pdb.GetTotalRecords();

print "Database ", junk, " has ", elements, " elements";

total = 10;
if elements > 0:
    rset = pdb.VSearchSmart(query);
    print type(rset);
    print rset;
    total = rset.GetTotalEntries();
    print "Searching for: ", query;
    print "Got = ", total, " Records";
    # Print the results....
    for i in range(1,total+1):
        result = rset.GetEntry(i);
        area = pdb.Context(result, "____", "____") ;
        datum = result.GetDate();

        score  = result.GetScore();
        hits   = result.GetHitTable();
        print "[", i , "] ", rset.GetScaledScore(score, 100), " ", score, " ", pdb.Present(result, ELEMENT_Brief);
        print "\tFormat: ", result.GetDoctype();
        print "\tFile:", result.GetFullFileName(), "  [", result.GetRecordStart(), "-", result.GetRecordEnd(), "]";
        print "\tDate: ", datum.RFCdate();
        print "\tMatch: ", area;
else:
    print 'Empty Index!';

pdb = None; # Delete
```



