# Software Foundations

Mirror of [“Software Foundations”](http://www.cis.upenn.edu/~bcpierce/sf/), by Benjamin Pierce et al., version 4.0 (May 2016).

Includes a [PDF version](doc/pdf/pierce-2016.pdf) of the book.

#### Usage

*OSX Requirements*

* [mactex](https://tug.org/mactex/mactex-download.html)

To rebuild the PDF, ensure Coq and LaTeX are installed, then:

```
$ make
```

### Notes

The original [`Makefile`](src/Makefile) has been modified to work around the LaTeX nesting limit, and to generate chapters in the right order.  A syntax error has also been fixed in [`MoreStlc.v`](src/MoreStlc.v).

```diff
diff --git a/src/Makefile b/src/Makefile
index 18ddc1e..9faec71 100644
--- a/src/Makefile
+++ b/src/Makefile
@@ -53,7 +53,7 @@ OPT?=
 COQDEP?="$(COQBIN)coqdep" -c
 COQFLAGS?=-q $(OPT) $(COQLIBS) $(OTHERFLAGS) $(COQ_XML)
 COQCHKFLAGS?=-silent -o
-COQDOCFLAGS?=-interpolate -utf8
+COQDOCFLAGS?=-interpolate -utf8 -p '\usepackage{enumitem}\setlistdepth{9}\setlist[itemize,1]{label=$$\bullet$$}\setlist[itemize,2]{label=$$\bullet$$}\setlist[itemize,3]{label=$$\bullet$$}\setlist[itemize,4]{label=$$\bullet$$}\setlist[itemize,5]{label=$$\bullet$$}\setlist[itemize,6]{label=$$\bullet$$}\setlist[itemize,7]{label=$$\bullet$$}\setlist[itemize,8]{label=$$\bullet$$}\setlist[itemize,9]{label=$$\bullet$$}\renewlist{itemize}{itemize}{9}'
 COQC?="$(COQBIN)coqc"
 GALLINA?="$(COQBIN)gallina"
 COQDOC?="$(COQBIN)coqdoc"
@@ -65,7 +65,7 @@ COQCHK?="$(COQBIN)coqchk"
 #                    #
 ######################

-VFILES:=Symbols.v\
+RVFILES:=Symbols.v\
   Preface.v\
   Basics.v\
   Induction.v\
@@ -106,6 +106,9 @@ VFILES:=Symbols.v\
   Postscript.v\
   Bib.v

+reverse=$(if $1,$(call reverse,$(wordlist 2,$(words $1),$1))) $(firstword $1)
+VFILES:=$(call reverse,$(RVFILES))
+
 -include $(addsuffix .d,$(VFILES))
 .SECONDARY: $(addsuffix .d,$(VFILES))
```
