#
# libtsm - Global Makefile
# Copyright (c) 2012-2013 David Herrmann <dh.herrmann@gmail.com>
#

#
# Library Version Numbers
#

LIBTSM_CURRENT = 1
LIBTSM_REVISION = 0
LIBTSM_AGE = 0

#
# Global Configurations and Initializations
#

ACLOCAL_AMFLAGS = -I m4 ${ACLOCAL_FLAGS}
AM_MAKEFLAGS = --no-print-directory
AUTOMAKE_OPTIONS = color-tests
AM_DISTCHECK_CONFIGURE_FLAGS = \
	--enable-debug \
	--enable-optimizations

SUBDIRS = .

.DELETE_ON_ERROR:

include_HEADERS =
EXTRA_DIST = \
	README \
	COPYING \
	NEWS \
	docs/libtsm.pc.in \
	docs/libtsm.sym
CLEANFILES =
pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA =
TPHONY =

check_PROGRAMS =
lib_LTLIBRARIES =
noinst_LTLIBRARIES =

#
# Default CFlags
# Make all files include "config.h" by default. This shouldn't cause any
# problems and we cannot forget to include it anymore.
#
# Also make the linker discard all unused symbols.
#
# When compiling in debug mode, we enable debug symbols so debugging with gdb
# is easier. If optimizations are disabled, we pass -O0 to the compiler.
# Otherwise, we use standard optimizations -O2.
#

AM_CFLAGS = \
	-Wall \
	-pipe \
	-fno-common \
	-ffast-math \
	-fdiagnostics-show-option \
	-fno-strict-aliasing \
	-fvisibility=hidden \
	-ffunction-sections \
	-fdata-sections \
	-fstack-protector
AM_CPPFLAGS = \
	-include $(top_builddir)/config.h \
	-I $(srcdir)/src
AM_LDFLAGS = \
	-Wl,--as-needed \
	-Wl,--gc-sections \
	-Wl,-z,relro \
	-Wl,-z,now

if BUILD_ENABLE_DEBUG
AM_CFLAGS += -g
endif

if BUILD_ENABLE_OPTIMIZATIONS
AM_CFLAGS += -O2
else
AM_CFLAGS += -O0
endif

#
# SHL - Static Helper Library
# The SHL subsystem contains several small code pieces used all over libtsm and
# other applications.
#

noinst_LTLIBRARIES += libshl.la

libshl_la_SOURCES = \
	src/shl_dlist.h \
	src/shl_array.h \
	src/shl_hashtable.h \
	external/htable.h \
	external/htable.c \
	src/shl_timer.h \
	src/shl_llog.h \
	src/shl_misc.h
libshl_la_CPPFLAGS = $(AM_CPPFLAGS)
libshl_la_LDFLAGS = $(AM_LDFLAGS)
libshl_la_LIBADD = $(AM_LIBADD)

#
# libtsm
# Main library build instructions
#

lib_LTLIBRARIES += libtsm.la
include_HEADERS += \
	src/tsm_screen.h \
	src/tsm_unicode.h \
	src/tsm_vte.h \
	src/tsm.h
pkgconfig_DATA += docs/libtsm.pc

libtsm_la_SOURCES = \
	src/tsm.h \
	src/tsm_unicode.h \
	src/tsm_unicode.c \
	src/tsm_screen.h \
	src/tsm_screen.c \
	src/tsm_vte.h \
	src/tsm_vte.c \
	src/tsm_vte_charsets.c \
	external/wcwidth.h \
	external/wcwidth.c

libtsm_la_CPPFLAGS = $(AM_CPPFLAGS)
libtsm_la_LIBADD = libshl.la
EXTRA_libtsm_la_DEPENDENCIES = $(top_srcdir)/docs/libtsm.sym
libtsm_la_LDFLAGS = \
	$(AM_LDFLAGS) \
	-version-info $(LIBTSM_CURRENT):$(LIBTSM_REVISION):$(LIBTSM_AGE) \
	-Wl,--version-script="$(top_srcdir)/docs/libtsm.sym"

if BUILD_HAVE_XKBCOMMON
libtsm_la_CPPFLAGS += $(XKBCOMMON_CFLAGS)
endif

#
# Phony targets
#

.PHONY: $(TPHONY)

#
# Empty .SECONDARY target causes alle intermediate files to be treated as
# secondary files. That is, they don't get deleted after make finished.
#

.SECONDARY:
