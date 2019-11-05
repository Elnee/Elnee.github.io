---
title: "MySQL service can not start on Windows after changes in my.ini"
categories:
  - Troubleshooting
tags:
  - Windows
  - MySQL
---

After changes in `my.ini` configuration file you can face issue when MySQL service restart takes forever. Actually it just hang. That problem occurs especially on Windows 7 when `notepad.exe` saves file with wrong encoding after your changes. It can be `UTF-8 BOM`. That's why MySQL service can not start. Check encoding of `my.ini`, it have to be `UTF-8`.

