=======
Install
=======

There are multiple ways to install Odoo, or not install it at all, depending
on the intended use case.

This documents attempts to describe most of the installation options.

:ref:`setup/install/online`
    The easiest way to use Odoo in production or to try it.

:ref:`setup/install/packaged`
    Suitable for testing Odoo, developing modules and can be used for
    long-term production use with additional deployment and maintenance work.

:ref:`setup/install/source`
    Provides greater flexibility:  e.g. allow multiple running Odoo versions on
    the same system. Good for developing modules, can be used as base for
    production deployment.

:ref:`setup/install/docker`
    If you usually use docker_ for development or deployment, an official
    docker_ base image is available.


.. _setup/install/editions:

Editions
========

There are two different Editions_ of Odoo: the Community and Enterprise versions.
Using the Enterprise version is possible on `Odoo Online`_ and accessing the code is
restricted to Enterprise customers and partners. The Community version is freely
available to anyone.

If you already use the Community version and wish to upgrade to Enterprise, please
refer to :ref:`setup/enterprise` (except for :ref:`setup/install/source`).

.. toctree::

   install/online
   install/packages
   install/source
   install/deploy
   install/cdn
   install/email_gateway
