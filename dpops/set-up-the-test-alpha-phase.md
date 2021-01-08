---
description: >-
  These page is needed only by the people who were selected to join the
  alpha-test phase.
---

# Appendix

### Debug Code on a Server

Having the ability to debug the code while it is running on the server will be essential. In this part, we will explain how to use [Visual Studio Code](https://code.visualstudio.com/) and [GDB](https://www.gnu.org/software/gdb/) to visually debug the program on your desktop.

{% hint style="info" %}
This works best on a Linux host machine, although there are ways to get it to work with MacOS and Windows.
{% endhint %}

{% hint style="warning" %}
**It is strongly recommended to run this on a Linux host inside of a virtual machine. This is due to the fact that** _**X11 forwarding**_ **can allow someone on the server to run X11 programs on your machine, or to collect data on your currently running programs.**
{% endhint %}

#### Install GDB and VSCode

First, install [GDB](https://www.gnu.org/software/gdb/) on your server

```bash
sudo apt install gdb
```

Then, install Visual Studio Code on the server:

Go to the Visual Studio Code [download page](https://code.visualstudio.com/Download) and download the binary for your OS.

Upload the binary to your server using `secure copy` then install vscode

```bash
scp FILE root@server:/root/xcash-official
cd /root/xcash-official
dpkg -i ./code*
```

Installation will fail, so now install the dependencies and retry installation:

```bash
sudo apt-get -f install
dpkg -i ./code*
```

Now, install **X11** dependencies

```bash
sudo apt install xauth libx11-xcb1 libasound2 x11-apps libice6 libsm6 libxaw7 libxft2 libxmu6 libxpm4 libxt6 x11-apps xbitmaps libxtst6
```

After running vscode, [install the C++/C extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools).

#### Run vscode on the Server and Forward the GUI to a Desktop

Add `-XC -c aes128-gcm@openssh.com` to the front of any ssh command that you would normally use to connect to the server.

{% hint style="info" %}
On faster internet connections, you can optionally try **`-X -c aes128-gcm@openssh.com`** to run without compression.
{% endhint %}

Once connected type `code` to start **vscode**. If root you might have to run `code --user-data-dir PATH_TO_FOLDER` to get it to run. Make sure it's an empty folder only used for vscode.

{% hint style="info" %}
It might take 10-30 seconds for the **vscode** window to appear on your desktop.
{% endhint %}

You can now use **vscode** on your desktop but it will have the file system of the server. It will be a little laggy though, so its probably not good for writing new functions, but for quick file edits \(since you have the files panel\) and for debugging it should work fine.

#### Change vscode Keyboard Shortcuts

Optionally change the vscode keyboard shortcuts to the following

Open the keyboard shortcuts  
`File => Preferences => Keyboard Shortcuts`

At the search bar at the top type `saveall`. For the `File: Save All` edit it to `Control+S` and ignore the warning about there already being that keyboard shortcut

At the search bar at the top type `start debugging`. For the `Debug: Start Debugging` edit it to `Control+Shift+D` and ignore the warning about there already being that keyboard shortcut

This will now allow you to save all files with `Control+S`

To rebuild, and start the debugger use `Control+Shift+D`

If the keyboard shortcuts are not changed, then to rebuild and start the debugger it is `F5` and `Control+Shift+D` to switch to the debugger panel

The **vscode** files have already been written for you and are included in the `.vscode` directory

