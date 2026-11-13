# ca.dcloud.ICAClient
Build and install the Citrix Workspace app (ICAClient) + HDX RealTime Media Engine for Skype as a Flatpak application for Linux.

# Features tested
* Connection to citrix server via ICA file
* Teams calls with audio and webcam (optimization enabled) and sharing screens
* Cardreader

# 🚨 Project maintenance
There are the follwing areas which needs maintenance on an ongoing basis:
* Flatpak packages for building/running (Plattform+SDK etc.) installed via CLI and defined in the yml file
* Modules for building/running defined in the yml file
* Shared modules of flathub defined in the yml file
* Potentially breaking changes in the Citrix Workspace app (ICAClient) or HDX RealTime Media Engine for Skype and download part 

## Disclaimer
This project and I are not affiliated with Citrix. This repository does not contain any Citrix software. When the user builds the Flatpak application using this template, the required packages are obtained from Citrix's website, where Citrix has made the installers available for download.

## Requirements
flatpak, flatpak-builder, elfutils and the Gnome SDK and runtime.
You should be able to install all of these through your distro's package manager.

## Instructions
Perform the [flatpak setup](https://flatpak.org/setup/).  
Add the flathub remote, and install the Gnome SDK and runtime:  
*flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo  
flatpak install --user flathub org.gnome.Platform//50  
flatpak install --user flathub org.gnome.Sdk//50*
flatpak install --user flathub org.freedesktop.Sdk.Extension.llvm20//25.08

### add flathub shared modules
git submodule add https://github.com/flathub/shared-modules.git

Clone/download this repo. Open a terminal in the folder where you downloaded this repo, and run the following:  
*flatpak-builder --user --install --force-clean icaclient ca.dcloud.ICAClient.yml*  
(If your distro uses musl libc rather than the tyipcal GNU libc, you may have to add the flag "--disable-rofiles-fuse" due to [this bug](https://github.com/flatpak/flatpak-builder/issues/329))  

It will take some time to download sources and build. Once it is finished, it should be automatially added to your application launcher (may need to log out and back in for it to show up), if not you can launch it via:  
*~/.local/share/flatpak/exports/share/applications/ca.dcloud.ICAClient.desktop*  
Or launch it via command line:  
*flatpak run ca.dcloud.ICAClient*  

## Updating
When you build the app, it will automatically grab the most recent versions of Workspace and HDX RTME. Therefore, if Citrix has published new verions you can update your flatpak app by simply re-running the *flatpak-builder* command listed above. However, you may need to empty the .flatpack-builder folder (located in the folder where you previously built the app), as Flatpak may see the build manifest hasn't changed and therefore use the cached files from the previous build, rather than re-downloading and grabbing the latest installers.

# Known limitations
* No Citrix app protection
* Sometimes the window is closed after 10 seconds and you need to reconnect. Seems to be a problem with webcam and gstreamer. If it does not close during the first seconds it is running fine.
* You can not use the workspacecheck.sh reliably to check the requirements of Citrix because it uses many unreachable commands (systemd, logind etc.)
* Other untested features of the Citrix App

## Notes
If you find any bugs, please report them to the issue tracker.
