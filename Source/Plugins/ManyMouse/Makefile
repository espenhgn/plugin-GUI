# Set this variable if you need to.
WINDOWS_JDK_PATH := C:\\Program\ Files\\Java\\jdk1.6.0_02\\
LINUX_JDK_PATH := /usr/lib/j2se/1.4/

linux := false
macosx := false
cygwin := false

uname_s := $(shell uname -s)
ifeq ($(strip $(uname_s)),Darwin)
  macosx := true
else
  uname_o := $(shell uname -o)
endif
ifeq ($(strip $(uname_s)),Linux)
  linux := true
endif
ifeq ($(strip $(uname_o)),Cygwin)
  cygwin := true
endif

CFLAGS += -O0 -Wall -g -c
CFLAGS += -I.

#CFLAGS += -ISDL-1.2.8/include
#LDFLAGS += -LSDL-1.2.8/lib -lSDL -lSDLmain

CC := gcc
LD := gcc

ifeq ($(strip $(macosx)),true)
  LDFLAGS += -framework Carbon -framework IOKit
  JAVAC := javac
  MANYMOUSEJNILIB := libManyMouse.jnilib
  JNICFLAGS += -I/System/Library/Frameworks/JavaVM.framework/Headers
  JNILDFLAGS += -bundle -framework JavaVM
endif

ifeq ($(strip $(linux)),true)
  CFLAGS += -fPIC -I/usr/src/linux/include
  LDFLAGS += -ldl
  JDKPATH := $(LINUX_JDK_PATH)
  JAVAC := $(JDKPATH)bin/javac
  MANYMOUSEJNILIB := libManyMouse.so
  JNICFLAGS += -I$(JDKPATH)include -I$(JDKPATH)include/linux
  JNILDFLAGS += -shared -Wl,-soname,$(MANYMOUSEJNILIB)
endif

ifeq ($(strip $(cygwin)),true)
  JDKPATH := $(WINDOWS_JDK_PATH)
  JAVAC := $(JDKPATH)bin\\javac
  MANYMOUSEJNILIB := ManyMouse.dll
  JNICFLAGS += -I$(JDKPATH)include -I$(JDKPATH)include\\win32
  JNILDFLAGS += -Wl,--add-stdcall-alias -shared
endif



BASEOBJS := linux_evdev.o macosx_hidutilities.o macosx_hidmanager.o windows_wminput.o x11_xinput2.o manymouse.o

.PHONY: clean all

all: detect_mice test_manymouse_stdio

clean:
	rm -rf *.o *.obj *.exe *.class $(MANYMOUSEJNILIB) *.o *.obj test_manymouse_stdio detect_mice

%.o : %c
	$(CC) $(CFLAGS) -o $@ $<

detect_mice: $(BASEOBJS) detect_mice.o
	$(LD) -o $@ $+ $(LDFLAGS)

test_manymouse_stdio: $(BASEOBJS) test_manymouse_stdio.o
	$(LD) -o $@ $+ $(LDFLAGS) 

# end of Makefile ...

