Getting Started
=====

.. _Download:

Download
------------

Github$B$N%Z!<%8$NNP?'$N(BCode$B$H$$$&%\%?%s$+$i(BDownload ZIP$B$r2!$9!"$b$7$/$O(B
.. code-block:: console
  git clone

.. code-block:: console

   (.venv) $ pip install lumache

Creating recipes
----------------

To retrieve a list of random ingredients,
you can use the ``lumache.get_random_ingredients()`` function:

.. autofunction:: lumache.get_random_ingredients

The ``kind`` parameter should be either ``"meat"``, ``"fish"``,
or ``"veggies"``. Otherwise, :py:func:`lumache.get_random_ingredients`
will raise an exception.

.. autoexception:: lumache.InvalidKindError

For example:

>>> import lumache
>>> lumache.get_random_ingredients()
['shells', 'gorgonzola', 'parsley']

