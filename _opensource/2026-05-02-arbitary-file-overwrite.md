---
title: "Fix Arbitrary File Overwrite via Path Traversal in Embedded Filesystem Extraction"
project: "google/osv-scalibr"
date: 2026-05-02
tags: [vulnerability research, go]
---

The embedded filesystem extraction logic previously trusted file path entries from untrusted filesystem images without sufficient validation. This allowed specially crafted entries such as paths containing / to be interpreted outside the intended extraction root.

An attacker could exploit this behavior to perform directory traversal during extraction, potentially causing the tool to overwrite arbitrary files on the host system. When executed with elevated privileges, this could lead to modification of system-protected paths and compromise host integrity.

This change introduces strict validation and sanitization of all extracted paths. All non-conforming entries are skipped.

Impact:
* Eliminates a path traversal primitive in filesystem extraction
* Prevents writes outside the intended extraction root
* Strengthens security when processing untrusted filesystem images

Logs:
```
[yuvraj@satanist embedded-anno]$ ./scalibr --extractors=embeddedfs/vmdk --result out.textproto ../osv-poc/malicious1.vmdk 
2026/05/02 19:16:16 Running scan with 1 plugins
2026/05/02 19:16:16 Paths to extract: [../osv-poc/p/malicious1.vmdk]
2026/05/02 19:16:16 Scan roots: [%!s(*fs.ScanRoot=&{/ /})]
2026/05/02 19:16:16 Starting filesystem walk for root: /
2026/05/02 19:16:16 End status: 0 dirs visited, 1 inodes visited, 1 Extract calls, 117.480997ms elapsed, 117.481393ms wall time
2026/05/02 19:16:16 Processing Entry => $AttrDef
2026/05/02 19:16:16 Processing Entry => $BadClus
2026/05/02 19:16:16 Processing Entry => $BadClus:$Bad
2026/05/02 19:16:16 Processing Entry => $Bitmap
2026/05/02 19:16:16 Processing Entry => $Boot
2026/05/02 19:16:16 Processing Entry => $Extend
2026/05/02 19:16:16 Processing Entry => $LogFile
2026/05/02 19:16:16 Processing Entry => $MFT
2026/05/02 19:16:16 Processing Entry => $MFTMirr
2026/05/02 19:16:16 Processing Entry => $Secure
2026/05/02 19:16:16 Processing Entry => $Secure:$SDS
2026/05/02 19:16:16 Processing Entry => $UpCase
2026/05/02 19:16:16 Processing Entry => $UpCase:$Info
2026/05/02 19:16:16 Processing Entry => $Volume
2026/05/02 19:16:16 Processing Entry => .
2026/05/02 19:16:16 Processing Entry => ../../tmp/forfun
2026/05/02 19:16:16 Entry Accepted => ../../tmp/forfun
2026/05/02 19:16:16 Processing Entry => tmp
2026/05/02 19:16:16 Entry Accepted => tmp
2026/05/02 19:16:16 Processing Entry => forfun
2026/05/02 19:16:16 Entry Accepted => forfun
2026/05/02 19:16:16 Starting filesystem walk for root: 
2026/05/02 19:16:16 End status: 2 dirs visited, 3 inodes visited, 0 Extract calls, 2.251791ms elapsed, 2.252146ms wall time
2026/05/02 19:16:16 Scan status: SUCCEEDED
2026/05/02 19:16:16 Found 0 software packages, 0 security findings
2026/05/02 19:16:16 Writing scan results to out.textproto
2026/05/02 19:16:16 Marshaled result proto has 272 bytes
[yuvraj@satanist embedded-anno]$ cat /tmp/forfun
You got hacked!


[yuvraj@satanist embedded-anno]$ ./scalibr --extractors=embeddedfs/vmdk --result out.textproto ../osv-poc/malicious2.vmdk 
2026/05/02 19:17:33 Running scan with 1 plugins
2026/05/02 19:17:33 Paths to extract: [../osv-poc/malicious2.vmdk]
2026/05/02 19:17:33 Scan roots: [%!s(*fs.ScanRoot=&{/ /})]
2026/05/02 19:17:33 Starting filesystem walk for root: /
2026/05/02 19:17:33 End status: 0 dirs visited, 1 inodes visited, 1 Extract calls, 144.667788ms elapsed, 144.668432ms wall time
2026/05/02 19:17:33 Processing Entry => $AttrDef
2026/05/02 19:17:33 Processing Entry => $BadClus
2026/05/02 19:17:33 Processing Entry => $BadClus:$Bad
2026/05/02 19:17:33 Processing Entry => $Bitmap
2026/05/02 19:17:33 Processing Entry => $Boot
2026/05/02 19:17:33 Processing Entry => $Extend
2026/05/02 19:17:33 Processing Entry => $LogFile
2026/05/02 19:17:33 Processing Entry => $MFT
2026/05/02 19:17:33 Processing Entry => $MFTMirr
2026/05/02 19:17:33 Processing Entry => $Secure
2026/05/02 19:17:33 Processing Entry => $Secure:$SDS
2026/05/02 19:17:33 Processing Entry => $UpCase
2026/05/02 19:17:33 Processing Entry => $UpCase:$Info
2026/05/02 19:17:33 Processing Entry => $Volume
2026/05/02 19:17:33 Processing Entry => .
2026/05/02 19:17:33 Processing Entry => etc
2026/05/02 19:17:33 Entry Accepted => etc
2026/05/02 19:17:33 Processing Entry => ../../etc/passwd
2026/05/02 19:17:33 Entry Accepted => ../../etc/passwd
2026/05/02 19:17:33 Processing Entry => passwd
2026/05/02 19:17:33 Entry Accepted => passwd
2026/05/02 19:17:33 Starting filesystem walk for root: 
2026/05/02 19:17:33 End status: 2 dirs visited, 3 inodes visited, 0 Extract calls, 820.445µs elapsed, 821.005µs wall time
2026/05/02 19:17:33 Scan status: SUCCEEDED
2026/05/02 19:17:33 Found 0 software packages, 0 security findings
2026/05/02 19:17:33 Writing scan results to out.textproto
2026/05/02 19:17:33 Marshaled result proto has 272 bytes
[yuvraj@satanist embedded-anno]$ cat /etc/passwd
You got hacked!
```