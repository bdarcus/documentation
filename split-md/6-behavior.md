# Style Behavior

## Options

Styles may be configured with [citation-specific
options](#citation-specific-options), set as attributes on set on
`cs:citation`, [bibliography-specific
options](#bibliography-specific-options), set on `cs:bibliography`, and
[global options](#global-options) (these affect both citations and the
bibliography), set on `cs:style`. [Inheritable name
options](#inheritable-name-options) may be set on `cs:style`,
`cs:citation` and `cs:bibliography`. Finally, [locale
options](#locale-options) may be set on `cs:locale` elements.

### Citation-specific Options

#### Disambiguation

A cite is ambiguous when it matches multiple bibliographic entries[^1].
There are four methods available to eliminate such ambiguity:

1.  Show more names
2.  Expand names (adding initials or full given names)
3.  Add a year-suffix
4.  Render the cite with the `disambiguate` attribute of `cs:choose`
    conditions testing \"true\"

Method 2 can also be used for global *name disambiguation*, covering all
cites, ambiguous and unambiguous, throughout the document.

Disambiguation methods are activated with the following optional
attributes, and are always tried in the listed order:

`disambiguate-add-names` \[Step (1)\]

:   If set to \"true\" (\"false\" is the default), names that would
    otherwise be hidden as a result of et-al abbreviation are added one
    by one to all members of a set of ambiguous cites, until no more
    cites in the set can be disambiguated by adding names.

`disambiguate-add-givenname` \[Step (2)\]

:   If set to \"true\" (\"false\" is the default), ambiguous names
    (names that are identical in their \"short\" or initialized \"long\"
    form, but differ when initials are added or the full given name is
    shown) are expanded. Name expansion can be configured with
    `givenname-disambiguation-rule`. An example of cite disambiguation:

      Original ambiguous cites       Disambiguated cites
      ------------------------------ ------------------------------------
      (Simpson 2005; Simpson 2005)   (H. Simpson 2005; B. Simpson 2005)
      (Doe 1950; Doe 1950)           (John Doe 1950; Jane Doe 1950)

    If cites cannot be (fully) disambiguated by expanding the rendered
    names, and if `disambiguate-add-names` is set to \"true\", then the
    names still hidden as a result of et-al abbreviation after the
    disambiguation attempt of `disambiguate-add-names` are added one by
    one to all members of a set of ambiguous cites, until no more cites
    in the set can be disambiguated by adding expanded names.

`givenname-disambiguation-rule`

:   Specifies a) whether the purpose of name expansion is limited to
    disambiguating cites, or has the additional goal of disambiguating
    names (only in the latter case are ambiguous names in unambiguous
    cites expanded, e.g. from \"(Doe 1950; Doe 2000)\" to \"(Jane Doe
    1950; John Doe 2000)\"), b) whether name expansion targets all, or
    just the first name of each cite, and c) the method by which each
    name is expanded.

    **Expansion of Individual Names**

    :   The steps for expanding individual names are:

        1.  If `initialize-with` is set and `initialize` has its default
            value of \"true\", then:

            \(a\) Initials can be shown by rendering the name with a
            `form` value of \"long\" instead of \"short\" (e.g. \"Doe\"
            becomes \"J. Doe\").

            \(b\) Full given names can be shown instead of initials by
            rendering the name with `initialize` set to \"false\" (e.g.
            \"J. Doe\" becomes \"John Doe\").

        2.  If `initialize-with` is *not* set, full given names can be
            shown by rendering the name with a `form` value of \"long\"
            instead of \"short\" (e.g. \"Doe\" becomes \"John Doe\").

    **Given Name Disambiguation Rules**

    :   Allowed values of `givenname-disambiguation-rule`:

        \"all-names\"

        :   Name expansion has the dual purpose of disambiguating cites
            and names. All rendered ambiguous names, in both ambiguous
            and unambiguous cites, are subject to disambiguation. Each
            name is progressively transformed until it is disambiguated.
            Names that cannot be disambiguated remain in their original
            form.

        \"all-names-with-initials\"

        :   As \"all-names\", but name expansion is limited to showing
            initials (see step 1(a) above). No disambiguation attempt is
            made when `initialize-with` is not set or when `initialize`
            is set to \"false\".

        \"primary-name\"

        :   As \"all-names\", but disambiguation is limited to the first
            name of each cite.

        \"primary-name-with-initials\"

        :   As \"all-names-with-initials\", but disambiguation is
            limited to the first name of each cite.

        \"by-cite\"

        :   Default. As \"all-names\", but the goal of name expansion is
            limited to disambiguating cites. Only ambiguous names in
            ambiguous cites are affected, and disambiguation stops after
            the first name that eliminates cite ambiguity.

`disambiguate-add-year-suffix` \[Step (3)\]

:   If set to \"true\" (\"false\" is the default), an alphabetic
    year-suffix is added to ambiguous cites (e.g. \"Doe 2007, Doe 2007\"
    becomes \"Doe 2007a, Doe 2007b\") and to their corresponding
    bibliographic entries. The assignment of the year-suffixes follows
    the order of the bibliographies entries, and additional letters are
    used once \"z\" is reached (\"z\", \"aa\", \"ab\", ..., \"az\",
    \"ba\", etc.). By default the year-suffix is appended to the cite,
    and to the first year rendered through `cs:date` in the
    bibliographic entry, but its location can be controlled by
    explicitly rendering the \"year-suffix\" variable using `cs:text`.
    If \"year-suffix\" is rendered through `cs:text` in the scope of
    `cs:citation`, it is suppressed for `cs:bibliography`, unless it is
    also rendered through `cs:text` in the scope of `cs:bibliography`,
    and vice versa.

If ambiguous cites remain after applying the selected disambiguation
methods described above, a final disambiguation attempt is made by
rendering these cites with the `disambiguate` condition testing \"true\"
\[Step (4)\] (see [Choose](#choose)).

#### Cite Grouping

With cite grouping, cites in in-text citations with identical rendered
names are grouped together, e.g. the year-sorted \"(Doe 1999; Smith
2002; Doe 2006; Doe et al. 2007)\" becomes \"(Doe 1999; Doe 2006; Smith
2002; Doe et al. 2007)\". The comparison is limited to the output of the
(first) `cs:names` element, but includes output rendered through
`cs:substitute`. Cite grouping takes places after cite sorting and
disambiguation. Grouped cites maintain their relative order, and are
moved to the original location of the first cite of the group.

Cite grouping can be activated by setting the `cite-group-delimiter`
attribute or the `collapse` attributes on `cs:citation` (see also [Cite
Collapsing](#cite-collapsing)).

`cite-group-delimiter`

:   Activates cite grouping and specifies the delimiter for cites within
    a cite group. Defaults to ", ". E.g. with `delimiter` on `cs:layout`
    in `cs:citation` set to "; ", `collapse` set to \"year\", and
    `cite-group-delimiter` set to ",", citations look like \"(Doe
    1999,2001; Jones 2000)\".

#### Cite Collapsing

Cite groups (author and author-date styles), and numeric cite ranges
(numeric styles) can be collapsed through the use of the `collapse`
attribute. Delimiters for collapsed cite groups can be customized with
the `year-suffix-delimiter` and `after-collapse-delimiter` attributes:

`collapse`

:   Activates cite grouping and collapsing. Allowed values:

    -   \"citation-number\" - collapses ranges of cite numbers (rendered
        through the \"citation-number\" variable) in citations for
        \"numeric\" styles (e.g. from \"\[1, 2, 3, 5\]\" to \"\[1 -- 3,
        5\]\"). Only increasing ranges collapse, e.g. \"\[3, 2, 1\]\"
        will not collapse (to see how to sort cites by
        \"citation-number\", see [Sorting](#sorting)).
    -   \"year\" - collapses cite groups by suppressing the output of
        the `cs:names` element for subsequent cites in the group, e.g.
        \"(Doe 2000, Doe 2001)\" becomes \"(Doe 2000, 2001)\".
    -   \"year-suffix\" - collapses as \"year\", but also suppresses
        repeating years within the cite group, e.g. \"(Doe 2000a, b)\"
        instead of \"(Doe 2000a, 2000b)\".
    -   \"year-suffix-ranged\" - collapses as \"year-suffix\", but also
        collapses ranges of year-suffixes, e.g. \"(Doe 2000a -- c,e)\"
        instead of \"(Doe 2000a, b, c, e)\".

    \"year-suffix\" and \"year-suffix-ranged\" fall back to \"year\"
    when `disambiguate-add-year-suffix` is \"false\" (see
    [Disambiguation](#disambiguation)), or when a cite has a locator
    (e.g. \"(Doe 2000a-c, 2000d, p. 5, 2000e,f)\", where the cite for
    \"Doe 2000d\" has a locator that prevents the cite from further
    collapsing).

`year-suffix-delimiter`

:   Specifies the delimiter for year-suffixes. Defaults to the delimiter
    set on `cs:layout` in `cs:citation`. E.g. with `collapse` set to
    \"year-suffix\", `delimiter` on `cs:layout` in `cs:citation` set to
    "; ", and `year-suffix-delimiter` set to ",", citations look like
    \"(Doe 1999a,b; Jones 2000)\".

`after-collapse-delimiter`

:   Specifies the cite delimiter to be used *after* a collapsed cite
    group. Defaults to the delimiter set on `cs:layout` in
    `cs:citation`. E.g. with `collapse` set to \"year\", `delimiter` on
    `cs:layout` in `cs:citation` set to ", ", and
    `after-collapse-delimiter` set to "; ", citations look like \"(Doe
    1999, 2001; Jones 2000, Brown 2001)\".

#### Note Distance

`near-note-distance`

:   A cite tests true for the \"near-note\" position (see
    [Choose](#choose)) when a preceding note exists that a) refers to
    the same item and b) does not precede the current note by more
    footnotes or endnotes than the value of `near-note-distance`
    (default value is \"5\").

### Bibliography-specific Options

#### Whitespace

`hanging-indent`

:   If set to \"true\" (\"false\" is the default), bibliographic entries
    are rendered with hanging-indents.

`second-field-align`

:   If set, subsequent lines of bibliographic entries are aligned along
    the second field. With \"flush\", the first field is flush with the
    margin. With \"margin\", the first field is put in the margin, and
    subsequent lines are aligned with the margin. An example, where the
    first field is `<text variable="citation-number" suffix=". "/>`:

        9.  Adams, D. (2002). The Ultimate Hitchhiker's Guide to the
            Galaxy (1st ed.).
        10. Asimov, I. (1951). Foundation.

`line-spacing`

:   Specifies vertical line distance. Defaults to \"1\"
    (single-spacing), and can be set to any positive integer to specify
    a multiple of the standard unit of line height (e.g. \"2\" for
    double-spacing).

`entry-spacing`

:   Specifies vertical distance between bibliographic entries. By
    default (with a value of \"1\"), entries are separated by a single
    additional line-height (as set by the line-spacing attribute). Can
    be set to any non-negative integer to specify a multiple of this
    amount.

#### Reference Grouping

`subsequent-author-substitute`

:   If set, the value of this attribute replaces names in a
    bibliographic entry that also occur in the preceding entry. The
    exact method of substitution depends on the value of the
    `subsequent-author-substitute-rule` attribute. Substitution is
    limited to the names of the first `cs:names` element rendered.

`subsequent-author-substitute-rule`

:   Specifies when and how names are substituted as a result of
    `subsequent-author-substitute`. Allowed values:

    -   \"complete-all\" - (default), when all rendered names of the
        name variable match those in the preceding bibliographic entry,
        the value of `subsequent-author-substitute` replaces the entire
        name list (including punctuation and terms like \"et al\" and
        \"and\"), except for the affixes set on the `cs:names` element.
    -   \"complete-each\" - requires a complete match like
        \"complete-all\", but now the value of
        `subsequent-author-substitute` substitutes for each rendered
        name.
    -   \"partial-each\" - when one or more rendered names in the name
        variable match those in the preceding bibliographic entry, the
        value of `subsequent-author-substitute` substitutes for each
        matching name. Matching starts with the first name, and
        continues up to the first mismatch.
    -   \"partial-first\" - as \"partial-each\", but substitution is
        limited to the first name of the name variable.

    For example, take the following bibliographic entries:

        Doe. 1999.
        Doe. 2000.
        Doe, Johnson & Williams. 2001.
        Doe & Smith. 2002.
        Doe, Stevens & Miller. 2003.
        Doe, Stevens & Miller. 2004.
        Doe, Williams et al. 2005.
        Doe, Williams et al. 2006.

    With `subsequent-author-substitute` set to \"\-\--\", and
    `subsequent-author-substitute-rule` set to \"complete-all\", this
    becomes:

        Doe. 1999.
        ---. 2000.
        Doe, Johnson & Williams. 2001.
        Doe & Smith. 2002.
        Doe, Stevens & Miller. 2003.
        ---. 2004.
        Doe, Williams et al. 2005.
        ---. 2005.

    With `subsequent-author-substitute-rule` set to \"complete-each\",
    this becomes:

        Doe. 1999.
        ---. 2000.
        Doe, Johnson & Williams. 2001.
        Doe & Smith. 2002.
        Doe, Stevens & Miller. 2003.
        ---, --- & ---. 2004.
        Doe, Williams et al. 2005.
        ---, --- et al. 2006.

    With `subsequent-author-substitute-rule` set to \"partial-each\",
    this becomes:

        Doe. 1999.
        ---. 2000.
        Doe, Johnson & Williams. 2001.
        --- & Smith. 2002.
        Doe, Stevens & Miller. 2003.
        ---, --- & ---. 2004.
        Doe, Williams et al. 2005.
        ---, --- et al. 2005.

    With `subsequent-author-substitute-rule` set to \"partial-first\",
    this becomes:

        Doe. 1999.
        ---. 2000.
        Doe, Johnson & Williams. 2001.
        --- & Smith. 2002.
        Doe, Stevens & Miller. 2003.
        ---, Stevens & Miller. 2004.
        Doe, Williams et al. 2005.
        ---, Williams et al. 2005.

### Global Options

#### Hyphenation of Initialized Names

`initialize-with-hyphen`

:   Specifies whether compound given names (e.g. \"Jean-Luc\") should be
    initialized with a hyphen (\"J.-L.\", value \"true\", default) or
    without (\"J.L.\", value \"false\").

#### Page Ranges

`page-range-format`

:   Activates expansion or collapsing of page ranges: \"chicago\" (\"321
    -- 28\"), \"expanded\" (e.g. \"321 -- 328\"), \"minimal\" (\"321 --
    8\"), or \"minimal-two\" (\"321 -- 28\") (see also [Appendix V -
    Page Range Formats](#appendix-v---page-range-formats)). Delimits
    page ranges with the \"page-range-delimiter\" term (introduced with
    CSL 1.0.1, and defaults to an en-dash). If the attribute is not set,
    page ranges are rendered without reformatting.

#### Name Particles

Western names frequently contain one or more name particles (e.g. \"de\"
in the Dutch name \"W. de Koning\"). These name particles can be either
kept or dropped when only the surname is shown: these two types are
referred to as non-dropping and dropping particles, respectively. A
single name can contain particles of both types (with non-dropping
particles always following dropping particles). For example, \"W. de
Koning\" and the French name \"Jean de La Fontaine\" can be
deconstructed into:

>     {
>         "author": [
>             {
>                 "given": "W.",
>                 "non-dropping-particle": "de",
>                 "family": "Koning"
>             },
>             {
>                 "given": "Jean",
>                 "dropping-particle": "de",
>                 "non-dropping-particle": "La",
>                 "family": "Fontaine"
>             }
>         ]
>     }

When just the surname is shown, only the non-dropping-particle is kept:
\"De Koning\" and \"La Fontaine\".

In the case of inverted names, where the family name precedes the given
name, the dropping-particle is always appended to the family name, but
the non-dropping-particle can be either prepended (e.g. \"de Koning,
W.\") or appended (after initials or given names, e.g. \"Koning, W.
de\"). For inverted names where the non-dropping-particle is prepended,
names can either be sorted by keeping the non-dropping-particle together
with the family name as part of the primary sort key (sort order A), or
by separating the non-dropping-particle from the family name and have it
become (part of) a secondary sort key, joining the dropping-particle, if
available (sort order B):

**Sort order A: non-dropping-particle not demoted**

-   primary sort key: \"La Fontaine\"
-   secondary sort key: \"de\"
-   tertiary sort key: \"Jean\"

**Sort order B: non-dropping-particle demoted**

-   primary sort key: \"Fontaine\"
-   secondary sort key: \"de La\"
-   tertiary sort key: \"Jean\"

The handling of the non-dropping-particle can be customized with the
`demote-non-dropping-particle` option:

`demote-non-dropping-particle`

:   Sets the display and sorting behavior of the non-dropping-particle
    in inverted names (e.g. \"Koning, W. de\"). Allowed values:

    -   \"never\": the non-dropping-particle is treated as part of the
        family name, whereas the dropping-particle is appended (e.g.
        \"de Koning, W.\", \"La Fontaine, Jean de\"). The
        non-dropping-particle is part of the primary sort key (sort
        order A, e.g. \"de Koning, W.\" appears under \"D\").
    -   \"sort-only\": same display behavior as \"never\", but the
        non-dropping-particle is demoted to a secondary sort key (sort
        order B, e.g. \"de Koning, W.\" appears under \"K\").
    -   \"display-and-sort\" (default): the dropping and
        non-dropping-particle are appended (e.g. \"Koning, W. de\" and
        \"Fontaine, Jean de La\"). For name sorting, all particles are
        part of the secondary sort key (sort order B, e.g. \"Koning, W.
        de\" appears under \"K\").

Some names include a particle that should never be demoted. For these
cases the particle should just be included in the family name field, for
example for the French general Charles de Gaulle:

>     {
>         "author": [
>             {
>                 "family": "de Gaulle",
>                 "given": "Charles"
>             }
>         ]
>     }

### Inheritable Name Options

Attributes for the `cs:names` and `cs:name` elements may also be set on
`cs:style`, `cs:citation` and `cs:bibliography`. This eliminates the
need to repeat the same attributes and attribute values for every
occurrence of the `cs:names` and `cs:name` elements.

The available inheritable attributes for `cs:name` are `and`,
`delimiter-precedes-et-al`, `delimiter-precedes-last`, `et-al-min`,
`et-al-use-first`, `et-al-use-last`, `et-al-subsequent-min`,
`et-al-subsequent-use-first`, `initialize`, `initialize-with`,
`name-as-sort-order` and `sort-separator`. The attributes `name-form`
and `name-delimiter` correspond to the `form` and `delimiter` attributes
on `cs:name`. Similarly, `names-delimiter` corresponds to the
`delimiter` attribute on `cs:names`.

When an inheritable name attribute is set on `cs:style`, `cs:citation`
or `cs:bibliography`, its value is used for all `cs:names` elements
within the scope of the element carrying the attribute. If an attribute
is set on multiple hierarchical levels, the value set at the lowest
level is used.

### Locale Options

`limit-day-ordinals-to-day-1`

:   Date formats are defined by the `cs:date` element and its
    `cs:date-part` child elements (see [Date](#date)). By default, when
    the `cs:date-part` element with `name` set to \"day\" has `form` set
    to \"ordinal\", all days (1 through 31) are rendered in the ordinal
    form, e.g. \"January 1st\", \"January 2nd\", etc. By setting
    `limit-day-ordinals-to-day-1` to \"true\" (\"false\" is the
    default), the \"ordinal\" form is limited to the first day of each
    month (other days will use the \"numeric\" form). This is desirable
    for some languages, such as French: \"1er janvier\", but \"2
    janvier\", \"3 janvier\", etc.

`punctuation-in-quote`

:   For `cs:text` elements rendered with the `quotes` attribute set to
    \"true\" (see [Formatting](#formatting)), and for which the output
    is followed by a comma or period, `punctuation-in-quote` specifies
    whether this punctuation is placed outside (value \"false\",
    default) or inside (value \"true\") the closing quotation mark.

## Sorting

`cs:citation` and `cs:bibliography` may include a `cs:sort` child
element before the `cs:layout` element to specify the sorting order of
respectively cites within citations, and bibliographic entries within
the bibliography. In the absence of `cs:sort`, cites and bibliographic
entries appear in the order in which they are cited.

The `cs:sort` element must contain one or more `cs:key` child elements.
The sort key, set as an attribute on `cs:key`, must be a variable (see
[Appendix IV - Variables](#appendix-iv---variables)) or macro name. For
each `cs:key` element, the sort direction can be set to either
\"ascending\" (default) or \"descending\" with the `sort` attribute. The
attributes `names-min`, `names-use-first`, and `names-use-last` may be
used to override the values of the corresponding
`et-al-min`/`et-al-subsequent-min`,
`et-al-use-first`/`et-al-subsequent-use-first` and `et-al-use-last`
attributes, and affect all names generated via macros called by
`cs:key`.

Sort keys are evaluated in sequence. A primary sort is performed on all
items using the first sort key. A secondary sort, using the second sort
key, is applied to items sharing the first sort key value. A tertiary
sort, using the third sort key, is applied to items sharing the first
and second sort key values. Sorting continues until either the order of
all items is fixed, or until the sort keys are exhausted. Items with an
empty sort key value are placed at the end of the sort, both for
ascending and descending sorts.

An example, where cites are first sorted by the output of the \"author\"
macro, with overriding settings for et-al abbreviation. Cites sharing
the primary sort key are subsequently sorted in descending order by the
\"issued\" date variable.

``` {.xml}
<citation>
  <sort>
    <key macro="author" names-min="3" names-use-first="3"/>
    <key variable="issued" sort="descending"/>
  </sort>
  <layout>
    <!-- rendering elements -->
  </layout>
</citation>
```

The sort key value of a variable or macro can differ from the \"normal\"
rendered output. The specifics of sorting variables and macros:

### Sorting Variables

The sort key value for a variable called by `cs:key` via the `variable`
attribute consists of the string value, without rich text markup.
Exceptions are name, date and numeric variables:

**names:** [Name variables](#name-variables) called via the `variable`
attribute (e.g. `<key variable="author"/>`) are returned as a name list
string, with the `cs:name` attributes `form` set to \"long\", and
`name-as-sort-order` set to \"all\".

**dates:** [Date variables](#date-variables) called via the `variable`
attribute are returned in the YYYYMMDD format, with zeros substituted
for any missing date-parts (e.g. 20001200 for December 2000). As a
result, less specific dates precede more specific dates in ascending
sorts, e.g. \"2000, May 2000, May 1st 2000\". Negative years are sorted
inversely, e.g. \"100BC, 50BC, 50AD, 100AD\". Seasons are ignored for
sorting, as the chronological order of the seasons differs between the
northern and southern hemispheres. In the case of date ranges, the start
date is used for the primary sort, and the end date is used for a
secondary sort, e.g. \"2000 -- 2001, 2000 -- 2005, 2002 -- 2003, 2002 --
2009\". Date ranges are placed after single dates when they share the
same (start) date, e.g. \"2000, 2000 -- 2002\".

**numbers:** [Number variables](#number-variables) called via the
`variable` attribute are returned as integers (`form` is \"numeric\").
If the original variable value only consists of non-numeric text, the
value is returned as a text string.

### Sorting Macros

The sort key value for a macro called via `cs:key` via the `macro`
attribute generally consists of the string value the macro would
ordinarily generate, without rich text markup (exceptions are discussed
below).

For name sorting, there are four advantages in using the same macro for
rendering and sorting, instead of sorting directly on the name variable.
First, substitution is available (e.g. the \"editor\" variable might
substitute for an empty \"author\" variable). Secondly, et-al
abbreviation can be used (using either the
`et-al-min`/`et-al-subsequent-min`,
`et-al-use-first`/`et-al-subsequent-use-first`, and `et-al-use-last`
options defined for the macro, or the overriding `names-min`,
`names-use-first` and `names-use-last` attributes set on `cs:key`). When
et-al abbreviation occurs, the \"et-al\" and \"and others\" terms are
excluded from the sort key values. Thirdly, names can be sorted by just
the surname (using a macro for which the `form` attribute on `cs:name`
is set to \"short\"). Finally, it is possible to sort by the number of
names in a name list, by calling a macro for which the `form` attribute
on `cs:name` is set to \"count\". As for names sorted via the `variable`
attribute, names sorted via `macro` are returned with the `cs:name`
attribute `name-as-sort-order` set to \"all\".

[Number variables](#number-variables) rendered within the macro with
`cs:number` and [date variables](#date-variables) are treated the same
as when they are called via `variable`. The only exception is that the
complete date is returned if a date variable is called via the
`variable` attribute. In contrast, macros return only those date-parts
that would otherwise be rendered (respecting the value of the
`date-parts` attribute for localized dates, or the listing of
`cs:date-part` elements for non-localized dates).

## Range Delimiters

Collapsed ranges for the \"citation-number\" and \"year-suffix\"
variables are delimited by an en-dash (e.g. \"(1 -- 3, 5)\" and \"(Doe
2000a -- c,e)\").

The \"locator\" variable is always rendered with an en-dash replacing
any hyphens. For the \"page\" variable, this replacement is only
performed if the `page-range-format` attribute is set on `cs:style` (see
[Page Ranges](#page-ranges)).

## Formatting

The following formatting attributes may be set on `cs:date`,
`cs:date-part`, `cs:et-al`, `cs:group`, `cs:label`, `cs:layout`,
`cs:name`, `cs:name-part`, `cs:names`, `cs:number` and `cs:text`:

`font-style`

:   Sets the font style, with values:

    -   \"normal\" (default)
    -   \"italic\"
    -   \"oblique\" (i.e. slanted)

`font-variant`

:   Allows for the use of small capitals, with values:

    -   \"normal\" (default)
    -   \"small-caps\"

`font-weight`

:   Sets the font weight, with values:

    -   \"normal\" (default)
    -   \"bold\"
    -   \"light\"

`text-decoration`

:   Allows for the use of underlining, with values:

    -   \"none\" (default)
    -   \"underline\"

`vertical-align`

:   Sets the vertical alignment, with values:

    -   \"baseline\" (default)
    -   \"sup\" (superscript)
    -   \"sub\" (subscript)

## Affixes

The affixes attributes `prefix` and `suffix` may be set on `cs:date`
(except when `cs:date` defines a localized date format), `cs:date-part`
(except when the parent `cs:date` element calls a localized date
format), `cs:group`, `cs:label`, `cs:layout`, `cs:name`, `cs:name-part`,
`cs:names`, `cs:number` and `cs:text`. The attribute value is either
added before (`prefix`) or after (`suffix`) the output of the element
carrying the attribute, but affixes are only rendered if the element
produces output. With the exception of affixes set on `cs:layout`,
affixes are outside the scope of [formatting](#formatting),
[quotes](#quotes), [strip-periods](#strip-periods) and
[text-case](#text-case) attributes set on the same element (as a
workaround, these attributes take effect on affixes when set on a parent
`cs:group` element).

## Delimiter

The `delimiter` attribute, whose value delimits non-empty pieces of
output, may be set on `cs:date` (delimiting the date-parts; `delimiter`
is not allowed when `cs:date` calls a localized date format), `cs:names`
(delimiting name lists from different [name
variables](#name-variables)), `cs:name` (delimiting names within name
lists), `cs:group` and `cs:layout` (delimiting the output of the child
elements).

## Display

The `display` attribute (similar the \"display\" property in CSS) may be
used to structure individual bibliographic entries into one or more text
blocks. If used, all rendering elements should be under the control of a
display attribute. The allowed values:

-   \"block\" - block stretching from margin to margin.
-   \"left-margin\" - block starting at the left margin. If followed by
    a \"right-inline\" block, the \"left-margin\" blocks of all
    bibliographic entries are set to a fixed width to accommodate the
    longest content string found among these \"left-margin\" blocks. In
    the absence of a \"right-inline\" block the \"left-margin\" block
    extends to the right margin.
-   \"right-inline\" - block starting to the right of a preceding
    \"left-margin\" block (behaves as \"block\" in the absence of such a
    \"left-margin\" block). Extends to the right margin.
-   \"indent\" - block indented to the right by a standard amount.
    Extends to the right margin.

**Examples**

(A) Instead of using `second-field-align` (see
    [Whitespace](#whitespace)), a similar layout can be achieved with a
    \"left-margin\" and \"right-inline\" block. A potential benefit is
    that the styling of blocks can be further controlled in the final
    output (e.g. using CSS for HTML, styles for Word, etc.).

    ``` {.xml}
    <bibliography>
      <layout>
        <text display="left-margin" variable="citation-number"
            prefix="[" suffix="]"/>
        <group display="right-inline">
          <!-- rendering elements -->
        </group>
      </layout>
    </bibliography>
    ```

------------------------------------------------------------------------

(B) A per-author publication listing. With
    `subsequent-author-substitute` (see [Reference
    Grouping](#reference-grouping)) set to an empty string, the block
    with the author names is only rendered once for items by the same
    authors.

    ``` {.xml}
    <bibliography subsequent-author-substitute="">
      <sort>
        <key variable="author"/>
        <key variable="issued"/>
      </sort>
      <layout>
        <group display="block">
          <names variable="author"/>
        </group>
        <group display="left-margin">
          <date variable="issued">
            <date-part name="year" />
          </date>
        </group>
        <group display="right-inline">
          <text variable="title"/>
        </group>
      </layout>
    </bibliography>
    ```

    The output of this example would look like:

      ------------------- -----------------------
      Author1             

      year-publication1   title-publication1

      year-publication2   title-publication2

      Author2             

      year-publication3   title-publication3

      year-publication4   title-publication4
      ------------------- -----------------------

------------------------------------------------------------------------

(C) An annotated bibliography, where the annotation appears in an
    indented block below the reference.

    ``` {.xml}
    <bibliography>
      <layout>
        <group display="block">
          <!-- rendering elements -->
        </group>
        <text display="indent" variable="abstract" />
      </layout>
    </bibliography>
    ```

## Quotes

The `quotes` attribute may set on `cs:text`. When set to \"true\"
(\"false\" is default), the rendered text is wrapped in quotes (the
quotation marks used are terms). The localized `punctuation-in-quote`
option controls whether an adjoining comma or period appears outside
(default) or inside the closing quotation mark (see [Locale
Options](#locale-options)).

## Strip-periods

The `strip-periods` attribute may be set on `cs:date-part` (but only if
`name` is set to \"month\"), `cs:label` and `cs:text`. When set to
\"true\" (\"false\" is the default), any periods in the rendered text
are removed.

## Text-case

The `text-case` attribute may be set on `cs:date`, `cs:date-part`,
`cs:label`, `cs:name-part`, `cs:number` and `cs:text`. The allowed
values:

-   \"lowercase\": renders text in lowercase
-   \"uppercase\": renders text in uppercase
-   \"capitalize-first\": capitalizes the first character of the first
    word, if the word is lowercase
-   \"capitalize-all\": capitalizes the first character of every
    lowercase word
-   \"sentence\": renders text in sentence case
-   \"title\": renders text in title case

### Sentence Case Conversion

Sentence case conversion (with `text-case` set to \"sentence\") is
performed by:

1.  For uppercase strings, the first character of the string remains
    capitalized. All other letters are lowercased.
2.  For lower or mixed case strings, the first character of the first
    word is capitalized if the word is lowercase. The case of all other
    words stays the same.

CSL processors don\'t recognize proper nouns. As a result, strings in
sentence case can be accurately converted to title case, but not vice
versa. For this reason, it is generally preferable to store strings such
as titles in sentence case, and only use `text-case` if a style desires
another case.

### Title Case Conversion

Title case conversion (with `text-case` set to \"title\") for
English-language items is performed by:

1.  For uppercase strings, the first character of each word remains
    capitalized. All other letters are lowercased.
2.  For lower or mixed case strings, the first character of each
    lowercase word is capitalized. The case of words in mixed or
    uppercase stays the same.

In both cases, stop words are lowercased, unless they are the first or
last word in the string, or follow a colon. The stop words are \"a\",
\"an\", \"and\", \"as\", \"at\", \"but\", \"by\", \"down\", \"for\",
\"from\", \"in\", \"into\", \"nor\", \"of\", \"on\", \"onto\", \"or\",
\"over\", \"so\", \"the\", \"till\", \"to\", \"up\", \"via\", \"with\",
and \"yet\".

#### Non-English Items

As many languages do not use title case, title case conversion (with
`text-case` set to \"title\") only affects English-language items.

If the `default-locale` attribute on `cs:style` isn\'t set, or set to a
locale code with a primary language tag of \"en\" (English), items are
assumed to be English. An item is only considered to be non-English if
its metadata contains a `language` field with a non-nil value that
doesn\'t start with the \"en\" primary language tag.

If `default-locale` is set to a locale code with a primary language tag
other than \"en\", items are assumed to be non-English. An item is only
considered to be English if the value of its `language` field starts
with the \"en\" primary language tag.

Appendix I - Categories
-----------------------

-   anthropology
-   astronomy
-   biology
-   botany
-   chemistry
-   communications
-   engineering
-   generic-base - used for generic styles like Harvard and APA
-   geography
-   geology
-   history
-   humanities
-   law
-   linguistics
-   literature
-   math
-   medicine
-   philosophy
-   physics
-   political\_science
-   psychology
-   science
-   social\_science
-   sociology
-   theology
-   zoology

