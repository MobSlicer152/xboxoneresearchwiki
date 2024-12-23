<!-- TITLE: Xbox Operating System -->
<!-- SUBTITLE: Structure of the Xbox Operating System -->

# Xbox Operating System

The Xbox One can run multiple different Operating Systems at the same time through the use of virtualization technology. 

## Host

The primary operating system known as the Host OS is responsible for
managing the virtual machines, providing drivers for hardware components
and more. It was based on a variation of Windows 8 LNM, a stripped down
operating system for bare minimum requirements, with many customisations
that implement Microsoft's Xbox Virtual Machine stack.

### Host Volumes/Drives

| Partition Name  | Mount Point as seen by HostOS | Notes                                                                                                                                                                                        |
| --------------- | ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Internal Flash  | F:\\                          | The flash does not contain a normal file system, it is using the [Xbox boot filesystem](../boot/xbox-boot-file-system.md) and is exposed as a normal NTFS partition via a special driver.    |
| host.xvd        | C:\\                          | Host's Windows Installation.                                                                                                                                                                 |
| User Content    | E:\\                          | Direct access to the HDD User Content partition. XVCs are stored here.                                                                                                                       |
| System Update   | R:\\                          | Direct access to the HDD System Update partition. Boot slots and SystemOS XVDs.                                                                                                              |
| System Update 2 | J:\\                          | Direct access to the secondary HDD System Update partition. Can also contain boot slots and SystemOS XVDs. However it seems to be empty on most HDDs                                         |
| System Support  | G:\\ (or sometimes Q:\\)      | Direct access to the HDD System Support partition. Containing SystemOS's settings, temp, etc.                                                                                                |
| Host Tools      | D:\\                          | Special XVD called HostTools.xvd in the HDD with extra utils to be used in the Host Operating System. If the XVD exists in the bootslot in the HDD, it will be mounted to this volume letter |
| Temp Content    | V:\\                          | Direct access to the HDD Temp Content partition. Containing temporary XVD's, paging data, etc.                                                                                               |

## System

The System OS is built to provide the main visual and interactive layer
of the console. This operating system is currently based on Windows
'OneCoreUAP' with a custom shell, services and other components. The apps that users can download from the Microsoft Store run in this operating system.

### SRA Virtual Drives

| File Name          | Mount Point as seen by SystemOS | Dev-Only | Read-Only | Desc                                       |
| ------------------ | ----------- | -------- | --------- | -------------------------------------------------------------- |
| system.xvd         | C:\\        | false    | true      | System Boot.                                                   |
| systemaux.xvd      | X:\\        | false    | true      | Inbox apps.                                                    |
| systemauxf.xvd     | Y:\\        | false    | true      | Inbox apps.                                                    |
| $sosrst.xvd        | T:\\        | false    | true      | Temporary storage.                                             |
| systemtools.xvd    | J:\\        | true     | true      | Developer utilities.                                           |
| settings-saved.xvd | S:\\        | false    | false     | Console settings data. (Licenses, prefetch, registry and more) |
| systemmisc.xvd     | M:\\        | false    | true      | Miscellaneous system boot files.                               |
| user.xvd           | U:\\        | false    | false     | User and application data.                                     |
| user-devkit.xvd    | U:\\        | true     | false     | User and application data.                                     |

Other Mount points

| Device                        | Mount Point as seen by SystemOS | Dev-Only | Read-Only | Desc                                              |
| ----------------------------- | ----------- | -------- | --------- | --------------------------------------------------------------------- |
| USB Storage                   | D:\\        | false    | false     | 1st USB Storage device, plain NTFS, for media storage only, not Games.|
| Optical Disc Drive            | O:\\        | false    | true      | Optical Disc Drive (ODD)                                              |

## Game/Era

The Game operating system is used exclusively for games that target the
XDK due to the higher allocation of console resources. It is built upon
the same variant of Windows 8 LNM as the Host OS but is essentially
smaller and more efficient.  Information on how GameOS is launched can be found [here](../games/launching.md).

### ERA Virtual Drives

| File Name    | Mount Point | Notes                                                                                                                                                                                 |
| ------------ | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ERA.xvd      | C:\\        | GameOS's Windows installation. Packaged with games via the XVC's user data. Attempts to read from this partition will result in the hypervisor terminating the ERA VM.                |
| Game.XVC     | G:\\        | Running game's binaries/data. The XVC mounted and decrypted.                                                                                                                          |
| tempXX       | T:\\        | ERA Temporary XVD - Used by ERA-Games for temporary storage, `XX` being a placeholder                                                                                                 |
| ERATools.xvd | J:\\        | (MS Internal (and ERA Test?) Devkits with XDK recoveries only). Contains tools like ipconfig, telnetd, debug support dlls and dlls needed to connect to the XDK via the XB CLI tools. |

## GameCore

A new operating system that currently appears to be the new foundation
for games developed on Windows 10 devices. Based on a 'headless' Windows
Core OS (WCOS).
