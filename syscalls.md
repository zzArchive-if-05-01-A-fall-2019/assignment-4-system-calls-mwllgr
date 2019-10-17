# Assgmt. 2: System Calls

## Documentation
### fork  
Creates a new process by duplicating the calling process.

### stat 
Returns information about a file.

#### Arguments
1. `const char* pathname`  
Path to requested file
2. `struct stat *statbuf`  
Return buffer

### kill
Sends a specified signal to a process.

#### Arguments
1. `pid_t pid`  
The process ID.
2. `int sig`  
The signal to send to the PID. (e.g. `-TERM` or `-15` for a `SIGTERM`)

### mmap
Maps a file or device into memory.

#### Arguments
1. `void *addr`  
Starting address for the new mapping.
2. `size_t length`  
Length of the mapping (must be > 0).
3. `int prot`  
Memory protection of the mapping.
4. `int flags`  
Determines whether updates to the mapping are visible to other processes that are mapping the same region and whether updates should be carried through to the underlying file or not.
5. `int fd`  
File descriptor. The contents of a file mapping are initialized using `length` starting at `offset`.
6. `off_t offset`  
Offset for file descriptor.

### chmod
Changes the permissions of a file or device.

#### Arguments
1. `const char *pathname`  
Path to the file/device. Path gets dereferenced if it is a symlink.
2. `mode_t mode`  
The new file mode.

### waitpid
Wait for a process to change state.

#### Arguments
1. `pid_t pid`  
The process id (PID) to wait for. Values <= 0 reserved for special wait modes.
2. `int *wstatus`  
Pointer to int where the status information gets stored.
3. `int options`  
This parameter changes the waiting behaviour. Default: wait only for terminated children.

## Fail conditions
### fork
`ENOMEM`: Can't allocate because low on memory.

### exec
`EACCES`: e.g. file system mounted with `noexec` parameter.

### unlink
`EIO`: An IO error occurred.

### read
`EISDIR`: The file descriptor refers to a directory.

### mount
`EBUSY`: The source is already mounted.

### chmod
`ELOOP`: Too many symlinks were encountered while resolving the path name.

### kill
`EPERM`: The process doesn't have the needed permission to send the signal to any target processes.

## Trap instructions
A trap is used to switch from user-mode to the kernel-mode. This is used when a program in user-mode needs to execute a system call (these can only be handled by the OS). After that, the program then returns back to user-mode to execute possible next instructions.