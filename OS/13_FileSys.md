File Attributes
• Name – only information kept in human-readable form
• Identifier – unique tag (number) identifies file within file system
• Type – needed for systems that support different types
• Location – pointer to file location on device
• Size – current file size
• Protection – controls who can do reading, writing, executing
• Time, date, and user identification – data for protection, security, and usage
monitoring
• Information about files are kept in the directory structure, which is maintained on the disk
• Many variations, including extended file attributes such as file checksum



File Operations

* OS provides file operations to
    * create:
        * space in the file system should be found
        * an entry must be allocated in the directory
            * metadata存在一起的，且总数有限
    * open: most operations need to file to be opened first
        * return a handler(file descriptor) for other operations
    * read/write: need to maintain a pointer(offset from文件开头的位置，是文件内部的指针)
    * reposition within file - seek
    * delete
        * Release file space
        * Hardlink: <u>maintain a counter</u> - delete the file until the
            last link is deleted
    * truncate: delete a file but maintains its attributes
* Other operations can be implemented using these ones
    * Copying: create and read/write

