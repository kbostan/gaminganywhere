change export GADEPS=$HOME/gaminganywhere/deps.posix in env-setup to your path
change GADEPS ?= $(HOME)/gaminganywhere/deps.posix in deps.src/Makefile to your path
change GADEPS?= $(HOME)/gaminganywhere/deps.posix in ga/Makefile.def to your path

or source env-setup

    Merge environment variables from 'env-setup' by using '.' or 'source' command.
    Build dependencies by running 'make' in the 'deps.src' directory.
    Build GA by running 'make all' command in the 'ga' directory.
    Install GA by running 'make install' command in the 'ga' directory. All the generated files will be installed into 'bin' directory.

