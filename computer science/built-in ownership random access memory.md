# Built-In Ownership Random Access Memory

Built-in ownership random access memory (BIORAM) is RAM where each cell is paired with an owner address: the address of the process for which this cell is allocated.

This model doubles the cost of the total cells in a RAM chip but tremendously decreases allocation/deallocation time which would traditionally be done using [memory management units](https://en.wikipedia.org/wiki/Memory_management_unit).
