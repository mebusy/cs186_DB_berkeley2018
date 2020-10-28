
# File Organizations

## Cost Model Overview

- We want "big picture" estimates for data access
    - We’ll (overly) simplify performance models to provide insight, not to get perfect performance
    - Still, a bit of discipline:
        - **Clearly identify assumptions up front**
        - **Then estimate cost in a principled way**
- Foundation for query optimization
    - Can’t choose the fastest scheme without a speed estimate!

## Cost Model for Analysis

- Basic variables in this cost model
    - **B**: Number of data blocks in the file
    - **R**: Number of records per block
    - **D**: (Average) time to read/write disk block
- Focus: Average case analysis for uniform random workloads
- **Assumptions**: For now, we will ignore
    - Sequential vs Random I/O
    - Pre-fetching and cache eviction costs
    - Any CPU costs after fetching data into memory
    - Reading/writing of header pages for heap files
- Will assume data need to be brought into memory before operated on (and potentially written back to disk afterwards)
    - Both will cost I/O!
- Good enough to show the overall trends

- More Assumptions
    - **Single record** insert and delete
        - in practice that might not be truee, but today we're gonna simplify our calculations.
    - Equality selection – **exactly one match**
    - For Heap Files:
        - Insert always **appends to end of file**.
    - For Sorted Files:
        - **Packed**: Files compacted after deletions (i.e., no holes)
        - Sorted according to search key


