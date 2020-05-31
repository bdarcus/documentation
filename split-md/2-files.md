# Namespacing

The CSL [XML namespace URI](http://en.wikipedia.org/wiki/XML_Namespace)
is \"<http://purl.org/net/xbiblio/csl>\". The namespace prefix `cs:` is
used throughout this specification when referring to CSL elements, but
is generally omitted in favor of a default namespace declaration (set
with the `xmlns` attribute) on the root `cs:style` or `cs:locale`
element.

# File Types

There are three types of CSL files: independent and dependent styles
(both types use the ".csl" extension), and locale files (named
\"locales-xx-XX.xml\", where \"xx-XX\" is a language dialect, e.g.
\"en-US\" for American English).

## Independent Styles

Independent styles contain formatting instructions for citations, notes
and bibliographies. While mostly self-contained, they rely on locale
files for (default) localization data.

## Dependent Styles

A dependent style is an alias for an independent style. Its contents are
limited to style metadata, and doesn\'t include any formatting
instructions (the sole exception is that dependent styles can specify an
overriding style locale). By linking dependent styles for journals that
share the same citation style (e.g., \"Nature Biotechnology\", \"Nature
Nanotechnology\", etc.) to a single independent style (e.g., \"Nature
Journals\"), there is no need to duplicate formatting instructions.

## Locale Files

Each locale file contains a set of localization data (term translations,
localized date formats, and grammar options) for a particular language
dialect.

# XML Declaration

Each style or locale should begin with an XML declaration, specifying
the XML version and character encoding. In most cases, the declaration
will be:

``` {.xml}
<?xml version="1.0" encoding="UTF-8"?>
```

