# Gemnasium Enterprise Docs Sources

This repositoring hosts the [ Gemnasium Enterprise ](http://enterprise.gemnasium.com) documentation files.
The documentation is generated automatically by [readthedocs](https://readthedocs.org), for each commit.

## Generate files locally

Install sphinx, and sphinx-autobuild to live build the docs:

     pip install sphinx sphinx-autobuild sphinx_rtd_theme

Build the doc:

    cd docs
    make html

To serve the documentation site, with live-reload:

    cd docs
    make livehtml
