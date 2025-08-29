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

## Details
With the Smidge module you can configure “bundles” which is packaging of multiple files. This is done by defining a name for the bundle/package and paths to files that should be included in the package. When you make a HTTP-request to the bundle, 
you also need to specify the version, e.g:<br>
<code>https://{domain}/{bundle-name}.js.v123</code>

The output of this request is cached in files on the server. Files are stored in the folder: <br>
<code>{Web application name}/Smidge/Cache/{Hostname}/{Version}</code>

By manipulating the version parameter in the request, you can control where the files are stored.
Based on the response of the HTTP-request, you can interpret whether a folder exists or not. This can be used, for example, to enumerate users in “C:\Users\”.<br>
It is also possible to specify a unique version string for each request, which leads to allocating file storage on the server. Which ultimately will lead to depleting available storage. This can be exploited by unauthenticated users.

## Screenshots
<i>1. None existing user</i><br>
Request with version string “c:\users\noexistinguser”. Error message is referring to CreateDirectory which can be interpreted that the folder (user) is not existing. The version string needs to be URL-encoded. Response code 500.
<img width="864" height="218" alt="image" src="https://github.com/user-attachments/assets/4a79fa8c-8e0e-4fcd-8704-be504f217bc8" />
<br>
<i>2. Existing user</i><br>
Request with version string “c:\users\John”. Error message is referring to CreateFile which can be interpreted that the folder (user) is existing. Response code 500.
<img width="847" height="194" alt="image" src="https://github.com/user-attachments/assets/03b486ae-ce45-4471-b7dc-349029ae02c8" />
<br>
<br>
<i>3. Writing to users folder</i><br>
Request with version string “c:\users\WebAdmin” which is an existing user on the web server. Image below shows files in the users folder before and after the request. Response code 200.
<img width="768" height="698" alt="image" src="https://github.com/user-attachments/assets/5019d230-3e80-4507-96c8-4b469a4efe8c" />

## Affected versions
Up to 4.5.1

## Summary of vulnerabilities
- Arbitrary file creation (CWE-22)
- Allocation of disk storage (CWE-770)

## Recommendation
Sanitize input, version parameter.

