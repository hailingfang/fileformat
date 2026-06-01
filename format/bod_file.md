BOD file  
==================

Oii file
------------------

Stored individual information.

.. code::

    def filetype plaintext;
    def encode ascii;
    export line_num;
    export oii_order;

    [+:line_num]{
        [1]<string>(name="family ID")
        [1]<string>(name="individual ID")
        [1]<string>(name="patertal ID")
        [1]<string>(name="maternal ID")
        [1]<string>(name="sex"; datatype=int; NA="0";
                    choices={"1", "2", "0"}; id="sex")
    }(element_end="\n"; element_sub_sep="\t"; order=oii_order)

    [# Notes
        sex: 1 for male; 2 for female; 0 for unknown.
    ]


Opi file
----------------

Storded probe information.

.. code::

    def filetype plaintext;
    def encode ascii;
    export line_num;
    export opi_order;

    [+:line_num]{
        [1]<string>(name="chromosome ID/name")
        [1]<string>(name="probe ID")
        [1]<string>(name="physical position")
        [1]<string>(name="gene ID")
        [1]<string>(name="gene orientation")
    }(element_end="\n"; element_sub_sep="\t"; order=opi_order)


Bod file
-------------------

.. code::

    def filetype binary;
    def endianness little;
    import oii_format;
    import opi_format;

    [1]<char>(name="value type"; choices={0, 1, 2}; id="value_type")
    [1]<char>(name="data type"; choices={0, 1, 2}; id="data_type")
    [2]<char; =0>(name="reserved bytes")
    [1]<uint32; :indi_num>(name="number of individuals")
    [2]<uint32; :probe_num>(name="numer of probes")
    assert indi_num == oii_format.line_num;
    assert probe_num == opi_format.line_num;
    [probe_num]{
        [indi_num]<double>(align_with=oii_format.oii_order)
    }(align_with=opi_format.opi_order)


