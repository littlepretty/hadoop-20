#
# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

#
# configure.ac for hadoop native code. 
#

# Notes: 
# 1. This configure.ac depends on the following environment variables to function correctly:
#    * HADOOP_NATIVE_SRCDIR 
#    * JAVA_HOME
#    * JVM_DATA_MODEL
#    * OS_NAME
#    * OS_ARCH 
#    All these are setup by build.xml. 

#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.
#

AC_PREREQ(2.59)
AC_INIT(src/org_apache_hadoop.h)
AC_CONFIG_SRCDIR([src/org_apache_hadoop.h])
AC_CONFIG_AUX_DIR(config)
AC_CONFIG_HEADER([config.h])
AC_SYS_LARGEFILE

AM_INIT_AUTOMAKE(hadoop,1.0.0)

# Checks for programs.
AC_PROG_CC
AC_PROG_LIBTOOL

# Checks for libraries.
dnl Check for '-ldl'
AC_CHECK_LIB([dl], [dlopen])

dnl Check for '-ljvm'
JNI_LDFLAGS=""
if test $JAVA_HOME != ""
then
  JNI_LDFLAGS="-L$JAVA_HOME/jre/lib/$OS_ARCH/server"
fi
ldflags_bak=$LDFLAGS
LDFLAGS="$LDFLAGS $JNI_LDFLAGS"
AC_CHECK_LIB([jvm], [JNI_GetCreatedJavaVMs])
LDFLAGS=$ldflags_bak
AC_SUBST([JNI_LDFLAGS])

# Checks for header files.
dnl Check for Ansi C headers
AC_HEADER_STDC

dnl Check for other standard C headers
AC_CHECK_HEADERS([stdio.h stddef.h], [], AC_MSG_ERROR(Some system headers not found... please ensure their presence on your platform.))

dnl Check for JNI headers
JNI_CPPFLAGS=""
if test $JAVA_HOME != ""
then
  for dir in `find $JAVA_HOME/include -follow -type d`
  do
    JNI_CPPFLAGS="$JNI_CPPFLAGS -I$dir"
  done
fi
cppflags_bak=$CPPFLAGS
CPPFLAGS="$CPPFLAGS $JNI_CPPFLAGS"
AC_CHECK_HEADERS([jni.h], [], AC_MSG_ERROR([Native java headers not found. Is \$JAVA_HOME set correctly?]))
CPPFLAGS=$cppflags_bak
AC_SUBST([JNI_CPPFLAGS])

dnl Check for zlib headers
AC_CHECK_HEADERS([zlib.h zconf.h], AC_COMPUTE_NEEDED_DSO(z,HADOOP_ZLIB_LIBRARY), AC_MSG_ERROR(Zlib headers were not found... native-hadoop library needs zlib to build. Please install the requisite zlib development package.))


dnl Check for lzma headers
AC_CHECK_HEADERS([lzma/lzma.h], AC_COMPUTE_NEEDED_DSO(lzma,HADOOP_LZMA_LIBRARY), AC_MSG_ERROR(lzma headers were not found... native-hadoop library needs lzma to build. Please install the requisite lzma-4.999.5alpha development package.
))

dnl check for posix_fadvise
AC_CHECK_HEADERS(fcntl.h, [AC_CHECK_FUNCS(posix_fadvise)])

dnl check for sync_file_range
AC_CHECK_HEADERS(fcntl.h, [AC_CHECK_FUNCS(sync_file_range)])

dnl Check for snappy headers
AC_CHECK_HEADERS([snappy-c.h], AC_COMPUTE_NEEDED_DSO(snappy,HADOOP_SNAPPY_LIBRARY), AC_MSG_WARN(Snappy headers were not found... building without snappy.))

# Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST

# Checks for library functions.
AC_CHECK_FUNCS([memset])

AC_CONFIG_FILES([Makefile
                 src/org/apache/hadoop/io/compress/snappy/Makefile
                 src/org/apache/hadoop/io/compress/zlib/Makefile
                 src/org/apache/hadoop/io/compress/lzma/Makefile
                 src/org/apache/hadoop/syscall/Makefile
                 lib/Makefile])
AC_OUTPUT

#
#vim: sw=2: ts=2: noet
#
