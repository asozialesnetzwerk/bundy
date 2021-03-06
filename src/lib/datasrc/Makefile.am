SUBDIRS = memory . tests

AM_CPPFLAGS = -I$(top_srcdir)/src/lib -I$(top_builddir)/src/lib
AM_CPPFLAGS += -I$(top_srcdir)/src/lib/dns -I$(top_builddir)/src/lib/dns
AM_CPPFLAGS += $(BOOST_INCLUDES)
AM_CPPFLAGS += $(SQLITE_CFLAGS)

AM_CXXFLAGS = $(BUNDY_CXXFLAGS)

pkglibdir = $(libexecdir)/@PACKAGE@/backends

datasrc_config.h: datasrc_config.h.pre
	$(SED) -e "s|@@PKGLIBDIR@@|$(pkglibdir)|" datasrc_config.h.pre >$@

# The top config.h defines "PACKAGE_STRING".  When it's changed we neeed to
# regenerate this zone file.
static.zone: static.zone.pre $(top_builddir)/config.h $(top_srcdir)/AUTHORS
	$(SED) -e "s|@@VERSION_STRING@@|$(PACKAGE_STRING)|" $(srcdir)/static.zone.pre >$@
	$(SED) -e 's/\(.*\)/AUTHORS.BIND.	0	CH	TXT	"\1"/' $(top_srcdir)/AUTHORS >>$@

CLEANFILES = *.gcno *.gcda datasrc_messages.h datasrc_messages.cc
CLEANFILES += sqlite3_datasrc_messages.h sqlite3_datasrc_messages.cc
CLEANFILES += datasrc_config.h
CLEANFILES += static.zone
CLEANFILES += s-messages1 s-messages2

lib_LTLIBRARIES = libbundy-datasrc.la
libbundy_datasrc_la_SOURCES = exceptions.h
libbundy_datasrc_la_SOURCES += zone.h zone_finder.h zone_finder.cc
libbundy_datasrc_la_SOURCES += zone_finder_context.cc
libbundy_datasrc_la_SOURCES += zone_iterator.h
libbundy_datasrc_la_SOURCES += result.h
libbundy_datasrc_la_SOURCES += logger.h logger.cc
libbundy_datasrc_la_SOURCES += client.h client.cc
libbundy_datasrc_la_SOURCES += database.h database.cc
libbundy_datasrc_la_SOURCES += factory.h factory.cc
libbundy_datasrc_la_SOURCES += client_list.h client_list.cc
libbundy_datasrc_la_SOURCES += master_loader_callbacks.h
libbundy_datasrc_la_SOURCES += master_loader_callbacks.cc
libbundy_datasrc_la_SOURCES += rrset_collection_base.h rrset_collection_base.cc
libbundy_datasrc_la_SOURCES += zone_loader.h zone_loader.cc
libbundy_datasrc_la_SOURCES += cache_config.h cache_config.cc
libbundy_datasrc_la_SOURCES += zone_table_accessor.h
libbundy_datasrc_la_SOURCES += zone_table_accessor_cache.h
libbundy_datasrc_la_SOURCES += zone_table_accessor_cache.cc
nodist_libbundy_datasrc_la_SOURCES = datasrc_messages.h datasrc_messages.cc
libbundy_datasrc_la_LDFLAGS = -no-undefined -version-info 1:0:1

pkglib_LTLIBRARIES = sqlite3_ds.la

sqlite3_ds_la_SOURCES = sqlite3_accessor.h sqlite3_accessor.cc
sqlite3_ds_la_SOURCES += sqlite3_accessor_link.cc
nodist_sqlite3_ds_la_SOURCES = sqlite3_datasrc_messages.h sqlite3_datasrc_messages.cc
sqlite3_ds_la_LDFLAGS = -module -avoid-version
sqlite3_ds_la_LDFLAGS += -no-undefined -version-info 1:0:0
sqlite3_ds_la_LIBADD = $(top_builddir)/src/lib/exceptions/libbundy-exceptions.la
sqlite3_ds_la_LIBADD += libbundy-datasrc.la
sqlite3_ds_la_LIBADD += $(SQLITE_LIBS)

libbundy_datasrc_la_LIBADD = $(top_builddir)/src/lib/exceptions/libbundy-exceptions.la
libbundy_datasrc_la_LIBADD += $(top_builddir)/src/lib/dns/libbundy-dns++.la
libbundy_datasrc_la_LIBADD += $(top_builddir)/src/lib/log/libbundy-log.la
libbundy_datasrc_la_LIBADD += $(top_builddir)/src/lib/cc/libbundy-cc.la
libbundy_datasrc_la_LIBADD += $(top_builddir)/src/lib/datasrc/memory/libdatasrc_memory.la
libbundy_datasrc_la_LIBADD += $(SQLITE_LIBS)

BUILT_SOURCES = datasrc_config.h datasrc_messages.h datasrc_messages.cc
BUILT_SOURCES += sqlite3_datasrc_messages.h sqlite3_datasrc_messages.cc
datasrc_messages.h datasrc_messages.cc: s-messages1

s-messages1: Makefile datasrc_messages.mes
	$(top_builddir)/src/lib/log/compiler/message $(top_srcdir)/src/lib/datasrc/datasrc_messages.mes
	touch $@

sqlite3_datasrc_messages.h sqlite3_datasrc_messages.cc: s-messages2

s-messages2: Makefile sqlite3_datasrc_messages.mes
	$(top_builddir)/src/lib/log/compiler/message $(top_srcdir)/src/lib/datasrc/sqlite3_datasrc_messages.mes
	touch $@

EXTRA_DIST = datasrc_messages.mes sqlite3_datasrc_messages.mes static.zone.pre

zonedir = $(pkgdatadir)
zone_DATA = static.zone
