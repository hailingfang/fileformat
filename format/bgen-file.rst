BGEN format
================

version 1.2

.. code::

    def filetype binary;
    def endianness little;

    [1]<uint32; :offset_to_first>(name="offset to the first data block")
    [1]<uint32; :header_size>(name="header size in byte")
    [1]<uint32; :variants_num>(name="number of variants")
    [1]<uint32; :sample_num>(name="number of samples")
    [4]<char; =['b', 'g', 'e', 'n']>(name="magic number")
    [header_size - 20]<byte>(name="conserved bytes")

    #flags
    [2]<bit>(name="compression flat"; choices={0, 1}; id="compression")
    [4]<bit>(name="layout type"; id="layout_type"; choices={0, 1, 2})
    [25]<bit>(name="not defined bits")
    [1]<bit; :sample_data>(name="indication for whether including sample indentifier data"; choices={0, 1}; id="sample_data")

    if (sample_data) {
        [1]<uint32>(name="size of sample data block in byte")
        [1]<uint32; :sample_num_2>(name="sample name")
        assert sample_num == sample_num_2;
        [sample_num] {
            [1]<uint16; :indentifier_len>(name="length of indentifier of one sample")
            [indentifier_len]<char>(name="indentifier")(encode=ascii)
        }
    }

    [variants_num]{
        [1]<uint32>
        [1]<uint16; :vari_id_len>(name="length of variant identifier")
        [vari_id_len]<char>(name="the variant identifier"; encode=ascii)
        [1]<uint16; :rsid_len>(name="characteristic lenght of rsid")
        [rsid_len]<char>(name="rsid"; encode=ascii)
        [1]<uint16; :len_chrom> (name="lenght of chromosome string")
        [len_chrom]<char>(name="chromosome"; encode="ascii")
        [1]<uint32>(name="the variant position")
        [1]<uint16; :allel_num>(name="number of allels")
        [allel_num] {
            [1]<uint32; :allel_len>(name="length of allel string")
            [allel_len]<char>(name="the allel string", encode=ascii)
        }     
    }

    if (layout == 1) {
        if (compression == 1) {
            [1]<uint32; compressed_data_len>(name="size of compressed genotype data")
            [compressed_data_len]<byte>(name="compressed data, uing zlib compressed")
        }
        if (compression == 0) {
            [variants_num * 6]<byte>(name="variants_num")
        }
    }

    if (layout == 2) {
        [1]<uint32; data_size>(name="the offset to the next variant block or end of file")     
            if (compress_flg == 1) {
                [1]<uint32; :decompressed_size>(name="size after decompress")
                [data_size - 4]<byte; :vair_data_compressed>(name="compressed data")
            }
            if (compression == 0) {
                [data_size]<byte; :vair_data_ori>(name="data which not compressed")
            }
    }

