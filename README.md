# irssi-nickserv_osx_keychain

Irssi script to identify yourself to NickServ using a password stored in the OS
X keychain

If you want to get your plain text NickServ password out of your irssi
configuration this script might be for you...

When you are using irssi to connect to an irc network that uses NickServ to
perform nickname registration/identification chances are you have added an
`autosendcmd` to your configuration that immediately idenfies you when you
connect. While this works great you have to put your plain text password into
your irssi configuration. This might not be bothering you as your NickServ
password might be exposed in other ways (see [below](#A-Note-on-Security)).
However if you are using a (public) /dotfiles/ repository to sync your
configuration between different machines your password would be visible to
everyone.

## Requirements

This script was developed for irssi 0.8.15, but all recent irssi versions
should work. Just make sure that you have Perl scripting support compiled in.

## Installation

To get the most of this script it should be executed automatically at startup.
Put the script file in the autorun folder (`~/.irssi/scripts/autorun`), or
better place the script in the `scripts` folder and add a symlink to it in
`autorun`. See [irssi FAQ](http://irssi.org/documentation/faq).

## Usage

This script registers a new command called `/IDENTIFY` that requires an account
name as parameter. If you execute `/IDENTIFY MyNick@irc.freenode.net` from
irssi the script will look up the password for that account in the OS X
keychain. If the account information is found it will execute `/MSG NickServ
identify your_password`.

### Adding Your Password to the keychain

From a terminal run
```
security -s "irssi" -c "irss" -a "MyNick@irc.freenode.net" -l "irssi: MyNick@irc.freenode.net"
```
to create an entry for your account in the keychain. Customize the account name
(`-a`) and the label (`-l`), but leave the other options alone. You will later
use the account name as the paramater for the `/IDENTIFY` command.

To enter the actual password open _Keychain Access_ on your Mac, locate the
entry you have just created and add the password to it.

*You can actually set the password from the command line using the `-w` switch,
but then it would end up in your shell's history...*

### Automatically Identify on Connection (optional)

If you want to be identified automatically every time you connect to a network,
you can add an `autosendcmd` for the network to your configuration. For
example:
```
chatnets = {
  freenode = {
    type = "IRC";
    autosendcmd = "/IDENTIFY MyNick@irc.freenode.net";
    # your other settings
  };
  # ...
};
```
For more details check out Section 5.2 of the [irssi
manual](http://irssi.org/documentation/manual).

## A Note on Security

Please note that the identification is a regular private message that is send
to NickServ. That means that the plain text password is transmitted to the
server. The password will also be visible in the query window that is opened
when talking to NickServ and (depending on your configuration) might be written
to the chat logs.

## License

Copyright (c) 2013, Sven Strothoff
All rights reserved.

This script is released under the [GNU GPL
v3](http://www.gnu.org/licenses/gpl.txt).
