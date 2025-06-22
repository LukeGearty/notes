File Systems have 3 requirements:
It must be possible to store a very large amount of information
The information must survive the termination of process using it
	Start a program, execute, go back
		Think of databases or files
Multiple processes must be able to access information concurrently
	Web Page: on Amazon, there may be thousands who want to access a web page for example

A disk is a linear sequence of fixed size blocks supporting:
		Read block
	Write Block K

Questions:
How to find the info?
How to keep one user from reading another user's data
How do you know which block is free?

What is a file? How do we represent it?
What is a directory?

File Naming:
What is the meaning of a file without reading it? The extension.
Ex: 
.bak = backup
.html = hypertext markup language 
.o = object file, compiler output, not yet linked

These tell the meaning of a file without having to read the contents, give some semantic meaning to the file. 



File Structure:
A sequence of Bytes - this is done in unix and windows - every byte from beginning to end
A sequence of records
	Like a customer database, broken up into blocks
A Tree - like a BST, where each node has several entries

File Types: contents can change
Executable file needs a location of code, data segments, so it knows how to layout the internal memory for the process
Archive file type - zip, tar, compressed 
	Headers, followed by file
		Headers tell attributes

File Attributes may be stored with different files of different types
	Attributes:
		Protection
		Password
		Owner
		Creator
		Read only flag
		Hidden files - big in windows to hide system files
		System flag - for OS or for users

Binary Files - in machine language, readable by computers
Text Files - in ASCII, UTF, human readable

seek - reposition read/write file offset
	see man lseek

File operations: create, delete, open, close, read, write, append, seek (a pointer to the location where we read next), attribute setters and getters, rename
	System calls for everything here


