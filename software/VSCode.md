# Visual Studio Code on Linux

URL: https://code.visualstudio.com/docs/setup/linux

## Installation (Debian and Ubuntu based distributions)

The easiest way to install Visual Studio Code for Debian/Ubuntu based distributions is to download and install the [.deb package (64-bit)](https://go.microsoft.com/fwlink/?LinkID=760868), either through the graphical software center if it's available, or through the command line with:

```bash
sudo apt install ./<file>.deb

# If you're on an older Linux distribution, you will need to run this instead:
# sudo dpkg -i <file>.deb
# sudo apt-get install -f # Install dependencies
```

Installing the `.deb` package will automatically install the apt repository and signing key to enable auto-updating using the system's package manager. 

