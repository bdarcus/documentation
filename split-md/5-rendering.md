# Rendering Elements

Rendering elements specify which, and in what order, pieces of
bibliographic metadata are included in citations and bibliographies, and
offer control over their formatting.

## Layout

The `cs:layout` rendering element is a required child element of
`cs:citation` and `cs:bibliography`. It must contain one or more of the
other rendering elements described below, and may carry
[affixes](#affixes) and [formatting](#formatting) attributes. When used
within `cs:citation`, the [delimiter](#delimiter) attribute may be used
to specify a delimiter for cites within a citation. For example, a
citation like \"(1, 2)\" can be achieved with:

``` {.xml}
<citation>
  <layout prefix="(" suffix=")" delimiter=", ">
    <text variable="citation-number"/>
  </layout>
</citation>
```

## Text

The `cs:text` rendering element outputs text. It must carry one of the
following attributes to select what should be rendered:

-   `variable` - renders the text contents of a variable. Attribute
    value must be one of the [standard variables](#standard-variables).
    May be accompanied by the `form` attribute to select the \"long\"
    (default) or \"short\" form of a variable (e.g. the full or short
    title). If the \"short\" form is selected but unavailable, the
    \"long\" form is rendered instead.
-   `macro` - renders the text output of a macro. Attribute value must
    match the value of the `name` attribute of a `cs:macro` element (see
    [Macro](#macro)).
-   `term` - renders a term. Attribute value must be one of the terms
    listed in [Appendix II - Terms](#appendix-ii---terms). May be
    accompanied by the `plural` attribute to select the singular
    (\"false\", default) or plural (\"true\") variant of a term, and by
    the `form` attribute to select the \"long\" (default), \"short\",
    \"verb\", \"verb-short\" or \"symbol\" form variant (see also
    [Terms](#terms)).
-   `value` - renders the attribute value itself.

An example of `cs:text` rendering the \"title\" variable:

``` {.xml}
<text variable="title"/>
```

`cs:text` may also carry [affixes](#affixes), [display](#display),
[formatting](#formatting), [quotes](#quotes),
[strip-periods](#strip-periods) and [text-case](#text-case) attributes.

## Date

The `cs:date` rendering element outputs the date selected from the list
of [date variables](#date-variables) with the required `variable`
attribute. A date can be rendered in either a localized or non-localized
format.

[Localized date formats](#localized-date-formats) are selected with the
optional `form` attribute, which must be set to either \"numeric\" (for
fully numeric formats, e.g. \"12-15-2005\"), or \"text\" (for formats
with a non-numeric month, e.g. \"December 15, 2005\"). Localized date
formats can be customized in two ways. First, the `date-parts` attribute
may be used to show fewer date parts. The possible values are:

-   \"year-month-day\" - (default), renders the year, month and day
-   \"year-month\" - renders the year and month
-   \"year\" - renders the year

Secondly, `cs:date` may have one or more `cs:date-part` child elements
(see [Date-part](#date-part)). The attributes set on these elements
override those specified for the localized date formats (e.g. to get
abbreviated months for all locales, the `form` attribute on the
month-`cs:date-part` element can be set to \"short\"). These
`cs:date-part` elements do not affect which, or in what order, date
parts are rendered. [Affixes](#affixes), which are very locale-specific,
are not allowed on these `cs:date-part` elements.

In the absence of the `form` attribute, `cs:date` describes a
self-contained non-localized date format. In this case, the date format
is constructed using `cs:date-part` child elements. With a required
`name` attribute set to either `day`, `month` or `year`, the order of
these elements reflects the display order of respectively the day,
month, and year. The date can be formatted with
[formatting](#formatting) attributes on the `cs:date-part` elements, as
well as several `cs:date-part`-specific attributes (see
[Date-part](#date-part)). The [delimiter](#delimiter) attribute may be
set on `cs:date` to specify the delimiter for the `cs:date-part`
elements, and [affixes](#affixes) may be applied to the `cs:date-part`
elements.

For both localized and non-localized dates, `cs:date` may carry
[affixes](#affixes), [display](#display), [formatting](#formatting) and
[text-case](#text-case) attributes.

### Date-part

The `cs:date-part` elements control how date parts are rendered. Unless
the parent `cs:date` element calls a localized date format, they also
determine which, and in what order, date parts appear. A `cs:date-part`
element describes the date part selected with the required `name`
attribute:

\"day\"

:   For \"day\", `cs:date-part` may carry the `form` attribute, with
    values:

    -   \"numeric\" - (default), e.g. \"1\"
    -   \"numeric-leading-zeros\" - e.g. \"01\"
    -   \"ordinal\" - e.g. \"1st\"

    Some languages, such as French, only use the \"ordinal\" form for
    the first day of the month (\"1er janvier\", \"2 janvier\", \"3
    janvier\", etc.). Such output can be achieved with the \"ordinal\"
    form and use of the `limit-day-ordinals-to-day-1` attribute (see
    [Locale Options](#locale-options)).

\"month\"

:   For \"month\", `cs:date-part` may carry the
    [strip-periods](#strip-periods) and `form` attributes. In locale
    files, month abbreviations (the \"short\" form of the month
    [terms](#terms)) should be defined with periods if applicable (e.g.
    \"Jan.\", \"Feb.\", etc.). These periods can be removed by setting
    [strip-periods](#strip-periods) to \"true\" (\"false\" is the
    default). The `form` attribute can be set to:

    -   \"long\" - (default), e.g. \"January\"
    -   \"short\" - e.g. \"Jan.\"
    -   \"numeric\" - e.g. \"1\"
    -   \"numeric-leading-zeros\" - e.g. \"01\"

\"year\"

:   For \"year\", `cs:date-part` may carry the `form` attribute, with
    values:

    -   \"long\" - (default), e.g. \"2005\"
    -   \"short\" - e.g. \"05\"

`cs:date-part` may also carry [formatting](#formatting),
[text-case](#text-case) and `range-delimiter` (see [Date
Ranges](#date-ranges)) attributes. Attributes for [affixes](#affixes)
are allowed, unless `cs:date` calls a localized date format.

### Date Ranges

The default delimiter for dates in a date range is an en-dash (e.g.
\"May -- July 2008\"). Custom range delimiters can be set on
`cs:date-part` elements with the optional `range-delimiter` attribute.
When a date range is rendered, the range delimiter is drawn from the
`cs:date-part` element matching the largest date part (\"year\",
\"month\", or \"day\") that differs between the two dates. For example,

``` {.xml}
<style>
  <citation>
    <layout>
      <date variable="issued">
        <date-part name="day" suffix=" " range-delimiter="-"/>
        <date-part name="month" suffix=" "/>
        <date-part name="year" range-delimiter="/"/>
      </date>
    </layout>
  </citation>
</style>
```

would result in \"1-4 May 2008\", \"May -- July 2008\" and \"May
2008/June 2009\".

### AD and BC

The \"ad\" term (Anno Domini) is automatically appended to positive
years of less than four digits (e.g. \"79\" becomes \"79AD\"). The
\"bc\" term (Before Christ) is automatically appended to negative years
(e.g. \"-2500\" becomes \"2500BC\").

### Seasons

If a date includes a season instead of a month, a season term
(\"season-01\" to \"season-04\", respectively Spring, Summer, Autumn and
Winter) take the place of the month term. E.g.,

``` {.xml}
<style>
  <citation>
    <layout>
      <date variable="issued">
        <date-part name="month" suffix=" "/>
        <date-part name="year"/>
      </date>
    </layout>
  </citation>
</style>
```

would result in \"May 2008\" and \"Winter 2009\".

### Approximate Dates

Approximate dates test \"true\" for the `is-uncertain-date` conditional
(see [Choose](#choose)). For example,

``` {.xml}
<style>
  <citation>
    <layout>
      <choose>
        <if is-uncertain-date="issued">
          <text term="circa" form="short" suffix=" "/>
        </if>
      </choose>
      <date variable="issued">
        <date-part name="year"/>
      </date>
    </layout>
  </citation>
</style>
```

would result in \"2005\" (normal date) and \"ca. 2003\" (approximate
date).

## Number

The `cs:number` rendering element outputs the number variable selected
with the required `variable` attribute. [Number
variables](#number-variables) are a subset of the list of [standard
variables](#standard-variables).

If a number variable is rendered with `cs:number` and only contains
numeric content (as determined by the rules for `is-numeric`, see
[Choose](#choose)), the number(s) are extracted. Variable content is
rendered \"as is\" when the variable contains any non-numeric content
(e.g. \"Special edition\").

During the extraction, numbers separated by a hyphen are stripped of
intervening spaces (\"2 - 4\" becomes \"2-4\"). Numbers separated by a
comma receive one space after the comma (\"2,3\" and \"2 , 3\" become
\"2, 3\"), while numbers separated by an ampersand receive one space
before and one after the ampersand (\"2&3\" becomes \"2 & 3\").

Extracted numbers can be formatted via the optional `form` attribute,
with values:

-   \"numeric\" - (default), e.g. \"1\", \"2\", \"3\"
-   \"ordinal\" - e.g. \"1st\", \"2nd\", \"3rd\". Ordinal suffixes are
    defined with terms (see [Ordinal Suffixes](#ordinal-suffixes)).
-   \"long-ordinal\" - e.g. \"first\", \"second\", \"third\". Long
    ordinals are defined with the [terms](#terms) \"long-ordinal-01\" to
    \"long-ordinal-10\", which are used for the numbers 1 through 10.
    For other numbers \"long-ordinal\" falls back to \"ordinal\".
-   \"roman\" - e.g. \"i\", \"ii\", \"iii\"

Numbers with prefixes or suffixes are never ordinalized or rendered in
roman numerals (e.g. \"2E\" remains \"2E). Numbers without affixes are
individually transformed (\"2, 3\" can become \"2nd, 3rd\", \"second,
third\" or \"ii, iii\").

`cs:number` may carry [affixes](#affixes), [display](#display),
[formatting](#formatting) and [text-case](#text-case) attributes.

## Names

The `cs:names` rendering element outputs the contents of one or more
[name variables](#name-variables) (selected with the required `variable`
attribute), each of which can contain multiple names (e.g. the
\"author\" variable contains all the author names of the cited item). If
multiple variables are selected (separated by single spaces, see example
below), each variable is independently rendered in the order specified,
with one exception: when the selection consists of \"editor\" and
\"translator\", and when the contents of these two name variables is
identical, then the contents of only one name variable is rendered. In
addition, the \"editortranslator\" term is used if the `cs:names`
element contains a `cs:label` element, replacing the default \"editor\"
and \"translator\" terms (e.g. resulting in \"Doe (editor &
translator)\"). The [delimiter](#delimiter) attribute may be set on
`cs:names` to separate the names of the different name variables (e.g.
the semicolon in \"Doe, Smith (editors); Johnson (translator)\").

``` {.xml}
<names variable="editor translator" delimiter="; ">
  <label prefix=" (" suffix=")"/>
</names>
```

`cs:names` has four child elements (discussed below): `cs:name`,
`cs:et-al`, `cs:substitute` and `cs:label`. The `cs:names` element may
carry [affixes](#affixes), [display](#display) and
[formatting](#formatting) attributes.

### Name

The `cs:name` element, an optional child element of `cs:names`, can be
used to describe the formatting of individual names, and the separation
of names within a name variable. `cs:name` may carry the following
attributes:

`and`

:   Specifies the delimiter between the second to last and last name of
    the names in a name variable. Allowed values are \"text\" (selects
    the \"and\" term, e.g. \"Doe, Johnson and Smith\") and \"symbol\"
    (selects the ampersand, e.g. \"Doe, Johnson & Smith\").

`delimiter`

:   Specifies the text string used to separate names in a name variable.
    Default is ", " (e.g. \"Doe, Smith\").

`delimiter-precedes-et-al`

:   Determines when the name delimiter or a space is used between a
    truncated name list and the \"et-al\" (or \"and others\") term in
    case of et-al abbreviation. Allowed values:

    -   \"contextual\" - (default), name delimiter is only used for name
        lists truncated to two or more names
        -   1 name: \"J. Doe et al.\"
        -   2 names: \"J. Doe, S. Smith, et al.\"
    -   \"after-inverted-name\" - name delimiter is only used if the
        preceding name is inverted as a result of the
        `name-as-sort-order` attribute. E.g. with `name-as-sort-order`
        set to \"first\":
        -   \"Doe, J., et al.\"
        -   \"Doe, J., S. Smith et al.\"
    -   \"always\" - name delimiter is always used
        -   1 name: \"J. Doe, et al.\"
        -   2 names: \"J. Doe, S. Smith, et al.\"
    -   \"never\" - name delimiter is never used
        -   1 name: \"J. Doe et al.\"
        -   2 names: \"J. Doe, S. Smith et al.\"

`delimiter-precedes-last`

:   Determines when the name delimiter is used to separate the second to
    last and the last name in name lists (if `and` is not set, the name
    delimiter is always used, regardless of the value of
    `delimiter-precedes-last`). Allowed values:

    -   \"contextual\" - (default), name delimiter is only used for name
        lists with three or more names
        -   2 names: \"J. Doe and T. Williams\"
        -   3 names: \"J. Doe, S. Smith, and T. Williams\"
    -   \"after-inverted-name\" - name delimiter is only used if the
        preceding name is inverted as a result of the
        `name-as-sort-order` attribute. E.g. with `name-as-sort-order`
        set to \"first\":
        -   \"Doe, J., and T. Williams\"
        -   \"Doe, J., S. Smith and T. Williams\"
    -   \"always\" - name delimiter is always used
        -   2 names: \"J. Doe, and T. Williams\"
        -   3 names: \"J. Doe, S. Smith, and T. Williams\"
    -   \"never\" - name delimiter is never used
        -   2 names: \"J. Doe and T. Williams\"
        -   3 names: \"J. Doe, S. Smith and T. Williams\"

`et-al-min` / `et-al-use-first`

:   Use of these two attributes enables et-al abbreviation. If the
    number of names in a name variable matches or exceeds the number set
    on `et-al-min`, the rendered name list is truncated after reaching
    the number of names set on `et-al-use-first`. The \"et-al\" (or
    \"and others\") term is appended to truncated name lists (see also
    [Et-al](#et-al)). By default, when a name list is truncated to a
    single name, the name and the \"et-al\" (or \"and others\") term are
    separated by a space (e.g. \"Doe et al.\"). When a name list is
    truncated to two or more names, the name delimiter is used (e.g.
    \"Doe, Smith, et al.\"). This behavior can be changed with the
    `delimiter-precedes-et-al` attribute.

`et-al-subsequent-min` / `et-al-subsequent-use-first`

:   If used, the values of these attributes replace those of
    respectively `et-al-min` and `et-al-use-first` for subsequent cites
    (cites referencing earlier cited items).

`et-al-use-last`

:   When set to \"true\" (the default is \"false\"), name lists
    truncated by et-al abbreviation are followed by the name delimiter,
    the ellipsis character, and the last name of the original name list.
    This is only possible when the original name list has at least two
    more names than the truncated name list (for this the value of
    `et-al-use-first`/`et-al-subsequent-min` must be at least 2 less
    than the value of `et-al-min`/`et-al-subsequent-use-first`). An
    example:

        A. Goffeau, B. G. Barrell, H. Bussey, R. W. Davis, B. Dujon, H.
        Feldmann, … S. G. Oliver

The remaining attributes, discussed below, only affect personal names.
Personal names require a \"family\" name-part, and may also contain
\"given\", \"suffix\", \"non-dropping-particle\" and
\"dropping-particle\" name-parts. These name-parts are defined as:

-   \"family\" - surname minus any particles and suffixes
-   \"given\" - given names, either full (\"John Edward\") or
    initialized (\"J. E.\")
-   \"suffix\" - name suffix, e.g. \"Jr.\" in \"John Smith Jr.\" and
    \"III\" in \"Bill Gates III\"
-   \"non-dropping-particle\" - name particles that are not dropped when
    only the surname is shown (\"de\" in the Dutch surname \"de
    Koning\") but which may be treated separately from the family name,
    e.g. for sorting
-   \"dropping-particle\" - name particles that are dropped when only
    the surname is shown (\"van\" in \"Ludwig van Beethoven\", which
    becomes \"Beethoven\")

The attributes affecting personal names:

`form`

:   Specifies whether all the name-parts of personal names should be
    displayed (value \"long\", the default), or only the family name and
    the non-dropping-particle (value \"short\"). A third value,
    \"count\", returns the total number of names that would otherwise be
    rendered by the use of the `cs:names` element (taking into account
    the effects of et-al abbreviation and editor/translator collapsing),
    which allows for advanced [sorting](#sorting).

`initialize`

:   When set to \"false\" (the default is \"true\"), given names are no
    longer initialized when \"initialize-with\" is set. However, the
    value of \"initialize-with\" is still added after initials present
    in the full name (e.g. with `initialize` set to \"false\", and
    `initialize-with` set to ".", \"James T Kirk\" becomes \"James T.
    Kirk\").

`initialize-with`

:   When set, given names are converted to initials. The attribute value
    is added after each initial ("." results in \"J.J. Doe\"). For
    compound given names (e.g. \"Jean-Luc\"), hyphenation of the
    initials can be controlled with the global `initialize-with-hyphen`
    option (see [Hyphenation of Initialized
    Names](#hyphenation-of-initialized-names)).

`name-as-sort-order`

:   Specifies that names should be displayed with the given name
    following the family name (e.g. \"John Doe\" becomes \"Doe, John\").
    The attribute has two possible values:

    -   \"first\" - attribute only has an effect on the first name of
        each name variable
    -   \"all\" - attribute has an effect on all names

    Note that even when `name-as-sort-order` changes the name-part
    order, the display order is not necessarily the same as the sorting
    order for names containing particles and suffixes (see [Name-part
    order](#name-part-order)). Also, `name-as-sort-order` only affects
    names written in scripts where the given name typically precedes the
    family name, such as Latin, Greek, Cyrillic and Arabic. In contrast,
    names written in Asian scripts are always displayed with the family
    name preceding the given name.

`sort-separator`

:   Sets the delimiter for name-parts that have switched positions as a
    result of `name-as-sort-order`. The default value is ", " (\"Doe,
    John\"). As is the case for `name-as-sort-order`, this attribute
    only affects names in scripts that know \"given-name family-name\"
    order.

`cs:name` may also carry [affixes](#affixes) and
[formatting](#formatting) attributes.

#### Name-part Order

The order of name-parts depends on the values of the `form` and
`name-as-sort-order` attributes on `cs:name`, the value of the
`demote-non-dropping-particle` attribute on `cs:style` (one of the
[global options](#global-options)), and the script of the individual
name. Note that the display and sorting order of name-parts often
differs. An overview of the possible orders:

**Display order of names in \"given-name family-name\" scripts (Latin,
etc.)**

------------------------------------------------------------------------

Conditions

:   `form` set to \"long\"

Order

:   1)  given
    2)  dropping-particle
    3)  non-dropping-particle
    4)  family
    5)  suffix

Example

:   \[Jean\] \[de\] \[La\] \[Fontaine\] \[III\]

------------------------------------------------------------------------

Conditions

:   `form` set to \"long\", name-as-sort-order active,
    `demote-non-dropping-particle` set to \"never\" or \"sort-only\"

Order

:   1)  non-dropping-particle
    2)  family
    3)  given
    4)  dropping-particle
    5)  suffix

Example

:   \[La\] \[Fontaine\], \[Jean\] \[de\], \[III\]

------------------------------------------------------------------------

Conditions

:   `form` set to \"long\", name-as-sort-order active,
    `demote-non-dropping-particle` set to \"display-and-sort\"

Order

:   1)  family
    2)  given
    3)  dropping-particle
    4)  non-dropping-particle
    5)  suffix

Example

:   \[Fontaine\], \[Jean\] \[de\] \[La\], \[III\]

------------------------------------------------------------------------

Conditions

:   `form` set to \"short\"

Order

:   1)  non-dropping-particles
    2)  family

Example

:   \[La\] \[Fontaine\]

------------------------------------------------------------------------

**Sorting order of names in \"given-name family-name\" scripts (Latin,
etc.)**

N.B. The sort keys are listed in descending order of priority.

------------------------------------------------------------------------

Conditions

:   `demote-non-dropping-particle` set to \"never\"

Order

:   1)  non-dropping-particle + family
    2)  dropping-particle
    3)  given
    4)  suffix

Example

:   \[La Fontaine\] \[de\] \[Jean\] \[III\]

------------------------------------------------------------------------

Conditions

:   `demote-non-dropping-particle` set to \"sort-only\" or
    \"display-and-sort\"

Order

:   1)  family
    2)  dropping-particle + non-dropping-particle
    3)  given
    4)  suffix

Example

:   \[Fontaine\] \[de La\] \[Jean\] \[III\]

------------------------------------------------------------------------

**Display and sorting order of names in \"family-name given-name\"
scripts (Chinese, etc.)**

------------------------------------------------------------------------

Conditions

:   `form` set to \"long\"

Order

:   1)  family
    2)  given

Example

:   毛 泽 东 \[Mao Zedong\]

------------------------------------------------------------------------

Conditions

:   `form` set to \"short\"

Order

:   1)  family

Example

:   毛 \[Mao\]

------------------------------------------------------------------------

Non-personal names lack name-parts and are sorted as is, although
English articles (\"a\", \"an\" and \"the\") at the start of the name
are stripped. For example, \"The New York Times\" sorts as \"New York
Times\".

#### Name-part Formatting

The `cs:name` element may contain one or two `cs:name-part` child
elements for name-part-specific formatting. `cs:name-part` must carry
the `name` attribute, set to either \"given\" or \"family\".

If set to \"given\", [formatting](#formatting) and
[text-case](#text-case) attributes on `cs:name-part` affect the
\"given\" and \"dropping-particle\" name-parts. [affixes](#affixes)
surround the \"given\" name-part, enclosing any demoted name particles
for inverted names.

If set to \"family\", [formatting](#formatting) and
[text-case](#text-case) attributes affect the \"family\" and
\"non-dropping-particle\" name-parts. [affixes](#affixes) surround the
\"family\" name-part, enclosing any preceding name particles, as well as
the \"suffix\" name-part for non-inverted names.

The \"suffix\" name-part is not subject to name-part formatting. The use
of `cs:name-part` elements does not influence which, or in what order,
name-parts are rendered. An example, yielding names like \"Jane DOE\":

``` {.xml}
<names variable="author">
  <name>
    <name-part name="family" text-case="uppercase"/>
  </name>
</names>
```

### Et-al

Et-al abbreviation, controlled via the `et-al-…` attributes (see
[Name](#name)), can be further customized with the optional `cs:et-al`
element, which must follow the `cs:name` element (if present). The
`term` attribute may be set to either \"et-al\" (the default) or to
\"and others\" to use either term. The [formatting](#formatting)
attributes may also be used, for example to italicize the \"et-al\"
term:

``` {.xml}
<names variable="author">
  <et-al term="and others" font-style="italic"/>
</names>
```

### Substitute

The optional `cs:substitute` element, which must be included as the last
child element of `cs:names`, adds substitution in case the [name
variables](#name-variables) specified in the parent `cs:names` element
are empty. The substitutions are specified as child elements of
`cs:substitute`, and must consist of one or more [rendering
elements](#rendering-elements) (with the exception of `cs:layout`). A
shorthand version of `cs:names` without child elements, which inherits
the attributes values set on the `cs:name` and `cs:et-al` child elements
of the original `cs:names` element, may also be used. If `cs:substitute`
contains multiple child elements, the first element to return a
non-empty result is used for substitution. Substituted variables are
suppressed in the rest of the output to prevent duplication. An example,
where an empty \"author\" name variable is substituted by the \"editor\"
name variable, or, when no editors exist, by the \"title\" macro:

``` {.xml}
<macro name="author">
  <names variable="author">
    <substitute>
      <names variable="editor"/>
      <text macro="title"/>
    </substitute>
  </names>
</macro>
```

### Label in `cs:names`

The optional `cs:label` element (see [label](#label)) must be included
after the `cs:name` and `cs:et-al` elements, but before the
`cs:substitute` element. When used as a child element of `cs:names`,
`cs:label` does not carry the `variable` attribute; it uses the
variable(s) set on the parent `cs:names` element instead. A second
difference is that the `form` attribute may also be set to \"verb\" or
\"verb-short\", so that the allowed values are:

-   \"long\" - (default), e.g. \"editor\" and \"editors\" for the
    \"editor\" term
-   \"short\" - e.g. \"ed.\" and \"eds.\" for the term \"editor\"
-   \"verb\" - e.g. \"edited by\" for the term \"editor\"
-   \"verb-short\" - e.g. \"ed.\" for the term \"editor\"
-   \"symbol\" - e.g. \"§\" and \"§§\" for the term \"section\"

## Label

The `cs:label` rendering element outputs the term matching the variable
selected with the required `variable` attribute, which must be set to
\"locator\", \"page\", or one of the [number
variables](#number-variables). The term is only rendered if the selected
variable is non-empty. For example,

``` {.xml}
<group delimiter=" ">
  <label variable="page"/>
  <text variable="page"/>
</group>
```

can result in \"page 3\" or \"pages 5-7\". `cs:label` may carry the
following attributes:

`form`

:   Selects the form of the term, with allowed values:

    -   \"long\" - (default), e.g. \"page\"/\"pages\" for the \"page\"
        term
    -   \"short\" - e.g. \"p.\"/\"pp.\" for the \"page\" term
    -   \"symbol\" - e.g. \"§\"/\"§§\" for the \"section\" term

`plural`

:   Sets pluralization of the term, with allowed values:

    -   \"contextual\" - (default), the term plurality matches that of
        the variable content. Content is considered plural when it
        contains multiple numbers (e.g. \"page 1\", \"pages 1-3\",
        \"volume 2\", \"volumes 2 & 4\"), or, in the case of the
        \"number-of-pages\" and \"number-of-volumes\" variables, when
        the number is higher than 1 (\"1 volume\" and \"3 volumes\").
    -   \"always\" - always use the plural form, e.g. \"pages 1\" and
        \"pages 1-3\"
    -   \"never\" - always use the singular form, e.g. \"page 1\" and
        \"page 1-3\"

`cs:label` may also carry [affixes](#affixes),
[formatting](#formatting), [text-case](#text-case) and
[strip-periods](#strip-periods) attributes.

## Group

The `cs:group` rendering element must contain one or more [rendering
elements](#rendering-elements) (with the exception of `cs:layout`).
`cs:group` may carry the [delimiter](#delimiter) attribute to separate
its child elements, as well as [affixes](#affixes) and
[display](#display) attributes (applied to the output of the group as a
whole) and [formatting](#formatting) attributes (transmitted to the
enclosed elements). `cs:group` implicitly acts as a conditional:
`cs:group` and its child elements are suppressed if a) at least one
rendering element in `cs:group` calls a variable (either directly or via
a macro), and b) all variables that are called are empty. This
accommodates descriptive `cs:text` elements. For example,

``` {.xml}
<layout>
  <group delimiter=" ">
    <text term="retrieved"/>
    <text term="from"/>
    <text variable="URL"/>
  </group>
</layout>
```

can result in \"retrieved from
<http://dx.doi.org/10.1128/AEM.02591-07>\", but doesn\'t generate output
when the \"URL\" variable is empty.

## Choose

The `cs:choose` rendering element allows for conditional rendering of
[rendering elements](#rendering-elements). An example that renders the
\"issued\" date variable when it exists, and the \"no date\" term when
it doesn\'t:

``` {.xml}
<choose>
  <if variable="issued">
    <date variable="issued" form="numeric"/>
  </if>
  <else>
    <text term="no date"/>
  </else>
</choose>
```

`cs:choose` requires a `cs:if` child element, which may be followed by
one or more `cs:else-if` child elements, and an optional closing
`cs:else` child element. The `cs:if` and `cs:else-if` elements may
contain any number of [rendering elements](#rendering-elements) (except
for `cs:layout`). As an empty `cs:else` element would be superfluous,
`cs:else` must contain at least one rendering element. `cs:if` and
`cs:else-if` elements must carry one or more conditions, which are set
with the attributes:

`disambiguate`

:   When set to \"true\" (the only allowed value), the element content
    is only rendered if it disambiguates two otherwise identical
    citations. This attempt at [disambiguation](#disambiguation) is only
    made when all other disambiguation methods have failed to uniquely
    identify the target source.

`is-numeric`

:   Tests whether the given variables ([Appendix IV -
    Variables](#appendix-iv---variables)) contain numeric content.
    Content is considered numeric if it solely consists of numbers.
    Numbers may have prefixes and suffixes (\"D2\", \"2b\", \"L2d\"),
    and may be separated by a comma, hyphen, or ampersand, with or
    without spaces (\"2, 3\", \"2-4\", \"2 & 4\"). For example, \"2nd\"
    tests \"true\" whereas \"second\" and \"2nd edition\" test
    \"false\".

`is-uncertain-date`

:   Tests whether the given [date variables](#date-variables) contain
    [approximate dates](#approximate-dates).

`locator`

:   Tests whether the locator matches the given locator types (see
    [Locators](#locators)). Use \"sub-verbo\" to test for the \"sub
    verbo\" locator type.

`position`

:   Tests whether the cite position matches the given positions
    (terminology: citations consist of one or more cites to individual
    items). When called within the scope of cs:bibliography, `position`
    tests \"false\". The positions that can be tested are:

    -   \"first\": position of cites that are the first to reference an
        item

    -   \"ibid\"/\"ibid-with-locator\"/\"subsequent\": cites referencing
        previously cited items have the \"subsequent\" position. Such
        cites may also have the \"ibid\" or \"ibid-with-locator\"
        position when:

        a)  the current cite immediately follows on another cite, within
            the same citation, that references the same item

        or

        b)  the current cite is the first cite in the citation, and the
            previous citation consists of a single cite referencing the
            same item

        If either requirement is met, the presence of locators
        determines which position is assigned:

        -   **Preceding cite does not have a locator**: if the current
            cite has a locator, the position of the current cite is
            \"ibid-with-locator\". Otherwise the position is \"ibid\".
        -   **Preceding cite does have a locator**: if the current cite
            has the same locator, the position of the current cite is
            \"ibid\". If the locator differs the position is
            \"ibid-with-locator\". If the current cite lacks a locator
            its only position is \"subsequent\".

    -   \"near-note\": position of a cite following another cite
        referencing the same item. Both cites have to be located in foot
        or endnotes, and the distance between both cites may not exceed
        the maximum distance (measured in number of foot or endnotes)
        set with the `near-note-distance` option (see [Note
        Distance](#note-distance)).

    Whenever position=\"ibid-with-locator\" tests true,
    position=\"ibid\" also tests true. And whenever position=\"ibid\" or
    position=\"near-note\" test true, position=\"subsequent\" also tests
    true.

`type`

:   Tests whether the item matches the given types ([Appendix III -
    Types](#appendix-iii---types)).

`variable`

:   Tests whether the default (long) forms of the given variables
    ([Appendix IV - Variables](#appendix-iv---variables)) contain
    non-empty values.

With the exception of `disambiguate`, all conditions allow for multiple
test values (separated with spaces, e.g. \"book thesis\").

The `cs:if` and `cs:else-if` elements may carry the `match` attribute to
control the testing logic, with allowed values:

-   \"all\" - (default), element only tests \"true\" when all conditions
    test \"true\" for all given test values
-   \"any\" - element tests \"true\" when any condition tests \"true\"
    for any given test value
-   \"none\" - element only tests \"true\" when none of the conditions
    test \"true\" for any given test value

