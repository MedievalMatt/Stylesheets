VERSION=`cat VERSION`
SAXONOPT=useFixedDate=true
SAXON=java -jar lib/Saxon-HE-9.4.0.6.jar 
OXY=/usr/share/oxygen
SCRIPTS=teitobibtex \
	teitodocbook \
	teitodocx \
	teitoepub \
	teitoepub3 \
	teitofo \
	teitohtml \
	teitohtml5 \
	teitojson \
	teitolatex \
	teitoodt \
	teitopdf \
	teitordf \
	teitoslides \
	teitotxt 

SCHEMASCRIPTS=teitornc \
	teitorelaxng \
	teitoxsd \
	teitodtd 

default: test.rng test-html test-latex test-fo test-to-docx test-from-docx test-other test-rdf test-text test-odt p4top5 oddity pureant scripts test-epub 

p4top5:
	$(SAXON) -o:test17.tei test17.xml ../profiles/default/p4/from.xsl
	diff test17.tei expected-results/test17.tei
	$(SAXON) A54031.tcp ../tcp/tcp2tei.xsl | xmllint --format - > A54031.xml
	diff A54031.xml expected-results/A54031.xml

test.rng:
	../teitorelaxng --odd test.odd test.rng

test-html:
	$(SAXON)  test.xml ../html/html.xsl cssFile=../tei.css $(SAXONOPT) | xmllint --format - > test.html
	diff test.html expected-results/test.html
	$(SAXON) test.html checklinks.xsl
	$(SAXON) test.xml ../html/html.xsl cssFile=../tei.css pageLayout=Complex  $(SAXONOPT)  | xmllint --format - > test-complex.html
	diff test-complex.html expected-results/test-complex.html
	$(SAXON) test.xml ../profiles/ota/html/to.xsl cssFile=../tei.css $(SAXONOPT)  | xmllint --format - | perl cleanup.pl > test-ota.html
	diff test-ota.html expected-results/test-ota.html
	../teitohtml5 test.xml test.html5
	xmllint --encode utf-8 --format test.html5 | perl cleanup.pl > test.temp
	mv test.temp test.html5
	diff test.html5 expected-results/test.html5
	$(SAXON) test20.xml ../html/html.xsl $(SAXONOPT) | xmllint --format - > test20.html
	diff test20.html expected-results/test20.html
	$(SAXON) -versionmsg:off  test3.xml ../html/html.xsl $(SAXONOPT)| xmllint --format - > test3.html
	$(SAXON) -versionmsg:off  test4.xml ../html/html.xsl $(SAXONOPT)| xmllint --format - > test4.html
	diff test4.html test3.html
	$(SAXON) test5.xml ../html/html.xsl  autoBlockQuote=true $(SAXONOPT)  | xmllint --format - > test5.xhtml
	diff test5.xhtml expected-results/test5.xhtml
	jing -c xhtml.rnc test5.xhtml
	$(SAXON) test6.xml ../html/html.xsl  autoBlockQuote=true $(SAXONOPT)  | xmllint --format - > test6.html
	diff test6.html expected-results/test6.html
	jing -c xhtml.rnc test6.html
	$(SAXON) test14.xml ../html/html.xsl $(SAXONOPT)| xmllint --format - > test14.html
	diff test14.html expected-results/test14.html
	$(SAXON) test22.xml ../html/html.xsl $(SAXONOPT)| xmllint --format - > test22.html
	diff test22.html expected-results/test22.html
	$(SAXON) test23.xml ../html/html.xsl $(SAXONOPT)| xmllint --format - > test23.html
	diff test23.html expected-results/test23.html
	(mkdir files; cd files; saxon ../testnotes1.xml ../../html/html.xsl splitLevel=4 useFixedDate=true STDOUT=false cssFile=../../tei.css)
	xmllint --format files/index.html > x ; mv x files/index.html
	xmllint --format files/one.html > x ; mv x files/one.html
	xmllint --format files/two.html > x ; mv x files/two.html
	xmllint --format files/three.html > x ; mv x files/three.html
	diff -r  files expected-results/testnotes
	rm -rf files
	(mkdir files; cd files; saxon ../testnotes2.xml ../../html/html.xsl splitLevel=0 useFixedDate=true STDOUT=false cssFile=../../tei.css)
	xmllint --format files/index.html > x ; mv x files/index.html
	xmllint --format files/one.html > x ; mv x files/one.html
	xmllint --format files/three.html > x ; mv x files/three.html
	diff -r  files expected-results/testnotes2
	rm -rf files
	(mkdir files; cd files; saxon ../testnotes2.xml ../../html/html.xsl splitLevel=4 useFixedDate=true STDOUT=false  cssFile=../../tei.css)
	xmllint --format files/index.html > x ; mv x files/index.html
	xmllint --format files/one.html > x ; mv x files/one.html
	xmllint --format files/two.html > x ; mv x files/two.html
	xmllint --format files/three.html > x ; mv x files/three.html
	diff -r  files expected-results/testnotes3
	$(SAXON) -o:test24.html test24.xml ../html/html.xsl $(SAXONOPT)
	diff test24.html expected-results/test24.html
	$(SAXON) test25.xml ../html/html.xsl $(SAXONOPT) | xmllint --format - > test25.html 
	diff test25.html expected-results/test25.html
	$(SAXON) test27.xml ../html/html.xsl $(SAXONOPT) cssFile=../tei.css cssSecondaryFile=../msdescription.css  | xmllint --format - > test27.html
	diff test27.html expected-results/test27.html
	rm -rf files
	$(SAXON) test28.xml ../html/html.xsl splitLevel=0  STDOUT=false $(SAXONOPT) 
	xmllint --format index.html > x ; mv x index.html
	diff index.html expected-results/test28.html
foo2:
	$(SAXON) test31.xml ../html/html.xsl cssFile="" cssInlineFile=../tei.css $(SAXONOPT)  | xmllint --format - > test31.html
	diff test31.html expected-results/test31.html
	$(SAXON) test32.xml ../html/html.xsl cssFile="" cssInlineFile=../tei.css $(SAXONOPT)  | xmllint --format - > test32.html
	diff test32.html expected-results/test32.html

testsplit:
	$(SAXON) test6.xml ../html/html.xsl STDOUT=false verbose=true splitLevel=1 useFixedDate=true


test-latex:
	$(SAXON) -o:test.tex test.xml ../latex/latex.xsl  $(SAXONOPT)
	diff test.tex expected-results/test.tex
	$(SAXON) -o:testnotes1.tex testnotes1.xml ../latex/latex.xsl  $(SAXONOPT)
	diff testnotes1.tex expected-results/testnotes1.tex
	$(SAXON) -o:test5.tex test5.xml ../latex/latex.xsl  $(SAXONOPT)
	diff test5.tex expected-results/test5.tex
	$(SAXON) -o:test6.tex test6.xml ../slides/teilatex-slides.xsl $(SAXONOPT)
	diff test6.tex expected-results/test6.tex
	$(SAXON) -o:test20.tex test20.xml ../latex/latex.xsl $(SAXONOPT)
	diff test20.tex expected-results/test20.tex
	$(SAXON) -o:test23.tex test23.xml ../latex/latex.xsl $(SAXONOPT)
	diff test23.tex expected-results/test23.tex
	$(SAXON) -o:test24.tex test24.xml ../latex/latex.xsl $(SAXONOPT)
	diff test24.tex expected-results/test24.tex
	$(SAXON) -o:test25.tex test25.xml ../latex/latex.xsl $(SAXONOPT)
	diff test25.tex expected-results/test25.tex
	$(SAXON) -o:test27.tex test27.xml ../latex/latex.xsl $(SAXONOPT)
	diff test27.tex expected-results/test27.tex

test-fo:
	$(SAXON) -o:test.fo test.xml ../fo/fo.xsl  $(SAXONOPT)
	jing fo.xsd test.fo
	diff test.fo expected-results/test.fo
	$(SAXON)  test5.xml ../fo/fo.xsl  $(SAXONOPT)  | xmllint --format - > test5.fo
	jing fo.xsd test5.fo
	diff test5.fo expected-results/test5.fo
	$(SAXON) testnotes1.xml ../fo/fo.xsl  $(SAXONOPT)   | xmllint --format - > testnotes1.fo
	jing fo.xsd testnotes1.fo
	diff testnotes1.fo expected-results/testnotes1.fo 
	$(SAXON) -o:test24.fo test24.xml ../fo/fo.xsl $(SAXONOPT)
	jing fo.xsd test24.fo 
	diff test24.fo expected-results/test24.fo
	$(SAXON) -o:test25.fo test25.xml ../fo/fo.xsl $(SAXONOPT)
	diff test25.fo expected-results/test25.fo
	$(SAXON) test27.xml ../fo/fo.xsl $(SAXONOPT) | xmllint --format - > test27.fo 
	diff test27.fo expected-results/test27.fo

test-epub:
	../teitoepub3  test.xml test.epub
	java -jar epubcheck3.jar test.epub
	../teitoepub  test.xml test.epub
	java -jar epubcheck3.jar  test.epub
	unzip -t test.epub > test.epub.listing
	diff test.epub.listing expected-results/test.epub.listing
	rm test.epub.listing
	../teitoepub  --fileperpage test.xml test-pages.epub
	unzip -t test-pages.epub > test.epub.listing2
	diff test.epub.listing2 expected-results/test.epub.listing2
	rm test.epub.listing2

test-other:
	../teitornc  test21.odd test21.odd.rnc
	perl -p -i -e 's/generated from ODD source.*//' test21.odd.rnc
	diff test21.odd.rnc expected-results/test21.odd.rnc 
	../teitornc  test15.odd test15.odd.rnc
	perl -p -i -e 's/generated from ODD source.*//' test15.odd.rnc
	diff test15.odd.rnc expected-results/test15.odd.rnc 

test-rdf:
	$(SAXON) -o:test12.rdf test12.xml ../profiles/default/rdf/to.xsl
	diff test12.rdf expected-results/test12.rdf

test-text:
	../teitotxt test14.xml test.text
	diff test.text expected-results/test.text

test-odt: 
	../teitoodt test.xml 
	unzip -t test.xml.odt > test.xml.odt.listing
	unzip -o -q test.xml.odt content.xml
	xmllint --format content.xml > content.xml.odt.listing
	diff test.xml.odt.listing expected-results/test.xml.odt.listing
	rm test.xml.odt.listing
	rm content.xml
	../odttotei test7.odt test7.xml
	jing test.rng test7.xml
	diff test7.xml expected-results/test7.xml

test-from-docx:
	../docxtotei test-indexes.docx temp.xml
	xmllint --format temp.xml | perl cleanup.pl > test-indexes.xml  && rm temp.xml
	jing test.rng test-indexes.xml
	diff test-indexes.xml expected-results/test-indexes.xml
	../docxtotei test11.docx temp.xml
	xmllint --format temp.xml | perl cleanup.pl> test11.xml  && rm temp.xml
	jing test.rng test11.xml
	diff test11.xml expected-results/test11.xml
	../docxtotei test19.docx temp.xml
	xmllint --format temp.xml | perl cleanup.pl > test19.xml  && rm temp.xml
	jing test.rng test19.xml
	diff test19.xml expected-results/test19.xml
	../docxtotei test18.docx temp.xml
	xmllint --format temp.xml | perl cleanup.pl > test18.xml  && rm temp.xml
	jing test.rng test18.xml
	diff test18.xml expected-results/test18.xml
	../docxtotei --profile=transcription test-rtf2tei.docx temp.xml
	xmllint --format temp.xml | perl cleanup.pl > test-rtf2tei.xml  && rm temp.xml
	diff test-rtf2tei.xml expected-results/test-rtf2tei.xml
	../docxtotei --profile=transcription test29.docx temp.xml
	xmllint --format temp.xml | perl cleanup.pl > test29.xml  && rm temp.xml
	diff test29.xml expected-results/test29.xml

test-to-docx:
	../teitodocx test.xml 
	unzip -p test.xml.docx word/document.xml | xmllint --format - > test.xml.docx.document
	unzip -p test.xml.docx docProps/core.xml | xmllint --format - | sed 's/>20.*Z</>20Z</' > test.xml.docx.core
	diff test.xml.docx.document expected-results/test.xml.docx.document
	diff test.xml.docx.core expected-results/test.xml.docx.core
	rm test.xml.docx.document
	../teitodocx test6.xml 
	unzip -p test6.xml.docx word/document.xml | xmllint --format - > test6.xml.docx.document
	diff test6.xml.docx.document expected-results/test6.xml.docx.document
	rm test6.xml.docx.document
	../teitodocx testnotes1.xml 
	unzip -p testnotes1.xml.docx word/document.xml | xmllint --format - > testnotes1.xml.docx.document
	diff testnotes1.xml.docx.document expected-results/testnotes1.xml.docx.document
	rm testnotes1.xml.docx.document

oddity:
	../teitohtml5 --summaryDoc --lang=es --profile=tei --odd test15.odd test.odd.html
	xmllint --encode utf-8 --format test.odd.html | perl cleanup.pl> x ; mv x test.odd.html
	diff test.odd.html expected-results/test.odd.html
	$(SAXON) -it:main -o:oddbyexample.odd  -xsl:../tools/oddbyexample.xsl corpus=`pwd`/bare 
	perl -p -i -e 's+<p>generated by oddbyexample.*</p>+<p></p>+' oddbyexample.odd
	perl -p -i -e 's+<date>.*</date>+<date></date>+' oddbyexample.odd
	diff oddbyexample.odd expected-results/oddbyexample.odd
	../teitoxsd  oddbyexample.odd  oddbyexample.xsd
	../teitornc  oddbyexample.odd  oddbyexample.rnc
	../teitodtd  oddbyexample.odd  oddbyexample.dtd
	jing -c oddbyexample.rnc bare/test2.xml
	jing oddbyexample.xsd bare/test2.xml
	xmllint --noout --dtdvalid oddbyexample.dtd bare/test2.xml
	jing -c oddbyexample.rnc bare/test.xml
	jing oddbyexample.xsd bare/test.xml
	xmllint --noout --dtdvalid oddbyexample.dtd bare/test.xml
	$(SAXON) -s:testdrama.odd -xsl:../odds/odd2odd.xsl -o:testdrama.compiled.xml \
	localsource=`pwd`/p5subset.xml $(SAXONOPT) 
	perl -p -i -e 's+ xml:base=".*testdrama.odd"++' testdrama.compiled.xml
	diff testdrama.compiled.xml expected-results/testdrama.compiled.xml
	../teitornc test30.odd test30.rnc
	perl -p -i -e 's/generated from ODD source .*//' test30.rnc
	jing -c test30.rnc test11.xml
	diff test30.rnc expected-results/test30.rnc
	$(SAXON) test.odd ../odds/odd2odd.xsl > test.odd2odd
	$(SAXON) test.odd2odd ../odds/odd2dtd.xsl > test.odd2dtd
	$(SAXON) test.odd2odd ../odds/odd2html.xsl > test.odd2html
	$(SAXON) test.odd2odd ../odds/odd2json.xsl > test.odd2json
	$(SAXON) test.odd2odd ../odds/odd2lite.xsl > test.odd2lite
	$(SAXON) test.odd2odd ../odds/odd2relax.xsl > test.odd2relax
	$(SAXON) test.odd2odd ../odds/odd2xslstripspace.xsl > test.odd2xslstripspace
	rm test.odd2odd
	rm test.odd2dtd
	rm test.odd2html
	rm test.odd2json
	rm test.odd2lite
	rm test.odd2relax
	rm test.odd2xslstripspace
foo:
	../teitoxsd test.odd test.xsd
	../teitodtd test.odd test.dtd
	../teitornc test.odd test.rnc

scripts:
	for i in $(SCRIPTS); do ../$$i maria.xml $$i.result && rm $$i.result; done
	../teitoodd test16.odd temp.odd
	for i in $(SCHEMASCRIPTS) ; do ../$$i temp.odd $$i.result && rm $$i.result; done
	rm temp.odd

pureant:
	java -Xmx256m \
	-classpath  "$(OXY)/tools/ant/lib/ant-launcher.jar" \
	-Dant.home=$(OXY)/tools/ant \
	org.apache.tools.ant.launch.Launcher \
	-f ../docx/build-to.xml \
	-lib $(OXY)/lib/saxon9ee.jar \
	-DoutputFile=`pwd`/test7.docx \
	-DinputFile=`pwd`/test7.xml -Dprofile=default 
	java -Xmx256m \
	-classpath  "$(OXY)/tools/ant/lib/ant-launcher.jar" \
	-Dant.home=$(OXY)/tools/ant \
	org.apache.tools.ant.launch.Launcher \
	-f ../docx/build-from.xml \
	-lib $(OXY)/lib/saxon9ee.jar \
	-DoutputFile=`pwd`/test29.xml \
	-DinputFile=`pwd`/test29.docx -Dprofile=default 

test-oxgarage:
	curl -v  -F upload=@test.odd  -o test.rng http://oxgarage.oucs.ox.ac.uk:8080/ege-webservice/Conversions/ODD%3Atext%3Axml/ODDC%3Atext%3Axml/relaxng%3Aapplication%3Axml-relaxng
	curl -v  -F upload=@test.odd  -o test.zip http://oxgarage.oucs.ox.ac.uk:8080/ege-webservice/Conversions/ODD%3Atext%3Axml/ODDC%3Atext%3Axml/xsd%3Aapplication%3Axml-xsd/

test-oxgarage-tei:
	curl -v  -F upload=@test.odd  -o test.rng http://www.tei-c.org/ege-webservice/Conversions/ODD%3Atext%3Axml/ODDC%3Atext%3Axml/relaxng%3Aapplication%3Axml-relaxng
	curl -v  -F upload=@test.odd  -o test.zip http://www.tei-c.org/ege-webservice/Conversions/ODD%3Atext%3Axml/ODDC%3Atext%3Axml/xsd%3Aapplication%3Axml-xsd/

profile:
	$(SAXON) -o:/dev/null	-TP test.xml ../html/html.xsl >& profile1.html
	$(SAXON) -o:/dev/null	-TP test.xml ../latex/latex.xsl >& profile2.html
	$(SAXON) test.odd ../odds/odd2odd.xsl > test.odd2odd
	$(SAXON) -o:/dev/null -TP test.odd2odd ../odds/odd2relax.xsl >& profile3.html

clean:
	rm -f  test*.odd.rnc test*epub test*pdf test*rdf test*text test*.xml.docx test*.xml.odt test*.tex test*.fo *.html *.xhtml *~ dummy* test.html test3.html test4.html *aux *out *log *nav *ent *snm *toc *.end *.1 *.2 test.xsd test.zip
	rm -f test.rng test.rnc
	rm -f  oddbyexample.* index*div*html
	rm -f  testdrama.compiled.xml
	rm -f  test5.odt
	rm -f  test7.xml test11.xml test18.xml test19.xml test-index.xml
	rm -f  test7.zip
	rm -f  test9.docx test9.odt
	rm -f  test*listing*
	rm -rf temp-dir-for-ant Pictures OEBPS META-INF mimetype
	rm -f test17.tei
	rm -f content.xml.odt.listing
	rm -f test.xml.docx.core
	rm -f test7.docx
	rm -f test.html5
	rm -f A54031.xml
	rm -f content.xml.odt
	rm -f dcr.result
	rm -f test-rtf2tei.xml
	rm -f test29.xml
	rm -f test30.rnc
	rm -f xml.result
	rm -rf charts/ embeddings/ media/
