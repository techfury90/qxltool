dnl Process this file with autoconf (2.69 or later) to produce a configure script.

AC_PREREQ([2.69])
AC_INIT([qxltool], m4_esyscmd_s([sed -e 's/.*"\(.*\)".*/\1/' version.h]))
AC_CONFIG_SRCDIR([qxltool.c])
AC_CONFIG_HEADERS([config.h])

AC_PROG_CC

dnl Plain test rather than AC_CHECK_FILE, which refuses to run when cross
dnl compiling (e.g. building the NT version with mingw)
AS_IF([test -f ../qxltool.c], [],
	[AC_MSG_ERROR([You must run this in the Unix or NT sub-directory])])

dnl Test library availability with the same static linkage used by the NT build.
AS_CASE([`pwd`], [*/NT], [LDFLAGS="$LDFLAGS -static"])

dnl Checks for libraries.
AC_SEARCH_LIBS([floor], [m])

dnl zlib is required, for the unzip command
AC_CHECK_HEADER([zlib.h], [],
	[AC_MSG_ERROR([zlib.h not found, please install zlib (development files)])])
AC_SEARCH_LIBS([inflateInit2_], [z], [],
	[AC_MSG_ERROR([zlib not found, please install zlib])])

dnl readline: some distributions need termcap/ncurses linked explicitly
AC_ARG_WITH([readline],
	[AS_HELP_STRING([--without-readline], [do not use GNU readline])],
	[], [with_readline=yes])
AS_IF([test "x$with_readline" != xno], [
	AC_SEARCH_LIBS([readline], [readline], [have_readline=yes], [
		AC_SEARCH_LIBS([tgetent], [tinfo ncurses termcap])
		AS_UNSET([ac_cv_search_readline])
		AC_SEARCH_LIBS([readline], [readline], [have_readline=yes],
			[have_readline=no])])
	AS_IF([test "x$have_readline" = xyes],
		[AC_DEFINE([HAVE_LIBREADLINE], [1], [Define if you have the readline library])])
])

dnl Checks for header files.
AC_CHECK_HEADERS([fcntl.h limits.h sys/time.h unistd.h getopt.h alloca.h fnmatch.h])
AS_IF([test "x$have_readline" = xyes],
	[AC_CHECK_HEADERS([readline/readline.h readline/history.h])])

dnl Checks for typedefs, structures, and compiler characteristics.
AC_C_INLINE
AC_TYPE_SIZE_T
AC_C_BIGENDIAN
AC_CHECK_TYPE([u_char], [],
	[AC_DEFINE([NEED_U_CHAR], [1], [Define if <sys/types.h> lacks u_char])],
	[#include <sys/types.h>])
AC_CHECK_TYPE([u_short], [],
	[AC_DEFINE([NEED_U_SHORT], [1], [Define if <sys/types.h> lacks u_short])],
	[#include <sys/types.h>])
AC_CHECK_TYPE([u_long], [],
	[AC_DEFINE([NEED_U_LONG], [1], [Define if <sys/types.h> lacks u_long])],
	[#include <sys/types.h>])

dnl Checks for library functions.
AC_FUNC_ALLOCA
AC_FUNC_FNMATCH
AC_CHECK_FUNCS([strdup strspn strtoul stpcpy getopt ftruncate strftime])

AC_CONFIG_FILES([Makefile])
AC_OUTPUT
