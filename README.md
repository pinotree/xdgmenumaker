ABOUT
=====

**xdgmenumaker** is a command line tool, written in python, that generates
application menus using xdg information, by scanning `*.desktop` files
in all `$XDG_DATA_DIRS/applications` directories. All applications are
sorted according to the main categories as specified by
[freedesktop.org](http://standards.freedesktop.org/menu-spec/latest/apa.html).

The menu entries that are generated by xdgmenumaker are localised
according to the running user locale settings.

xdgmenumaker currently supports generating menus for **blackbox**,
**compizboxmenu**, **fluxbox**, **icewm**, **jwm**, **pekwm** and
**windowmaker**.

pyxdg and pygtk are required by xdgmenumaker, in addition to python 2.


Blackbox
=======

To generate an application menu for blackbox, run xdgmenumaker like this:

    $ xdgmenumaker -f blackbox > ~/.blackbox/xdg_menu

and then change your main blackbox menu to include this file as a
submenu. For example, add this somewhere in your `~/.blackbox/menu` file:

    [include] (~/.blackbox/xdg_menu)

You can add the xdgmenumaker command as another item in your menu, if
you want to update it, without having to run the command manually again:

    [exec] (Update Blackbox Menu) {xdgmenumaker -f blackbox > ~/.blackbox/xdg_menu}


Compiz Boxmenu
==============

There are two ways to have an xdg menu in compiz-boxmenu. The first one,
auto-updates the menu, every time the menu is called. The second one,
updates the menu only when the user wants to.

Dynamic Menus
-------------

Edit your `~/.config/compiz/boxmenu/menu.xml` file with your favorite text 
editor and add a block of code like this inside the root `<menu>` element:

    <item type="launcher">
      <command mode2="pipe">xdgmenumaker -nif compizboxmenu</command>
      <icon>applications-other</icon>
      <name>Applications</name>
    </item>

Alternatively, you can also run `compiz-boxmenu-editor` and click the 
dropdown for new menu files or menu items. Select launcher to create a 
new launcher. Set the name of the launcher to whatever you want. This will
be the display name for the pipe menu. Then enter in:

    xdgmenumaker -nif compizboxmenu

for the command entry. Click the combobox next to the command text box
and switch that to "Pipe".

Static Menus
------------

Edit your `~/.config/compiz/boxmenu/menu.xml` file with your favorite text 
editor and paste the output of:

    $ xdgmenumaker -if compizboxmenu

into `~/.config/compiz/boxmenu/menu.xml`.

Alternatively, you can also run `compiz-boxmenu-editor` and click the
button that says "Generate menu entries from a pipemenu script". In the dialog
box that pops up, type in:

    xdgmenumaker -nif compizboxmenu

to append the statically generated menu to any menu file you want.


Fluxbox
=======

To generate an application menu for fluxbox, run xdgmenumaker like this:

    $ xdgmenumaker -f fluxbox > ~/.fluxbox/xdg_menu

and then change your main fluxbox menu to include this file as a
submenu. For example, add this somewhere in your `~/.fluxbox/menu` file:

    [include] (~/.fluxbox/xdg_menu)

You can add the xdgmenumaker command as another item in your menu, if
you want to update it, without having to run the command manually again:

    [exec] (Update Fluxbox Menu) {xdgmenumaker -f fluxbox > ~/.fluxbox/xdg_menu}


IceWM
=====

To generate an application menu for icewm, run xdgmenumaker like this:

    $ xdgmenumaker -f icewm > ~/.icewm/appmenu

or if you want icons in your menu:

    $ xdgmenumaker -i -f icewm > ~/.icewm/appmenu

and you can then edit your `~/.icewm/menu` file and add this line
somewhere:

    include appmenu

You can add the xdgmenumaker command as another item in your menu, if
you want to update it, without having to run the command manually again:

    prog "Update Menu" _none_ xdgmenumaker -i -f icewm > ~/.icewm/appmenu

**NOTE:** If you don't request icons in the menu, or if an icon is not found
for a certain app, the icon name in the menu for that app is set to
`"_none_"`. This doesn't actually set the icon for that app to none. Icewm
menu entries should always include an icon. So, by pointing it to a
non existing icon, you essentially set it to use no icon. If you
actually have an icewm icon named `"_none_"`, that one will be used
instead.


JWM
===

You can edit your `~/.jwmrc` file and add a line that generates the
applications menu, like this:

    <Include>exec: xdgmenumaker -n -i -f jwm</Include>

You need to put that line somewhere in the *RootMenu* section of the
`~/.jwmrc` file.

You can update the menu with:

    $ jwm -reload

Or you can restart JWM and the updated menu should appear. The menu will be
recreated every time JWM is started, restarted, or when the menu is
reloaded with the above command. You can even add a menu item that will
refresh the menu, like this:

    <Program label="Refresh Menu">jwm -reload</Program>


pekwm
=====

There are two ways to have an XDG menu in pekwm. The first one,
auto-updates the menu, every time the menu is called. The second one,
updates the menu only when the user wants to.

Dynamic Menus
-------------

Edit your `~/.pekwm/menu` file with your favourite text editor and add
a line like the following one in the location that you want the
dynamically generated menu to appear:

    Entry = "" { Actions = "Dynamic /usr/bin/xdgmenumaker -n -i -f pekwm --pekwm-dynamic" }

Restart pekwm and the generated menu should appear. The menu will be
automatically generated every time you access it, so it will always be
up to date. But since xdgmenumaker will run every time you access the
menu, the menu might not appear instantly, especially if you are using
an older PC.

Static Menus
------------

Run:

    $ xdgmenumaker -n -i -f pekwm > ~/.pekwm/appsmenu

to create a file with the menu contents. Then edit your
`~/.pekwm/menu` file to include that menu, by adding a line like the
following, in the location that you want the menu to appear:

    INCLUDE = "/home/your_user_name/.pekwm/appsmenu"

Restart pekwm and the generated menu should appear. The menu is static
and if you add/remove any applications, you will have to run the
xdgmenumaker command and restart pekwm all over again to update it. The
advantage is that there will be no delay in displaying the menu.


Window Maker
============

There are two ways to have an xdg menu in windowmaker. The first one,
auto-updates the menu, every time the menu is called. The second one,
updates the menu only when the user wants to.

xdgmenumaker uses utf8 encoding and localised strings by default and has
been tested only with wmaker-crm>=0.95.1. No idea if utf8 works properly
with older Window Maker versions.

Dynamic Menus
-------------

Open the WindowMaker preferences tool. In the Application Menu
Definition section, add a Generated Submenu in your menu, by dragging it
in. Click on the menu item you just dragged in and in the preferences
window, in Command, add:

    xdgmenumaker -f windowmaker

Save and close the preferences window.

That command will be run every time you access that submenu, so the
application list in there will be always up to date. The downside is
that it will be run every time you access that submenu, so especially if
you are on a very old PC, it might slow things down a bit, although
probably not anything considerable.

Static Menus
------------

Run:

    $ xdgmenumaker -f windowmaker > ~/GNUstep/Defaults/xdg_menu

Then open the WindowMaker preferences tool and in the Application Menu
Definition section, add an External Submenu by dragging it in your menu.
Click on the menu item you just dragged in and in the preferences
window, in Path for Menu, add the location of the menu file you just
created:

    ~/GNUstep/Defaults/xdg_menu


You can add the xdgmenumaker command as another item in your menu, if
you want to update it, without having to run the command manually again.
In the Application Menu Definition section in the WindowMaker
preferences window, add a Run Program item in your menu by dragging it
your menu. Click on the menu item you just dragged in and in the
preferences window, in Program to Run, add the xdgmenumaker command as
mentioned above.

The downside of this method, is that the menu contents will not be
updated when you install a new application or remove one. You will need
to run the xfgmenumaker command every time you want the menu to be
updated. The upside is that the menu will not be generated every time
you access the menu. This might be a better choice for (really) older
hardware.

