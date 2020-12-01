
# 7 Refinements on Indexes and B+ Trees

## Search Key and Ordering

- Defn: A **composite search key** on columns ( k₁,k₂,..., k<sub>n</sub> ) "matches" a query if:
    - The query is a conjunction of m≥0 equality clauses of the form
        - k₁=<val₁> AND k₂=<val₂> AND ... AND k<sub>m</sub>=<val<sub>m</sub>>
    - and at most 1 additional range clause of the form:
        - AND k<sub>m+1</sub> op <val>  (where op is one of { \<,\> })

- Why does this "match"? Lookup and scan in lexicographic order
    - Can do a lookup on equality conjuncts to find start-of-range
    - Can do a scan of contiguous data entries at leaves
        - satisfy the m+1<sup>st</sup> conjunct
        - of if there is no m+1<sup>st</sup> conjunct
            - scan the entire set of matches to the first m conjuncts

- **Composite Keys**:  more than one column
    - **Lexicographics order**
    - Search a range?
    - ( Age, Salary ):
        - Age==31 & Salary=400  (matched)
            - when scan a row with Age>31, we can end this query.
        - Age==55 & Salary>200  (matched)
        - Age>31 & Salary=400 (**not** matched)
        - Age=31  (matched)
        - Age>31  (matched)
        - Salary=300  (**not** matched)



---

[By-reference leaf node & clusterd](slides/6a-tree-index-extensions.pdf)

## Alternative 2 vs Alternative 3  Table Illustration

![](imgs/cs186_alt2_vs_alt3.png)

--- 


## Variable Length Keys and Compression 

- So far we have been using integer keys
- What would happen to our occupancy invariant with variable length keys?
- What about data in leaf level when different keys are different lengths?

## Redefine Occupancy Invariant

- Order (d) makes little sense with variable-length entries
    - Different nodes have different numbers of entries
    - **Index pages** often hold many **more entries** than leaf pages
    - Even with fixed length fields, alternative 3 gives variable length data entries.
- Use a physical criterion in practice: **at-least half-full**
    - Measured in **bytes**
- Many real systems are even sloppier than this
    - Only reclaim space when a page is completely empty
    - Basically the deletion policy we described above...

## Prefix Compress Keys ?

- How can we get more keys on a page?
    - `|Dan Ha|Danielle Yogurt|Davey Jones|David Yu|Diana Murthy|`
    - that is , get more index entries particularly more keys onto a page.
    - more index entries, more pointers -> bigger fan-out -> shorter tree
- What if we compress the keys ?
    - Just taking the distinguishing prefix
    - `|Dan|Dani|Dav|Davi|Di|`
- Are these the same
    - David Jones ?
    - Not the same partitioning of possible keys
        - `David Jones` will be located in 4th position, and its prefix compress key will be located at 5th position.
    - But why would we care ??
        - The truth is we don't really have to think about this trade-off because the way we're gonna achieve prefix key compression and practice in a B+ tree is during page split at the leaf level.


## Suffix Key Compression

- Another compression technique we can do is called suffix key compression.
- All keys have large common prefix
    - `| Sarah L | Sarah M | Sarah Z | Sarita | Saruman |`
- Move common prefix to header, leave only (compressed) suffix next to pointer
    - `Sar`  (suffix compression)
    - `| ah L | ah M | ah Z | i | u |`
- When might this be especially useful?
    - Composite Keys. Example ?
        - `<Zip code, Last Name, First Name>`

## B+ Tree Cost Modeling

[Variablelengthkeyrefinements & B+tree cost model](slides/6b-tree-costs.pdf)



