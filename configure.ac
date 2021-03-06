AC_INIT([webkitgtk3-sharp], [2.4.9])
AC_CONFIG_SRCDIR([README])
AC_CANONICAL_TARGET
AM_INIT_AUTOMAKE
AM_MAINTAINER_MODE
AC_PROG_INSTALL

dnl Package settings
ASSEMBLY_NAME=$PACKAGE
ASSEMBLY_VERSION="3.0.0.0"
ASSEMBLY_NAMESPACE="WebKit"
ASSEMBLY_SHORTNAME="webkit"
ASSEMBLY_TITLE="WebKit#"
ASSEMBLY_DESCRIPTION="WebKit bindings for Mono"
ASSEMBLY_COMPANY="Novell, Inc."
ASSEMBLY_COPYRIGHT="2008, 2009 Novell, Inc."
ASSEMBLY_PC=$ASSEMBLY_NAME-3.0
						 
AC_SUBST(ASSEMBLY_NAME)
AC_SUBST(ASSEMBLY_VERSION)
AC_SUBST(ASSEMBLY_NAMESPACE)
AC_SUBST(ASSEMBLY_SHORTNAME)
AC_SUBST(ASSEMBLY_TITLE)
AC_SUBST(ASSEMBLY_DESCRIPTION)
AC_SUBST(ASSEMBLY_COMPANY)
AC_SUBST(ASSEMBLY_COPYRIGHT)
AC_SUBST(ASSEMBLY_PC)

dnl Check for pkg-config
AC_PATH_PROG(PKG_CONFIG, pkg-config, no)
if test "x$PKG_CONFIG" = "xno"; then
	AC_MSG_ERROR(['pkg-config' is not in your PATH.])
fi

dnl Check for Mono
PKG_CHECK_MODULES(MONO_DEPENDENCY, mono >= 1.0, has_mono=true, has_mono=false)
PKG_CHECK_MODULES(MONO_DEPENDENCY, mint >= 1.0, has_mint=true, has_mint=false)

if test "x$has_mono" = "xtrue"; then
	AC_PATH_PROG(RUNTIME, mono, no)
	AC_PATH_PROG(CSC, mcs, no)
	case "$host" in
		*-*-mingw*|*-*-cygwin*)
			LIB_PREFIX=-
			LIB_SUFFIX=.dll
			;;
		*-*-darwin*)
			LIB_PREFIX=.
			LIB_SUFFIX=.dylib
			;;
		*)
			LIB_PREFIX=.so.
			LIB_SUFFIX=
			;;
	esac
else
	if test "x$has_mint" = "xtrue"; then
		AC_PATH_PROG(RUNTIME, mint, no)
		AC_PATH_PROG(CSC, mcs, no)
		LIB_PREFIX=.so
		LIB_SUFFIX=
	else
		AC_PATH_PROG(CSC, csc.exe, no)
		if test x$CSC = "xno"; then
			AC_MSG_ERROR([You need to install either mono or .Net])
		else
			RUNTIME=
			LIB_PREFIX=
			LIB_SUFFIX=.dylib
		fi
	fi
fi

CS="C#"
if test "x$CSC" = "xno" ; then
    AC_MSG_ERROR([No $CS compiler found])
fi

AC_PATH_PROG(GACUTIL, gacutil, no)
if test "x$GACUTIL" = "xno" ; then
	AC_MSG_ERROR([No gacutil tool found])
fi

GACUTIL_FLAGS="/package $ASSEMBLY_NAME /root "'$(DESTDIR)$(prefix)/lib'
                                     
AC_SUBST(RUNTIME)
AC_SUBST(CSC)
AC_SUBST(GACUTIL)
AC_SUBST(GACUTIL_FLAGS)
AC_SUBST(LIB_PREFIX)
AC_SUBST(LIB_SUFFIX)

dnl Check for gtk-sharp
PKG_CHECK_MODULES(GTK_SHARP, gtk-sharp-3.0 >= 3.6.0)
AC_SUBST(GTK_SHARP_CFLAGS)
AC_SUBST(GTK_SHARP_LIBS)
gtksharp_prefix=`pkg-config --variable=prefix gtk-sharp-3.0`
AC_SUBST(gtksharp_prefix)

dnl Check for WebKit
PKG_CHECK_MODULES(WEBKIT, webkitgtk-3.0 >= 1.10.0)
AC_SUBST(WEBKIT_LIBS)
webkit_prefix=`pkg-config --variable=prefix webkitgtk-3.0`
AC_SUBST(webkit_prefix)

dnl Check for gapi
AC_PATH_PROG(GAPI_PARSER, gapi3-parser, no)
if test "x$GAPI_PARSER" = "xno"; then
	AC_MSG_CHECKING(for gapi3-parser.exe)
	GAPI_PARSER=`which gapi3-parser.exe 2> /dev/null`
	if test "x$GAPI_PARSER" = "xno" ; then
		AC_MSG_ERROR(['gapi3_parser'/'gapi3-parser.exe' not found.])
	fi
	AC_MSG_RESULT($GAPI_PARSER)
	GAPI_PARSER="$MONO $GAPI_PARSER"
fi
AC_SUBST(GAPI_PARSER)

AC_PATH_PROG(GAPI_FIXUP, gapi3-fixup, no)
if test "x$GAPI_FIXUP" = "xno"; then
	AC_MSG_CHECKING(for gapi3-fixup.exe)
	GAPI_FIXUP=`which gapi3-fixup.exe 2> /dev/null`
	if test "x$GAPI_FIXUP" = "xno" ; then
		AC_MSG_ERROR(['gapi3_fixup'/'gapi3-fixup.exe' not found.])
	fi
	AC_MSG_RESULT($GAPI_FIXUP)
	GAPI_FIXUP="$MONO $GAPI_FIXUP"
fi
AC_SUBST(GAPI_FIXUP)

AC_PATH_PROG(GAPI_CODEGEN, gapi3-codegen, no)
if test "x$GAPI_CODEGEN" = "xno"; then
	AC_MSG_CHECKING(for gapi3_codegen.exe)
	GAPI_CODEGEN=`which gapi3_codegen.exe 2> /dev/null`
	if test "x$GAPI_CODEGEN" = "xno" ; then
		AC_MSG_ERROR(['gapi3-codegen'/'gapi3_codegen.exe' not found.])
	fi
	AC_MSG_RESULT([$GAPI_CODEGEN])
	GAPI_CODEGEN="$MONO $GAPI_CODEGEN"
fi
AC_SUBST(GAPI_CODEGEN)

dnl Check for monodoc
AC_PATH_PROG(MDASSEMBLER, mdassembler, no)
AC_PATH_PROG(MONODOCER, monodocer, no)
if test "x$MONODOCER" = "xno" -o "x$MDASSEMBLER" = "xno"; then
	enable_monodoc=no
	doc_sources_dir=
else
	enable_monodoc=yes
	doc_sources_dir="`pkg-config --variable=sourcesdir monodoc`"
fi
AC_SUBST(MDASSEMBLER)
AC_SUBST(MONODOCER)

AM_CONDITIONAL(ENABLE_MONODOC, test "x$enable_monodoc" = "xyes")

AC_CONFIG_FILES([
	Makefile
	webkitgtk3-sharp.spec
	doc/Makefile
	samples/Makefile
	sources/Makefile
	sources/AssemblyInfo.cs
	sources/webkitgtk3-sharp-3.0.pc:sources/webkitgtk3-sharp.pc.in
	sources/webkitgtk3-sharp.dll.config
])
AC_CONFIG_COMMANDS([default],[[ echo timestamp > stamp-h ]],[[]])
AC_OUTPUT

echo ""
echo "Configuration summary"
echo "---------------------"
echo ""
echo "   * Installation prefix: $prefix"
echo "   * compiler: $CSC"
echo "   * Documentation: $enable_monodoc ($MONODOC)"
echo ""

