dnl Process this file with autoconf to produce a configure script.

AC_INIT(install-sh)
AC_CONFIG_HEADER(config.h)

AC_SUBST(PL)
AC_SUBST(PLLIBS)
AC_SUBST(PLBASE)
AC_SUBST(PLARCH)
AC_SUBST(BOOTPL)
AC_SUBST(PLINCL)
AC_SUBST(COFLAGS)
AC_SUBST(CWFLAGS)
AC_SUBST(CMFLAGS)
AC_SUBST(ETAGS)
AC_SUBST(LD)
AC_SUBST(SO)
AC_SUBST(LDSOFLAGS)

AC_ARG_ENABLE(mt, [  --enable-mt		  Enable Multi-threading],
	      [case "$enableval" in
		   yes) AC_DEFINE(_REENTRANT)
			;;
		   *)  ;;
	       esac])

if test -z "$PLINCL"; then
plcandidates="swi-prolog swipl pl"
AC_CHECK_PROGS(PL, $plcandidates, "none")
AC_CHECK_PROGS(PLLD, plld, "none")
if test $PLLD = "none"; then
   AC_ERROR("Cannot find SWI-Prolog plld utility. SWI-Prolog must be installed first")
fi
if test $PL = "none"; then
   AC_ERROR("Cannot find SWI-Prolog. SWI-Prolog must be installed first")
else
   AC_CHECKING("Running $PL -dump-runtime-variables")
   eval `$PL -dump-runtime-variables`
fi
PLINCL=$PLBASE/include
AC_MSG_RESULT("		PLBASE=$PLBASE")
AC_MSG_RESULT("		PLARCH=$PLARCH")
AC_MSG_RESULT("		PLLIBS=$PLLIBS")
AC_MSG_RESULT("		PLLDFLAGS=$PLLDFLAGS")
AC_MSG_RESULT("		PLSHARED=$PLSHARED")
AC_MSG_RESULT("		PLSOEXT=$PLSOEXT")
else
PLLD=../plld.sh
PL=../pl.sh
fi

CC=$PLLD
LD=$PLLD
LDSOFLAGS=-shared
CMFLAGS=-fpic

SO="$PLSOEXT"

case $PLARCH in
    *darwin*)
	CMFLAGS="$CMFLAGS -cc-options,-no-cpp-precomp"
        undefine([__unix__])
	AC_DEFINE(__unix__)
	;;
esac

AC_CHECK_PROGS(MAKE, gmake make, "make")
AC_CHECK_PROGS(ETAGS, etags ctags, ":")
AC_PROG_INSTALL
AC_ISC_POSIX
AC_HEADER_STDC
CFLAGS="$CMFLAGS"
AC_FUNC_ALLOCA
AC_C_BIGENDIAN

if test ! -z "$GCC"; then
    COFLAGS="-O2 -fno-strict-aliasing"
    CWFLAGS=-Wall
else
    COFLAGS=-O
    CWFLAGS=
fi

AC_CHECK_HEADERS(malloc.h unistd.h sys/time.h fcntl.h floatingpoint.h)
AC_CHECK_FUNCS(mmap strerror)

AC_OUTPUT(Makefile)
