[Home](/) / 
[Reference](/reference/git/) / 
[Bugs](/reference/git/bugs)

# System Reports
The System Reports tool automatically gathers useful data when an application crashes. This information can be critical in fixing the bug that caused the crash.

### Launching

#### 1) From your desktop's start menu, launch the **System Reports** tool:

![Mint Report initial window](/screenshots/bugs-mintreport-crash-info/1-starting-window.png)

#### 2) Select **Crash reports** from the sidebar:

![Crash reports selected](/screenshots/bugs-mintreport-crash-info/2-select-crashes.png)

#### 3) Select the application entry in the crash list:

![Individual report selected](/screenshots/bugs-mintreport-crash-info/3-indiv-report-select.png)

> ℹ️ **Note:** Many Linux Mint tools are written in Python, so you may see python listed as the application that crashed, instead of your application's name. In these cases try to match the time of the crash listed with the time you experienced the issue.

#### 4) Click the <kbd>Local Files</kbd> button, this will open your file browser to the location where this crash's information is stored:

![Click local files](/screenshots/bugs-mintreport-crash-info/4-click-local.png)

In the directory you'll find a file named *crash.tar.gz* - this contains all of the available information on this crash, which could prove invaluable to a developer trying to fix it. If you want to see what this information is, you can have a look at the *crash* folder:

- `CoreDump`: This is binary information that a developer can feed into a debugging program to try and reproduce the applications state when the crash occurred. It is not human-readable.
- `Inxi`: This is a text file containing details about your system, some of which may be relevant to the issue you're having. This should *not* contain any personal information.
- `LinuxMintInfo`: This file has some Linux Mint-specific details about your computer. Again, there should not be any personal information here.
- `Packages`: This is a list of all packages installed on your system. There are situations where programs can have unintended interactions, so this can be an important reference when trying to reproduce the crash.
- `StackTrace`: This file contains low level details about what the program was doing at the point of the crash. It's generally incomplete, and of limited usefulness on its own, unless the user had previously installed debug packages for the libraries being used.

#### 5) Upload crash.tar.gz:

The easiest way to make this file available is to attach it to a comment in a Github issue. You should be able to drag-and-drop the file right into the comment section.

![Drag into issue](/screenshots/bugs-mintreport-crash-info/5-drag-to-issue.gif)

You can also use the 'attach' button:

![Attach button](/screenshots/bugs-mintreport-crash-info/6-attach-button.png)

> ℹ️ **Note:** Github does not allow files larger than 25MB to be uploaded. If your crash report is larger than this, you can use a service like [transfer.sh](https://transfer.sh) to upload the file, and then paste the link into the issue. Cloud storage services can also be used here, such as Dropbox and Google Drive.

## That's it!
