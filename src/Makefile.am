AUTOMAKE_OPTIONS = -Wno-override
lib_LTLIBRARIES = mod_authnz_crowd.la
mod_authnz_crowd_la_SOURCES = mod_authnz_crowd.c mod_authnz_crowd.h crowd_client.c crowd_client.h cache.c cache.h util.c util.h
mod_authnz_crowd_la_LDFLAGS = -module -lcurl `xml2-config --libs`
AM_CFLAGS = `apr-1-config --cflags`
AM_CPPFLAGS = -I@APACHE_INCLUDE_DIR@ `apr-1-config --cppflags --includes` `xml2-config --cflags`
CFLAGS=-g -Wmissing-prototypes -O1   # -O2 causes mysterious crashes
TESTS = test.py
TESTS_ENVIRONMENT = APACHE_BIN_DIR=@APACHE_BIN_DIR@

test.py : httpd/conf/common.conf

httpd/conf/common.conf : httpd/conf/common.conf.in
	APACHE_MODULES_DIR=@APACHE_MODULES_DIR@ CURDIR=$(CURDIR) envsubst < httpd/conf/common.conf.in > httpd/conf/common.conf

install:
	cp @HTTPD_CONF@ /tmp/httpd.conf.bak
	@APXS@ -i -a mod_authnz_crowd.la
	@APACHECTL@ configtest || mv /tmp/httpd.conf.bak @HTTPD_CONF@
	@APACHECTL@ graceful
