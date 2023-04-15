# Project.sh

This script provides simple project management on Linux systems.
It's capable of managing directories and groups on a shared system where ACLs may be either unsupported or too complex for users.

Additionally, it has a helper that can be added to a `.bashrc` file to change to a project directory and activate any virtual environments.

## How to use

Here's the help text for version 1.0:

```
Usage:
    project [action [project [user]]]

Actions:
    bashrc - exports source-able script for bashrc (see note in Misc)
    create - create a new project
    delete - delete a project
    fixperms - resets the ownership & permissions of a project's files
    help - see this help text
    paths - see the project's location
    users - see the users in a project
    useradd - adds a user to a project
    userrm - removed a user from a project

Examples:
    project create foo
    project useradd foo alice
    project userrm foo bob
    project fixperms foo
    project delete foo

Misc:
    System umask should be set to 007 for projects to work best.
    Add this to users' .bashrc for convience:
        source $(project bashrc)
    The .bashrc also enables the command \"proj <project>\" to easily change
    to a project's directory, set the umask, and open a venv if it's found
```

## How to install

> NOTE: Some actions, such as creating/deleting projects and adding/removing users to projects, require `sudo` permissions.

### User Install

If you have `sudo` access and only want to install this script for yourself, place the `project` file in the `PATH` entry within your home directory, likely at `~/.local/bin/project`

In one command (without `sudo`):

```sh
curl -o ~/.local/bin/project https://raw.githubusercontent.com/ct-martin/project.sh/main/project
```

To add the `proj` command for your use, add the following to your `~/.bashrc`

```
source $(project bashrc)
```

### System-wide Install

Place the `project` file somewhere in the system's `PATH`, for example, `/usr/local/bin/project`

The recommended ownership is `root:root` and permissions are `0711` - this allows users to only be able to execute the script.

Here are some commands to do this:

```sh
sudo curl -o /usr/local/bin/project https://raw.githubusercontent.com/ct-martin/project.sh/main/project
sudo chown root:root /usr/local/bin/project
sudo chmod 0711 /usr/local/bin/project
```

To add the `proj` command for all users, add the following to `/etc/bash.bashrc`

```
source $(project bashrc)
```

> NOTE: this will add a short MOTD-like message when a user logs in.
> Submit a feature request via the GitHub Issues if you'd

If the only people who will be able to manage projects already have `sudo` access, no extra changes are needed.

If you want to allow some people to manage projects without giving them full `sudo` access, you can add an entry to the `sudoers` file.

> NOTE: Anyone who has sudo access to the project script will be able to modify all projects

For example, this line will allow users in the `project-managers` group to manage projects without allowing them to use other commands:

```
%project-managers ALL=(ALL) /usr/local/bin/project
```

> NOTE: Use `visudo` or similar when editing any `sudoers` files to prevent accidentally breaking sudo access
