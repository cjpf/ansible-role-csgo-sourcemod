# Ansible Role: csgo-sourcemod

[![builds.sr.ht status](https://builds.sr.ht/~tleguern/ansible-role-sourcemod.svg)](https://builds.sr.ht/~tleguern/ansible-role-sourcemod?)

An Ansible role that installs and configures [SourceMod](https://www.sourcemod.net/), a [Metamod:Source](http://www.metamodsource.net/) plugin.

This project is a fork from https://github.com/tleguern/ansible-role-sourcemod

## Requirements

An ansible role dedicated to the installation of SteamCMD such as [ansible-steamcmd](https://github.com/tleguern/ansible-steamcmd) or any role providing the `{{ steamcmd_user }}` variable.

An ansible role dedicated to the installation of Counter-Strike: Global Offensive such as [ansible-role-csgo](https://github.com/cjpf/ansible-role-csgo) or any role providing the `Restart {{ metamod_source_game }}` handler.

An ansible role dedicated to the Installation of Metamod:Source such as [ansible-role-metamod-source](https://github.com/tleguern/ansible-role-metamod-source), or any role providing the `{{ metamod_source_install_path }}`,

## Role Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `steamcmd_user` | User name for steamcmd | `steam` |
| `sourcemod_url` | URL pointing to sourcemod releases | `https://sm.alliedmods.net/smdrop` |
| `sourcemod_branch` | Release branch (should generally be the same as `{{ metamod_source_branch }}` | `1.11` |
| `metamod_source_install_path` | Installation directory | mandatory |
| `sourcemod_admins_simple` | SourceMod admin declaration via the flat file format | See below |
| `sourcemod_plugins` | List of plugins to enable or disable | See below |
| `follow_guidelines` | Set `false` when you need `FollowCSGOServerGuidelines` set to `No`. Required for some plugins to function. Your GSLT could potentially be banned when this is disabled. | `true` |

### `sourcemod_admins_simple`

A list of hashes containing the identity and flags of any server administrator.
Optionnaly immunity levels and password can be suplied.

| Key | Description |
|-----|-------------|
| `identity` | A SteamID3 formatted SteamID, a bang-prefixed IP address or a simple name |
| `flags` | Letter encoded permission levels |
| `immunity` | Immunity level |
| `password` | Password, only for somple name `identity` |

More information [here](https://wiki.alliedmods.net/Adding_Admins_(SourceMod)).

Example:

```
sourcemod_admins_simple:
  - identity: STEAM_0:1:16
    flags: bce
  - identity: "!127.0.0.1"
    immunity: "99"
    flags: z
  - identity: BAILOPAN
    flags: abc
    password: Gab3n
```

## `sourcemod_plugins`

Allows to install, remove, enable or disable plugins.

| Key     | Description                            |
|---------|----------------------------------------|
| `name`  | The plugin's name without file suffix  |
| `state` | Only `absent`, `disabled` or `enabled` |

If the state is either `disabled` or `enabled` and the variable `sourcemod_extra_plugins_directory` is not an empty string the corresponding plugin will be looked for as `{{ sourcemod_extra_plugins_directory }}/{{ plugin.name }}.smx` and uploaded on the remote server.

Example:

```
sourcemod_plugins:
  - name: funcommands
    state: disable
  - name: unwanted
    state: absent
  - name: swapteam
    state: enabled
```

## Dependencies

None

## Example Playbook

```yaml
- hosts: game
  vars:
   sourcemod_admins_simple:
     - identity: STEAM_0:1:16
       flags: z
  roles:
    - role: ansible-steamcmd
    - role: ansible-role-cstrike-source
    - role: ansible-role-metamod-source
    - role: ansible-role-sourcemod
```

## License

ISC

## Contributing

Please send a Gitlab Pull Request to contribute to this project.
https://gitlab.com/csgo-services/ansible-role-csgo-sourcemod

## Author Information

CJ Pfenninger <cjpf@charliejuliet.net>
