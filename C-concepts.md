**Flexible array member**

C struct data types may end with a **flexible array member** with no specified size:

    struct vectord {
        short len;    // there must be at least one other data member
        double arr[]; // the flexible array member must be last
        // The compiler may reserve extra padding space here, like it can between struct members
    };

Typically, such structures serve as the header in a larger, variable memory allocation:

    struct vectord *vector = malloc(...);
    vector->len = ...;
    for (int i = 0; i < vector->len; i++)
        vector->arr[i] = ...;  // transparently uses the right type (double)


**Structure Alignment**


Here is a structure with members of various types, totaling 8 bytes before compilation:

    struct MixedData
    {
        char Data1;
        short Data2;
        int Data3;
        char Data4;
    };

After compilation the data structure will be supplemented with padding bytes to ensure a proper alignment for each of its members:

    struct MixedData  /* After compilation in 32-bit x86 machine */
    {
        char Data1; /* 1 byte */
        char Padding1[1]; /* 1 byte for the following 'short' to be aligned on a 2 byte boundary assuming that the address where structure begins is an even number */
        short Data2; /* 2 bytes */
        int Data3;  /* 4 bytes - largest structure member */
        char Data4; /* 1 byte */
        char Padding2[3]; /* 3 bytes to make total size of the structure 12 bytes */
    };

The compiled size of the structure is now 12 bytes. It is important to note that the last member is padded with the number of bytes required so that the total size of the structure should be a multiple of the largest alignment of any structure member (alignment(int) in this case, which = 4 on linux-32bit/gcc)[citation needed].

In this case 3 bytes are added to the last member to pad the structure to the size of a 12 bytes (alignment(int) × 3). 
