# Make Autoconf man pages.

## Copyright (C) 2001, 2004, 2005, 2006 Free Software Foundation, Inc.

## This program is free software; you can redistribute it and/or modify
## it under the terms of the GNU General Public License as published by
## the Free Software Foundation; either version 2, or (at your option)
## any later version.

## This program is distributed in the hope that it will be useful,
## but WITHOUT ANY WARRANTY; without even the implied warranty of
## MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
## GNU General Public License for more details.

## You should have received a copy of the GNU General Public License
## along with this program; if not, write to the Free Software
## Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA
## 02110-1301, USA.

dist_man_MANS = \
	$(srcdir)/autoconf.1 \
	$(srcdir)/autoheader.1 \
	$(srcdir)/autom4te.1 \
	$(srcdir)/autoreconf.1 \
	$(srcdir)/autoscan.1 \
	$(srcdir)/autoupdate.1 \
	$(srcdir)/ifnames.1 \
	$(srcdir)/config.guess.1 \
	$(srcdir)/config.sub.1

EXTRA_DIST = $(dist_man_MANS:.1=.x) common.x
MAINTAINERCLEANFILES = $(dist_man_MANS)

# Depend on configure.ac to get version number changes.
common_dep = $(top_srcdir)/configure.ac $(srcdir)/common.x
binsrcdir = $(top_srcdir)/bin
$(srcdir)/autoconf.1:   $(common_dep) $(binsrcdir)/autoconf.as
$(srcdir)/autoheader.1: $(common_dep) $(binsrcdir)/autoheader.in
$(srcdir)/autom4te.1:   $(common_dep) $(binsrcdir)/autom4te.in
$(srcdir)/autoreconf.1: $(common_dep) $(binsrcdir)/autoreconf.in
$(srcdir)/autoscan.1:   $(common_dep) $(binsrcdir)/autoscan.in
$(srcdir)/autoupdate.1: $(common_dep) $(binsrcdir)/autoupdate.in
$(srcdir)/ifnames.1:    $(common_dep) $(binsrcdir)/ifnames.in

# Independent from this package.
$(srcdir)/config.guess.1: $(top_srcdir)/build-aux/config.guess
$(srcdir)/config.sub.1:   $(top_srcdir)/build-aux/config.sub

remove_time_stamp = 's/^\(\.TH[^"]*"[^"]*"[^"]*\)"[^"]*"/\1/'

MOSTLYCLEANFILES = $(srcdir)/*.t

SUFFIXES = .x .1

.x.1:
	@program=`expr "/$*" : '.*/\(.*\)'` && \
	case $$program in \
	config.*) ;; \
	*) \
	  for dir in $(top_builddir)/bin $(top_builddir)/tests; do \
	    echo cd $$dir '&&' $(MAKE) $(AM_MAKEFLAGS) $$program && \
	    (cd $$dir && $(MAKE) $(AM_MAKEFLAGS) $$program) || exit; \
	  done;; \
	esac
	echo "Updating man page $@"
	PATH="$(top_builddir)/tests$(PATH_SEPARATOR)$(top_srcdir)/build-aux$(PATH_SEPARATOR)$$PATH"; \
	export PATH; \
	$(HELP2MAN) \
	    --include=$*.x \
	    --include=$(srcdir)/common.x \
	    --output=$@.t `echo '$*' | sed 's,.*/,,'`
	if sed $(remove_time_stamp) $@ >$@a.t 2>/dev/null && \
	   sed $(remove_time_stamp) $@.t | cmp $@a.t - >/dev/null 2>&1; then \
		touch $@; \
	else \
		mv $@.t $@; \
	fi
	rm -f $@*.t
