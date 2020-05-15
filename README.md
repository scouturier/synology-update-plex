# synology-update-plex
> Script to Auto Update Plex Media Server on Synology NAS

## Goals

- Make the echoed messages super clear
- Make the version checking logic as smart as possible
- Ensure the script fails if there are any errors
- Ensure temp files are cleaned up properly
- Write bash code as idiomatically as possible
- Attempt to find the "Plex Media Server" directory that contains Preferences.xml efficiently
- Attempt to support all Synology NAS architectures
- Support Ansible automation

## Usage

First, SSH into your NAS, save the [update-plex.sh](update-plex.sh) script somewhere and set it as executable:

```sh
$ ssh you@IP_OF_YOUR_NAS
you@yournas:~$ wget "https://raw.githubusercontent.com/cowboy/synology-update-plex/master/update-plex.sh"
you@yournas:~$ chmod a+x update-plex.sh
```

Then, create a Scheduled Task with a User-defined script in the Synology DSM Control Panel:
- Ensure the User is `root`
- Ensure the Run command is `/path/to/update-plex.sh`
- Add the `--plex-pass` option (eg. `/path/to/update-plex.sh --plex-pass`) if you have Plex Pass and want to enable early access / beta releases

## Use Ansible instead
Ansible is a powerful automation tool which allows you to perform repetitive tasks easily.

To use Ansible: 

- Clone this repository on your ansible machine
- Modify the inventory file to match your environment (`/inventory/hosts.ini`)
- Run the following command: 
    ```
    ansible-playbook playbooks/update-plex.yml -i inventory/hosts.ini
    ```

> It is recommended to use a password vault, ansible-vault or AWX/Tower credential vault to encrypt password so it is not stored in clear text in the inventory file

## Notes

[issue]: https://github.com/cowboy/synology-update-plex/issues
[pr]: https://github.com/cowboy/synology-update-plex/pulls

- Be careful when SSHing into your NAS. I'm not responsible if you break anything!
- This script may contain bugs. I'm not responsible if it breaks anything!
- This script has been tested on a Synology DS918+ & DS216J NAS. It should work with other Synology NAS models.
- If you find a bug, please [file an issue][issue] or [create a pull request][pr]. Explain the situation and include all script output.
- If the script outputs `Unable to find "Plex Media Server" directory` when `--plex-pass` is specified, you may need to manually change `/volume*` in the script to your volume's root path.
- This assumes Plex was installed manually from https://www.plex.tv/media-server-downloads/.
- You'll probably need to [add Plex as a trusted publisher for package installations](https://support.plex.tv/hc/en-us/articles/205165858).

## References

Adapted from work first published at:
- https://forums.plex.tv/t/script-to-auto-update-plex-on-synology-nas-rev4/479748

Inlcuding other update scripts such as:
- https://github.com/martinorob/plexupdate
- https://github.com/nitantsoni/plexupdate
- https://gist.github.com/seanhamlin/dcde16a164377dca87a798a4c2ea051c
- https://forums.plex.tv/t/script-to-auto-update-plex-on-synology-nas-rev4/479748/67

## License

[![CC0](http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, Ben Alman has waived all copyright and related or neighboring rights to this work.
