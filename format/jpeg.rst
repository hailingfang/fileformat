GPEG File Format
============================

.. code::

    [# JPEG]
    [2]<byte; =[0xff, 0xd8]>(id="start_marker")
    []{
        [2]<byte; =[0xff, 0xe0]>
        [2]<byte; =[0xff, 0xe1]>
    }(element_number="?"; element_exclusive=True)

    






    [#Notes:
        mark_byte:
            

    ]
