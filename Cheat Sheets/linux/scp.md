# Secure Copy Files (.dll) From Machine to Machine
`scp [OPTION] [user@]SRC_HOST:]file1 [user@]DEST_HOST:]file2`
1. Transferring a file from local machine to remote server:
      `scp /path/to/local/file.txt user@remotehost:/path/to/remote/directory/`
2. Transfer a file from a remote server to your local machine:
   `scp user@remotehost:/path/to/remote/file.txt /path/to/local/directory/`
3. Securely Copy a file from one remote server to another remote server:
   `scp user1@sourcehost:/path/to/file.txt user2@destinationhost:/path/to/destination/`

**Options:**
- `-P port` : Specifies the port to connect to on the remote host. Note that the option is a capital `P`.
- `-p` : Preserves the modification times, access times, and modes from the original file.
- `-r` : Recursively copy entire directories.
