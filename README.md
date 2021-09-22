# sabs
A thin wrapper to make ABS overpowered

Arch has its own in-built build system called the 'Arch build system'(ABS). You may have encountered ABS when you used `makepkg` to install an AUR helper.'

Through the use of some `fakeroot` dark magic, `makepkg` converts a PKGBUILD file(written in bash) into a `.tar.zst`
file which can then be used with `pacman -U` to install the package.

SABS is a framework on top of ABS to make it easy to update, patch, and maintain PKGBUILD file ans source files.

# Quick start
Install SABS
``` bash
git clone https://github.com/pheonix9001/sabs.git
[doas|sudo] ./install
```

Edit ~/.config/sabs.conf

now lets install a package. atm you can only install custom and git packages. so
``` bash
sabs-git https://aur.archlinux.org/baph.git
```

Lets see what changed.

```
 ~>
 λ baph
bash: baph: command not found
```

Whut? well you have to now compile your software.

`sabs build baph`

now it should be fine

```
 ~>
 λ baph
error: no operation specified (use -h for help)
```

Great but baph has one issue. it depends on `sudo`. and I run a `doas` based system. it would be nice if you could patch baph to remove `sudo`. Run

```
 ~>
 λ sabs ebd baph
:: Generating pre-download patch
:: Initializing build
patch: **** Only garbage was found in the patch input.
:: Opening shell...
The patch is automatically generated when you close this shell.
Use this shell to modify the package files
 ~/.local/abs-pkg/baph/src>
 λ ls
 PKGBUILD
```

`ebd` stands for 'edit before download'. now you can edit the PKGBUILD however you like. In this case modify the `depends` array.
Once you are done, close the shell with `exit` or `ctrl-d`. It should then rebuild the code

```
Depends On      : awk  curl  sed  coreutils
```

But you also have to edit the source code.
In order to do that, use `ebb`('edit before build'). This will take a bit longer but it should give you a similliar shell.

```
 ~/.local/abs-pkg/baph/src>
 λ ls
 PKGBUILD  baph      src
```

Now you can edit the files under `src/` and close the shell. it should now rebuild baph properly

```
 ~>
 λ baph -u
doas (asdf@desktop) password:
...
```

Perfect! but this is where it gets even cooler, if you then rebuild baph(`sabs build baph`) or update baph(`sabs update baph`) it will automatically reapply
all your changes. If you then do `sabs ebb baph` again for some reason, SABS will take you exactly where you left off so you dont have to redo `s/sudo/doas/`.
This means you can use SABS to keep track of your suckless projects with ease.

# Name
sabs stands for 'super ABS'. pretty bad name tbh.

# Usage
`sabs (build|update|ebb|ebd) <package>
