PE (Portable Executable)

.. code::

    [2]<char; =['M', 'Z']>(name="magic number")
    [*]<byte>(name="DOS stub")
    [4]<char; =['P', 'E', 0, 0]>(name="PE start marker")
    {
        [1]<uint16>(name="machine")
        [1]<uint16>(name="number of sections")
        [1]<uint32>(name="time date stamp")
        [1]<uint32>(name="pointer to symbol table")
        [1]<uint32>(name="number of symbols")
        [1]<uint16>(name="size of optional header")
        [1]<uint16; :characteristics>(name="characteristics")
    }(name="COFF Header")

    if (characteristics == 0x0002) {
    
    
    }