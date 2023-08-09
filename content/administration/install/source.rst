==============
Source install
==============

The source "installation" is really about not installing Odoo, and running it directly from source
instead.

It can be more convenient for module developers as the Odoo source is more easily accessible than
using packaged installation.

It also makes starting and stopping Odoo more flexible and explicit than the services set up by the
packaged installations, and allows overriding settings using
:ref:`command-line parameters <reference/cmdline>` without needing to edit a configuration file.

Finally it provides greater control over the system's set up, and allows to more easily keep
(and run) multiple versions of Odoo side-by-side.

Fetch the sources
-----------------

There are two ways to obtain the source code of Odoo: as a zip **archive** or through **git**.

Archive
~~~~~~~

Community Edition:

* `Official download page <download_>`_
* `GitHub repository <community-repository_>`_
* `Nightly server <nightly_>`_

Enterprise Edition:

* `Official download page <download_>`_
* `GitHub repository <enterprise-repository_>`_

.. _setup/install/source/git:

Git
~~~

The following requires `Git <git_>`_ to be installed on your machine and that you have basic
knowledge of Git commands. To clone a Git repository, you must choose between cloning with HTTPS or
SSH. If you do not know the difference between the two, the best option is most likely HTTPS. If you
are following the :doc:`Getting started </developer/tutorials/getting_started>` developer tutorial,
or plan on contributing to Odoo source code, choose SSH.

.. tabs::

   .. group-tab:: Windows

      .. tabs::

         .. tab:: Clone with HTTPS

            .. code-block:: doscon

               C:\> git clone https://github.com/odoo/odoo.git
               C:\> git clone https://github.com/odoo/enterprise.git

         .. tab:: Clone with SSH

            .. code-block:: doscon

               C:\> git clone git@github.com:odoo/odoo.git
               C:\> git clone git@github.com:odoo/enterprise.git

   .. group-tab:: Linux

      .. tabs::

         .. tab:: Clone with HTTPS

            .. code-block:: console

               $ git clone https://github.com/odoo/odoo.git
               $ git clone https://github.com/odoo/enterprise.git

         .. tab:: Clone with SSH

            .. code-block:: console

               $ git clone git@github.com:odoo/odoo.git
               $ git clone git@github.com:odoo/enterprise.git

   .. group-tab:: Mac OS

      .. tabs::

         .. tab:: Clone with HTTPS

            .. code-block:: console

               $ git clone https://github.com/odoo/odoo.git
               $ git clone https://github.com/odoo/enterprise.git

         .. tab:: Clone with SSH

            .. code-block:: console

               $ git clone git@github.com:odoo/odoo.git
               $ git clone git@github.com:odoo/enterprise.git

.. note::
   **The Enterprise git repository does not contain the full Odoo source code**. It is only a
   collection of extra add-ons. The main server code is in the Community version. Running the
   Enterprise version actually means running the server from the Community version with the
   addons-path option set to the folder with the Enterprise version. You need to clone both the
   Community and Enterprise repository to have a working Odoo Enterprise installation. See
   :ref:`setup/install/editions` to get access to the Enterprise repository.

.. _setup/install/source/prepare:

Prepare
-------

Python
~~~~~~

.. tabs::

   .. group-tab:: Windows

      Odoo requires Python 3.7 or later to run. Visit `Python's download page <https://www.python.org/downloads/windows/>`_
      to download and install the latest version of Python 3 on your machine.

      During installation, check **Add Python 3 to PATH**, then click **Customize Installation** and make
      sure that **pip** is checked.

      .. note::
         If Python 3 is already installed, make sure that the version is 3.7 or above, as previous
         versions are not compatible with Odoo.

         .. code-block:: doscon

            C:\> python --version

         Verify also that pip_ is installed for this version.

         .. code-block:: doscon

            C:\> pip --version

   .. group-tab:: Linux

      Odoo requires Python 3.7 or later to run. Use your package manager to download and install Python 3
      on your machine if it is not already done.

      .. note::
         If Python 3 is already installed, make sure that the version is 3.7 or above, as previous
         versions are not compatible with Odoo.

         .. code-block:: console

            $ python3 --version

         Verify also that pip_ is installed for this version.

         .. code-block:: console

            $ pip3 --version

   .. group-tab:: Mac OS

      Odoo requires Python 3.7 or later to run. Use your preferred package manager (homebrew_, macports_)
      to download and install Python 3 on your machine if it is not already done.

      .. note::
         If Python 3 is already installed, make sure that the version is 3.7 or above, as previous
         versions are not compatible with Odoo.

         .. code-block:: console

            $ python3 --version

         Verify also that pip_ is installed for this version.

         .. code-block:: console

            $ pip3 --version

PostgreSQL
~~~~~~~~~~

.. tabs::

   .. group-tab:: Windows

      Odoo uses PostgreSQL as database management system. `Download and install PostgreSQL
      <https://www.postgresql.org/download/windows/>`_ (supported version: 12.0 and later).

      By default, the only user is `postgres` but Odoo forbids connecting as `postgres`, so you need
      to create a new PostgreSQL user:

      #. Add PostgreSQL's `bin` directory (by default:
         :file:`C:\\Program Files\\PostgreSQL\\<version>\\bin`) to your `PATH`.
      #. Create a postgres user with a password using the pg admin gui:

         1. Open **pgAdmin**.
         2. Double-click the server to create a connection.
         3. Select :menuselection:`Object --> Create --> Login/Group Role`.
         4. Enter the username in the **Role Name** field (e.g. `odoo`).
         5. Open the **Definition** tab and enter the password (e.g. `odoo`), then click **Save**.
         6. Open the **Privileges** tab and switch **Can login?** to `Yes` and **Create database?**
            to `Yes`.

   .. group-tab:: Linux

      Odoo uses PostgreSQL as database management system. Use your package manager to download and
      install PostgreSQL (supported version: 12.0 and later).

      It can be achieved by executing the following:

      .. code-block:: console

          $ sudo apt install postgresql postgresql-client

      By default, the only user is `postgres` but Odoo forbids connecting as `postgres`, so you need
      to create a new PostgreSQL user:

      .. code-block:: console

        $ sudo -u postgres createuser -s $USER
        $ createdb $USER

      .. note::
         Because your PostgreSQL user has the same name as your Unix login, you will be able to
         connect to the database without password.

   .. group-tab:: Mac OS

      Odoo uses PostgreSQL as database management system. Use `postgres.app
      <https://postgresapp.com>`_ to download and install PostgreSQL (supported version: 12.0 and
      later).

      .. tip::
         To make the command line tools bundled with `postgres.app` available, make sure to setup your
         `$PATH` variable by following the `Postgres.app CLI Tools Instructions
         <https://postgresapp.com/documentation/cli-tools.html>`_.

      By default, the only user is `postgres` but Odoo forbids connecting as `postgres`, so you need
      to create a new PostgreSQL user:

      .. code-block:: console

        $ sudo -u postgres createuser -s $USER
        $ createdb $USER

      .. note::
         Because your PostgreSQL user has the same name as your Unix login, you will be able to
         connect to the database without password.

.. _install/dependencies:

Dependencies
~~~~~~~~~~~~

.. tabs::

   .. group-tab:: Windows

      Before installing the dependencies, you must download and install the `Build Tools for Visual
      Studio <https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019>`_.
      When prompted, select **C++ build tools** in the **Workloads** tab and install them.

      Odoo dependencies are listed in the `requirements.txt` file located at the root of the Odoo
      community directory.

      .. tip::
         It can be preferable to not mix python modules packages between different instances of Odoo
         or with your system. You can use virtualenv_ to create isolated Python environments.

      Navigate to the path of your Odoo Community installation (`CommunityPath`) and run **pip** on
      the requirements file in a terminal **with Administrator privileges**:

      .. code-block:: doscon

          C:\> cd \CommunityPath
          C:\> pip install setuptools wheel
          C:\> pip install -r requirements.txt

      For languages with right-to-left interface (such as Arabic or Hebrew), the package `rtlcss`
      is needed:

      #. Download and install `nodejs <https://nodejs.org/en/download/>`_.
      #. Install `rtlcss`:

         .. code-block:: doscon

             C:\> npm install -g rtlcss

      #. Edit the System Environment's variable `PATH` to add the folder where `rtlcss.cmd` is
         located (typically: :file:`C:\\Users\\<user>\\AppData\\Roaming\\npm\\`).

   .. group-tab:: Linux

      Using your **distribution packages** is the preferred way of installing dependencies.
      Alternatively, you can install the python dependencies with **pip**.

      .. tabs::

         .. tab:: Debian/Ubuntu

            For Debian-based systems, the packages are listed in the `debian/control
            <{GITHUB_PATH}/debian/control>`_ file of the Odoo sources.

            On Debian/Ubuntu, the following commands should install the required packages:

            .. code-block:: console

               $ cd /CommunityPath
               $ sed -n -e '/^Depends:/,/^Pre/ s/ python3-\(.*\),/python3-\1/p' debian/control | sudo xargs apt-get install -y

         .. tab:: Install with pip

            As some of the python packages need a compilation step, they require system libraries to
            be installed.

            On Debian/Ubuntu-based systems, the following command should install these required
            libraries:

            .. code-block:: console

               $ sudo apt install python3-pip libldap2-dev libpq-dev libsasl2-dev

            Odoo dependencies are listed in the :file:`requirements.txt` file located at the root of
            the Odoo community directory.

            .. note::
               | The python packages in :file:`requirements.txt` are based on their stable/LTS
                 Debian/Ubuntu corresponding version at the moment of the Odoo release.
               | E.g., for Odoo 15.0, the `python3-babel` package version is 2.8.0 in Debian
                 Bullseye and 2.6.0 in Ubuntu Focal. The lowest version is then chosen in the
                 :file:`requirements.txt`.

            .. tip::
               It can be preferable to not mix python modules packages between different instances
               of Odoo or with your system. You can use virtualenv_ to create isolated Python
               environments.

            Navigate to the path of your Odoo Community installation (:file:`CommunityPath`) and run
            **pip** on the requirements file to install the requirements for the current user.

            .. code-block:: console

               $ cd /CommunityPath
               $ pip install -r requirements.txt

      For languages with right-to-left interface (such as Arabic or Hebrew), the package `rtlcss` is
      needed:

      #. Download and install **nodejs** and **npm** with your package manager.
      #. Install `rtlcss`:

         .. code-block:: console

            $ sudo npm install -g rtlcss

   .. group-tab:: Mac OS

      Odoo dependencies are listed in the `requirements.txt` file located at the root of the Odoo
      community directory.

      .. tip::
         It can be preferable to not mix python modules packages between different instances of Odoo
         or with your system. You can use virtualenv_ to create isolated Python environments.

      Navigate to the path of your Odoo Community installation (`CommunityPath`) and run **pip** on
      the requirements file:

      .. code-block:: console

         $ cd /CommunityPath
         $ pip3 install setuptools wheel
         $ pip3 install -r requirements.txt

      .. warning::
         Non-Python dependencies need to be installed with a package manager:

         #. Download and install the **Command Line Tools**:

            .. code-block:: console

               $ xcode-select --install

         #. Download and install the package manager of your choice (homebrew_, macports_).
         #. Install non-python dependencies.

      For languages with right-to-left interface (such as Arabic or Hebrew), the package `rtlcss` is
      needed:

      #. Download and install **nodejs** with your preferred package manager (homebrew_, macports_).
      #. Install `rtlcss`:

         .. code-block:: console

             $ sudo npm install -g rtlcss

.. important::
   `wkhtmltopdf` is not installed through **pip** and must be installed manually in version `0.12.5
   <the wkhtmltopdf download page_>`_ for it to support headers and footers. See our `wiki
   <https://github.com/odoo/odoo/wiki/Wkhtmltopdf>`_ for more details on the various versions.

.. _setup/install/source/running_odoo:

Running Odoo
------------

Once all dependencies are set up, Odoo can be launched by running `odoo-bin`, the
command-line interface of the server. It is located at the root of the Odoo Community directory.

To configure the server, you can either specify :ref:`command-line arguments
<reference/cmdline/server>` or a :ref:`configuration file <reference/cmdline/config>`.

.. tip::
   For the Enterprise edition, you must add the path to the `enterprise` addons to the `addons-path`
   argument. Note that it must come before the other paths in `addons-path` for addons to be loaded
   correctly.

Common necessary configurations are:

- PostgreSQL user and password.
- Custom addon paths beyond the defaults, to load your own modules.

A typical way to run the server would be:

.. tabs::

   .. group-tab:: Windows

      .. code-block:: doscon

          C:\> cd CommunityPath/
          C:\> python odoo-bin -r dbuser -w dbpassword --addons-path=addons -d mydb

      Where `CommunityPath` is the path of the Odoo Community installation, `dbuser` is the
      PostgreSQL login, `dbpassword` is the PostgreSQL password, and `mydb` is the name of the
      PostgreSQL database.

   .. group-tab:: Linux

      .. code-block:: console

          $ cd /CommunityPath
          $ python3 odoo-bin --addons-path=addons -d mydb

      Where `CommunityPath` is the path of the Odoo Community installation, and `mydb` is the name
      of the PostgreSQL database.

   .. group-tab:: Mac OS

      .. code-block:: console

          $ cd /CommunityPath
          $ python3 odoo-bin --addons-path=addons -d mydb

      Where `CommunityPath` is the path of the Odoo Community installation, and `mydb` is the name
      of the PostgreSQL database.

After the server has started (the INFO log `odoo.modules.loading: Modules loaded.` is printed), open
http://localhost:8069 in your web browser and log in with the base administrator account: Use
`admin` for the :guilabel:`Email` and, again, `admin` for the :guilabel:`Password`. That's it, you
just logged into your own Odoo database!

.. tip::
   - From there, you can create and manage new :doc:`users
     </applications/general/users/manage_users>`.
   - The user account you use to log into Odoo's web interface differs from the :option:`--db_user
     <odoo-bin -r>` CLI argument.

.. seealso::
   :doc:`The exhaustive list of CLI arguments for odoo-bin </developer/reference/cli>`.
