# Makefile.am --- automake input file for systemtap dyninst tools

AUTOMAKE_OPTIONS = subdir-objects

AM_CFLAGS = -Wall -Werror -Wunused -W -Wformat # -Wformat=2
AM_CXXFLAGS = -Wall -Werror -Wunused -W -Wformat # -Wformat=2
AM_CPPFLAGS = -D_GNU_SOURCE
AM_CPPFLAGS += -I$(srcdir)/../includes
AM_CPPFLAGS += -I$(builddir)/../includes/sys
AM_CPPFLAGS += -DBINDIR='"$(bindir)"' -DSYSCONFDIR='"$(sysconfdir)"' -DPKGDATADIR='"${pkgdatadir}"' -DPKGLIBDIR='"$(pkglibexecdir)"' -DLOCALEDIR='"$(localedir)"'

AM_CFLAGS += @PIECFLAGS@
AM_CXXFLAGS += @PIECXXFLAGS@
AM_LDFLAGS = @PIELDFLAGS@ @BOOST_SYSTEM_LIB@

if HAVE_SELINUX
AM_CPPFLAGS += $(selinux_CFLAGS)
AM_LDFLAGS += $(selinux_LIBS)
endif

bin_PROGRAMS =
noinst_PROGRAMS =

if HAVE_DYNINST

bin_PROGRAMS += stapdyn
noinst_PROGRAMS += dynsdt

man_MANS = stapdyn.8

stapdyn_SOURCES = stapdyn.cxx mutator.cxx mutatee.cxx \
				  dynprobe.cxx dynutil.cxx ../util.cxx
stapdyn_CXXFLAGS = $(AM_CXXFLAGS) @DYNINST_CXXFLAGS@
stapdyn_LDFLAGS = $(AM_LDFLAGS) @DYNINST_LDFLAGS@
stapdyn_LDADD = -ldl -ldyninstAPI -lsymtabAPI -lpthread

dynsdt_SOURCES = dynsdt.cxx dynutil.cxx ../util.cxx
dynsdt_CPPFLAGS = $(AM_CPPFLAGS)
dynsdt_CXXFLAGS = $(AM_CXXFLAGS) @DYNINST_CXXFLAGS@
dynsdt_LDFLAGS = $(AM_LDFLAGS) @DYNINST_LDFLAGS@
dynsdt_LDADD = -ldl -lelf -ldyninstAPI -lsymtabAPI -lpthread

if BUILD_ELFUTILS
dynsdt_CPPFLAGS += -I../include-elfutils
dynsdt_LDFLAGS += -L../lib-elfutils -Wl,-rpath-link,lib-elfutils \
		-Wl,--enable-new-dtags,-rpath,$(pkglibdir)
endif

endif # HAVE_DYNINST

BUILT_SOURCES =

# Arrange for the top-level git_version.h to be regenerated at every "make".
BUILT_SOURCES += git_version.stamp
git_version.stamp ../git_version.h:
	$(MAKE) -C .. $(notdir $@)
