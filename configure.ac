dnl
dnl
dnl    Pixie configure.in file
dnl
dnl
AC_INIT([Pixie], [2.2.6])
AC_CONFIG_SRCDIR([src/common/global.h])
AM_INIT_AUTOMAKE
AM_CONFIG_HEADER(config.h)
AC_PREFIX_DEFAULT(/opt/pixie)
AC_CONFIG_MACRO_DIR([m4])

dnl ---------------------------------------------------
dnl Check the compiler/programs
dnl
AC_PROG_CC
AC_PROG_CPP
AC_PROG_CXX
AM_DISABLE_STATIC
AM_PROG_LIBTOOL
AC_PROG_INSTALL
AC_PROG_MAKE_SET

dnl ---------------------------------------------------
dnl Check standard libraries
dnl
AC_CHECK_LIB(dl, dlopen,,[AC_CHECK_LIB(c, dlopen,,[AC_MSG_ERROR([dlopen routine is not found])],)],)
AC_CHECK_LIB(m, sin,,[AC_MSG_ERROR([math library is not found])],)
AC_CHECK_LIB(pthread, pthread_create,,[AC_MSG_ERROR([pthread library is not found])],)

dnl Check the single precision version of some math functions
AC_CHECK_LIB([m], [acosf],	[AC_DEFINE([HAVE_ACOSF],[1],[libm includes acosf])])

AC_CHECK_LIB([m], [asinf],	[AC_DEFINE([HAVE_ASINF],[1],[libm includes asinf])])

AC_CHECK_LIB([m], [atan2f], [AC_DEFINE([HAVE_ATAN2F],[1],[libm includes atan2f])])

AC_CHECK_LIB([m], [atanf],	[AC_DEFINE([HAVE_ATANF],[1],[libm includes atanf])])

AC_CHECK_LIB([m], [cosf],	[AC_DEFINE([HAVE_COSF],[1],[libm includes cosf])])

AC_CHECK_LIB([m], [expf],	[AC_DEFINE([HAVE_EXPF],[1],[libm includes expf])])

AC_CHECK_LIB([m], [fabsf],	[AC_DEFINE([HAVE_FABSF],[1],[libm includes fabsf])])

AC_CHECK_LIB([m], [floorf], [AC_DEFINE([HAVE_FLOORF],[1],[libm includes floorf])])

AC_CHECK_LIB([m], [fmodf],	[AC_DEFINE([HAVE_FMODF],[1],[libm includes fmodf])])

AC_CHECK_LIB([m], [hypotf], [AC_DEFINE([HAVE_HYPOTF],[1],[libm includes hypotf])])

AC_CHECK_LIB([m], [logf],	[AC_DEFINE([HAVE_LOGF],[1],[libm includes logf])])

AC_CHECK_LIB([m], [powf],	[AC_DEFINE([HAVE_POWF],[1],[libm includes powf])])

AC_CHECK_LIB([m], [sinf],	[AC_DEFINE([HAVE_SINF],[1],[libm includes sinf])])

AC_CHECK_LIB([m], [sqrtf],	[AC_DEFINE([HAVE_SQRTF],[1],[libm includes sqrtf])])

AC_CHECK_LIB([m], [tanf],	[AC_DEFINE([HAVE_TANF],[1],[libm includes tanf])])

dnl ---------------------------------------------------
dnl Find X
dnl
AC_PATH_X
AC_PATH_XTRA
X_LDFLAGS="$X_LDFLAGS -lX11 -lXext"

saved_CFLAGS="$CFLAGS"
saved_LDFLAGS="$LDFLAGS"
saved_LIBS="$LIBS"

CFLAGS="$CFLAGS $X_CFLAGS"
LDFLAGS="$X_LDFLAGS $X_LIBS $LDFLAGS"

have_x11=true
AC_CHECK_LIB(X11, XOpenDisplay,,[AC_MSG_WARN([X11 is not found, it is required by framebuffer]);have_x11=false],)
AC_CHECK_LIB(Xext, XShmAttach,,[AC_MSG_WARN([Xext is not found, it is required by framebuffer]);have_x11=false],)
AM_CONDITIONAL(BUILD_FRAMEBUFFER, test "x$have_x11" = "xtrue")

CFLAGS="$saved_CFLAGS"
LDFLAGS="$saved_LDFLAGS"
LIBS="$saved_LIBS"

dnl ---------------------------------------------------
dnl Find Qt
dnl

dnl no_qt=false
dnl AM_PATH_QT
dnl AM_CONDITIONAL(BUILD_SHOW, test "x$no_qt" = "x" )

dnl ---------------------------------------------------
dnl Find FLTK
dnl

have_fltk=false
AM_PATH_FLTK(have_fltk=true,have_fltk=false)
AM_CONDITIONAL(BUILD_SHOW, test x$have_fltk = xtrue)

dnl ---------------------------------------------------
dnl Find libtiff
dnl

saved_LIBS="$LIBS"

AC_CHECK_HEADER(tiffio.h,,AC_MSG_ERROR([tiffio.h not found]))
AC_CHECK_LIB(tiff,TIFFOpen,,[AC_MSG_ERROR([libtiff not found])],)

TIFF_LIBS="$LIBS"
LIBS="$saved_LIBS"

dnl ---------------------------------------------------
dnl Find libpng
dnl

saved_LIBS="$LIBS"

have_libpng=true
AC_CHECK_HEADER(png.h,,AC_MSG_WARN([png.h not found]))
AC_CHECK_LIB(png,png_create_write_struct,,[AC_MSG_WARN([libpng not found]);have_libpng=false],)

if test "x$have_libpng" = "xtrue"; then
    AC_DEFINE_UNQUOTED(HAVE_LIBPNG,1, [Is libpng avaiable?])
    PNG_LIBS="$LIBS"
else
    PNG_LIBS=""
fi

LIBS="$saved_LIBS"

dnl ---------------------------------------------------
dnl Find OpenEXR
dnl

AM_PATH_OPENEXR
AM_CONDITIONAL(BUILD_OPENEXR, test "x$no_openexr" = "x")

dnl ---------------------------------------------------
dnl Find zlib
dnl
ZLIB_LIBS="-lz"

saved_CFLAGS="$CFLAGS"
saved_LDFLAGS="$LDFLAGS"
saved_LIBS="$LIBS"

LDFLAGS="$LDFLAGS $ZLIB_LIBS"

have_zlib=true
AC_CHECK_LIB(z, gzopen,,[AC_MSG_WARN([libz is not found]);have_zlib=false],)
if test "x$have_zlib" = "xtrue"; then
	AC_DEFINE_UNQUOTED(HAVE_ZLIB,1, [Is Zlib avaiable?])
else
	AC_DEFINE_UNQUOTED(HAVE_ZLIB,0, [Is Zlib avaiable?])
	ZLIB_LIBS=""
fi

CFLAGS="$saved_CFLAGS"
LDFLAGS="$saved_LDFLAGS"
LIBS="$saved_LIBS"

dnl ---------------------------------------------------
dnl Check system/headers
dnl
AC_HEADER_STDC
AC_CHECK_HEADERS([fcntl.h limits.h malloc.h sys/time.h unistd.h])

AC_C_CONST
AC_C_INLINE
AC_TYPE_SIZE_T
AC_HEADER_TIME

AC_FUNC_ALLOCA
AC_FUNC_VPRINTF
AC_CHECK_FUNCS(gethostname gettimeofday mkdir rmdir socket strdup strstr strtod strtol random)

dnl ---------------------------------------------------
dnl Check for bison and flex
dnl

dnl I am intentionally not using AC_PROG_LEX and AC_PROG_YACC here
dnl we have never tested with lex / yacc, and they probably won't work '

AC_PATH_PROG(FLEX,flex,false)
AM_CONDITIONAL(HAVE_FLEX, test "x$FLEX" != xfalse )
if test "x$FLEX" == xfalse; then
	AC_MSG_WARN([[flex not found, pre-generated files will be used, any changes to .l files will not affect build]])
fi

AC_PATH_PROG(BISON,bison,false)
AM_CONDITIONAL(HAVE_BISON, test "x$BISON" != xfalse )
if test "x$BISON" == xfalse; then
	AC_MSG_WARN([[bison not found, pre-generated files will be used, any changes to .y files will not affect build]])
fi

dnl ---------------------------------------------------
dnl Check for Mac OS X
dnl
have_carbon=no
AC_MSG_CHECKING([for Mac OS X Carbon support])
AC_TRY_CPP([
#include <Carbon/Carbon.h>
#include <CoreServices/CoreServices.h>
], have_carbon=yes)
AC_MSG_RESULT([$have_carbon])

if test "x$have_carbon" = "xyes"; then
  LIBS="$LIBS -Wl,-framework,Carbon"
fi

dnl ---------------------------------------------------
dnl Do variable substitution
dnl
AC_SUBST(X_LIBS)
AC_SUBST(X_CFLAGS)
AC_SUBST(X_LDFLAGS)
AC_SUBST(TIFF_LIBS)
AC_SUBST(PNG_LIBS)
AC_SUBST(BUILD_SHOW)
AC_SUBST(ZLIB_LIBS)
AC_SUBST(FLTK_CXXFLAGS)
AC_SUBST(FLTK_LDFLAGS)

dnl ---------------------------------------------------
dnl Are we building for self contained distribution
dnl		Note: man goes in prefix for selfcontained (leave default as is)
dnl

AC_ARG_ENABLE(selfcontained,
AS_HELP_STRING([--disable-selfcontained],[Build for a selfcontained setup (overrides custom directory settings)]),
[case "${enableval}" in
	yes)	docdir="\${prefix}/doc"
		shaderdir="\${prefix}/shaders"
		ribdir="\${prefix}/ribs"
		texturedir="\${prefix}/textures"
		displaysdir="\${prefix}/displays"
		proceduraldir="\${prefix}/procedurals"
		modulesdir="\${prefix}/modules"
		mandir="\${prefix}/man"
		;;
	no)
		docdir="\${datadir}/doc/Pixie"
		shaderdir="\${datadir}/Pixie/shaders"
		ribdir="\${datadir}/Pixie/ribs"
		texturedir="\${datadir}/Pixie/textures"
		proceduraldir="${libdir}/Pixie/procedurals"
		displaysdir="\${libdir}/Pixie/displays"
		modulesdir="\${libdir}/Pixie/modules"
		;;
	*) AC_MSG_ERROR(bad value ${enableval} for --enable-selfcontained) ;;
esac],
[
docdir="\${prefix}/doc"
shaderdir="\${prefix}/shaders"
ribdir="\${prefix}/ribs"
texturedir="\${prefix}/textures"
displaysdir="\${prefix}/displays"
proceduraldir="\${prefix}/procedurals"
modulesdir="\${prefix}/modules"
])

dnl ---------------------------------------------------
dnl Set custom directories for shaders, models, procedurals, and displays
dnl these defaults are set above, which is why we do overrides after
dnl

AC_ARG_WITH(docdir,
	AS_HELP_STRING([--with-docdir=DIR],[directory where we install documentation (Default: DATADIR/doc/Pixie)]),
	docdir="${withval}")

AC_ARG_WITH(shaderdir,
	AS_HELP_STRING([--with-shaderdir=DIR],[directory where we store shaders (Default: DATADIR/Pixie/shaders)]),
	shaderdir="${withval}")

AC_ARG_WITH(ribdir,
	AS_HELP_STRING([--with-ribdir=DIR],[directory where we store rib files (Default: DATADIR/Pixie/ribs)]),
	ribdir="${withval}")

AC_ARG_WITH(texturedir,
	AS_HELP_STRING([--with-texturedir=DIR],[directory where we store textures (Default: DATADIR/Pixie/textures)]),
	texturedir="${withval}")

AC_ARG_WITH(proceduraldir,
	AS_HELP_STRING([--with-shaderdir=DIR],[directory to store shaders (Default: LIBDIR/Pixie/procedurals)]),
	proceduraldir="${withval}")

AC_ARG_WITH(displaysdir,
	AS_HELP_STRING([--with-displaysdir=DIR],[directory to store displays (Default: LIBDIR/Pixie/displays)]),
	displaysdir="${withval}")

AC_ARG_WITH(modulesdir,
	AS_HELP_STRING([--with-modulesdir=DIR],[directory to store modules (Default: LIBDIR/Pixie/modules)]),
	modulesdir="${withval}")


dnl ---------------------------------------------------
dnl Now tell the c preprocessor and the Makefiles where stuff goes
dnl 

AC_DEFUN([AC_DEFINE_DIR], [
  prefix_NONE=
  exec_prefix_NONE=
  test "x$prefix" = xNONE && prefix_NONE=yes && prefix=$ac_default_prefix
  test "x$exec_prefix" = xNONE && exec_prefix_NONE=yes && exec_prefix=$prefix
  eval ac_define_dir="[$]$2"
  eval ac_define_dir="\"[$]ac_define_dir\""
  AC_SUBST($1, "$ac_define_dir")
  AC_SUBST($2, "$ac_define_dir")
  AC_DEFINE_UNQUOTED($1, "$ac_define_dir", [$3])
  test "$prefix_NONE" && prefix=NONE
  test "$exec_prefix_NONE" && exec_prefix=NONE
])

AC_DEFINE_DIR(PIXIE_DOCS,docdir,[Directory where we store documentation])
AC_DEFINE_DIR(PIXIE_SHADERS,shaderdir,[Directory where we store shaders])
AC_DEFINE_DIR(PIXIE_RIBS,ribdir,[Directory where we store ribs])
AC_DEFINE_DIR(PIXIE_TEXTURES,texturedir,[Directory where we store textures])
AC_DEFINE_DIR(PIXIE_DISPLAYS,displaysdir,[Directory where we store displays])
AC_DEFINE_DIR(PIXIE_PROCEDURALS,proceduraldir,[Directory where we store procedurals])
AC_DEFINE_DIR(PIXIE_MODULES,modulesdir,[Directory where we store modules])

dnl ---------------------------------------------------
dnl Write the output
dnl
AC_CONFIG_FILES([Makefile doc/Makefile src/Makefile src/common/Makefile src/file/Makefile src/framebuffer/Makefile src/openexr/Makefile src/gui/Makefile src/precomp/Makefile src/rgbe/Makefile src/sdr/Makefile src/sdrc/Makefile src/ri/Makefile src/rndr/Makefile src/texmake/Makefile src/sdrinfo/Makefile src/show/Makefile])
AC_OUTPUT

if test "x$have_x11" != "xtrue"; then
echo "------------------------------------------------"
echo "--->            X11 not found."
echo "---> You should an X11 distribution for your OS"
echo "---> The framebuffer display will not be built."
fi

dnl if test "x$no_qt" != "x"; then
dnl echo "------------------------------------------------"
dnl echo "--->            Qt not found."
dnl echo "---> You can download it from www.trolltech.com"
dnl echo "---> show will not be built."
dnl fi

if test "x$have_fltk" != "xtrue"; then
echo "------------------------------------------------"
echo "--->            FLTK not found."
echo "---> You can download it from www.fltk.org"
echo "---> show will not be built."
fi

if test "x$no_openexr" != "x"; then
echo "------------------------------------------------"
echo "--->            OpenEXR not found."
echo "---> You can download it from www.openexr.org"
echo "---> openexr display driver will not be built."
fi

if test "x$have_zlib" != "xtrue"; then
echo "------------------------------------------------"
echo "--->            ZLib not found."
echo "---> You can download it from www.zlib.net"
echo "---> compressed rib support will not be built."
fi

if test "x$have_libpng" != "xtrue"; then
echo "------------------------------------------------"
echo "--->            libpng not found."
echo "---> You can download it from www.libpng.org"
echo "---> Writing PNG files will not be supported."
fi

echo "------------------------------------------------"
echo " Installation info:"
echo "     PIXIEHOME: " $prefix
echo " Documentation: " $docdir
echo "       Shaders: " $shaderdir
echo "          RIBs: " $ribdir
echo "      Textures: " $texturedir
echo "      Displays: " $displaysdir
echo "   Procedurals: " $proceduraldir
echo "       Modules: " $modulesdir
echo "------------------------------------------------"
echo "To build Pixie:"
echo ">make"
echo ">make install"
echo "              Terminus Est"
echo

