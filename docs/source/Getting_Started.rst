Getting Started
=====

.. _Download:

Download
------------

Githubのサイトで右上の緑色のCodeのDownload ZIPを押すか、もしくは

.. code-block:: console

    git clone

でダウンロードすることが可能。その後実行する環境下へ転送する。

.. _Directory:

Directory
----------------

ディレクトリの構造は以下のようになっている.

.. code-block:: console

  SFUMATO_sharp/
         |-- src              # ソースコード
         |-- julia            # 解析用
         |-- works            # 各テスト計算を格納
              |-- shock_tube  # shocktube
              |-- HII         # 電離領域膨張
              |-- BE          # Bonnor-Ebert 球
              |-- GMC         # 星団形成

実行したいテスト計算まで移動したのちに、コードのコンパイルを行う。

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

