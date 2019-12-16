# TEI text extraction for the Chronicles project 

This repository provides code for the extraction of text from TEI files for the **Chronicles** project. 

## Quick start

Download the content of this repository, and run:

>   mvn clean package


This will generate code for binding TEI XML elements to Java objects, as well as an executable jar for text extraction. 

Binding was tested with `xjc` version 2.3.1 under Java 10. Compilation may run under Java 9, but this was not tested.

To extract text from a TEI file, run:

>   bash extract_text.sh <path-to-TEI-input-file> <out-dir>
  

## XML-Java binding and code generation

The code relies on [Jaxb](https://javaee.github.io/jaxb-v2/) to map the TEI XML representation to java objects. The classes for these objects are generated at compilation with the [maven-jaxb-plugin](https://github.com/highsource/maven-jaxb2-plugin). The code can be generated (without `jar` packaging) with:

>   mvn clean compile

Code binding relies on a XSD schema for the input TEI files, and on a bindings file. The schema and bindings are located in `./src/resources/teixlite/`.

## Binding schema 

The XSD file was obtained from the [teixlite.rng](https://tei-c.org/Vault/P4/xml/custom/schema/relaxng/teixlite.rng) schema provided by the TEI Consortium, converting it to XSD with [trang](https://relaxng.org/jclark/trang.html).

This schema is not exactly the same as the [DBNL schema](https://www.dbnl.org/xml/dtd/teixlite.dtd) used for the chronicles. This notably means that `cf` sections are not extracted for now.  


## Formatting 

The following formatting decisions have been taken when extracting the text from TEI-XML to plain text:

- Table and Lg elements are marked with a [TABLE] or [<type>] tag, e.g., [POEM];
- Margin notes are marked by square brackets;
- Foot notes are referred to by their number in the text, and are placed at the bottom of a page;
- Paragraphs that are interrupted by page numbers are regrouped to appear on the same page (some page files may be empty as a result);
- interpGrp elements are ignored, whereas they may be useful (they provide dates);
- Line breaks have been added to increase readability (after headings, before tables, etc.)
