# Locale Files - Structure

While localization data can be included in styles (see
[Locale](#locale)), locale files conveniently provide sets of default
localization data, consisting of terms, date formats and grammar
options.

Each locale file contains localization data for a single language
dialect. This [locale
code](http://books.xmlschemata.org/relaxng/ch19-77191.html) is set on
the required `xml:lang` attribute on the `cs:locale` root element. The
same locale code must also be used in the file name of the locale file
(the \"xx-XX\" in \"locales-xx-XX.xml\"). The root element must carry
the `version` attribute, indicating the CSL version of the locale file
(must be \"1.0\" for CSL 1.0-compatible locale files). Locale files have
the same requirements for [namespacing](#namespacing) as styles. The
`cs:locale` element may contain `cs:info` as its first child element,
and requires the child elements `cs:terms`, `cs:date` and
`cs:style-options` (these elements are described below). An example
showing part of a locale file:

``` {.xml}
<?xml version="1.0" encoding="UTF-8"?>
<locale xml:lang="en-US" version="1.0" xmlns="http://purl.org/net/xbiblio/csl">
  <style-options punctuation-in-quote="true"/>
  <date form="text">
    <date-part name="month" suffix=" "/>
    <date-part name="day" suffix=", "/>
    <date-part name="year"/>
  </date>
  <date form="numeric">
    <date-part name="year"/>
    <date-part name="month" form="numeric" prefix="-" range-delimiter="/"/>
    <date-part name="day" prefix="-" range-delimiter="/"/>
  </date>
  <terms>
    <term name="no date">n.d.</term>
    <term name="et-al">et al.</term>
    <term name="page">
      <single>page</single>
      <multiple>pages</multiple>
    </term>
    <term name="page" form="short">
      <single>p.</single>
      <multiple>pp.</multiple>
    </term>
  </terms>
</locale>
```

## Info

The `cs:info` element may be used to give metadata on the locale file.
It has the following child elements:

`cs:translator` (optional)

:   `cs:translator`, used to acknowledge locale translators, may be used
    multiple times. Within the element, the child element `cs:name` must
    appear once, while `cs:email` and `cs:uri` each may appear once.
    These child elements should contain respectively the name, email
    address and URI of the translator.

`cs:rights` (optional)

:   May appear once. The contents of `cs:rights` specifies the license
    under which the locale file is released. The element may carry a
    `license` attribute to specify the URI of the license, and a
    `xml:lang` attribute to specify the language of the element\'s
    content (the value must be an [xsd:language locale
    code](http://books.xmlschemata.org/relaxng/ch19-77191.html)).

`cs:updated` (optional)

:   May appear once. The contents of `cs:updated` must be a
    [timestamp](http://books.xmlschemata.org/relaxng/ch19-77049.html)
    that shows when the locale file was last updated.

## Terms

Terms are localized strings (e.g. by using the \"and\" term, \"Doe and
Smith\" automatically becomes \"Doe und Smith\" when the style locale is
switched from English to German). Terms are defined with `cs:term`
elements, child elements of `cs:terms`. Each `cs:term` element must
carry a `name` attribute, set to one of the terms listed in [Appendix II
- Terms](#appendix-ii---terms).

Terms are either directly defined in the content of `cs:term`, or, in
cases where singular and plural variants are needed (e.g. \"page\" and
\"pages\"), in the content of the child elements `cs:single` and
`cs:multiple`, respectively.

Terms may be defined for specific forms by using `cs:term` with the
optional `form` attribute set to:

-   \"long\" - (default), e.g. \"editor\" and \"editors\" for the
    \"editor\" term
-   \"short\" - e.g. \"ed.\" and \"eds.\" for the term \"editor\"
-   \"verb\" - e.g. \"edited by\" for the term \"editor\"
-   \"verb-short\" - e.g. \"ed.\" for the term \"editor\"
-   \"symbol\" - e.g. \"§\" and \"§§\" for the term \"section\"

If a style uses a term in a form that is undefined (even after [Locale
Fallback](#locale-fallback)), there is fallback to other forms:
\"verb-short\" first falls back to \"verb\", \"symbol\" first falls back
to \"short\", and \"verb\" and \"short\" both fall back to \"long\". If
no locale or form fallback is available, the term is rendered as an
empty string.

The `match`, `gender`, and `gender-form` attributes can be used on
`cs:term` for the formatting of number variables rendered as ordinals
(e.g. \"first\", \"2nd\"). See [Ordinal Suffixes](#ordinal-suffixes) and
[Gender-specific Ordinals](#gender-specific-ordinals) below.

Term content should not contain markup such as LaTeX or HTML.
[Superscripted Unicode
characters](http://unicode.org/reports/tr30/datafiles/SuperscriptFolding.txt)
can be used for superscripting.

### Ordinal Suffixes

Number variables can be rendered with `cs:number` in the \"ordinal\"
form, e.g. \"2nd\" (see [Number](#number)). The ordinal suffixes (\"nd\"
for \"2nd\") are defined with terms.

The \"ordinal\" term defines the default ordinal suffix. This default
suffix may be overridden for certain numbers with the following terms:

-   \"ordinal-00\" through \"ordinal-09\" - by default, a term in this
    group is used when the last digit in the term name matches the last
    digit of the rendered number. E.g. \"ordinal-00\" would match the
    numbers \"0\", \"10\", \"20\", etc. By setting the optional `match`
    attribute to \"last-two-digits\" (\"last-digit\" is the default),
    matches are limited to numbers where the two last digits agree
    (\"0\", \"100\", \"200\", etc.). When `match` is set to
    \"whole-number\", there is only a match if the number is the same as
    that of the term.
-   \"ordinal-10\" through \"ordinal-99\" - by default, a term in this
    group is used when the last two digits in the term name match the
    last two digits of the rendered number. When the optional `match`
    attribute is set to \"whole-number\" (\"last-two-digits\" is the
    default), there is only a match if the number is the same as that of
    the term.

When a number has matching terms from both groups (e.g. \"13\" can match
\"ordinal-03\" and \"ordinal-13\"), the term from the \"ordinal-10\"
through \"ordinal-99\" group is used.

Ordinal terms work differently in CSL 1.0.1 than they did in CSL 1.0.
When neither the style or locale file define the \"ordinal\" term, but
do define the terms \"ordinal-01\" through \"ordinal-04\", the original
CSL 1.0 scheme is used: \"ordinal-01\" is used for numbers ending on a 1
(except those ending on 11), \"ordinal-02\" for those ending on a 2
(except those ending on 12), \"ordinal-03\" for those ending on a 3
(except those ending on 13) and \"ordinal-04\" for all other numbers.

### Gender-specific Ordinals

Some languages use gender-specific ordinals. For example, the English
\"1st\" and \"first\" translate in French to \"1^er^\" and \"premier\"
if the target noun is masculine, and \"1^re^\" and \"première\" if the
noun is feminine.

Feminine and masculine variants of the ordinal terms (see
[Ordinals](#ordinals)) may be specified by setting the `gender-form`
attribute to \"feminine\" or \"masculine\" (the term without
`gender-form` represents the neuter variant). There are two types of
target nouns: a) the terms accompanying the [number
variables](#number-variables), and b) the month terms (see
[Months](#months)). The gender of these nouns may be specified on the
\"long\" (default) form of the term using the `gender` attribute (set to
\"feminine\" or \"masculine\"). When a number variable is rendered with
`cs:number` in the \"ordinal\" or \"long-ordinal\" form, the ordinal
term of the same gender is used, with a fallback to the neuter variant
if the feminine or masculine variant is undefined. When the \"day\"
date-part is rendered in the \"ordinal\" form, the ordinal gender is
matched against that of the month term.

The example below gives \"1re éd.\" (\"1st ed.\"), \"1er janvier\"
(\"January 1st\"), and \"3e édition\" (\"3rd edition\"):

``` {.xml}
<?xml version="1.0" encoding="UTF-8"?>
<locale xml:lang="fr-FR">
  <terms>
    <term name="edition" gender="feminine">
      <single>édition</single>
      <multiple>éditions</multiple>
    </term>
    <term name="edition" form="short">éd.</term>
    <term name="month-01" gender="masculine">janvier</term>
    <term name="ordinal">e</term>
    <term name="ordinal-01" gender-form="feminine" match="whole-number">re</term>
    <term name="ordinal-01" gender-form="masculine" match="whole-number">er</term>
  </terms>
</locale>
```

## Localized Date Formats

Two localized date formats can be defined with `cs:date` elements: a
\"numeric\" (e.g. \"12-15-2005\") and a \"text\" format (e.g. \"December
15, 2005\"). The format is set on `cs:date` with the required `form`
attribute.

A date format is constructed using `cs:date-part` child elements (see
[Date-part](#date-part)). With a required `name` attribute set to either
`day`, `month` or `year`, the order of these elements reflects the
display order of respectively the day, month, and year. The date can be
formatted with [formatting](#formatting) and [text-case](#text-case)
attributes on the `cs:date` and `cs:date-part` elements. The
[delimiter](#delimiter) attribute may be set on `cs:date` to specify the
delimiter for the `cs:date-part` elements, and [affixes](#affixes) may
be applied to the `cs:date-part` elements.

**Note** Affixes are not allowed on `cs:date` when defining localized
date formats. This restriction is in place to separate locale-specific
affixes (set on the `cs:date-part` elements) from any style-specific
affixes (set on the calling `cs:date` element), such as parentheses. An
example of a macro calling a localized date format:

``` {.xml}
<macro name="issued">
 <date variable="issued" form="numeric" prefix="(" suffix=")"/>
</macro>
```

## Localized Options

There are two localized options, `limit-day-ordinals-to-day-1` and
`punctuation-in-quote` (see [Locale Options](#locale-options)). These
global options (which affect both citations and the bibliography) are
set as optional attributes on `cs:style-options`.

