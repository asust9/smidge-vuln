# Vulnerability identified in Smidge
Smidge – A lightweight library for runtime CSS and JavaScript file management, minification, combination & compression in Microsoft .NET.

In Program.cs of a .NET web application a Smidge Javascript bundle is defined with “CreateJs”. 
<img width="787" height="100" alt="image" src="https://github.com/user-attachments/assets/648d7dcd-2413-4df5-921d-832da92cd9f0" />

This module is vulnerable to arbitrary file creation.
## Background
Smidge is a file management module that can be integrated with web applications in Microsoft .NET. The module has more than 10M downloads on Nuget (https://www.nuget.org/packages/Smidge) and is integrated in some CMS, for example in Umbraco versions 10,11, 12 and 13.
A vulnerability has been identified that can be used for arbitrary file creation. By exploiting this vulnerability, it might be possible to:

- Enumerate usernames on the web server.
- Deplete available hard disk space and thus affect availability.


## Affected versions
Up to 4.5.1

## Summary of vulnerabilities
- Arbitrary file creation (CWE-22)
- Allocation of disk storage (CWE-770)

## Recommendation
Sanitize input, version parameter.

## Details and PoC
https://www.osco.se/2025/08/29/vulnerability-identified-in-smidge/

