============
Installation
============

Required GFLOW version
------------------------------------------------
Linesink-maker requires GFLOW 2.2 or higher, which has the capability of importing linesink string (LSS) xml files.

Installing python dependencies with Conda
-----------------------------------------
``Linesink-maker`` depends on a number of python packages, many of which have external C library dependencies. The easiest way to install most of these is with a package installer like `Conda`_. A few packages are not available via conda, and must be installed with `pip`_. If you are on the USGS internal network, see the `Considerations for USGS Users`_ section below first.

Download and install a python distribution and Conda-like package installer 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
There are many ways to do this:

    * The `Anaconda python distribution`_ comes with a large selection of popular data science and scientific packages pre-installed.

    * `Miniconda <https://docs.conda.io/en/latest/miniconda.html>`_ is a minimal installer with a much smaller footprint, making it ideal for creating python environments dedicated to specific tasks (a recommended practice).

    * `Mambaforge <https://github.com/conda-forge/miniforge#mambaforge>`_ is like Miniconda, but pre-configured to use the `Mamba`_ installer, and only the `conda-forge <https://conda-forge.org/docs/user/introduction.html>`_ channel for getting packages (more below). If the above two options don't work (for example, the Conda installer fails or gets stuck on the "solve" step), this may be your best option.

  * **Make sure to install Anaconda or Miniconda to your username** (not at the system level). More often than not, installing at the system level (for all users) seems to result in issues with library dependencies (for example, import of ``fiona`` or ``rasterio`` failing because gdal isn't found). It is also good practice to periodically do a `clean uninstall`_ of Anaconda, which at the system level requires admin. privileges.

    * In the installer, at the “Destination Select” step, select “Install for me only.” It should say something about how the software will be installed to your home folder.
    * If your installer skips the “Destination Select” step, when you get to "Installation Type", click “Change Install Location” and then “Install for me only.”


Download an environment file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

  * `requirements.yml`_ for a `conda environment`_ with the minimum packages required to run Linesink-maker, or
  * `gis.yml`_ for a more full set of packages in the python geospatial stack, including Jupyter Notebooks and packages needed for testing, documentation and packaging. Note that the environment described by ``requirements.yml`` is called `lsmaker`, while the environment in ``gis.yml`` is called `gis`.

    .. note::
        To download the above YAML files, simply follow the links to get the raw text and then go to File > Save within your web browser, and save the text as a YAML file (with the `.yaml` or `.yml` extension).

  * Alternatively, clone (`using git`_) or `download`_ the ``linesink-maker`` repository, which includes the two environment files at the root level.
  * Note that both of these environment files contain a ``pip`` section of packages that will be installed with pip, after the ``conda`` packages are installed.

Creating a `Conda environment`_ using `Mamba`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
If you are on the USGS internal network, see the `Considerations for USGS Users`_ section below first.

While Conda can of course be used to create a Conda environment, the Mamba package solver is generally faster and more robust, especially for larger, more complex environments like the included ``requirements.yml``. Mamba is a reimplementation of the conda package manager in C++.

Before using Mamba, you will need to `install it first <https://mamba.readthedocs.io/en/latest/installation.html>`_.

Python packages are available from conda via channels. Conda comes preconfigured to install packages from the default channel, which is maintained by Anaconda, Inc. In general, you may have better luck exclusively using the `conda-forge <https://conda-forge.org/docs/user/introduction.html>`_ channel instead, which is community-based and intended to provide a single location to get any package, with a minimum of hassle. In general, it is bad practice to mix package channels within a single environment. You can read more `here <https://conda-forge.org/docs/user/introduction.html>`__, but to set conda-forge as the default:

.. code-block:: bash

    conda config --add channels conda-forge

.. note::
    If you are having trouble installing Mamba (for example, the conda package solver fails when you try to install it, or takes an excessively long time), you may have better luck uninstalling `Anaconda completely <clean uninstall>`_ and installing `Mambaforge <https://github.com/conda-forge/miniforge#mambaforge>`_ instead, as directed in the Mamba install instructions. Mambaforge solves both of the above problems by providing a minimal python distribution and conda-style package installer that is preconfigured to use both `conda-forge <https://conda-forge.org/docs/user/introduction.html>`_ and `Mamba`_.

Once you have a python distribution and mamba installed, to create the conda environment, open a new Anaconda Command Prompt on Windows or a new terminal window on OSX and point it to the location of ``requirements.yml`` or ``gis.yml`` and enter:

.. code-block:: bash

    mamba env create -f requirements.yml

Building the environment will probably take a while. If the build fails because of an SSL error, fix the problem (see `Considerations for USGS Users`_ below) and either:

    .. note::
        Creating the ``requirements.yml`` environment (or any environment with ``git+https: ...`` installs) requires Git to be installed and visible in the system path where ``env create`` is being run. If Git is installed and somehow not in the system path, it can be added to the system path on Windows 10 without admin. rights via the "environment variables" editor under User Accounts in the Control Panel (Google it).

    a) 	Update the environment

        .. code-block:: bash

            mamba env update -f requirements.yml

    b) 	or remove and reinstall it:

        .. code-block:: bash

            conda env remove -n lsmaker
            mamba env create -f requirements.yml

Keeping the Conda environment up to date
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The python packages and other open source software libraries that Linesink-maker depends on are continually changing. Linesink-maker aims to mostly follow the `Numpy guidelines for package support <https://numpy.org/neps/nep-0029-deprecation_policy.html>`_, which effectively means that the two latest minor versions of Python (e.g. 3.8 and 3.7) and their associated Numpy versions will be supported. However, occasionally backwards compatability with a particular package may be broken in a shorter timeframe, in which case the minimum required version of that package will be specified in the ``requirements.yml`` file. All of this to say that your Conda environment will eventually get out of date. The `Conda documentation <https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html>`_ has instructions for updating packages within a Conda environment, but at some point (perhaps a few times a year) it is good practice to simply delete the environment and rebuild it from the `.yml` file. Every so often, you may also want to reinstall Anaconda after a `clean uninstall`_.

Installing Linesink-maker
---------------------------
There are several ways to install Linesink-maker. Regardless of the method, the installation must be performed in a python
environment with the required dependencies. In the case of the Conda environment created above, the environment must be activated, so that right version of python is called when ``python`` is entered at the command line:

.. code-block:: bash

    conda activate lsmaker

Installing and updating Linesink-maker from `PyPI <https://pypi.org/>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Once a suitable conda environment (that contains ALL of the dependencies) is made and activated, the simplest way to install Linesink-maker is from the Python Package Index using pip.

.. code-block:: bash

    pip install linesink-maker

Subsequent releases of Linesink-maker to PyPI can then be installed with

.. code-block:: bash

    pip install --upgrade linesink-maker

Note that in some situations you may have to ``pip uninstall linesink-maker`` and then ``pip install linesink-maker``. You can always check
what version of Linesink-maker you have within a python session with

.. code-block:: python

    import lsmaker
    lsmaker.__version__

Or if you are using Conda, at the command line with

.. code-block:: bash

    conda list

Installing the latest develop version of Linesink-maker
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In some situations you may want the bleeding-edge version of Linesink-maker that is being actively developed on GitHub. For example,
to incorporate a bug fix that was made after the latest release. Pip can also be used to fetch Linesink-maker directly from GitHub:

.. code-block:: bash

    pip install git+git://github.com/doi-usgs/linesink-maker@develop

(for the develop branch). Subsequent updates can then be made with

.. code-block:: bash

    pip uninstall linesink-maker
    pip install git+git://github.com/doi-usgs/linesink-maker@develop

Installing the Linesink-maker source code in-place
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Finally, if you intend to contribute to Linesink-maker (please do!) or update your install frequently, the best route is probably to clone the source code from git and install it in place.

.. code-block:: bash

    git clone https://github.com/doi-usgs/linesink-maker.git
    cd linesink-maker
    pip install -e .

.. note::
    Don't forget the ``.`` after ``pip install -e``!

Your local copy of the Linesink-maker repository can then be subsequently updated with

.. code-block:: bash

    git pull origin develop

.. note::
    If you are making local changes to Linesink-maker that you want to contribute, the workflow is slightly different. See the :ref:`Contributing to Linesink-maker` page for more details.


The advantage of installing the source code in-place is that any changes you make are automatically incorporated into your python environment, without any additional install commands. When debugging in an interactive development environment (IDE) such as Pycharm or VS Code, error tracebacks and inspection features go to the actual source code, not the version installed in the ``site-packages`` folder. Additionally, since this install is done through pip, ``pip uninstall``
will work to remove the package, and the current version of the package (including the latest commit information) will be visible with ``conda list``.

Installing the IPython kernel to use Linesink-maker in Jupyter Notebooks
------------------------------------------------------------------------------------------------
This step may not be needed if you already have an existing Python environment with the packages required by Linesink-maker *and* Jupyter Notebook installed. However, if you'd like to use Linesink-maker in a Jupyter Notebook with the included ``lsmaker`` environment (specified in ``requirements.yml``), you'll most likely need to install the IPython kernel in that environment. You can do this at the command line or terminal window (with ``lsmaker`` activated):

.. code-block:: bash

    python -m ipykernel install --user --name lsmaker --display-name "lsmaker"


The first instance of ``lsmaker`` in this command is the environment to install the kernel to, and the second instance (in quotes) is the name that will appear in the ``Kernel`` menu within Jupyter Notebook. To use the kernel, simply select it from the ``Kernel > Change kernel`` menu within  Jupyter Notebook. 


Best practices
------------------------

* Install the \*conda distribution of your choice to your user account, NOT at the system level. Installing to your user means you have rights to delete and reinstall Anaconda as-needed, as well as to edit any configuration files for pip, Conda, etc. Installing at the system level also just seems to lead to more confusing problems with dependencies, at least in the USGS.
* Periodically (maybe a few times a year?) fully remove your \*conda distribution and reinstall it. If you just can't get things to work (packages won't import or produce DLL errors on import, adding or upgrading a package takes a very long time or results in excessive upgrades or downgrades of other packages, etc.), fully removing and reinstalling \*conda just may resolve your issues.
* Don't use your base environment; create and delete environments as needed. Conda is generally pretty good about managing packages between environments without wasting a lot of disk space.
* Use an environment file (as above) to create a conda environment, instead of installing packages ad-hoc.
* Use Mamba instead of Conda; it just works better for environments with a lot of packages.
* After setting up the above conda environment, scan the screen output to make sure that everything installed correctly, especially the packages installed through pip.
* Avoid mixing package channels within a Conda environment. Strictly sticking to conda-forge may yield the best results.
* Use `conda-pack`_, rather than an overly-detailed environment file, to guarantee reproducibility.


_`Considerations for USGS Users`
--------------------------------
Using conda or pip on the USGS network requires SSL verification, which can cause a number of issues.
If you are encountering persistant issues with creating the conda environment,
you may have better luck trying the install off of the USGS network (e.g. at home).
See `here <https://tst.usgs.gov/applications/application-and-script-signing/>`_ for more information
about SSL verification on the USGS network, and to download the DOI SSL certificate.

_`Installing the DOI SSL certificate for use with pip`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
1) `Download the DOI SSL certificate`_
2) On Windows, create the file ``C:\Users\<your username>\AppData\Roaming\pip\pip.ini``.
   On OSX, create ``/Users/<your username>/Library/Application Support/pip/pip.conf``.

Include the following in this file:

::

    [global]
    cert = <path to DOI certificate file (e.g. DOIRootCA2.cer)>

Note that when you are off the USGS network, you may have to comment out the ``cert=`` line in the above pip configuration file to get ``pip`` to work.

Installing the DOI SSL certificate for use with conda
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
See `these instructions <https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html#ssl-verification-ssl-verify>`_.
This may or may not work. Basically, ``ssl_verify:`` needs to be set in your `condarc`_ file to point
to a valid SSL certificate, which may be different from the basic ``DOIRootCA2.cer`` file.

You can find the location of your `condarc`_ file with::

    conda info -a

which displays information about how Conda is configured. Note that you may have multiple `condarc`_
files at the system, user and possibly project levels.

Common issues:

* Conda Install fails on the USGS network without a certificate, or with an incorrectly formatted certificate.
  Possible solutions are to either get a correctly formatted SSL certificate from your IT person, or try installing off the network.
* Conda Install fails off the USGS network with a certificate (may or may not be correctly formatted). Solution:
  open your `condarc`_ file
  and comment out the SSL certificate file, if it is specified. E.g.::

    ssl_verify: #D:\certificates\DOIRootCA2.cer



Troubleshooting issues with the USGS network
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

SSL-related error messages when using conda
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
(with ``SSL`` mentioned in the message and possibly ``bad handshake``)

Make sure that the ``conda`` package installer is configured to use the USGS certificate
(see :ref:`Installing the DOI SSL certificate for use with conda` above).


SSL-related error messages when using pip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
(something similar to ``SSL: CERTIFICATE_VERIFY_FAILED``).

Make sure that the ``pip`` package installer is configured to use the USGS certificate
(see `Installing the DOI SSL certificate for use with pip`_ above).

If you are on the USGS network, using Windows, and you get this error message:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
..

    CondaHTTPError: HTTP 500 INTERNAL ERROR for url <https://repo.anaconda.com/pkgs/msys2/win-64/m2w64-gettext-0.19.7-2.tar.bz2>
    Elapsed: 00:30.647993

    An HTTP error occurred when trying to retrieve this URL.
    HTTP errors are often intermittent, and a simple retry will get you on your way.

Adding the following line to ``environment.yml`` should work:

.. code-block:: yaml

    - msys2::m2w64-gettext


This tells conda to fetch ``m2w64-gettext`` from the ``msys2`` channel instead. Note that this is only a dependency on Windows,
so it needs to be commented out on other operating systems (normally it wouldn't need to be listed, but the above HTTP 500 error indicates that installation from the default source location failed.)


.. _Anaconda python distribution: https://www.anaconda.com/distribution/
.. _clean uninstall: https://docs.anaconda.com/anaconda/install/uninstall/
.. _conda: https://docs.conda.io/en/latest/
.. _Mamba: https://mamba.readthedocs.io/en/latest/
.. _conda environment: https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/environments.html
.. _conda-pack: https://conda.github.io/conda-pack/
.. _condarc: https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html
.. _download: https://github.com/doi-usgs/linesink-maker/archive/develop.zip
.. _gis.yml: https://raw.githubusercontent.com/doi-usgs/linesink-maker/develop/gis.yml
.. _Download the DOI SSL certificate: https://tst.usgs.gov/applications/application-and-script-signing/
.. _pip: https://packaging.python.org/tutorials/installing-packages/#use-pip-for-installing
.. _Readme file: https://github.com/doi-usgs/linesink-maker/blob/develop/Readme.md
.. _requirements.yml: https://raw.githubusercontent.com/doi-usgs/linesink-maker/develop/requirements.yml
.. _using git: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git


.. _linesink-maker repository: https://github.com/doi-usgs/linesink-maker
