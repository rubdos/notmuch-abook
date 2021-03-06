Notmuch Addressbook manager for vim
===================================

DEPENDENCES
-----------

* notmuch with python bindings

INSTALL
-------

* Standalone install

if you do not want to use the vim script file, you can install the module as follows:

```
python setup.py install
```

or using:

```
pip install notmuch_abook
```

* Vimscript install

Use vundle to install this script, add to your vimrc:

```
Bundle "guyzmo/notmuch-abook"
```

for convenience, you can create a symlink to your bin directory:
```
ln -s $HOME/.vim/bundle/notmuch-abook/pylibs/notmuch_addresses.py ~/bin/notmuch-abook
```

CONFIGURATION
-------------

Open notmuch configuration file (usually $HOME/.notmuch-config) and add:

```
[addressbook]
path=/home/USER/.notmuch-abook.db
backend=sqlite3
```

where USER is your username (or at any other place)

Default notmuch query string is

```
NOT tag:junk AND NOT folder:drafts AND NOT tag:deleted
```

If you prefer something else, you can specify it in notmuch configuration file:

```
[addressbook]
path=/home/USER/.notmuch-abook.db
backend=sqlite3
query=folder:Inbox OR folder:Sent
```

If you use a non-default notmuch configuration file, you can set it in your vimrc with:

```
let g:notmuchconfig = "~/.notmuch-config-custom"
```

In your favorite mail filter program, add a rule such as (for procmail), so all new mail will be parsed:

```
:0 Wh
| python $HOME/.vim/bundle/notmuch-abook/pylibs/notmuch_addresses.py update
```

If you can't use procmail (eg if you are using offlineimap) then you could put the following few lines at the start of the [post-new hook](http://notmuchmail.org/manpages/notmuch-hooks-5/) (**before** you remove the new tag).  Also note this is shell syntax, so you'll have to adapt if your hook is in another language.

```
# first update notmuch-abook
for file in $(notmuch search --output=files tag:new) ; do
    cat "$file" | $HOME/bin/notmuch-abook update
done
```

USAGE
-----

For the first time, you shall launch the script as follows to create the addresses database (it takes ~20 seconds for 10000 mails):

```
python $HOME/.vim/bundle/notmuch-abook/pylibs/notmuch_addresses.py create
```

and then, to lookup an address, either you use the vimscript to complete (<c-x><c-u>) the name in a header field,
or you can call it from commandline:

```
python $HOME/.vim/bundle/notmuch-abook/pylibs/notmuch_addresses.py lookup Guyz
```

the script will match any word beginning with the pattern in the name and address parts of the entry.

LICENSE
-------

(c)2013, Bernard Guyzmo Pratz, guyzmo at m0g dot net

Even though it is a WTFPL license, if you do improve that code, it's great, but if you
don't tell me about that, you're just a moron. And if you like that code, you have the
right to buy me a beer, thank me, or [flattr](http://flattr.com/profile/guyzmo)/[gittip](http://gittip.com/guyzmo) me.

```
DO WHAT THE FUCK YOU WANT TO PUBLIC LICENSE 
Version 2, December 2004 

Copyright (C) 2004 Sam Hocevar <sam@hocevar.net> 

Everyone is permitted to copy and distribute verbatim or modified 
copies of this license document, and changing it is allowed as long 
as the name is changed. 

DO WHAT THE FUCK YOU WANT TO PUBLIC LICENSE 
TERMS AND CONDITIONS FOR COPYING, DISTRIBUTION AND MODIFICATION 

0. You just DO WHAT THE FUCK YOU WANT TO.
```

