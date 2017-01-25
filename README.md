# Ansible Role: kodi

An Ansible role that installs kodi on Fedora.

## Requirements

The [UnitedRPMs](https://unitedrpms.github.io/) or [rpmfusion-free](http://rpmfusion.org/) repository has to be
installed before running this role.

If you want to use creation of the WatchedList database, MySQL has to be installed first.
The role [mjanser.mysql](https://github.com/mjanser/ansible-role-mysql) can be used for this task.

## Role Variables

Available variables are listed below, along with default values:

    kodi_type: service # service, desktop
    kodi_user: kodi
    kodi_user_create: yes
    kodi_user_groups: audio,video

    kodi_pvr_addons: []

    kodi_database_password: secret

    kodi_watched_list_database: false
    kodi_watched_list_database_backup: ~

    kodi_firewall_zones: []

### Type

The variable `kodi_type` specifies how kodi will be used. Possible values are:

- `desktop`: just installs kodi ready for usage as a desktop user
- `service`: installs a service which can start and stop kodi without a desktop environment

### User

The user which is defined in the variable `kodi_user` is used to set permissions on directories and files created
during the setup.

If `kodi_type` is set to `service` this user will be created on the system including a home directory by default.
You can disable that by setting `kodi_user_create` to `no`. The service will be started as that user.

The created user will be in the groups `audio` and `video` by default.
Additional groups can be added to the variable `kodi_user_groups` as a comma separated string.
This can be useful if you, for example, want to run pulseaudio in system mode.
Then you can add the kodi user to the `pulse` group.

The user won't have a password set and cannot login for security reasons.

### PVR add-ons

You can add PVR add-ons to the variable `kodi_pvr_addons` which will then be installed, for example:

    kodi_pvr_addons:
      - mythtv
      - hts

### Database password

The database password will only be used if there are databases to setup, see the other variables for possible use cases.

### Watched List add-on

The Kodi add-on [WatchedList](http://kodi.wiki/view/Add-on:WatchedList) can be used to save the state of watched movies.
Those states are stored in a MySQL database which will be created you set the `kodi_watched_list_database` to true.

If you also set `kodi_watched_list_database_backup` to a path to a SQL file, that SQL will be used to fill the database
on creation.

### Firewall

The variable `kodi_firewall_zones` can be used to declare firewall zones in which kodi should be accessible.
This means the ports `8080/tcp` and `9777/udp` will be opened.

Currently only `firewalld` is supported which is default on Fedora.

## Dependencies

None

## Example Playbook

    - hosts: all
      roles:
        - { role: mjanser.kodi }

## License

MIT
