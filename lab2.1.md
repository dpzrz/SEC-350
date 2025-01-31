# Lab 2.1 Standardizing on Time

### RW01:
The log server has no concept of time. It has no indication of the timezone or the year.

#### The fix:
Enter the path `/etc/rsyslog.conf` and edit the config file and commment out the config line;

`$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat`

After starting a logging session our log time should look like this now:

![{FE5D6404-D214-4BED-B929-C1E7CC6EB15E}](https://github.com/user-attachments/assets/489b12be-5e9b-4127-b890-c40e8a32b152)

this proccess is similar for the `web01` + `log01`



