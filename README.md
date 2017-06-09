---
title: 'rdfxml.py: An RDF/XML Parser in under 10KB of Python'
...

rdfxml.py - An RDF/XML Parser in Python
=======================================

rdfxml.py is a standalone Python module in under 10KB that parses
RDF/XML using SAX. It was written to be used as a simple drop-in module
for larger projects—for when you just want the smallest and simplest
possible module to get the job done. Since it's standalone, it only
requires modules that are in the Python standard library; it doesn't
force you do download any RDF specific APIs etc. It's been released
under both GPL 2 and the W3C's software license.

The module is available as [rdfxml.py.txt](rdfxml.py.txt). Don't forget
to rename it to `rdfxml.py`. The file should have 228 lines and 9433
bytes. Its MD5 is ec83de4e97d4f7e8f05b1eb9dc14a0c1.

Ancillary Documentation
-----------------------

Documentation in the module itself is sparse, but there's not much to
know. The parser was written to conform to the [23 January 2003 RDF/XML
Syntax
Specification](http://www.w3.org/TR/2003/WD-rdf-syntax-grammar-20030123)
Working Draft; with the exception that bagID is no longer supported
since the RDF Core WG decided to remove it from the syntax.

It can be used as a command line tool: it takes a URI as the only
argument, and produces a list of NTriples. For example:-

<div class="pre">

<div>

\$ ./rdfxml.py http://infomesh.net/2003/rdfparser/meta.rdf

</div>

<div>

&lt;http://infomesh.net/2003/rdfparser/rdfxml.py.txt&gt;
&lt;http://purl.org/dc/elements/1.1/title&gt; "rdfxml.py - An RDF/XML
Parser in Python" .

</div>

<div>

&lt;http://infomesh.net/2003/rdfparser/rdfxml.py.txt&gt;
&lt;http://purl.org/dc/elements/1.1/creator&gt; \_:id0 .

</div>

<div>

\_:id0 &lt;http://xmlns.com/foaf/0.1/name&gt; "Sean B. Palmer" .

</div>

<div>

\_:id0 &lt;http://xmlns.com/foaf/0.1/homePage&gt;
&lt;http://purl.org/net/sbp/&gt; .

</div>

</div>

Otherwise, the only functions of concern to users are:-

`parseRDF(s, base=None, sink=None)`
:   Takes in a string s, and parses it as RDF/XML, calling the "triple"
    method on the sink with the subject predicate and object as the
    three arguments for each time a triple is found; try [the Sinks
    section](#sink) for more information. base gives the optional base
    URI of the input.

`parseURI(uri, sink=None)`
:   A wrapper that opens a URI with urllib first before passing the
    dereferenced content to the parseRDF function.

### Sinks

Sinks must be classes with a `triple(self, s, p, o)` method. s, p, and o
are Python strings corresponding to the subject, predicate, and object
productions of the of the
[N-Triples](http://www.w3.org/TR/rdf-testcases/#ntriples) format's
[EBNF](http://www.w3.org/TR/rdf-testcases/#ntrip_grammar). It is hoped
that this is a usable API-independent way of returning the terms of the
triples. Here's an example function that will return a list of (s, p, o)
tuples from a string containing RDF/XML:-

    import rdfxml

    class Sink(object): 
       def __init__(self): self.result = []
       def triple(self, s, p, o): self.result.append((s, p, o))

    def rdfToPython(s, base=None): 
       sink = Sink()
       return rdfxml.parseRDF(s, base=None, sink=sink).result

Note that terms may be str or unicode instances.

License
-------

The file [rdfxml.py.txt](rdfxml.py.txt) is released under the [GNU GPL
version 2](http://www.gnu.org/licenses/gpl.txt) or later. There is also
[another version](rdfxml.w3c.py.txt) released under the [W3C's software
license](http://www.w3.org/Consortium/Legal/2002/copyright-software-20021231).
The only difference between the two files is the \_\_doc\_\_ string. The
W3C software license is [fully
compatible](http://www.w3.org/Consortium/Legal/IPR-FAQ-20000620#GNU)
with GPL.

Rationale and Uses
------------------

This code was not written as a competitor to
[Eikeon](http://eikeon.com/)'s comprehensive RDF parser in his
[rdflib](http://rdflib.net/). I'm sure that his code has undergone a lot
more testing than this. This code was written simply because sometimes
you don't want to download an entire API when you just want a quick
hack. This is a good tool for when you just want triples quickly.

For example, [Ken MacLeod](http://bitsko.slc.ut.us/) has used
*rdfxml.py* in his
[foaf-check](http://bitsko.slc.ut.us/blog/2003/06/24/foaf-check) FOAF
application. [Dan Brickley](http://rdfweb.org/people/danbri/) appears to
want a version in Ruby for some project... (Having two users before it's
even been released is rather enjoyable.)

But Seriously... Why?
---------------------

I've been working on an RDF API for a while now. I needed an RDF/XML
parser for it, and so here it is. I've been building the API very
carefully, with the goal of making it *practical*. That is, I've only
worked on it when I've actually needed it for a project. In that way, I
think that it's become quite useful. It's still undergoing major
revision, though, so no public releases yet. You can mail me if you want
more information or a sneak preview.

To Whom Should I Address Comments/Feedback?
-------------------------------------------

The author, Sean B. Palmer, preferably at sean@mysterylights.com with a
CC to www-archive or www-rdf-interest if it's relevant; or sometimes you
can find me at \#sbp on freenode.

Waxing homepage: [Sean B. Palmer](http://inamidst.com/sbp/)\
Waning homepage: [Sean B. Palmer](http://infomesh.net/sbp/)
