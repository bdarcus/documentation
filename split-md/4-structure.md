# Styles - Structure

## The Root Element - `cs:style`

The root element of styles is `cs:style`. In independent styles, the
element carries the following attributes:

`class`

:   Determines whether the style uses in-text citations (value
    \"in-text\") or notes (\"note\").

`default-locale` (optional)

:   Sets a default locale for style localization. Value must be a
    [locale code](http://books.xmlschemata.org/relaxng/ch19-77191.html).

`version`

:   The CSL version of the style. Must be \"1.0\" for CSL 1.0-compatible
    styles.

In addition, `cs:style` may carry any of the [global
options](#global-options) and [inheritable name
options](#inheritable-name-options).

Of these attributes, only `version` is required on `cs:style` in
dependent styles, while the `default-locale` attribute may be set to
specify an overriding style locale. The other attributes are allowed but
ignored.

An example of `cs:style` for an independent style, preceded by the XML
declaration:

``` {.xml}
<?xml version="1.0" encoding="UTF-8"?>
<style xmlns="http://purl.org/net/xbiblio/csl" version="1.0" class="in-text" default-locale="fr-FR"/>
```

## Child Elements of `cs:style`

In independent styles, the `cs:style` root element has the following
child elements:

`cs:info`

:   Must appear as the first child element of `cs:style`. Contains the
    metadata describing the style (style name, ID, authors, etc.).

`cs:citation`

:   Must appear once. Describes the formatting of in-text citations or
    notes.

`cs:bibliography` (optional)

:   May appear once. Describes the formatting of the bibliography.

`cs:macro` (optional)

:   May appear multiple times. Macros allow formatting instructions to
    be reused, keeping styles compact and maintainable.

`cs:locale` (optional)

:   May appear multiple times. Used to specify (overriding) localization
    data.

In [dependent styles](#dependent-styles), `cs:style` has only one child
element, `cs:info`.

### Info

The `cs:info` element contains the style\'s metadata. Its structure is
based on the [Atom Syndication
Format](http://tools.ietf.org/html/rfc4287).

In independent styles, `cs:info` has the following child elements:

`cs:author` and `cs:contributor` (optional)

:   `cs:author` and `cs:contributor`, used to respectively acknowledge
    style authors and contributors, may each be used multiple times.
    Within these elements, the child element `cs:name` must appear once,
    while `cs:email` and `cs:uri` each may appear once. These child
    elements should contain respectively the name, email address and URI
    of the author or contributor.

`cs:category` (optional)

:   Styles may be assigned one or more categories. `cs:category` may be
    used once to describe how in-text citations are rendered, using the
    `citation-format` attribute set to one of the following values:

    -   \"author-date\" - e.g. "... (Doe, 1999)"
    -   \"author\" - e.g. "... (Doe)"
    -   \"numeric\" - e.g. "... \[1\]"
    -   \"label\" - e.g. "... \[doe99\]"
    -   \"note\" - the citation appears as a footnote or endnote

    `cs:category` may be used multiple times with the `field` attribute,
    set to one of the discipline categories (see [Appendix I -
    Categories](#appendix-i---categories)), to indicates the field(s)
    for which the style is relevant.

`cs:id`

:   Must appear once. The element should contain a URI to establish the
    identity of the style. A stable, unique and dereferenceable URI is
    desired for publicly available styles.

`cs:issn`/`cs:eissn`/`cs:issnl` (optional)

:   The `cs:issn` element may be used multiple times to indicate the
    ISSN identifier(s) of the journal for which the style was written.
    The `cs:eissn` and `cs:issnl` elements may each be used once for the
    eISSN and
    [ISSN-L](http://www.issn.org/2-22637-What-is-an-ISSN-L.php)
    identifiers, respectively.

`cs:link` (optional)

:   May be used multiple times. `cs:link` must carry two attributes:
    `href`, set to a URI (usually a URL), and `rel`, whose value
    indicates how the URI relates to the style. The possible values of
    `rel`:

    -   \"self\" - style URI
    -   \"template\" - URI of the style from which the current style is
        derived
    -   \"documentation\" - URI of style documentation

    The `cs:link` element may contain content describing the link.

`cs:published` (optional)

:   May appear once. The contents of `cs:published` must be a
    [timestamp](http://books.xmlschemata.org/relaxng/ch19-77049.html),
    indicating when the style was initially created or made available.

`cs:rights` (optional)

:   May appear once. The contents of `cs:rights` specifies the license
    under which the style file is released. The element may carry a
    `license` attribute to specify the URI of the license.

`cs:summary` (optional)

:   May appear once. The contents of `cs:summary` gives a (short)
    description of the style.

`cs:title`

:   Must appear once. The contents of `cs:title` should be the name of
    the style as shown to users.

`cs:title-short` (optional)

:   May appear once. The contents of `cs:title-short` should be a
    shortened style name (e.g. \"APA\").

`cs:updated`

:   Must appear once. The contents of `cs:updated` must be a
    [timestamp](http://books.xmlschemata.org/relaxng/ch19-77049.html)
    that shows when the style was last updated.

The `cs:link`, `cs:rights`, `cs:summary`, `cs:title` and
`cs:title-short` elements may carry a `xml:lang` attribute to specify
the language of the element\'s content (the value must be an
[xsd:language locale
code](http://books.xmlschemata.org/relaxng/ch19-77191.html)). For
`cs:link`, the attribute can also be used to indicate the language of
the link target.

In [dependent styles](#dependent-styles), `cs:link` must be used with
`rel` set to \"independent-parent\", with the URI of the independent
parent style set on `href`. In addition, `cs:link` may not be used with
`rel` set to \"template\".

An example of `cs:info` for an independent style:

``` {.xml}
<info>
  <title>Style Title</title>
  <id>http://www.zotero.org/styles/style-title</id>
  <link href="http://www.zotero.org/styles/style-title" rel="self"/>
  <link href="http://www.example.org/instructions-to-authors#references" rel="documentation"/>
  <author>
    <name>Author Name</name>
    <email>name@example.org</email>
    <uri>http://www.example.org/name</uri>
  </author>
  <category citation-format="author-date"/>
  <category field="zoology"/>
  <updated>2011-10-29T21:01:24+00:00</updated>
  <rights license="http://creativecommons.org/licenses/by-sa/3.0/">This work
  is licensed under a Creative Commons Attribution-ShareAlike 3.0 License</rights>
</info>
```

### Citation

The `cs:citation` element describes the formatting of citations, which
consist of one or more references (\"cites\") to bibliographic sources.
Citations appear in the form of either in-text citations (in the author
(e.g. \"\[Doe\]\"), author-date (\"\[Doe 1999\]\"), label
(\"\[doe99\]\") or number (\"\[1\]\") format) or notes. The required
`cs:layout` child element describes what, and how, bibliographic data
should be included in the citations (see [Layout](#layout)). `cs:layout`
may be preceded by a `cs:sort` element, which can be used to specify how
cites within a citation should be sorted (see [Sorting](#sorting)). The
`cs:citation` element may carry attributes for [Citation-specific
Options](#citation-specific-options) and [Inheritable Name
Options](#inheritable-name-options). An example of a `cs:citation`
element:

``` {.xml}
<citation>
  <sort>
    <key variable="citation-number"/>
  </sort>
  <layout>
    <text variable="citation-number"/>
  </layout>
</citation>
```

**A note to CSL processor developers** In note styles, a citation is
often a sentence by itself. Therefore, the first character of a citation
should preferably be uppercased when there is no preceding text in the
note. In all other cases (e.g. when a citation is inserted into the
middle of a pre-existing footnote), the citation should be printed as
is.

### Bibliography

The `cs:bibliography` element describes the formatting of
bibliographies, which list one or more bibliographic sources. The
required `cs:layout` child element describes how each bibliographic
entry should be formatted. `cs:layout` may be preceded by a `cs:sort`
element, which can be used to specify how references within the
bibliography should be sorted (see [Sorting](#sorting)). The
`cs:bibliography` element may carry attributes for
[Bibliography-specific Options](#bibliography-specific-options) and
[Inheritable Name Options](#inheritable-name-options). An example of a
`cs:bibliography` element:

``` {.xml}
<bibliography>
  <sort>
    <key macro="author"/>
  </sort>
  <layout>
    <group delimiter=". ">
      <text macro="author"/>
      <text variable="title"/>
    </group>
  </layout>
</bibliography>
```

### Macro

Macros, defined with `cs:macro` elements, contain formatting
instructions. Macros can be called with `cs:text` from within other
macros and the `cs:layout` element of `cs:citation` and
`cs:bibliography`, and with `cs:key` from within `cs:sort` of
`cs:citation` and `cs:bibliography`. It is recommended to place macros
after any `cs:locale` elements and before the `cs:citation` element.

Macros are referenced by the value of the required `name` attribute on
`cs:macro`. The `cs:macro` element must contain one or more [rendering
elements](#rendering-elements).

The use of macros can improve style readability, compactness and
maintainability. It is recommended to keep the contents of `cs:citation`
and `cs:bibliography` compact and agnostic of item types (e.g. books,
journal articles, etc.) by depending on macro calls. To allow for easy
reuse of macros in other styles, it is recommended to use common macro
names.

In the example below, cites consist of the item title, rendered in
italics when the item type is \"book\":

``` {.xml}
<style>
  <macro name="title">
    <choose>
      <if type="book">
        <text variable="title" font-style="italic"/>
      </if>
      <else>
        <text variable="title"/>
      </else>
    </choose>
  </macro>
  <citation>
    <layout>
      <text macro="title"/>
    </layout>
  </citation>
</style>
```

### Locale

Localization data, by default drawn from the \"locales-xx-XX.xml\"
locale files, may be redefined or supplemented with `cs:locale`
elements, which should be placed directly after the `cs:info` element.

The value of the optional `xml:lang` attribute on `cs:locale`, which
must be set to an [xsd:language locale
code](http://books.xmlschemata.org/relaxng/ch19-77191.html), determines
which languages or language dialects are affected (see [Locale
Fallback](#locale-fallback)).

See [Terms](#terms), [Localized Date Formats](#localized-date-formats)
and [Localized Options](#localized-options) for further details on the
use of `cs:locale`.

An example of `cs:locale` in a style:

``` {.xml}
<style>
  <locale xml:lang="en">
    <terms>
      <term name="editortranslator" form="short">
        <single>ed. &amp; trans.</single>
        <multiple>eds. &amp; trans.</multiple>
      </term>
    </terms>
  </locale>
</style>
```

#### Locale Fallback

Locale files provide localization data for language dialects (e.g.
\"en-US\" for American English), whereas the optional `cs:locale`
elements in styles can either lack the `xml:lang` attribute, or have it
set to either a language (e.g. \"en\" for English) or dialect. Locale
fallback is the mechanism determining from which of these sources each
localizable unit (a date format, localized option, or specific form of a
term) is retrieved.

For dialects of the same language, one is designated the primary
dialect. All others are secondaries. At the moment of writing, the
available locale files include:

  -----------------------------------------------------------
  Primary dialect      Secondary dialect(s)
  -------------------- --------------------------------------
  de-DE (German)       de-AT (Austria), de-CH (Switzerland)

  en-US (English)      en-GB (UK)

  es-ES (Spanish)      es-CL (Chile), es-MX (Mexico)

  fr-FR (French)       fr-CA (Canada)

  pt-PT (Portuguese)   pt-BR (Brazil)

  zh-CN (Chinese)      zh-TW (Taiwan)
  -----------------------------------------------------------

Locale fallback is best described with an example. If the chosen output
locale is \"de-AT\" (Austrian German), localizable units are
individually drawn from the following sources, in decreasing order of
priority:

A.  In-style `cs:locale` elements
    1.  `xml:lang` set to chosen dialect, \"de-AT\"
    2.  `xml:lang` set to matching language, \"de\" (German)
    3.  `xml:lang` not set
B.  Locale files
    4.  `xml:lang` set to chosen dialect, \"de-AT\"
    5.  `xml:lang` set to matching primary dialect, \"de-DE\" (Standard
        German) (only applicable when the chosen locale is a secondary
        dialect)
    6.  `xml:lang` set to \"en-US\" (American English)

If the chosen output locale is a language (e.g. \"de\"), the (primary)
dialect is used in step 1 (e.g. \"de-DE\").

Fallback stops once a localizable unit has been found. For terms, this
even is the case when they are defined as empty strings (e.g.
`<term name="and"/>` or `<term name="and"></term>`). Locale fallback
takes precedence over fallback of term forms (see [Terms](#terms)).

