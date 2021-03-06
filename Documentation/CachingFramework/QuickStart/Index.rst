.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../../Includes.txt



.. _caching-quickstart:

Quick start for Integrators
^^^^^^^^^^^^^^^^^^^^^^^^^^^

This section gives come simple instructions for getting started with using
the caching framework without giving the whole details under the hood.


.. _caching-quickstart-tuning:

Change specific cache options
"""""""""""""""""""""""""""""

By default, most core caches use the database backend. Default cache configuration
is defined in :file:`t3lib/stddb/DefaultConfiguration.php` and can be overridden in :file:`LocalConfiguration.php`.

If specific settings should be applied to the configuration, they should be added to :file:`LocalConfiguration.php`.
All settings in :file:`LocalConfiguration.php` will be merged with :file:`DefaultConfiguration.php`. The easiest way to see
the final cache configuration is to use the TYPO3 Backend module Admin Tools > Configuration > $TYPO3_CONF_VARS.

Example for a configuration of redis cache backend on redis database number 42 instead of the default
database backend with compression for the pages cache:


.. code-block:: php

   return array(
   ...
      'SYS' => array(
      ...
         'caching' => array(
            ...
            'cache_pages' => array(
               'backend' => 'TYPO3\CMS\Core\Cache\Backend\RedisBackend',
               'options' => array(
                  'database' => 42,
               ),
            ),
         ),
      ),
   );

.. _caching-quickstart-garbage:

Garbage collection task
"""""""""""""""""""""""

Most cache backends do not have an internal system to remove old cache entries that exceeded their lifetime.
A cleanup must be triggered externally to find and remove those entries, otherwise caches could grow to
arbitrary size. This could lead to a slow website performance, might sum up to significant hard disk or
memory usage and could render the server system unusable.

It is advised to always enable the scheduler and run the "Caching framework garbage collection" task to retain
clean and small caches. This housekeeping could be done once a day when the system is otherwise mostly idle.