---
title: "How to build universal MySQL driver for Qt 5.x.x on Windows using MSVC"
categories:
  - Development
tags:
  - Windows
  - MySQL
  - Qt
---

As a prerequisite you must have Qt sources of version and architecture you want to build driver against. You can download them and extract manually or via installer [here](https://download.qt.io/archive/qt/).

Even if possibility exists to build driver against libraries and headers that MySQL Server installation provides that is incorrect in context of Qt and will lead to not working driver across different MySQL Server versions. I want to take your attention that I don't know how everything exactly works and only want share my experience building and maintain Qt applications that use MySQL. Documentation about building MySQL driver constantly changes from one Qt release to another and never worked for me but now. So, that guide will be very similar to current [official documentation](https://doc.qt.io/qt-5/sql-driver.html#how-to-build-the-qmysql-plugin-on-windows) but I want to keep it for myself (in case they change it again) and make a several my own additions. Next instructions allowed me to create driver that flawless works with 5.7.23 and 8.0.19 MySQL versions.

## Preparation

Download MySQL Connector/C version 6.1.11 [here](https://downloads.mysql.com/archives/c-c/) as a ZIP archive (32 or 64-bit corresponding to your Qt) and extract wherever you want. I prefer `Program Files (x86)` or `Program Files` and `MySQL` folder. Check if directory structure contains `lib` folder with `libmysql.lib` and `libmysql.dll`, and `include` folder with `mysql.h` header. 

**Note:** Also I recommend use `noinstall` instances of MySQL Server for development.
{: .notice--warning}

As a next step you need to run `cmd` with all exported variables and tools. The easiest way to do this is to find in windows start menu  `Visual Studio` folder, expand it and choose `x86 Native Tools Command Prompt` or `x64 Native Tools Command Prompt` then open `bin` folder from your Qt installation (for example `C:\Qt\Qt5.13.0\5.13.0\msvc2017\bin`), find `qtenv2.bat` simply drag-and-drop it to opened console window and execute. Now you done preparation of environment.

## Building and installing driver

In early prepared console you should move to directory with `sqldrivers` plugins' sources. 

Type next: 
```
cd %QTSRCDIR%\qtbase\src\plugins\sqldrivers
```

Where `%QTSRCDIR%`  is root directory of Qt's sources. If you installed sources with Qt installation then they should be somewhere in installation path as `Src` folder.

Next command will create makefiles with instrcutions for building:

```bash
qmake -- MYSQL_INCDIR="C:/Program Files/MySQL/MySQL Connector C 6.1/include" MYSQL_LIBDIR="C:/Program Files/MySQL/MySQL Connector C 6.1/lib"
```

Note that slashes should be forward and not backwards. `qmake` has troubles with parsing backward slashes natural to windows. Don't forget to tweak path corresponding to your MySQL Connector/C folder.

After that type `nmake sub-mysql` and `nmake install` to build driver and automatically copy output files to your Qt installation. Or you can manually grab output files at `.\plugins\sqldrivers`

**Attention!** Don't forget to copy `libmysql.dll` from MySQL Connector/C `lib` folder to folder with your application executable.
{: .notice--warning}

---

### Troubleshooting "Authentication plugin 'caching_sha2_password' cannot be loaded" error

If driver not working and `lastError().text()` method of `QSqlDatabase` instance returns string as above you can solve it by changing password encryption of user password to legacy method:

```sql
ALTER USER 'yourusername'@'localhost' IDENTIFIED WITH mysql_native_password BY 'youpassword';
```





