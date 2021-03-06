Package
=======

Unlike many other RPC framworks/IDL compilers, Nirum's single unit of
compilation is not a source file, but a package of source files.
Each package is compiled to a package of target language (e.g. Python package
with setup.py file).  The following example is an ordinary Nirum package:

 -  /
     -  foo/
         -  bar.nrm
         -  baz.nrm
         -  baz/
             -  qux.nrm
             -  quux.nrm
     -  package.toml

If the target is Python the above package is compiled to the below Python
package:

 -  /
     -  foo/
         -  \_\_init\_\_.py
         -  bar/
             -  \_\_init\_\_.py
         -  baz/
             -  \_\_init\_\_.py
         -  baz/
             -  \_\_init\_\_.py
             -  qux/
                 -  \_\_init\_\_.py
             -  quux/
                 -  \_\_init\_\_.py
     -  setup.py


Package manifest
----------------

Every Nirum package has its own package.toml file.  It contains metadata
about the package (e.g. `version`, `authors`).  The following [TOML][] code
is an example:

    version = "1.0.0"  # (required)
    description = "Short description on the package"
    license = "MIT"
    keywords = "sample example nirum"

    [[authors]]
    name = "John Doe"  # (required)
    email = "johndoe@example.com"
    uri = "http://example.com/~johndoe/"

It consists of the following fields (*emphasized fields* are required):

*`version`* (string)
:   The required [semver][] of the package.

`description` (string)
:   An optional short description of the package.

`license` (string)
:   An optional license of the package.

`keywords` (string)
:   An optional keywords of the package.

`authors` (array of tables)
:   The list of authors.  Note that it can be empty, but `name` field is
    required if any author is provided.  Each author table consists of
    the following fields:

    *`name`* (string)
    :   The required full name of the author.

    `email` (string)
    :   An optional email address of the author.

    `uri` (string)
    :   An optional URI to author's website.

`targets` (table)
:   Settings of each target.  See also the below target settings section
    for details.

[semver]: http://semver.org/
[toml]: https://github.com/toml-lang/toml


Target settings
---------------

Beside common metadata to every target, there can be configurations specific
to each target.  For example, Python target need a PyPI distribution name
(which is specified of `setup()` function's `name` parameter in setup.py file)
of the package to be generated by Nirum, hence `name`:

    [targets.python]
    name = "py-foobar"  # will be submitted to: pypi.python.org/pypi/py-foobar
    minimum_runtime = "0.3.9"
    classifiers = [
        "Development Status :: 3 - Alpha",
        "License :: OSI Approved :: GNU General Public License v3 or later (GPLv3+)"
    ]

There are two optional fields, `minimum_runtime` and `classifiers`.
`minimum_runtime` is a version of [nirum-python][] that
is required by generated python package. `classifiers` contains a list of
classifier which is listed on [classifier list][pypi-classifier-list].

[pypi-classifier-list]: https://pypi.org/pypi?%3Aaction=list_classifiers


Text encoding
-------------

All files in a Nirum package must be in UTF-8:

 -  A package manifest file (package.toml) must be in UTF-8, as the TOML
    specification requires.
 -  Nirum module source files must be in UTF-8.  If a file has a leading
    [BOM][] (byte order mark) it will be ignored.

[bom]: http://unicode.org/faq/utf_bom.html#BOM
