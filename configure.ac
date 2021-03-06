# configuration cantools                        -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

# Copyright (C) 2007-2013 Andreas Heitmann

# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.

# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.

# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.


AC_PREREQ(2.69)
AC_INIT(cantools, 0.10, andreas.heitmann@gmail.com)
AC_CONFIG_SRCDIR([src/libcandbc/parser.y])
AC_CONFIG_HEADERS([config.h])
AM_INIT_AUTOMAKE
AC_CONFIG_MACRO_DIR([m4])
AC_LIBTOOL_WIN32_DLL

dnl common library version
version_info='0:10:0'
AC_SUBST(version_info)

# Checks for programs.
 	
# Turn off shared libraries during beta-testing, since they
# make the build process take too long.
LT_INIT([disable_shared])
AC_PROG_LIBTOOL

AC_SUBST(LIBTOOL_DEPS)
AC_PROG_YACC
AC_PROG_CC
AC_PROG_CXX
AC_PROG_LEX
AC_PROG_INSTALL
AC_PROG_AWK
AC_PROG_CPP
AC_PROG_MKDIR_P
AC_PROG_RANLIB
AC_PROG_LN_S

# Checks for functions
AC_REPLACE_FUNCS([strtok_r])
AC_CHECK_DECLS([strtok_r],
 , , [
#include <stdio.h>
#include <string.h>
#ifdef HAVE_STRINGS_H
# include <strings.h>
#endif])

AH_BOTTOM([
#if !HAVE_DECL_STRTOK_R
extern char *strtok_r (char *s, const char *delim, char **save_ptr);
#endif
])


# Checks for header files.
AC_FUNC_ALLOCA
AC_CHECK_HEADERS([fcntl.h inttypes.h libintl.h limits.h malloc.h stddef.h stdint.h stdlib.h string.h unistd.h endian.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_CHECK_HEADER_STDBOOL
AC_C_INLINE
AC_TYPE_INT8_T
AC_TYPE_INT16_T
AC_TYPE_INT32_T
AC_TYPE_INT64_T
AC_TYPE_OFF_T
AC_TYPE_SIZE_T
AC_TYPE_UINT8_T
AC_TYPE_UINT16_T
AC_TYPE_UINT32_T
AC_TYPE_UINT64_T
AC_C_CONST
	
AC_C_BIGENDIAN

# Checks for libraries
AC_CHECK_LIB([hdf5], [H5Fcreate])
AC_CHECK_LIB([m], [floor])
AC_CHECK_LIB([z], [deflate])

AC_SUBST(MATIO_LIB)
matio=1
AC_CHECK_HEADER(matio.h, [], [matio=0])
AC_CHECK_LIB(matio, Mat_VarCreate, [matio=matio], [matio=0])
if test $matio == 0
then
	echo "Error! You need to install libmatio first."
        exit -1
else
	MATIO_LIB=-lmatio
fi

AM_WITH_DMALLOC

AC_ARG_ENABLE(clg,
[  --enable-clg            Enable CLG file format],
[case "$enableval" in \
	yes)  enable_clg=yes ;; \
	no)   enable_clg=no;; \
	*)    enable_clg=no;; \
esac], [enable_clg=no])

# pass option to Makefile.am
AM_CONDITIONAL([ENABLE_CLG], [test "$enable_clg" = "yes"])

# Check for CLG library
if test $enable_clg = yes; then
clg_available=1
AC_CHECK_FILE([src/libcanclg/clgReader.h], [], [clg_available=0])
if test $clg_available == 0
then
	echo "Error! CLG source not installed."
        exit -1
fi
fi

AC_ARG_ENABLE(efence,
[  --enable-efence         Use electric fence (www.perens.com)],
[case "$enableval" in \
	yes)  efence=yes ;; \
	no)   efence=no;; \
	*)    efence=no;; \
esac], [efence=no])

# Check for efence
if test $efence = yes; then
AC_CHECK_LIB(efence,memalign)
fi

# Checks for library functions.
#AC_FUNC_ERROR_AT_LINE
AC_HEADER_ASSERT
AC_FUNC_MMAP
AC_CHECK_FUNCS([memset munmap strchr strdup strndup strpbrk strspn strstr strtol strtoul])
AC_CONFIG_FILES([Makefile])
AC_OUTPUT
