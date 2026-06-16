Executable and Linkable Format
===============================================

.. code::

    [4]<char; =[0x7f, 'E', 'L', 'F']>(name="magic number")
    [1]<uint8; ={1, 2}; :format_marker>(name="fomat marker for 32 or 64")
    [1]<uint8; ={1, 2}>(name="endainness")
    [1]<uint8; =1>(name="version number")
    [1]<uint8; =(0x00, 0x12)>(name="operating system ABI"; id="os_abi")
    [1]<uint8>(name="operating system ABI subdivision")
    [7]<byte>(name="reserved unused bytes")
    
    [1]<uint16>(name="Identifies of file type"; id="file_type")
    [1]<uint16>(name="Specifies target instruction set architecture"; id="instruction_set")
    [1]<uint32>(name="version")

    if (format_marker == 1) {
        [1]<uint32; :e_entry>(name="memory address of the entry point"; id="entry_point")
        [1]<uint32; :e_phoff>(name="Points to the start of the program header table")
        [1]<uint32; :e_shoff>(name="Points to the start of the section header table")
    }
    elif (format_marker == 2) {
        [1]<uint64; :e_entry>(name="memory address of the entry point"; id="entry_point")
        [1]<uint64; :e_phoff>(name="Points to the start of the program header table")
        [1]<uint64; :e_shoff>(name="Points to the start of the section header table")
    }

    [1]<uint32>(name="flage")
    [1]<uint16>(name="header size")
    [1]<uint16; :e_phentsize>(name="size of a program header table entry")
    [1]<uint16; :e_phnum>(name="contains the number of entries in the program header table")
    [1]<uint16>(name="size of a section header table entry")
    [1]<uint16>(name=" number of entries in the section header table")
    [1]<uint16>(name="index of the section header table entry")

    goto (e_phoff) {
        [e_phnum] {
            [1]<uint32>(name="Identifies the type of the segment")
            if (format_marker == 1) {
                [1]<uint32>(name="Offset of the segment in the file image")
                [1]<uint32>(name="Virtual address of the segment in memory")
                [1]<uint32>(name="On systems where physical address is relevant, reserved for segment's physical address")
                [1]<uint32>(name="Size in bytes of the segment in the file image")
                [1]<uint32>(name="Size in bytes of the segment in memory")
                [1]<uint32>(name="Segment-dependent flags (position for 32-bit structure)")
                [1]<uint32>(name="alignment, 0 and 1 specify no alignment")
            }
            elif (format_marker == 2){
                [1]<uint32>(name="Segment-dependent flags")
                [1]<uint64>(name="Offset of the segment in the file image")
                [1]<uint64>(name="Virtual address of the segment in memory")
                [1]<uint64>(name="On systems where physical address is relevant, reserved for segment's physical address")
                [1]<uint64>(name="Size in bytes of the segment in the file image")
                [1]<uint64>(name="ize in bytes of the segment in memory")
                [1]<uint64>(name="alignment, 0 and 1 specify no alignment")
            }
        }(byte_length="e_phentsize")
    }


    [#Note:
        id note:
        os_abi: Identifies the target operating system ABI
            0x00: System V
            0x01: HP-UX
            0x02: NetBSD
            0x03: Linux
            0x04: GNU Hurd
            0x06: Solaris
            0x07: AIX
            0x08: IRIX
            0x09: FreeBSD
            0x0a: Tru64
            0x0b: Novell Modesto
            0x0c: OpenBSD
            0x0d: OpenVMS
            0x0e: NonStop Kernel
            0x0f: AROS
            0x10: FenixOS
            0x11: Nuxi CloudABI
            0x12: Stratus Technologies OpenVOS
        
        file_type: Identifies file type
            0: Unknown
            1: Relocatable file
            2: Executable file
            3: Shared object
            4: Core file

        instruction_set: Specifies target instruction set architecture
            following is a part of all
            0: No specific instruction set
            1: AT&T WE 32100
            2: SPARC
            3: x86
            4: Motorola 68000 (M68k)
            5: Motorola 88000 (M88k)
            62: AMD x86-64
            243: RISC-V

        entry_point: This is the memory address of the entry point from
        where the process starts executing. This field is either 32 or 64 bits
        long, depending on the format defined earlier (byte 0x04). If the file
        doesn't have an associated entry point, then this holds zero.
    ]

    [#Reference:
        https://en.wikipedia.org/wiki/Executable_and_Linkable_Format
    ]

