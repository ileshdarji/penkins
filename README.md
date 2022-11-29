To run application using PyCharm runner configuration.

In Run/Debug Configurations, for new Python configuration:

Point to your behave module, specific to your installation. e.g:

    c:\Users\43978999\AppData\Roaming\Python\Python36\site-packages\behave\__main__.py

Point to feature you want to execute in "Script parameters", e.g:

    tests\features\logon.feature

Set "Working directory" to reflect desired starting point for behave script, e.g:

    C:\Users\43978999\PycharmProjects\myPEAP-automation

Please note that report generation is not working when you are in debug mode.

To run single feature from terminal (and have reports generated locally) use following example.
First "cd" to "tests" subdirectory!

    "C:\swdtools\Python3.6.3 X64\python.exe" -m behave features\logon.feature
