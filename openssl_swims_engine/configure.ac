#
# configure.in for the OpenSSL swims engine project
#
AC_INIT([openssl_swims_engine], [1.5], [manishma@cisco.com])
AM_INIT_AUTOMAKE(1.6.3)
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([m4])

# Debugging support
AC_ARG_ENABLE(debug,
		AC_HELP_STRING([--enable-debug], [turn on all debugging flags [default is off]]),
		[enable_debug="yes"
		 AC_MSG_RESULT([*** Enabling debugging at user request ***])],)

# If the user has not set CFLAGS, do something appropriate
test_CFLAGS=${CFLAGS+set}
if test "$test_CFLAGS" != set; then
	if test "x$enable_debug" == "xyes"; then
		CFLAGS="-O0 -g -DDEBUG"
	else
		CFLAGS="-O2"
	fi
fi

if test "${OPENSSL_LIB_DIR+set}" != set; then
	OPENSSL_LIB_DIR="/usr/local/ssl/lib"
fi
if test "${OPENSSL_INCLUDE_DIR+set}" != set; then
	OPENSSL_INCLUDE_DIR="/usr/local/ssl/include"
fi

# Non-standard OpenSSL
AC_MSG_CHECKING([Non-standard OpenSSL])
AC_ARG_WITH(openssl,
	[--with-openssl=PATH      Location of openssl libs/includes],
	[OPENSSL_INCLUDE_DIR="$withval/include"
	 OPENSSL_LIB_DIR="$withval/lib"
	 if [[ ! -d $OPENSSL_INCLUDE_DIR -o ! -d $OPENSSL_LIB_DIR ]]; then
		AC_MSG_ERROR([$OPENSSL_INCLUDE_DIR or $OPENSSL_LIB_DIR doen't exist!])
	 else
		AC_MSG_RESULT([yes])
		CFLAGS="$CFLAGS -L$OPENSSL_LIB_DIR -I$OPENSSL_INCLUDE_DIR"
	 fi],
	[AC_MSG_RESULT([no])
	 AC_SUBST(OPENSSL_LIB_DIR)
	 AC_SUBST(OPENSSL_INCLUDE_DIR)])

AC_CHECK_LIB([curl], [curl_global_init], [],[AC_MSG_ERROR([curl development package not installed])])

#AC_DISABLE_STATIC
AC_PROG_CC
AC_PROG_INSTALL
LT_INIT
AC_PROG_LIBTOOL
AM_PROG_CC_C_O

CFLAGS="$CFLAGS -Wall"
AC_SUBST(CFLAGS)

AC_OUTPUT(Makefile 
          test/Makefile 
          src/Makefile 
          obs/openssl_swims_engine.spec 
          obs/Makefile)

echo \
"-------------------------------------------------

 ${PACKAGE_NAME} Version ${PACKAGE_VERSION}

 Prefix: '${prefix}'.
 Compiler: '${CC} ${CFLAGS} ${CPPFLAGS}'

 Now type 'make @<:@<target>@:>@'
   where the optional <target> is:
     all                - build all binaries
     install            - install everything

--------------------------------------------------"
