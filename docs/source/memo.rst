cudaにおける注意点
=====

cudaを書く際の注意点をメモしてます。

.. _new/delete:

new/delete
------------

kernel内では、配列の動的な割り当ては行わない方がよい。バグとなる。


.. _debugger:

debugger
------------

cudaでコンパイルした実行コードに

.. code-block:: console

  compute-sanitizer ./sfumato


のように、 ``compute-sanitizer`` を使用するとdebugできる(ただし、hostポインターをMPI通信する際に生じるエラーが大量にでる)。

