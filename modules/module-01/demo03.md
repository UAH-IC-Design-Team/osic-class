# demo03 - Magic Part 1
Please clone the [demo03](https://github.com/UAH-IC-Design-Team/demo03) to follow along.

# A few notes about extra things:

### Trouble with `git fetch`
```
git fetch [remote] [branch]

git checkout -b [branch] [start_point] 

git switch [branch]


```

### Package Managers
Rather than having a "store" or other gui installers, Linux uses a package manager. Since Docker is running linux you may need to install things into the image with the package manager.

Package managers work by having a list of repositories. Before you are able to install any packages you must update the list of package repositories.

```
# Ubuntu
apt update 
apt upgrade
apt install

# Alpine
apk update
apk add

```

# Magic
[Official Magic Documentation](http://opencircuitdesign.com/magic/index.html)

[Extra Notes](https://github.com/UAH-IC-Design-Team/documentation/wiki/Magic-and-Netgen)

[Dr. Pretl's Cheat Sheet](https://github.com/iic-jku/iic-osic/blob/main/magic-cheatsheet/magic_cheatsheet.pdf)

[Video from Tim Edwards on Magic](https://us06web.zoom.us/rec/play/EFHC4L8Xi1lmg6nev1HHwrlEfF-yKZA0PvR9i9eObBRprVjZHw3-ylQ2-97cNWjQbKdkZXgLQBjKzE1h.Wa-s9bzRRZwRaVrz?startTime=1658500417000&_x_zm_rtaid=-kxcJqW7SoyxOjWqxfwREg.1658935344346.c723fee525ef47c93a4a9ba2497bcbdc&_x_zm_rhtaid=669)

The Magic tutorials from the official documentation are some of the best resources.

### What does Magic Do?
- Layout
- DRC
- LEF/DEF Reading
- GDS Generation
- Extration and Netlisting

### Starting Magic
You must either define an RC file with `-rcfile` or have a `.magicrc` file in the directory that starts magic.

Magic has different gui rendering: Cairo is the recomended and it is started with `-d XR`

```
magic -d XR -rcfile $PDK_ROOT/sky130A/libs.tech/magic/sky130A.magicrc

# Or if .magicrc exists
magic -d XR
```

### Two windows
- Layout Window: This is where all the painting and such happens.
- Tcl command window.

### What is tcl?
[Tcl](https://en.wikipedia.org/wiki/Tcl) is a high level interpreted programing language. It is extremely common for IC development in both the open source and proprietary tools. The author of tcl is John Ousterhout who also was one of the original maintainers of Magic.

The `help` command is extremely helpful. 

### Magic Theory
- There are no objects in Magic. Instead Magic works by 'painting' and 'erasing' layers. 
- Behind the hood it uses a pointer reference and you can see how magic creates shapes by `*watch <layer` and hide with `*watch`.
- Magic also uses tcl algorithms in the background to draw and generate layers that are hidden. You can show the hidden layers with `cif see <layer>` and hide them with `feedback clear`

### Navigation
- Zoom in: `z`
- Zoom out: `Shift-z`
- Zoom box: `Ctrl-z`
- Box set bottom left corner:

### Resistor
- pwell
- poly
- npolyres
- polycont
- locali
- viali
- m1

- Labeling
- Port make

### LVS Extraction
```
extract 
# or extract -noall

ext2spice lvs

ext2spice
```
