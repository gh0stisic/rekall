## Process this file with automake to produce Makefile.in

# Make sure that when we re-make ./configure, we get the macros we need
ACLOCAL_AMFLAGS = -I m4

# This is so we can #include <package/foo>
AM_CPPFLAGS = -I$(top_srcdir) -DHAVE_SSTREAM

# These are good warnings to turn on by default
if GCC
AM_CXXFLAGS = -Wall -Wwrite-strings -Woverloaded-virtual -Wno-sign-compare
endif

# Support static builds.
if STATIC_BUILD
STATIC_LIBLDFLAGS = -static -static-libstdc++ -static-libgcc
STATIC_LINKERLDFLAGS = -all-static
endif

# The -no-undefined flag allows libtool to generate shared libraries for
# Cygwin and MinGW.  LIBSTDCXX_LA_LINKER_FLAG is used to fix a Solaris bug.
# -version-info tells libtool what to put after the '.so' in the filename.
AM_LDFLAGS = -no-undefined $(LIBSTDCXX_LA_LINKER_FLAG) \
             -version-info @SO_VERSION@

if WIN32_NATIVE_HOST
bin_PROGRAMS = winpmem

winpmem_LDFLAGS = $(STATIC_LINKERLDFLAGS)
winpmem_CXXFLAGS = -std=c++11 -g -Wall -O0
winpmem_SOURCES = pmem_imager.cc win_pmem.cc
winpmem_LDADD = -lrpcrt4 -lshlwapi -laff4 -lglog -lyaml-cpp


all-local: winpmem$(EXEEXT)
	@echo Appending pmem AFF4 drivers to binary.
	$(STRIP) winpmem$(EXEEXT)
	cat resources/pmem.aff4 >> winpmem$(EXEEXT)
endif

if LINUX_NATIVE_HOST
bin_PROGRAMS = linpmem

linpmem_LDFLAGS = $(STATIC_LINKERLDFLAGS)
linpmem_CXXFLAGS = -std=c++11 -g -Wall -O0
linpmem_SOURCES = pmem_imager.cc linux_pmem.cc
linpmem_LDADD = -laff4 -lglog
endif
