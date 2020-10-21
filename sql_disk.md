
# Disks, Buffers and Files

## Pages

- Tables stored as logical files
    - Consist of pages
        - Pages contain a collection of records
- Pages are managed
    - On disk
        - by the disk space manger: pages read/written to physical disk/files
    - In memory
        - by the buffer manager: higher levels of DBMS only operate in memory

- ![](imgs/disk_pages.png )


### Files of Pages of Records

- **DB FILE**: A collection of pages, each containing a collection of records.
- API for higher layers of the DBMS:
    - insert/delete/modify record
    - Fetch a particular record by **record id**...
        - Record id is a pointer encoding pair of (**pageID, location** on page)
    - Scan all records
        - Possibly with some conditions on the records to be retrieved

### Many DB File Structures

- Unordered Heap Files
    - Records placed arbitrarily across pages
- Clustered Heap Files
    - Records and pages are grouped into records with similar values
- Sorted Files
    - Pages and records are in sorted order
- Index Files
    - B+ Tres, Linear Hashing, ...
    - May contain records or point to records in other files

### Heap File Implemented as List

- Heap file has one special “Header page”( the location is its ID ), And a name associated with the heap file as well.
- Header page ID and Heap file name are stored at some particular location on the disk, called database catalog
- Every page in heap file contains 2 “pointers” plus **free space** and **data**
    - all the data pages will have some free space potentially and some data.
    - the header page itself is just going to have 2 pointers , one to a double linked list of full pages that have no more room for inserts, and one to a double linked list of pages with free space.

- ![](imgs/header_page.png)

- What is wrong with this?
    - How do I find a page with enough space for a 20 byte record
    - A: Need to access many pages (w/ free space) to check


### Use a Page Directory

- The page directory scheme is gonna have multiple header pages.
    - the 1st one will be stored in the catalog.
    - these header pages will be in a linked list of header page, that linked list is called **directory**.
- In this directory , each page can have entries. 
    - the entries will include a pair of #free bytes on a referenced page, and a pointer to that page.
- So for every data page, we'll have one pointer to it from the header page, and the header pages will keep the number of free bytes associated with each page.

- ![](imgs/page_directory.png)

- Header pages accessed often -> likely in cache
- Finding a page to fit a record required far fewer page loads than linked list. Why?
    - One header page load reveals free space of many pages
- You can imagine optimizing the page directory further
    - E.g., compressing header page, keeping header page in sorted order based on free space, etc.
    - but it's unclear that it's necessary. Because with a small number of header pages, you've already covered a pretty large data file.


## PAGE LAYOUT

### Page Basics: The Header

- Header may contain “metadata” about the page, e.g.
    - Number of records
    - Free space
    - Maybe a next/last pointer
    - Bitmaps, Slot Table

### Things to Address

- Some options:
    - Record length? Fixed or Variable
    - Page layout? Packed or Unpacked
        - in terms of its free space
- Some questions:
    - Find records by record id?
        - Record id = (Page, Location in Page) 
    - How do we add and delete records?


### Fixed Length Records, Packed

- Pack records densely
- Record id = (pageId, “location in page”)?
    - (pageId, record number in page)!
    - We know the offset from start of page!
        - Offset = header + (record size) x (n-1) 

- ![](imgs/fixed_len_record_packed.png)

- Easy to add: just append
- Delete? ( delete page 2, record 3 )
    - Packed implies re-arrange!
    - “record id” - (Page 2, Record 4) now need to be updated to (Page 2, Record 3)
    - Record Ids need to be updated!
    - Could be expensive if they’re in other files.


### Fixed Length Records: Unpacked

- Keep a bitmap in the page header, that's gonna have a bit for each sort of slot on the page.
- Bitmap denotes “slots” with records
- Record id now is gonna be the page ID, and then the slot number in page.
    - (pageId, slotId)

- Insert: find first empty slot in bitmap
- Delete: Clear bit
    - No reorganization needed!
    - Small cost of a bitmap, which can be very compact

- ![](imgs/fixed_len_record_unpacked.png)





