.. _start:

=======================================
reStructuredText Primer for TYPO3 Users
=======================================

This section is a brief introduction to reStructuredText (reST) concepts and
syntax, intended to provide TYPO3 authors with enough information to author
documents productively.  Since reST was designed to be a simple, unobtrusive
markup language, this will not take too long.

.. seealso::

    The authoritative `reStructuredText User Documentation
    <http://docutils.sourceforge.net/rst.html>`_.


.. contents:: Overview
    :local:

.. toctree::
    AdvancedMarkup.rst


.. _introduction:

Introduction
============

.. _file-extensions:

File Extensions
---------------

reST documents are using the extension (ending) ``.rst``. If you happen to include
other files, such as when centralizing directives or substitutions (see below), you
should use another file extension, such as ``.txt`` to prevent uncessary warnings.


.. _paragraphs:

Paragraphs
----------

The paragraph is the most basic block in a reST document.  Paragraphs are simply
chunks of text separated by one or more blank lines.  Indentation is significant
in reST, so all lines of the same paragraph must be left-aligned to the same level
of indentation.


.. _indentation:

Indentation
-----------

In TYPO3 documentation, we suggest to indent with either 1 tab ("\\t") or 4 spaces.


.. _text-formatting:

Text Formatting
===============


.. _inlinemarkup:

Inline markup (bold, italic, verbatim)
--------------------------------------

The standard reST inline markup is quite simple: use

* one asterisk: ``*italic*`` for emphasis (*italics*),
* two asterisks: ``**bold**`` for strong emphasis (**boldface**), and
* double backquotes: ````text```` for ``code samples``.

If asterisks or backquotes appear in running text and could be confused with
inline markup delimiters, they have to be escaped with a backslash.

Be aware of some restrictions of this markup:

* it may not be nested,
* content may not start or end with whitespace: ``* text*`` is wrong,
* it must be separated from surrounding text by non-word characters.  Use a
  backslash escaped space to work around that: ``thisis\ *one*\ word``.

reST also allows for custom "interpreted text roles", which signify that the
enclosed text should be interpreted in a specific way.  Sphinx uses this to
provide semantic markup and cross-referencing of identifiers, as described in
the appropriate section.  The general syntax is ``:rolename:`content```.


.. _headings:

Headings
--------

Section headers are created by underlining (and optionally overlining) the section
title with a punctuation character, at least as long as the text:

.. code-block:: restructuredtext

    =================
    This is a heading
    =================

Normally, there are no heading levels assigned to certain characters as the
structure is determined from the succession of headings.  However, for the
TYPO3 documentation, this convention is used which you may follow:

* ``=`` with overline, for title of the documentation
* ``=``, for chapters
* ``-``, for sections
* ``^``, for subsections
* ``"``, for subsubsections

.. code-block:: restructuredtext

    ======================
    Title of your document
    ======================

    Chapter 1: whatever
    ===================

    Text goes here...

    Section 1.1: else
    -----------------

    and so on

Of course, you are free to use your own marker characters (see the reST
documentation), and use a deeper nesting level, but keep in mind that most
target formats (HTML, LaTeX for PDF) have a limited supported nesting depth.


.. _bullet-lists:

Lists, Bullets and Quote-like Blocks
------------------------------------

List markup is natural: just place an asterisk or a dash at the start of a
paragraph and indent properly.  The same goes for numbered lists; they can also
be autonumbered using a ``#`` sign. The following code:

.. code-block:: restructuredtext

    - This is a bulleted list.
    - It has two items, the second
      item uses two lines.

    * This is another bulleted list.
    * It has two items as well, the second
      item uses two lines

    1. This is a numbered list.
    2. It has two items too.

    #. This is a numbered list.
    #. It has two items too.

gives:

- This is a bulleted list.
- It has two items, the second
  item uses two lines.

* This is another bulleted list.
* It has two items as well, the second
  item uses two lines

1. This is a numbered list.
2. It has two items too.

#. This is a numbered list.
#. It has two items too.

Nested lists are possible, but be aware that they must be separated from the
parent list items by blank lines. The following code:

.. code-block:: none

    - this is
    - a list

      - with a nested list
      - and some subitems

    - and here the parent list continues

gives:

- this is
- a list

  - with a nested list
  - and some subitems

- and here the parent list continues

.. caution::

	The text immediately after the bullet determines the indentation:

	.. image:: Images/sublist_indent.png

Definition lists are created as follows::

    term (up to a line of text)
        Definition of the term, which must be indented

        and can even consist of multiple paragraphs

    next term
        Description.

and is rendered as:

term (up to a line of text)
    Definition of the term, which must be indented

    and can even consist of multiple paragraphs

next term
    Description.

Note that the term cannot have more than one line of text.

Quoted paragraphs are created by just indenting them more than the surrounding
paragraphs.

Line blocks are a way of preserving line breaks. The following code:

.. code-block:: restructuredtext

    | These lines are
    | broken exactly like in
    | the source file.

gives:

| These lines are
| broken exactly like in
| the source file.

.. _directives:

What are Directives
===================

reST is mainly based on *directives* that are defined as follows:

.. code-block:: restructuredtext

    .. <name>:: <arguments>
        :<option>: <option values>

        content

Example:

.. code-block:: restructuredtext

    .. image:: ../images/test.png
        :width: 200px

.. warning::
    Note the space between the directive and its argument as well as the blank
    line between the option and the content.

The directive content follows after a blank line and is indented relative to the
directive start.


.. _hyperlinks:

Hyperlinks
==========

External Links
--------------

Use ```Link text <http://example.com/>`_`` for inline web links.  If the link
text should be the web address, you don't need special markup at all, the parser
finds links and mail addresses in ordinary text.

You can also separate the link and the target definition, like this:

.. code-block:: restructuredtext

    This is a paragraph that contains `a link`_.

    .. _a link: http://example.com/


Internal Links
--------------

Internal linking is done via a special reST role provided by Sphinx.

Cross-referencing Arbitrary Locations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To support cross-referencing to arbitrary locations in any document, the
standard reST labels are used.  For this to work label names must be unique
throughout the entire documentation.  There are two ways in which you can
refer to labels:

* If you place a label directly before a section title, you can reference to
  it with ``:ref:`label-name```.  Example:

  .. code-block:: restructuredtext

      .. _my-reference-label:

      Section to cross-reference
      --------------------------

      This is the text of the section.

      It refers to the section itself, see :ref:`my-reference-label`.

  The ``:ref:`` role would then generate a link to the section, with the link
  title being "Section to cross-reference".  This works just as well when
  section and reference are in different source files.

  Automatic labels also work with figures: given :

  .. code-block:: restructuredtext

      .. _my-figure:

      .. figure:: whatever

          Figure caption

  a reference ``:ref:`my-figure``` would insert a reference to the figure
  with link text "Figure caption".

  The same works for tables that are given an explicit caption using the
  `table` directive.

* Labels that aren't placed before a section title can still be referenced
  to, but you must give the link an explicit title, using this syntax:
  ``:ref:`Link title <label-name>```.

* Labels should always use a minus sign to separate words and not an
  underscore in order to prevent troubles with PDF output.


.. _tables:

Tables
======

Two forms of tables are supported.  For *grid tables*, you have to "paint" the
cell grid yourself.  They look like this:

.. code-block:: restructuredtext

    +------------------------+------------+----------+----------+
    | Header row, column 1   | Header 2   | Header 3 | Header 4 |
    | (header rows optional) |            |          |          |
    +========================+============+==========+==========+
    | body row 1, column 1   | column 2   | column 3 | column 4 |
    +------------------------+------------+----------+ spanning |
    | body row 2             | ...        | ...      | here...  |
    +------------------------+------------+----------+----------+

and are rendered as:

+------------------------+------------+----------+----------+
| Header row, column 1   | Header 2   | Header 3 | Header 4 |
| (header rows optional) |            |          |          |
+========================+============+==========+==========+
| body row 1, column 1   | column 2   | column 3 | column 4 |
+------------------------+------------+----------+ spanning |
| body row 2             | ...        | ...      | here...  |
+------------------------+------------+----------+----------+

*Simple tables* are easier to write, but limited: they must contain more than one
row, and the first column cannot contain multiple lines.  They look like this:

.. code-block:: restructuredtext

    =====  =====  =======
    A      B      A and B
    =====  =====  =======
    False  False  False
    True   False  False
    False  True   False
    True   True   True
    =====  =====  =======

and are rendered as:

=====  =====  =======
A      B      A and B
=====  =====  =======
False  False  False
True   False  False
False  True   False
True   True   True
=====  =====  =======

.. _images:

Images and Figures
==================

reST supports an image directive, used like so:

.. code-block:: restructuredtext

    .. image:: gnu.png
        :width: 200px
        :alt: alternate text

When used within Sphinx, the file name given (here ``gnu.png``) must be relative
to the source file.  For example, the file ``sketch/spam.rst`` could refer
to the image ``images/spam.png`` as ``../images/spam.png``.

Sphinx will automatically copy image files over to a subdirectory of the output
directory on building (e.g. the ``_static`` directory for HTML output.)

Interpretation of image size options (``width`` and ``height``) is as follows:
if the size has no unit or the unit is pixels, the given size will only be
respected for output channels that support pixels (i.e. not in LaTeX output).
Other units (like ``pt`` for points) will be used for HTML and LaTeX output.

Figures should be generally preferred:

.. code-block:: restructuredtext

    .. figure:: gnu.png
        :width: 200px
        :alt: alternate text

        figures are like images but with a caption and may be relocated
        elsewhere (to better use available page space) when rendering a PDF


.. _admonitions:

Admonitions
===========

Admonitions are call-out boxes that attract the user's attention
in various, be it for important stuff, links to more information, etc.
This is a typical admonition structure:

.. code-block:: restructuredtext

    .. warning::

       Be careful when installing an extension, you never know
       who developed it ;-)

The following admonition types are commonly used in TYPO3 projects:

- note

  .. note::
     Text of the note.

- important

  .. important::
     Something important.

- tip

  .. tip::
     A Friendly tip.

- warning

  .. warning::
     Be careful when using a given command.

The special "todo" admonition will not be rendered. It is really just
like a comment block inside your reST document.


.. _toctree:

Including other Files
=====================

Sooner or later you will want to structure your project documentation by having
several reST files.  The toctree directive allows you to insert other files within
a reST file. The reason to use this directive is that reST does not have facilities
to interconnect several documents, or split documents into multiple output files.
The toctree directive looks like:

.. code-block:: restructuredtext

    .. toctree::
        :maxdepth: 2

        intro.rst
        chapter1.rst
        chapter2.rst
        otherDir/file.rst

It includes 4 reST files and shows a table of contents (TOC) that includes the title
found in the reST documents.

.. _others:

Others
======

.. _comments:

Comments
--------

Every explicit markup block which isn't a valid markup construct (like the
images above) is regarded as a comment.  For example:

.. code-block:: restructuredtext

    .. This is a comment.

You can indent text after a comment start to form multiline comments:

.. code-block:: restructuredtext

    ..
        This whole indented block
        is a comment.

        Still in the comment.


.. _substitutions:

Substitutions
-------------

reST supports "substitutions", which are pieces of text and/or markup referred to
in the text by ``|name|``.  They are defined like this:

.. code-block:: restructuredtext

    .. |name| replace:: replacement *text*

or this:

.. code-block:: restructuredtext

    .. |caution| image:: warning.png
        :alt: Warning!

Sphinx provides three substitutions that are defined by default.

.. describe:: |release|

   Replaced by the project release the documentation refers to.  This is meant
   to be the full version string including alpha/beta/release candidate tags,
   e.g. ``2.5.2b3``.

.. describe:: |version|

   Replaced by the project version the documentation refers to. This is meant to
   consist only of the major and minor version parts, e.g. ``2.5``, even for
   version 2.5.1.

.. describe:: |today|

   Replaced by either today's date (the date on which the document is read), or
   the date set in the build configuration file.  Normally has the format
   ``April 14, 2007``.


.. _source-code:

Source Code
-----------

Literal code blocks are introduced by ending a paragraph with the special
marker ``::``.  The literal block must be indented (and, like all paragraphs,
separated from the surrounding ones by blank lines):

.. code-block:: restructuredtext

    This is a normal text paragraph. The next paragraph is a code sample::

        It is not processed in any way, except
        that the indentation is removed.

        It can span multiple lines.

    This is a normal text paragraph again.

The handling of the ``::`` marker is smart:

* If it occurs as a paragraph of its own, that paragraph is completely left
  out of the document.
* If it is preceded by whitespace, the marker is removed.
* If it is preceded by non-whitespace, the marker is replaced by a single
  colon.

That way, the second sentence in the above example's first paragraph would be
rendered as "The next paragraph is a code sample:" (single colon at the end).


.. syntax-highlighting:

Syntax Highlighting
-------------------

Instead of using the special marker ``::``, you may prefer the ``code-block``
directive which lets you highlight the code for a specific language:

.. code-block:: restructuredtext

    .. code-block:: php

        <?php
        $foo = 'bar';
        ?>

to get:

.. code-block:: php

    <?php
    $foo = 'bar';
    ?>

You may number lines as well and emphasize one or more lines:

.. code-block:: restructuredtext
   :emphasize-lines: 2-3

    .. code-block:: yaml
        :linenos:
        :emphasize-lines: 1,4-5

        conf.py:
          copyright: 2013-2014
          project: Sphinx Python Documentation Generator and Viewer
          version: 1.3
          release: 1.3.0

which shows as:

.. code-block:: yaml
    :linenos:
    :emphasize-lines: 1,4-5

    conf.py:
      copyright: 2013-2014
      project: Sphinx Python Documentation Generator and Viewer
      version: 1.3
      release: 1.3.0

See http://pygments.org/docs/lexers/ for a list of supported languages (please
note that 'typoscript' is supported as well).
