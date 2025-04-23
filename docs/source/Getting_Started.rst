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
         |-- Configs          # 環境設定用ファイル
         |-- works            # 各テスト計算を格納
              |-- shock_tube  # shocktube
              |-- HII         # 電離領域膨張
              |-- BE          # Bonnor-Ebert 球
              |-- GMC         # 星団形成

基本的に、コードの実行はworks以下の各ディレクトリ内部で行う。

.. _環境設定:

環境設定
----------------

環境設定は、 ``Configuration.defs`` で行う。

.. code-block:: console

  #################################################
  ##                                             ##
  #               Configuration file              #
  ##                                             ##
  #################################################
  SFUMATO_DIR   = /home/hogehoge/SFUMATO_sharp
  SRC_DIR       = $(SFUMATO_DIR)/src
  WORK_DIR      = $(SFUMATO_DIR)/works/GMC
  DATA_DIR      = $(WORK_DIR)/data
  
  GENCODE_SM80    := -gencode arch=compute_80,code=sm_80
  GENCODE_SM90    := -gencode arch=compute_90,code=sm_90
  GENCODE_FLAGS   := $(GENCODE_SM80) $(GENCODE_SM90)
  
  
  ##################
  ## Compiler     ##
  ##################
  NVCC  = nvcc
  CC    = mpic++
  
  CFLAGS     = -std=c++11
  MPICFLAGS  = -I${MPI_HOME}/include
  CUDACFLAGS = -I${CUDA_INSTALL_PATH}/include
  
  NVCCFLAGS   = $(GENCODE_FLAGS) --default-stream per-thread -std=c++11 -lmpi -lcudadevrt -lcudart -Xcompiler -fopenmp
  CUDALDFLAGS = -I${CUDA_INSTALL_PATH}/include -L${CUDA_INSTALL_PATH}/lib64 -lcudart -lgomp
  
  #################
  ## Options     ##
  #################
  INCLUDE_SELF_GRAVITY = 1      # if 1 == ON
  INCLUDE_CHEMISTRY    = 1
  INCLUDE_RADTR        = 1

各項目の詳細:

  ``SFUMATO_DIR`` 
  計算実行するSFUMATOのdirectoryへの絶対パス
  
  ``SRC_DIR``
  SRC directoryへの絶対パス。異なるsrcで計算したい場合は変更する

  ``WORK_DIR``
  計算を実行するdirectoryへの絶対パス

  ``DATA_DIR``
  データを保存するdirectoryへの絶対パス

  ``GENCODE_FLAGS``
  NVIDIAのGPUで実行する際に計算を実行するGPUにあったarchitecturesを指定する。詳細については `こちら <https://qiita.com/k_ikasumipowder/items/1142dadba01b42ac6012>` を確認して、計算実行するGPUにあったものを選択する。

  


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

