===================
Packaged installers
===================

Odoo provides packaged installers for Windows, deb-based distributions
(Debian, Ubuntu, …) and RPM-based distributions (Fedora, CentOS, RHEL, …) for
both the Community and Enterprise versions.

These packages automatically set up all dependencies (for the Community version),
but may be difficult to keep up-to-date.

Official Community packages with all relevant dependency requirements are
available on our nightly_ server. Both Community and Enterprise packages can
be downloaded from our download_ page (you must to be logged in as a paying
customer or partner to download the Enterprise packages).

Windows
-------

#. Download the installer from our nightly_ server (Community only) or the Windows installer from
   the download_ page (any edition).
#. Execute the downloaded file.

   .. warning::
      | On Windows 8 and later you may see a warning titled "Windows protected your PC".
      | Click on **More Info** and then on **Run anyway**.

#. Accept the UAC_ prompt.
#. Go through the various installation steps.

Odoo will automatically be started at the end of the installation.

Linux
-----

Prepare
~~~~~~~

.. tabs::

   .. group-tab:: Debian/Ubuntu

      Odoo needs a `PostgreSQL`_ server to run properly. The default configuration for
      the Odoo 'deb' package is to use the PostgreSQL server on the same host as your
      Odoo instance. Execute the following command in order to install the PostgreSQL server:

      .. code-block:: console

         $ sudo apt install postgresql -y

   .. group-tab:: Fedora

      Odoo needs a `PostgreSQL`_ server to run properly. Make sure that the `sudo` command is
      available and well configured and, only then, execute the following command in order to
      install the PostgreSQL server:

      .. code-block:: console

         $ sudo dnf install -y postgresql-server
         $ sudo postgresql-setup --initdb --unit postgresql
         $ sudo systemctl enable postgresql
         $ sudo systemctl start postgresql

.. warning::
   `wkhtmltopdf` is not installed through **pip** and must be installed manually in version `0.12.5
   <the wkhtmltopdf download page_>`_ for it to support headers and footers. See our `wiki
   <https://github.com/odoo/odoo/wiki/Wkhtmltopdf>`_ for more details on the various versions.

Repository
~~~~~~~~~~

.. tabs::

   .. group-tab:: Debian/Ubuntu

      Odoo S.A. provides a repository that can be used with Debian and Ubuntu distributions. It can
      be used to install *Odoo Community Edition* by executing the following commands:

      .. code-block:: console

          $ wget -q -O - https://nightly.odoo.com/odoo.key | sudo gpg --dearmor -o /usr/share/keyrings/odoo-archive-keyring.gpg
          $ echo 'deb [signed-by=/usr/share/keyrings/odoo-archive-keyring.gpg] https://nightly.odoo.com/{CURRENT_MAJOR_BRANCH}/nightly/deb/ ./' | sudo tee /etc/apt/sources.list.d/odoo.list
          $ sudo apt-get update && sudo apt-get install odoo

      You can then use the usual `apt-get upgrade` command to keep your installation up-to-date.

   .. group-tab:: Fedora

      Odoo S.A. provides a repository that can be used with the Fedora distributions. It can be used
      to install *Odoo Community Edition* by executing the following commands:

      .. code-block:: console

         $ sudo dnf config-manager --add-repo=https://nightly.odoo.com/{CURRENT_MAJOR_BRANCH}/nightly/rpm/odoo.repo
         $ sudo dnf install -y odoo
         $ sudo systemctl enable odoo
         $ sudo systemctl start odoo

.. note::
   At this moment, there is no nightly repository for the Enterprise Edition.

Distribution package
~~~~~~~~~~~~~~~~~~~~

.. tabs::

   .. group-tab:: Debian/Ubuntu

      Instead of using the repository as described above, the 'deb' packages for both the
      *Community* and *Enterprise* editions can be downloaded from the `official download page
      <download_>`_.

      .. note::
         Odoo {CURRENT_MAJOR_VERSION} 'deb' package currently supports `Debian 11 (Bullseye)`_,
         `Ubuntu 22.04 (Jammy)`_ or above.

      Next, execute the following commands **as root**:

      .. code-block:: console

         # dpkg -i <path_to_installation_package> # this probably fails with missing dependencies
         # apt-get install -f # should install the missing dependencies
         # dpkg -i <path_to_installation_package>

      This will install Odoo as a service, create the necessary PostgreSQL_ user
      and automatically start the server.

      .. warning::
         - The `python3-xlwt` Debian package does not exists in Debian Buster nor Ubuntu 18.04. This
           python module is needed to export into xls format.

           If you need the feature, you can install it manually with:

           .. code-block:: console

              $ sudo pip3 install xlwt

         - The `num2words` python package does not exists in Debian Buster nor Ubuntu 18.04. Textual
           amounts will not be rendered by Odoo and this could cause problems with the `l10n_mx_edi`
           module.

           If you need this feature, you can install manually with:

           .. code-block:: console

              $ sudo pip3 install num2words

   .. group-tab:: Fedora

      Instead of using the repository as described above, the 'rpm' packages for both the
      *Community* and *Enterprise* editions can be downloaded from the `official download page
      <download_>`_.

      .. note::
         Odoo {CURRENT_MAJOR_VERSION} 'rpm' package supports Fedora 36.

      Once downloaded, the package can be installed using the 'dnf' package manager:

      .. code-block:: console

         $ sudo dnf localinstall odoo_{CURRENT_MAJOR_BRANCH}.latest.noarch.rpm
         $ sudo systemctl enable odoo
         $ sudo systemctl start odoo
