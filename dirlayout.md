# Directory Layout

```
...somewhere in the filesystem hierarchy...
   |
   \ 
     <package root>
     |
     +- (optional) stublibs
     +- (optional) postinstall
     +- (optional) postremove
     |
     +- package1
     |  |
     |  +- META
     |  +- archive files (cma, cmxa)
     |  +- interface definitions (cmi)
     |
     +- package2
     |  |
     |  +- META
     |  +- archive files (cma, cmxa)
     |  +- interface definitions (cmi)
     +
     :
     :
     \
        packageN
```

## Description

Every installation of findlib has a default location for package
directories, which we call the package root in the following.
Besides the package root there can be alternate locations for
storing packages. All of these locations are called "package
locations" in the following.

The name of a package is the name of the package directory. For
example, for the package root `/usr/local/lib/ocaml/site-lib`, the
package p will be installed in the subdirectory
`/usr/local/lib/ocaml/site-lib/p`. This subdirectory
must contain the META file and all other files belonging to the package.
Package names must not contain the '.' character.

### Search path

The environment variable `OCAMLPATH` is interpreted as a colon
(Windows: semicolon)-separated search path of further locations
of packages besides the package root. This search path takes
precedence over the package root. Besides `OCAMLPATH` a package
manager may provide other means of specifying package locations.

For systems with DLL support another directory may exist: `stublibs`,
at a configurable place.
If present, findlib will install DLLs into this directory that is
shared by all packages at the same package location. Findlib remembers
which DLL belongs to which package by special files with the suffix
".owner"; e.g. for the DLL "dllpcre.so" there is another file
"dllpcre.so.owner" containing the string "pcre", so findlib knows
that the package "pcre" owns this DLL. It is not possible that a DLL
is owned by several packages.

If the stublibs directory does not exist, DLLs are installed regularly
in the package directories like any other file.

For special needs, a postinstall and/or a postremove script may be
installed in the package location. These scripts are invoked after
installation or removal of a package, respectively.
