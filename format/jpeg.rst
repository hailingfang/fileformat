GPEG File Format
============================

.. code::

    [2]<byte; =[0xff, 0xd8]>(name="start marker"; id="SOI")

    while (1) {
        [2]<byte; :marker>
        if (marker == [0xff, 0xe0]) {
            [1]<uint16; data_len>(name="payload length"; endian="big")
            [data_len - 2]<byte>
            continue;
        }(name="app0")

        if (marker == [0xff, 0xdb]) {
            [1]<uint16; data_len>(name="payload length"; endian="big")
            [data_len - 2]<byte>
            continue;
        }(name="Define Quantization Table")

        if (marker == [0xff, 0xc0]) {
            [1]<uint16; data_len>(name="payload length"; endian="big")
            [data_len - 2]<byte>
            continue;
        }(name="Baseline JPEG"; class="start_of_frame")

        if (marker == [0xff, 0xc1]) {
            [1]<uint16; data_len>(name="payload length"; endian="big")
            [data_len - 2]<byte>
            continue;
        }(name="Extended Sequential"; class="start_of_frame")

        if (marker == [0xff, 0xc2]) {
            [1]<uint16; data_len>(name="payload length"; endian="big")
            [data_len - 2]<byte>
            continue;
        }(name="Progressive JPEG"; class="start_of_frame")

        if (marker == [0xff, 0xc4]) {
            [1]<uint16; data_len>(name="payload length"; endian="big")
            [data_len - 2]<byte>
            continue;
        }(name="Define Huffman Table")

        if (marker == [0xff, 0xdd]) {
            [1]<uint16; data_len>(name="payload length"; endian="big")
            [data_len - 2]<byte>
            continue;
        }(name="Define Restart Interval")

        if (marker == [0xff, 0xda]) {
            [1]<uint16; data_len>(name="payload length"; endian="big")
            [data_len - 2]<byte>
            break;
        }(name="Start of Scan")
    }

    [+]<byte>(name="Entropy-coded scan data";
              note="all 0xff bytes in original are stored as 0xff 0x00")
    
    [*]{
        [2]<byte; [0]=0xff; [1]=(0xd0, 0xd7)>(name="marker")
        [+]<byte>(note="all 0xff bytes in original are stored as 0xff 0x00")
    }(name="Restart Markers")

    [2]<byte; =[0xff, 0xd9]>(name="end marker"; id="EOI")

