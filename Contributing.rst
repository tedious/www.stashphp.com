.. _contributing:

============
Contributing
============

Contributions Welcome!
======================

Pull Requests and Community Contributions are the bread and butter of open source software. Every contribution- from bug
reports to feature requests, typos to full new features- are greatly appreciated.

Important Guidelines
====================

-  One Item Per Pull Request or Issue. This makes it much easier to review code and merge it back in, and prevents
   issues with one request from blocking another.

-  Make sure to update the CHANGELOG.md file with your changes.

-  Code Coverage is extremely important, and pull requests are much more likely to be accepted if testing is also
   improved. New code should be properly tested, and all tests must pass.

-  Read the LICENSE document and make sure you understand it, because your code is going to be released under it.

-  Be prepared to make revisions. Don't be discouraged if you're asked to make changes, as that is just another step towards refining the code and getting it merged back in.

-  Remember to add the relevant documentation, particular the docblock comments.

Code Styling
============

This project follows the PSR standards set forth by the `PHP Framework
Interop Group <http://www.php-fig.org/>`__.

-  `PSR-1: Basic coding standard <http://www.php-fig.org/psr/psr-1/>`__
-  `PSR-2: Coding style guide <http://www.php-fig.org/psr/psr-2/>`__
-  `PSR-3: Logger Interface <http://www.php-fig.org/psr/3/>`_.
-  `PSR-4: Autoloader <http://www.php-fig.org/psr/4/>`_.
-  `PSR-6: Caching Interface <http://www.php-fig.org/psr/6/>`_.


All code most follow these standards to be accepted. The easiest way to accomplish this is to run php-cs-fixer once the
new changes are finished. The php-cs-fixer package is installed as a development dependency of this project.

.. code-block:: bash

    composer install --dev
    vendor/bin/php-cs-fixer fix ./ -vv

Running the test suite
======================

First install dependencies using Composer. It's important to include the dev packages:

.. code-block:: bash

    composer install --dev

The "runTests.sh" script runs the full test suite- phpunit,
php-cs-fixer, as well as any environmental setup:

.. code-block:: bash

    ./tests/runTests.sh

To call phpunit directly:

.. code-block:: bash

    ./vendor/bin/phpunit

To call php-cs-fixer directly:

.. code-block:: bash

    ./vendor/bin/php-cs-fixer fix ./ -vv --dry-run



