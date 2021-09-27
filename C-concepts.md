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
