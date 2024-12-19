---
layout: post
title: GetLoggedOn - Remote Registry Enumeration
---

# Remote Registry to enumerate the logged on users?
---

During red team operations we, more time than not, are targeting the Active Directory. We have all been in the situation that we quickly want to check wether or not users are connected/logged in to certain devices, for let's say dumping their credentials if they are. Some tools already help us with that, for example Bloodhound/Sharphound does session enumeration using:

- NetWkstaUserEnum
- NetSessionEnum
- Remote Registry

<br>And however it is quite easy to just run Sharphound or Bloodhound.py, it might be the case that we cannot open a socks proxy or just run a .NET assembly in memory without being detected. So how to fix this? Well I tried writing a quick a dirty Cobalt Strike BOF to help us with just that.

## Original Idea

So a while back I saw this tweet from <a href="https://twitter.com/Geiseric4">@Geiseric4</a>: 

<p align="center">
    <img src="../assets/getloggedonbof/tweet.png" width="65%">
</p>

He tweeted about a <a href="https://gist.github.com/GeisericII/6849bc86620c7a764d88502df5187bd0">python script</a> that does just what we want to accomplish with our BOF:

- Start the Remote Registry (if not running)
- Session enumeration using Remote Registry
- SID to accountname conversion

# Project
--- 

So we want to create a BOF that will check if the remote registry is running, enumerate SIDs from a registry subkey that is readable for all users and convert the SIDs to usernames using the LSA.

## Implementation Details

We developed `getloggedon`, a BOF designed to function with minimal privileges, eliminating the need for administrative rights. Its operation involves:

1. **Connecting** to the target machine's Remote Registry service.
2. **Accessing** the `HKEY_USERS` hive to enumerate subkeys corresponding to user SIDs.
3. **Resolving** these SIDs to usernames through the local LSA.

<br>Here's a breakdown of the core functionality:

### Connecting to the Remote Registry

```c
dwresult = ADVAPI32$RegConnectRegistryA(hostname, HKEY_USERS, &rootkey);
```

This function establishes a connection to the HKEY_USERS hive on the specified remote machine (hostname). If hostname is NULL, it defaults to the local machine.

### Enumerating Subkeys (SIDs)

```c
while (TRUE) {
    retCode = ADVAPI32$RegEnumKeyExA(rootkey, i, keyname, &keysize, NULL, NULL, NULL, &filetime);
    if (retCode == ERROR_NO_MORE_ITEMS) {
        break;
    } else if (retCode == ERROR_SUCCESS) {
        // Process the SID
    }
    i++;
}
```

This loop iterates over each subkey in HKEY_USERS, with each subkey name representing a user SID. The RegEnumKeyExA function retrieves the name of each subkey.

### Converting SIDs to Usernames

```c
if (ADVAPI32$ConvertStringSidToSidA(keyname, &pSid)) {
    if (ADVAPI32$LookupAccountSidA(NULL, pSid, username, &usernamesize, domain, &domainsize, &sidtype)) {
        // Successfully resolved SID to username
    }
    KERNEL32$LocalFree(pSid);
}

```

For each SID string (keyname), ConvertStringSidToSidA converts it to a binary SID. Subsequently, LookupAccountSidA retrieves the associated account name and domain name.

### Compilation and Usage

To compile the BOF, utilize the provided Makefile with MinGW-w64 on a Linux system:

```c
make
```

This command generates the necessary object files for integration with Cobalt Strike.

In Cobalt Strike, load the accompanying Aggressor script getloggedon.cna, which introduces the getloggedon command. Execute it as follows:

```
getloggedon <optional: hostname>
```

If no hostname is specified, the BOF queries the local machine.

## Limitations and Considerations

- Dependency on Remote Registry: The BOF relies on the Remote Registry service. If the service cannot be started due to policy restrictions, the enumeration will fail.
- Permissions: While administrative privileges are not required, sufficient permissions to query the Remote Registry are necessary.

## Conclusion

The getloggedon BOF provides a lightweight and efficient method for enumerating logged-on users via the Remote Registry, particularly useful in scenarios where traditional tools may be impractical. Integrating this BOF into your Cobalt Strike toolkit can enhance situational awareness during red team engagements.


# Sources

- [Bloodhound Inner Workings - Part 3](https://blog.compass-security.com/2022/05/bloodhound-inner-workings-part-3/)
- [GetLoggedOn BOF](https://github.com/0xSH4RKS/getloggedonBOF)