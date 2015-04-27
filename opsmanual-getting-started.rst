.. _opsmanual-getting-started:

Getting started with the Ops Manual, reStructuredText, and Sphinx
=================================================================

Basics
------

The Ops Manual is maintained as a Sphinx_ project on `GitHub`_. This
means that the pages you're viewing are plain text files (actually, they're in a
format called `reStructuredText`_, of which more later) which live in `a git
repository`_. Editing the manual is as simple as clicking the "Edit on GitHub"
link which appears in the sidebar of each page. When you click "Commit changes,"
your changes will be built and pushed to the live copy of the manual that you're
looking at. It might take a minute or two before your changes are live.

Alternatively, if you're more comfortable on the command line, you can check out
the repository and work on the files on your own computer::

    git clone git@github.com:okfn/opsmanual
    cd opsmanual
    vim opsmanual-getting-started.rst
    git commit -am "Improve the getting started guide"
    git push

You can also build the project locally by running ``make html`` from the root of
the project. The built site will be placed in ``_build/html``. You don't need to
do this, but if you're making large edits it can help you preview your changes
before pushing.

.. _Sphinx: http://sphinx-doc.org
.. _GitHub: https://github.com
.. _a git repository: https://github.com/okfn/opsmanual

Ops Manual Structure
--------------------

The manual is divided into a few sections, each broadly aimed at a particular
group in a particular role. Existing sections include:

``guides/``
  Guides to getting stuff done at the Open Knowledge Foundation. Most people
  will want to start here.
``sysadmin/``
  Internal documentation and content aimed primarily at members of the OKF
  sysadmin team.

The tables of contents that form the main navigational structure of the site are
maintained in ``index.rst`` files at various points. See the ``toctree``
directives in, for example, `index.rst </_sources/index.txt>`__ or
`2nd-line/index.rst </_sources/2nd-line/index.txt>`__. If you're adding a new
page you should ensure it's included in at least one ``toctree`` so it's
discoverable from the root of the site. (Although even if you don't add it to a
ToC, it will still show up in the site search.)

reStructuredText
----------------

The manual pages are written in a format called reStructuredText_, or rST. At
its simplest, rST is much like the Markdown_ format with which you may be more
familiar. You can view the rST source of any page on this site by clicking the
"Show Source" link in the left-hand column.

Some useful resources for getting started editing rST documents:

- `Quick reStructuredText <http://docutils.sourceforge.net/docs/user/rst/quickref.html>`__
- `rST demonstration document <http://docutils.sourceforge.net/docs/user/rst/demo.txt>`__
  (and `its HTML output <http://docutils.sourceforge.net/docs/user/rst/demo.html>`__)
- `Markdown to rST converter <http://johnmacfarlane.net/pandoc/try/?from=markdown&to=rst>`__

.. _reStructuredText: http://docutils.sourceforge.net/rst.html
.. _Markdown: http://daringfireball.net/projects/markdown/

A note on linking
~~~~~~~~~~~~~~~~~

If you want to link to another part of the manual, you can use the ``:doc:``
Sphinx role (`more documentation about this here <http://sphinx-doc.org/markup/inline.html#role-doc>`__)
to reference another file in the repository. For example, you can say::

    More information on widgets can be found in :doc:`manufacturing/widgets`.

This will insert a reference to the ``manufacturing/widgets.rst`` file relative
to the current file.

If you want to reference a particular part of another document, you can do so by
inserting a label::

    .. _a-label:

Note the ``..``, the opening ``_`` and the closing ``:``. The label goes before
the heading you want to link to. It is ESSENTIAL that this label is followed by
a blank line.

You can then refer to the label from anywhere else in the manual using the
``:ref:`` Sphinx role (`more documentation about this here <http://sphinx-doc.org/markup/inline.html#role-ref>`__)::

    You should refer to :ref:`Whatever you want <a-label>` for more information
    on this subject.


The Build
---------

The content you push to GitHub constitutes the "source format" of the manual.
This source content is compiled by Sphinx_ into the web pages you are currently
viewing. This is done by a `Travis CI job
<https://travis-ci.org/okfn/opsmanual/>`__. The output of this job will also
inform you of any rST syntax errors or other problems Sphinx encountered when
building the site.

The build artefacts (a static HTML website) are copied into the ``gh-pages``
branch of the git repository and pushed to GitHub. The manual is then served at:

    http://ops.okfn.org/

