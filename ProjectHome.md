(Part of the larger WikiSense project aimed at understanding Wikipedia for semantic annotation of texts)

WikiXMLJ provides easy access to Wikipedia XML dumps.

**Latest ([r43](https://code.google.com/p/wikixmlj/source/detail?r=43)): What's new?**
  * Speedup in SAX parsing ([issue #9](https://code.google.com/p/wikixmlj/issues/detail?id=#9))

**Features:**
  * Easy access to important elements of a Wikipedia page
  * Also provides interfaces for Wiki text parsing.
  * Memory efficient
    * SAX interface for parsing
    * Lazy loading of files for DOM
    * Callback support with DOM
  * Directly operate on compressed wikipedia dumps (gzip/bzip2/native xml supported)
> Note: gzip streams are **way** faster than bzip2 for a slight trade off in space.

**DOMParser Example:**
```
import edu.jhu.nlp.wikipedia.*;

...

	WikiXMLParser wxp = WikiXMLParserFactory.getDOMParser(args[0]);
	try {
		wxp.parse();
		WikiPageIterator it = wxp.getIterator();
		while(it.hasMorePages()) {
			WikiPage page = it.nextPage();
			System.out.println(page.getTitle());
		}

	}catch(Exception e) {
		e.printStackTrace();
	}
```


**SAXParser Example:**
```
import edu.jhu.nlp.wikipedia.*;

...
	WikiXMLParser wxsp = WikiXMLParserFactory.getSAXParser(args[0]);
		
	try {
		  
	    wxsp.setPageCallback(new PageCallbackHandler() { 
                           public void process(WikiPage page) {
		                  System.out.println(page.getTitle());
	                   }
            });
		
	   wxsp.parse();
	}catch(Exception e) {
		e.printStackTrace();
	}
```


---

**Notes:**

1. The DOM parser is known to run out of memory despite of lazy loading the DOM tree for very large Wikipedia dumps (like English). This issue will be fixed eventually until then using the SAX parser interface is highly recommended. If you really want to use the DOM parser, try it with the callback interface.

2.This should not be confused with the [Java Wikipedia Parser](http://code.google.com/p/java-wikipedia-parser/) that converts wiki-text to HTML.


**Dependencies:**

All dependencies are packaged into this source. The dependencies might have different licensing terms.
  * Apache [Xerces](http://xerces.apache.org) DOM parser for lazy loading.
  * Optionally uses [bzip2 code](http://www.kohsuke.org/bzip2) refactored by Kohsuke Kawaguchi from Apache's [Ant](http://ant.apache.org) project.