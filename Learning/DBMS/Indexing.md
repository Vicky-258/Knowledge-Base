#DBMS [[03-10-2025]]

- Indexing is the process of storing the indexes of files or creating index for existing files and arranging them into a data structure (usually B or B+ trees) for faster lookups and efficient retrieval.

- I previously thought indexing is just storing a certain value that we can use to find a certain thing in a sequential array so we can look up in O(1).

- But this method will make inserting and maintaining a large array impossible without the things being sequential (1, 2, …., n).

- But we are using trees so that we can have node on different places and connect them through reference or pointers and have them in a balanced structure for O (log N) retrieval.

- Even-though O (log N) seems more than O (1) when considering the time for inserting, we can safely say that O (log N) is indeed a good option.

[[index fragmentation]]
