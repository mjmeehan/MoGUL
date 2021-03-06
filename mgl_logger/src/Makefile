##############################################################################
#
# The Linux-GCC Makefile
#
##############################################################################

#
# Object files.
#

OBJECTS = $(patsubst %.c,%.o,$(wildcard *.c))

#
# Compiler stuff -- adjust to your system.
#

# Linux
PROJECT = mgl_types
CC      = gcc
#CC      = clang
INC_PATHS = ../include ../../mgl_types/include
INC_DIR =$(foreach d, $(INC_PATHS), -I$d)
LIB_DIR = ../../libs
GLIB_CFLAGS = `pkg-config --cflags glib-2.0`
GLIB_LDFLAGS = `pkg-config --libs glib-2.0`
SDL_CFLAGS = `sdl2-config --cflags` $(INC_DIR)
SDL_LDFLAGS = `sdl2-config --libs` -lglib-2.0
LFLAGS = -g -shared -Wl,-soname,lib$(PROJECT).so.1 -o $(LIB_DIR)/lib$(PROJECT).so.1.0.1
CFLAGS = -g  -fPIC -Wall -pedantic -Wno-unknown-pragmas 
# -fgnu89-inline 
# -std=gnu99 
# -ffast-math for relase version

DOXYGEN = doxygen

#
# Targets
#

$(PROJECT): $(OBJECTS)
	$(CC) $(OBJECTS) $(LFLAGS) $(SDL_LDFLAGS) $(GLIB_LDFLAGS)

docs:
	$(DOXYGEN) doxygen.cfg

sources:
	echo $(patsubst %.c,%.o,$(wildcard *.c)) > makefile.sources
#	@-rm makefile.sources
#	@echo Building source list
#	@for i in *.c; do echo `basename $$i .c`.o\\ >> makefile.sources; done
#	@echo Done

makefile.dep: depend

depend:
	@touch makefile.dep
	@-rm makefile.dep
	@echo Creating dependencies.
	@for i in *.c; do $(CC) $(INC) -MM $(INC_DIR) $$i; done > makefile.dep
	@echo Done.

clean:
	rm *.o $(LIB_DIR)/lib$(PROJECT).a

count:
	wc -l *.c $(foreach d, $(INC_PATHS), $d/*.h) makefile

static:
	ar rcs $(LIB_DIR)/lib$(PROJECT).a $(OBJECTS)

#
# Dependencies.
#

include makefile.dep

#
# The default rule.
#

.c.o:
	$(CC) $(CFLAGS) $(SDL_CFLAGS) $(GLIB_CFLAGS) -c $<


