.PHONY: all clean debug release release_debug demo install

STATIC_PREFIX=lib
DYNLIB_PREFIX=lib
STATIC_EXT=.a
DYNLIB_EXT=.so
EXE_EXT=
INSTALL=install

DESTDIR=
PREFIX=/usr/local
LIBDIR=$(DESTDIR)$(PREFIX)/lib
INCDIR=$(DESTDIR)$(PREFIX)/include/nsfplay

OBJDIR := .objs

LIB_STATIC=$(STATIC_PREFIX)nsfplay$(STATIC_EXT)
LIB_DYNLIB=$(DYNLIB_PREFIX)nsfplay$(DYNLIB_EXT)

CFLAGS_WARN := -Wall -Wno-deprecated-declarations

CXXFLAGS_DEBUG := -g -O0 $(CFLAGS_WARN) -fPIC -std=c++17
CFLAGS_DEBUG := -g -O0 $(CFLAGS_WARN) -fPIC

CXXFLAGS_RELEASE := -g0 -O2 $(CFLAGS_WARN) -fPIC -std=c++17 -DNDEBUG
CFLAGS_RELEASE := -g0 -O2 $(CFLAGS_WARN) -fPIC -DNDEBUG

CXXFLAGS_RELEASE_DEBUG := -g -O2 $(CFLAGS_WARN) -fPIC -std=c++17 -DNDEBUG
CFLAGS_RELEASE_DEBUG := -g -O2 $(CFLAGS_WARN) -fPIC -DNDEBUG

ifeq ($(CFLAGS),)
CFLAGS := $(CFLAGS_DEBUG)
endif

ifeq ($(CXXFLAGS),)
CXXFLAGS := $(CXXFLAGS_DEBUG)
endif

ifeq ($(OS),Windows_NT)
	LDFLAGS_EXTRA := -liconv
else ifeq ($(shell uname),Darwin)
	LDFLAGS_EXTRA := -liconv
else
	# iconv assumed to be part of the standard C library
	LDFLAGS_EXTRA :=
endif

LIBXGM_CPP_SRCS = \
	../xgm/devices/Audio/MedianFilter.cpp \
	../xgm/devices/Audio/echo.cpp \
	../xgm/devices/Audio/filter.cpp \
	../xgm/devices/Audio/rconv.cpp \
	../xgm/devices/CPU/nes_cpu.cpp \
	../xgm/devices/Memory/nes_bank.cpp \
	../xgm/devices/Memory/nes_mem.cpp \
	../xgm/devices/Memory/nsf2_vectors.cpp \
	../xgm/devices/Memory/ram64k.cpp \
	../xgm/devices/Misc/detect.cpp \
	../xgm/devices/Misc/log_cpu.cpp \
	../xgm/devices/Misc/nes_detect.cpp \
	../xgm/devices/Misc/nsf2_irq.cpp \
	../xgm/devices/Sound/nes_apu.cpp \
	../xgm/devices/Sound/nes_dmc.cpp \
	../xgm/devices/Sound/nes_fds.cpp \
	../xgm/devices/Sound/nes_fme7.cpp \
	../xgm/devices/Sound/nes_mmc5.cpp \
	../xgm/devices/Sound/nes_n106.cpp \
	../xgm/devices/Sound/nes_vrc6.cpp \
	../xgm/devices/Sound/nes_vrc7.cpp \
	../xgm/player/nsf/nsf.cpp \
	../xgm/player/nsf/nsfconfig.cpp \
	../xgm/player/nsf/nsfplay.cpp \
	../xgm/player/nsf/pls/ppls.cpp \
	../xgm/player/nsf/pls/sstream.cpp \
	../xgm/fileutil.cpp

LIBXGM_C_SRCS = \
	../xgm/devices/Sound/legacy/emu2149.c \
	../xgm/devices/Sound/legacy/emu2212.c \
	../xgm/devices/Sound/legacy/emu2413.c

LIBVCM_CPP_SRCS = \
	../vcm/group.cpp \
	../vcm/value.cpp

LIBXGM_HEADERS = \
	../xgm/browser/browser.h \
	../xgm/debugout.h \
	../xgm/devices/Audio/MedianFilter.h \
	../xgm/devices/Audio/amplifier.h \
	../xgm/devices/Audio/echo.h \
	../xgm/devices/Audio/fader.h \
	../xgm/devices/Audio/filter.h \
	../xgm/devices/Audio/mixer.h \
	../xgm/devices/Audio/rconv.h \
	../xgm/devices/CPU/km6502/km6280.h \
	../xgm/devices/CPU/km6502/km6280m.h \
	../xgm/devices/CPU/km6502/km6502.h \
	../xgm/devices/CPU/km6502/km6502cd.h \
	../xgm/devices/CPU/km6502/km6502ct.h \
	../xgm/devices/CPU/km6502/km6502ex.h \
	../xgm/devices/CPU/km6502/km6502ft.h \
	../xgm/devices/CPU/km6502/km6502m.h \
	../xgm/devices/CPU/km6502/km6502ot.h \
	../xgm/devices/CPU/km6502/km65c02.h \
	../xgm/devices/CPU/km6502/km65c02m.h \
	../xgm/devices/CPU/km6502/kmconfig.h \
	../xgm/devices/CPU/nes_cpu.h \
	../xgm/devices/Memory/nes_bank.h \
	../xgm/devices/Memory/nes_mem.h \
	../xgm/devices/Memory/nsf2_vectors.h \
	../xgm/devices/Memory/ram64k.h \
	../xgm/devices/Misc/block.h \
	../xgm/devices/Misc/detect.h \
	../xgm/devices/Misc/log_cpu.h \
	../xgm/devices/Misc/nes_detect.h \
	../xgm/devices/Misc/nsf2_irq.h \
	../xgm/devices/Sound/legacy/2413tone.h \
	../xgm/devices/Sound/legacy/281btone.h \
	../xgm/devices/Sound/legacy/emu2149.h \
	../xgm/devices/Sound/legacy/emu2212.h \
	../xgm/devices/Sound/legacy/emu2413.h \
	../xgm/devices/Sound/legacy/emutypes.h \
	../xgm/devices/Sound/legacy/vrc7tone_ft35.h \
	../xgm/devices/Sound/legacy/vrc7tone_ft36.h \
	../xgm/devices/Sound/legacy/vrc7tone_kt1.h \
	../xgm/devices/Sound/legacy/vrc7tone_kt2.h \
	../xgm/devices/Sound/legacy/vrc7tone_mo.h \
	../xgm/devices/Sound/legacy/vrc7tone_nuke.h \
	../xgm/devices/Sound/legacy/vrc7tone_rw.h \
	../xgm/devices/Sound/nes_apu.h \
	../xgm/devices/Sound/nes_dmc.h \
	../xgm/devices/Sound/nes_fds.h \
	../xgm/devices/Sound/nes_fme7.h \
	../xgm/devices/Sound/nes_mmc5.h \
	../xgm/devices/Sound/nes_n106.h \
	../xgm/devices/Sound/nes_vrc6.h \
	../xgm/devices/Sound/nes_vrc7.h \
	../xgm/devices/all.h \
	../xgm/devices/device.h \
	../xgm/devices/devinfo.h \
	../xgm/player/all.h \
	../xgm/player/midi_interface.h \
	../xgm/player/nsf/nsf.h \
	../xgm/player/nsf/nsfconfig.h \
	../xgm/player/nsf/nsfplay.h \
	../xgm/player/nsf/pls/ppls.h \
	../xgm/player/nsf/pls/sstream.h \
	../xgm/player/player.h \
	../xgm/player/soundData.h \
	../xgm/utils/tagctrl.h \
	../xgm/version.h \
	../xgm/fileutil.h \
	../xgm/xgm.h \
	../xgm/xtypes.h

LIBVCM_HEADERS = \
	../vcm/vcm.h \
	../vcm/vcm_ui.h

LIBXGM_OBJS = $(patsubst %,$(OBJDIR)/%.o,$(basename $(subst ../,,$(LIBXGM_CPP_SRCS)))) $(patsubst %,$(OBJDIR)/%.o,$(basename $(subst ../,,$(LIBXGM_C_SRCS))))
LIBVCM_OBJS = $(patsubst %,$(OBJDIR)/%.o,$(basename $(subst ../,,$(LIBVCM_CPP_SRCS))))

OBJS = $(LIBXGM_OBJS) $(LIBVCM_OBJS)

$(shell mkdir -p $(dir $(OBJS)) > /dev/null)

COMPILE.c = $(CC) -c -o $@ $(CFLAGS) $(CPPFLAGS) $(CFLAGS_EXTRA)
COMPILE.cc = $(CXX) -c -o $@ $(CXXFLAGS) $(CPPFLAGS) $(CXXFLAGS_EXTRA)
LINK.o = $(CXX) -shared -o $@ $(LDFLAGS) $(LDLIBS) $(LDFLAGS_EXTRA)
VPATH = ../

all: debug

debug:
	"$(MAKE)" "CC=$(CC)" "CXX=$(CXX)" "STATIC_PREFIX=$(STATIC_PREFIX)" "DYNLIB_PREFIX=$(DYNLIB_PREFIX)" "STATIC_EXT=$(STATIC_EXT)" "DYNLIB_EXT=$(DYNLIB_EXT)" "CFLAGS=$(CFLAGS_DEBUG)" "CXXFLAGS=$(CXXFLAGS_DEBUG)" "$(LIB_STATIC)" "$(LIB_DYNLIB)" nsf2wav nsfmeta

release:
	"$(MAKE)" "CC=$(CC)" "CXX=$(CXX)" "STATIC_PREFIX=$(STATIC_PREFIX)" "DYNLIB_PREFIX=$(DYNLIB_PREFIX)" "STATIC_EXT=$(STATIC_EXT)" "DYNLIB_EXT=$(DYNLIB_EXT)" "CFLAGS=$(CFLAGS_RELEASE)" "CXXFLAGS=$(CXXFLAGS_RELEASE)" "$(LIB_STATIC)" "$(LIB_DYNLIB)" nsf2wav nsfmeta

release_debug:
	"$(MAKE)" "CC=$(CC)" "CXX=$(CXX)" "STATIC_PREFIX=$(STATIC_PREFIX)" "DYNLIB_PREFIX=$(DYNLIB_PREFIX)" "STATIC_EXT=$(STATIC_EXT)" "DYNLIB_EXT=$(DYNLIB_EXT)" "CFLAGS=$(CFLAGS_RELEASE_DEBUG)" "CXXFLAGS=$(CXXFLAGS_RELEASE_DEBUG)" "$(LIB_STATIC)" "$(LIB_DYNLIB)" nsf2wav nsfmeta

demo: nsf2wav$(EXE_EXT)

nsfmeta$(EXE_EXT): $(OBJDIR)/nsfmeta.o $(LIB_STATIC)
	$(CXX) -o $@ $^ $(LDFLAGS_EXTRA)

nsf2wav$(EXE_EXT): $(OBJDIR)/nsf2wav.o $(LIB_STATIC)
	$(CXX) -o $@ $^ $(LDFLAGS_EXTRA)

$(LIB_STATIC): $(OBJS)
	$(AR) rcs $@ $^

$(LIB_DYNLIB): $(OBJS)
	$(LINK.o) $^

$(OBJDIR)/%.o: %.c
$(OBJDIR)/%.o: %.c
	$(COMPILE.c) $<

$(OBJDIR)/%.o: %.cpp
$(OBJDIR)/%.o: %.cpp
	$(COMPILE.cc) $<

clean:
	$(RM) $(OBJS)
	$(RM) $(LIB_STATIC) $(LIB_DYNLIB)

# this rule winds up generating a huge command-line
# for installing headers, need to find a way to break
# it up
install-headers: $(LIBXGM_HEADERS) $(LIBVCM_HEADERS) nsfplay.h
	$(INSTALL) -d $(INCDIR)
	$(foreach header,$(LIBXGM_HEADERS), \
	  $(INSTALL) -d $(INCDIR)/$(dir $(subst ../,,$(header))) && \
	  $(INSTALL) -m 644 $(header) $(INCDIR)/$(subst ../,,$(header)) && \
	) true
	$(foreach header,$(LIBVCM_HEADERS), \
	  $(INSTALL) -d $(INCDIR)/$(dir $(subst ../,,$(header))) && \
	  $(INSTALL) -m 644 $(header) $(INCDIR)/$(subst ../,,$(header)) && \
	) true
	$(INSTALL) -m 644 nsfplay.h $(INCDIR)/nsfplay.h


install: install-headers $(LIB_STATIC) $(LIB_DYNLIB)
	$(INSTALL) -d $(LIBDIR)
	$(INSTALL) -m 644 $(LIB_STATIC) $(LIBDIR)/$(LIB_STATIC)
	$(INSTALL) -m 755 $(LIB_DYNLIB) $(LIBDIR)/$(LIB_DYNLIB)
