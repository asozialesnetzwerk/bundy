SUBDIRS = . tests

AM_CPPFLAGS = -I$(top_srcdir)/src/lib -I$(top_builddir)/src/lib
AM_CPPFLAGS += -I$(top_srcdir)/src/bin -I$(top_builddir)/src/bin
AM_CPPFLAGS += -I$(top_srcdir)/src/lib/dns -I$(top_builddir)/src/lib/dns
AM_CPPFLAGS += -I$(top_srcdir)/src/lib/cc -I$(top_builddir)/src/lib/cc
AM_CPPFLAGS += -I$(top_srcdir)/src/lib/asiolink
AM_CPPFLAGS += -I$(top_builddir)/src/lib/asiolink
AM_CPPFLAGS += $(BOOST_INCLUDES)

AM_CXXFLAGS = $(B10_CXXFLAGS)

if USE_STATIC_LINK
AM_LDFLAGS = -static
endif

pkglibexecdir = $(libexecdir)/@PACKAGE@

CLEANFILES = *.gcno *.gcda resolver.spec spec_config.h

man_MANS = b10-resolver.8
EXTRA_DIST = $(man_MANS) b10-resolver.xml

if ENABLE_MAN

b10-resolver.8: b10-resolver.xml
	xsltproc --novalid --xinclude --nonet -o $@ http://docbook.sourceforge.net/release/xsl/current/manpages/docbook.xsl $(srcdir)/b10-resolver.xml

endif

resolver.spec: resolver.spec.pre
	$(SED) -e "s|@@LOCALSTATEDIR@@|$(localstatedir)|" resolver.spec.pre >$@

spec_config.h: spec_config.h.pre
	$(SED) -e "s|@@LOCALSTATEDIR@@|$(localstatedir)|" spec_config.h.pre >$@

BUILT_SOURCES = spec_config.h 
pkglibexec_PROGRAMS = b10-resolver
b10_resolver_SOURCES = resolver.cc resolver.h
b10_resolver_SOURCES += $(top_builddir)/src/bin/auth/change_user.h
b10_resolver_SOURCES += $(top_builddir)/src/bin/auth/common.h
b10_resolver_SOURCES += main.cc
b10_resolver_LDADD =  $(top_builddir)/src/lib/dns/libdns++.la
b10_resolver_LDADD += $(top_builddir)/src/lib/config/libcfgclient.la
b10_resolver_LDADD += $(top_builddir)/src/lib/cc/libcc.la
b10_resolver_LDADD += $(top_builddir)/src/lib/exceptions/libexceptions.la
b10_resolver_LDADD += $(top_builddir)/src/lib/asiolink/libasiolink.la
b10_resolver_LDADD += $(top_builddir)/src/lib/xfr/libxfr.la
b10_resolver_LDADD += $(top_builddir)/src/lib/log/liblog.la
b10_resolver_LDADD += $(top_builddir)/src/bin/auth/change_user.o
b10_resolver_LDFLAGS = -pthread

# TODO: config.h.in is wrong because doesn't honor pkgdatadir
# and can't use @datadir@ because doesn't expand default ${prefix}
b10_resolverdir = $(DESTDIR)$(pkgdatadir)
b10_resolver_DATA = resolver.spec
