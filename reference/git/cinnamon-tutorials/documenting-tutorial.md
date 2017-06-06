[Home](/) / 
[Reference](/reference/git/) / 
[Cinnamon Tutorials](/reference/git/cinnamon-tutorials) /
[Documentation](/reference/git/cinnamon-tutorials/documentation.html)

## Writing tutorials

It is also possible to write tutorials that are not part of the code. For example, this. They can be found in `docs/reference/cinnamon-tutorials/`, and are included into the cinnamon documentation via the `docs/reference/cinnamon-tutorials/cinnamon-tutorials-docs.sgml.in` file. These are plain docbook items, and existing docbook tutorials are suitable.

This is intended to be a quick guide on the DocBook syntax.

### Overall structure

DocBook is a markup language that vaguely resembles HTML, but the tag names are more verbose. A significant difference is that DocBook is more structured, where we have a top level `<book>`, containing different `<parts>` that have `<chapter>`s that in turn contain `<section>`s. The general structure of a regular DocBook item might look like

```
<book>
  <part>
    <chapter>
      <section>
        <section>
          ...
        </section>
        <section>
          ...
        </section>
      </section>
      <section>
        ...
      </section>
    </chapter>
    <chapter>
      ...
    </chapter>
  </part>
  <part>
    <chapter>
      ...
    </chapter>
  </part>
</book>
```

As shown above, sections can be nested. In the example, we have layer 1 sections and layer 2 sections. It is possible to skip section layers and, say, directly include a layer 2 section inside a chapter. This can be done via using the `<sect2>` instead of the `<section>` tag (you can specify up to the 5th layer). You can also skip parts or chapters. For example, you might have

```
<book>
  <part>
    <sect2>
        ...
    </sect2>
    <chapter>
      ...
    </chapter>
  </part>
</book>
```

This is helpful because sections in layers behave differently. Parts are numbered with Roman numerals. For example, a part might be labeled

### I. Code documentation

On the other hand, chapters and sections are not numbered.

More importantly, using `<sect2>` directly will affect chunking and the table of contents. Chunking is the act of splitting different sections into different pages when presented to the user. Different parts and chapters are always chunked, and so are layer 1 sections. A section appears on the table of contents if and only if it is a part, a chapter, or a layer 1 section inside a chapter (but a layer 1 section directly inside a part is not displayed).

So if you wish to use chapters but do not want the layer 1 sections to be chunked, you will want to directly place a layer 2 section inside the chapter. Similarly, if you don't want your subsections to appear in the table of contents, use a layer 2 section without a layer 1 section.

#### Escaping characters

Similar to HTML, certain characters have to be escaped. For example, the > symbol is typed as > and < is <. Similarly, the ampersand &amp; has to be escaped as &amp;amp;.

#### xi:include

It is possible to include content from other xml files via xi:include. The precise syntax is

```
<xi:include href="building.xml"/>
```

The path is relative to the top-level document (ie. `$(DOC_MODULE)-docs.sgml`).

To use this feature, you need to declare the xi: namespace. This is done via using the following doctype declaration:

```
<!DOCTYPE book PUBLIC '-//OASIS//DTD DocBook XML V4.3//EN'
'http://www.oasis-open.org/docbook/xml/4.3/docbookx.dtd'
[
<!ENTITY % local.common.attrib "xmlns:xi  CDATA  #FIXED 'http://www.w3.org/2003/XInclude'">
]>
```

### ags and attributes

#### The "title" tag

It is (and recommended) to give each part/chapter/section a title. This can be done via the `<title>` tag. For example, a section might look like

```
<section>
  <title>The title</title>
  ...
</section>
```

You should give all part/chapter/sections a title. If you cannot think of any sensible title, you probably don't want a separate section.

#### The "para" tag

This is the tag you will use the most often. It denotes a paragraph, and is equivalent to the HTML `<p>` tag. For example,
      
```
<section>
  <title>The title</title>
  <para>Lorem ipsum dolor sit amet</para>
</section>
```


#### The "code" tag

To display inline code, you will need the `<code>` tag. This is functionally the same as the HTML `<code>` tag. For example,

```
<para>Lorem <code>ipsum</code> dolor sit amet.</para>
```

will be displayed as

```
Lorem `ipsum` dolor sit amet.
```

#### The "programlisting" tag

This is used to display multiple lines of code, such as the examples shown above. For example, the example above is

```
<programlisting>
    <section>
        <title>The title</title>
        <para>Lorem <code>ipsum</code> dolor sit amet.</para>
    </section>
</programlisting>
```

There are a few things to take note of. Firstly, the < and > items are escaped. Secondly, the contents of the `<programlisting>` is flushed to the left, regardless of the current indentation of the xml. This is since all whitepsace is rendered verbatim. Thirdly, the end `</programlisting tag>` is put on the same row as the last line, or else an extra row will appear in the output.

If you are intending to show code, you will want to do syntax highlighting. Doing so via the regular DocBook way is useless. Instead, you need some magic, namely enclosing your `<programlisting>` inside an `<informalexample>` tag, eg.

```
<informalexample>
  <programlisting>
    function foo () {
        return bar
    }
  </programlisting>
<informalexample>
```

If you do so, then you are allowed to have a line break before the closing `<programlisting>` tag as well as indent all your code by a common indent to follow the indent of your xml code. gtk-doc will automatically fix all those problems for you (assuming you are using gtk-doc 1.22 or above).

Note that the language of syntax highlighting is specified on a per-project basis inside the Makefile (for gtk-doc 1.24 or above; it is hardcoded to be always C for gtk-doc 1.23 or below). The syntax highlight for this tutorial is for Javascript, so if you are writing about something that doesn't resemble Javascript even vaguely (eg. xml), don't use syntax highlighting.

#### The "itemizedlist" tag

This generates an itemized list, similar to the HTML `<ul>` tag. Individual items are put inside `<listitem>` tags, similar to the HTML `<li>`. An example usage is as follows:

```
<itemizedlist>
  <listitem>
    A “<code>metadata.json</code>” file which contains information about the applet, such as its name, description etc..
  </listitem>
  <listitem>
    An “<code>applet.js</code>” file which contains its code.
  </listitem>
</itemizedlist>
```

Lists can be nested, and markup can also be used. Sometimes you might want to enclose the contents of each line with a `<para>`
