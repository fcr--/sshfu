# Shared config.

It is fair to say that one of the main reasons why somebody should use sshfu is probably because there are hundred or thousands of hosts on the configuration. And on those cases there's also a high chance of several users needing to use the same configuration, at least for a big subset.

That's why in this tutorial we provide some advices concerning the modularization and parametrization of sshfu configuration files.

## Modularization.

Since the final objective is to share configuration between users, the most basic way to modularize is to move a chunk of the configuration to a different file and include it from the main configuration. To include it just use `source` with its path:

```tcl
source ~/.ssh/sshfu/different-file.routes
```

:+1: **Hint**: That file could be located on a git/svn repository, enabling further collaboration between its users.

## Parametrization.

When the configuration is shared between different users, some configuration parameters vary more than others, here's a small list of the paratrization mechanisms, each with a reference to a parameter that almost always change:

### Variables.

Example: Username used to connect to the servers.

In most organizations there's a naming standard for usernames, and that same username given to each person is used for most servers uniformly. Requiring a variable with the username to be defined is a good way to enforce the user to define it.

```tcl
host org-fw address 198.51.100.43 user $ORG_USER
host org-mail address 192.168.1.42 user $ORG_USER gw org-fw
host org-router3 address 192.168.1.87 user root gw org-fw; # only has root access
host org-test address 192.168.1.30 user $ORG_USER gw org-fw
```

### Predicates.

Example: CONTEXT.

When sshfu is configured, most users choose parametric configurations where CONTEXT is assigned to different values depending on the location of the user. Since the values for CONTEXT are not always standarized, a parametrized configuration file can use predicates instead of CONTEXT.

**TODO**: show example

### Defaults parameters.

Example: Private keys.

On most cases it's recommended the configuration file to don't specify any key at all, leaving the user the option to set the default key parameter with `with` before including the file. However some servers with stringent security requirements may require different private keys; a simple alternative is just to call a procedure `key_for` that the user ought to implement.

**TODO**: show example
