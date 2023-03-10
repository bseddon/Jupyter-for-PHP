## Jupyter-for-PHP for Windows

The purpose of this project is to revive the ability to use PHP 8.2 on Windows as a kernel within Jupyter.  The [Litipk/Jupyter-PHP](https://github.com/Litipk/Jupyter-PHP) is great but because its not been maintained regularly it's not possible to use it in a comtemporary PHP environment on Windows.  There are two specific issue this project addresses:

1) The PHP kernel project relies on the extension php-zmq, a wrapper around the [ZeroMQ](https://zeromq.org/) project DLL.  Until PHP 7.4 this extension was available for Windows through PECL but not any more.  If you want to use a more recent verion of PHP you are out-of-luck.

2) Although the kernel project is fine if there is a php-zmq extension available, the [Litipk/Jupyter-PHP-Installer](https://github.com/Litipk/Jupyter-PHP-Installer) does not work with any version of composer 2.0 or later.  This is because the name included in the composer.json file (Jupyter-PHP-Install) generated by the installer is not compatible with the way composer validates the name value which must be in the format "xxx/yyy".

The releases available from this project provide two things to make it possible to use the Jupyter-PHP kernel on Windows again:

1) DLLs for ZeroMQ.  This includes the core ZeroMQ DLLs compiled using the same dependcies and architecture as PHP 8.2 and the php-zmq.dll extension compiled for PHP 8.2.

2) A replacement .phar file to install the Jupyter kernel.

### Installation

The instructions below assume you have a functioning Jupyter environment on Windows.  My use case is Visual Studio code and the VS Code extension for Jupyter.  However, this further assumes you have Python installed and iJupyter projects installed.

The alsop assume you have PHP 8.2 installed (8.2.1 is the release available at the time of writing) and that PHP folder containing PHP 8.2.x is on the path.  That is the command line command:

```
php --version
```

Will run correctly and return the value 8.2.x

Download the most recent release.  From the zip file copy the files **libzmq.dll** and **libzmq-mt-gd-4_3_5.dll** into the root folder of PHP 8.2.x.  Copy **php-zmq.dll** to the ./ext sub-directory.

Edit the php.ini file and add the following line;

```
extension=php-zmq.dll
```

Extract **PHP-Jupyter-Install.phar** from the zip file (doesn't matter where) the run the command:

```
php PHP-Jupyter-Install.phar install --verbose
```

This assumes you have **composer** installed and that it's a reasonably recent version.

The **--verbose** switch is not necessary but in the event there is a problem it will display more information about the issue.

Refer to the original [Litipk/Jupyter-PHP-Installer](https://github.com/Litipk/Jupyter-PHP-Installer) project for information about installation issues.  Except to work around the composer validation error, and to use a recent version of symfony (so deprecation messages do not appear) the project is unchanged and the error and workarounds described in the issues are still appropriate.

### On Windows Server

The ZeroMQ DLL has been compiled on Windows 10.  To be able to use this DLL with VS Code running on Windows Server the following four DLLs from the c:\Windows\System32 folder may be needed:

* ucrtbased.dll
* vcruntime140_1d.dll
* vcruntime140d.dll
* msvcp140d.dll

### Example PHP statements being used within a Jupyter notebook in Visual Studio Code

![php-jupyter](https://user-images.githubusercontent.com/1221824/212504015-37a0947b-222f-47d2-b601-bc66321bb36b.png)
