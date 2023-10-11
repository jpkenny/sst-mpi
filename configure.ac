dnl
dnl   This file is part of SST/macroscale: 
dnl                The macroscale architecture simulator from the SST suite.
dnl

dnl Copyright 2009-2023 National Technology and Engineering Solutions of Sandia,
dnl LLC (NTESS).  Under the terms of Contract DE-NA-0003525, the U.S. Government
dnl retains certain rights in this software.
dnl
dnl Sandia National Laboratories is a multimission laboratory managed and operated
dnl by National Technology and Engineering Solutions of Sandia, LLC., a wholly
dnl owned subsidiary of Honeywell International, Inc., for the U.S. Department of
dnl Energy's National Nuclear Security Administration under contract DE-NA0003525.
dnl
dnl Copyright (c) 2009-2023, NTESS
dnl
dnl All rights reserved.
dnl
dnl Redistribution and use in source and binary forms, with or without
dnl modification, are permitted provided that the following conditions are
dnl met:
dnl
dnl * Redistributions of source code must retain the above copyright
dnl   notice, this list of conditions and the following disclaimer.
dnl
dnl * Redistributions in binary form must reproduce the above
dnl   copyright notice, this list of conditions and the following
dnl   disclaimer in the documentation and/or other materials provided
dnl   with the distribution.
dnl
dnl * Neither the name of the copyright holder nor the names of its
dnl   contributors may be used to endorse or promote products derived
dnl   from this software without specific prior written permission.
dnl
dnl THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
dnl "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
dnl LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
dnl A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
dnl OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
dnl SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
dnl LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
dnl DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
dnl THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
dnl (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
dnl OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

# Version info, used both in library versioning and inside sstmacro.
m4_define([SSTMPI_VERSION_TAG], 13)
m4_define([SSTMPI_SUBVERSION_TAG], 0)
m4_define([SSTMPI_SUBSUBVERSION_TAG], 0)

dnl Enable this for releases
m4_define([SSTMAC_SNAPSHOT_TAG])
dnl Enable this for beta releases
dnl m4_define([SSTMAC_SNAPSHOT_TAG],-beta1)
dnl Enable this for development snapshots (should generally be enabled)
dnl m4_define([SSTMAC_SNAPSHOT_TAG],-snapshot)

# Construct the sstmacro version
m4_define([SSTMPI_ACVERSION_TAG],
          [SSTMPI_VERSION_TAG.SSTMPI_SUBVERSION_TAG.SSTMPI_SUBSUBVERSION_TAG])

AC_INIT([SST MPI],[SSTMPI_ACVERSION_TAG],[wg-sstp@sandia.gov])
AC_CONFIG_MACRO_DIR([acinclude])
AC_CONFIG_AUX_DIR(bin)
#AX_PREFIX_CONFIG_H(sst_mpi_config.h, SSTMPI)
AC_CANONICAL_TARGET

# Construct the libtool version
dnl Libtool library versioning is used to determine compatible libraries. A libtool version
dnl consists of three numbers CURRENT:REVISION:AGE. These have the following meanings:
dnl CURRENT:  The most recent interface number that this library implements.
dnl REVISION: The implementation number of the current interface.
dnl AGE:      The difference between the newest and oldest interfaces
dnl           that this library implements. In other words, the
dnl           library implements all the interface numbers in the
dnl           range from number current - age to current.
dnl For new releases the following procedure is used to determine the new version:
dnl If incompatible changes are made: CURRENT++, REVISION=0, AGE=0
dnl else if interfaces are added: CURRENT++, REVISION=0, AGE++
dnl else (no interface changes): REVISION++
m4_define([SSTMPI_CURRENT_LIBVERS],  12)
m4_define([SSTMPI_REVISION_LIBVERS], 1)
m4_define([SSTMPI_AGE_LIBVERS],      0)
m4_define([SSTMPI_LIBVERSION_TAG],
          [SSTMPI_CURRENT_LIBVERS:SSTMPI_REVISION_LIBVERS:SSTMPI_AGE_LIBVERS])

# More version info.
AH_TEMPLATE([VERSION], [Major version number])
AH_TEMPLATE([SUBVERSION], [Major version number])
AH_TEMPLATE([SUBSUBVERSION], [Major version number])
AC_DEFINE_UNQUOTED(VERSION, [SSTMPI_VERSION_TAG])
AC_DEFINE_UNQUOTED(SUBVERSION, [SSTMPI_SUBVERSION_TAG])
AC_DEFINE_UNQUOTED(SUBSUBVERSION, [SSTMPI_SUBSUBVERSION_TAG])
AC_SUBST(SSTMPI_LIBVERSION, [SSTMPI_LIBVERSION_TAG])

# Init automake
AM_INIT_AUTOMAKE([tar-pax -Wall -Werror foreign -Wno-portability subdir-objects])
AM_PROG_AS
m4_ifdef([AM_PROG_AR], [AM_PROG_AR])
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

# Check compilers and environment
dnl AC_PROG_RANLIB
AC_PROG_CC
AC_PROG_CXX
AC_LANG([C++])
LT_INIT

LT_INIT([shared disable-static dlopen])
LT_PREREQ([2.2.6])

AC_ARG_WITH([sst-core],
    AS_HELP_STRING([--with-sst-core@<:@=DIR@:>@],
        [Path to SST-Core install]
    ), [
      SST="$withval"
    ], [
      AC_MSG_ERROR([Please specify SST-Core using --with-sst-core])
    ]
)

AC_SUBST([sst_prefix], "$SST")
SST_INCLUDES="-I$SST/include -I$SST/include/sst -I$SST/include/sst/core"
SST_CPPFLAGS="-DSSTMAC_INTEGRATED_SST_CORE=1 $SST_INCLUDES -D__STDC_FORMAT_MACROS"
SAVE_CPPFLAGS="$CPPFLAGS"
SST_CPPFLAGS="$SST_CPPFLAGS"
CPPFLAGS="$CPPFLAGS $SST_CPPFLAGS"

SST_LDFLAGS=""

# We have to use CXXFLAGS from sst-config script
SAVE_CXXFLAGS="$CXXFLAGS"
SST_CXXFLAGS="`$SST/bin/sst-config --CXXFLAGS`"
CXXFLAGS="$CXXFLAGS $SST_CXXFLAGS"

SST_CORE_CXX=`$SST/bin/sst-config --CXX`
SST_CORE_CC=`$SST/bin/sst-config --CC`

AC_CHECK_HEADERS([sst/core/component.h], [],
      [AC_MSG_ERROR([Could not locate SST core header files at $SST])])

AC_ARG_WITH([sst-elements],
    AS_HELP_STRING([--with-sst-elements@<:@=DIR@:>@],
        [Path to valid installation of sst-elements library],
    ), [
      SST_ELEMENTS="$withval"
      have_elements="yes"
    ], [
      have_elements="no"
    ]
)

if test "X$have_elements" = "Xyes"; then
  AC_DEFINE_UNQUOTED([HAVE_SST_ELEMENTS], 1, [Build with sst-elements])
  AM_CONDITIONAL([HAVE_SST_ELEMENTS], true)

  SAVE_CPPFLAGS="$CPPFLAGS"
  SAVE_CXXFLAGS="$CXXFLAGS"
  SST_ELEMENTS_CPPFLAGS="-I$SST_ELEMENTS/include"
  CPPFLAGS="$CPPFLAGS $SST_ELEMENTS_CPPFLAGS $SST_CPPFLAGS"
  CXXFLAGS="$CXXFLAGS $SST_CXXFLAGS"

  AC_CHECK_HEADERS([sst/elements/mercury/sst_hg_config.h], [],
      [AC_MSG_ERROR([Could not locate SST element header files at $SST_ELEMENTS])])

  AC_SUBST(SST_ELEMENTS_CPPFLAGS)
  CPPFLAGS="$SAVE_CPPFLAGS"
  CXXFLAGS="$SAVE_CXXFLAGS"
else
  SST_ELEMENTS_CPPFLAGS=""
  AC_SUBST(SST_ELEMENTS_CPPFLAGS)
  AM_CONDITIONAL([HAVE_SST_ELEMENTS], false)
fi

CPPFLAGS="$CPPFLAGS $SST_ELEMENTS_CPPFLAGS -I$SST_ELEMENTS/include/sst/elements -I$SST_ELEMENTS/include/sst/elements/mercury -I$SST_ELEMENTS/include/sst/elements/iris"

AC_CONFIG_FILES([Makefile])

AC_OUTPUT
