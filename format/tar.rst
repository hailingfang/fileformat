Tar Format
========================

.. code::

    def filetype binary;
    
    [+]{
        [1]{
            [100]<char>(name="file_name"; string; encode="unicode"; have_string_end)
            [8]<char; [7]=0>(name="mode"; encode="ascii")
            [8]<char; [7]=0>(name="uid"; string; encode="ascii"; have_string_end;
                             stringtype="integer"; base="8")
            [8]<char; [7]=0>(name="gid"; string; encode="ascii"; have_string_end;
                             stringtype="integer"; base="8")
            [12]<char; [11]=0; :file_size>(name="file_size"; 
                                           string; encode="ascii"; have_string_end;
                                           stringtype="integer"; base="8")
            [12]<char; [11]=0>(name="modification time"; 
                               string; encode="ascii"; have_string_end;
                               stringtype="integer"; base="8")
            [8]<char; [7]=0>(name="header checksum")
            [1]<char; :file_type>(name="file type";
                    id="file_type";
                    element_value_choices={0, '0', '1', '2', '3', '4', '5', '6', '7'};
                    encode="ascii"; stringtype="integer"; base="8")
            [100]<char>(name="Target name for links"; encode="ascii"; string_end_with="0")
            [6]<char; =['u', 's', 't', 'a', 'r', '\0']>
                (name="magic"; string; encode="ascii"; have_string_end;
                 string_value="ustar")
            [2]<char>(name="version")
            [32]<char>(name="user name"; string; encode="ascii"; have_string_end)
            [32]<char>(name="group name"; string; encode="ascii"; have_string_end)
            [8]<char>(name="devmajor")
            [8]<char>(name="devminor")
            [155]<char>(name="prefix")
            [12]<char>(name="padding")
        }(name="file_header", byte_length="512") 
        
        if (file_type == '0' || file_type == 0) {
            file_size = int(file_size, base=8);
            padding_size = (file_size % 512)? 512 - (file_size % 512): 0;
            [file_size]<byte>(name="filedata")
            [padding_size]<byte; [:]=0>
        }(name="regular_file_data")
        
        else {
            file_size = int(file_size, base=8);
            assert file_size == 0;
            [0]<byte>
        }
    
    }(name="data_block")

    [2]{
        [512]<byte; [:]=0>
    }(name="end_of_tar")