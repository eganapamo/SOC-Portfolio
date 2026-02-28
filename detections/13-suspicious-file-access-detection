# Lab 13 – Suspicious File Access Detection

## Objective
Detect suspicious attempts to access sensitive files using common file-reading commands.


## Environment
Log Source: /var/log/auth.log 
System: Kali Linux Lab Environment


## Step 1 – Search for File Access Commands

Command Used:

sudo grep -aiE "\bcat\b|\bless\b|\bmore\b|\bvim\b|\bnano\b" /var/log/auth.log


## Step 2 – Extract Relevant Activity

Purpose:

This command searches authentication logs for usage of file-reading tools such as:

- cat
- less
- more
- vim
- nano

These tools are commonly used to read or inspect files.

In enterprise environments, sensitive files are usually stored in privileged locations.

When these commands are executed with elevated privileges (sudo), it may indicate:

- Viewing sensitive configuration files 
- Inspecting credentials 
- Accessing restricted system data 


## Step 3 – Review Output

Key Finding:

- Only privileged access attempts were logged.
- Non-privileged file reads did not appear.
- This confirms that sensitive files are protected and require elevated permissions.


## Step 4 – Security Interpretation

Use of these commands in authentication logs can indicate:

- Unauthorized file inspection 
- Manual reconnaissance 
- Privilege misuse 

This activity becomes suspicious when:

- Performed outside business hours 
- Done by unexpected users 
- Occurs on sensitive systems 


## Conclusion

The lab successfully demonstrated detection of file access activity using common read utilities.

Monitoring usage of:

cat, less, more, vim, nano, helps identify potential unauthorized inspection of sensitive system files.
