#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

  
#    Copyright (C) 2003, 2005, 2012, 2014, 2015  Dale Mellor
#   
#    This file is part of GNU mcron.
# 
#    GNU mcron is free software: you can redistribute it and/or modify it under
#    the terms of the GNU General Public License as published by the Free
#    Software Foundation, either version 3 of the License, or (at your option)
#    any later version.
# 
#    GNU mcron is distributed in the hope that it will be useful, but WITHOUT
#    ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
#    FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for
#    more details.
# 
#    You should have received a copy of the GNU General Public License along
#    with GNU mcron.  If not, see <http://www.gnu.org/licenses/>.


AC_PREREQ(2.61)
AC_INIT([GNU Mcron], [1.0.8], [bug-mcron@gnu.org])
AC_CONFIG_AUX_DIR([build-aux])
AM_INIT_AUTOMAKE([silent-rules])

# Enable silent rules by default.
AM_SILENT_RULES([yes])

guilemoduledir="${datarootdir}/guile/site/2.0"
AC_SUBST([guilemoduledir])

AC_MSG_CHECKING([whether debugging is requested])
AC_ARG_ENABLE(debug,
              AC_HELP_STRING([--enable-debug],
                             [enable debugging and traceback on error]),
              CONFIG_DEBUG=$enableval,
              CONFIG_DEBUG=no)
AC_MSG_RESULT($CONFIG_DEBUG)
if test "$CONFIG_DEBUG" = "no"; then
   CONFIG_DEBUG="#f"
else
   CONFIG_DEBUG="#t"
fi
AC_SUBST(CONFIG_DEBUG)


# We have no interest (hence a no-op), but Debian wants this.
AC_ARG_ENABLE(maintainer-mode)

AC_CANONICAL_HOST
AC_PROG_AWK
AC_PROG_EGREP
AM_PROG_CC_C_O

PKG_CHECK_MODULES([GUILE], [guile-2.0 >= 2.0.7])
AC_PATH_PROG([GUILE], [guile])

# search guild
AC_PATH_PROG([GUILD], [guild])
if test "x$GUILD" = "x"; then
   AC_MSG_ERROR(['guild' binary not found; please check your guile-2.x installation.])
fi

# Checks for programs.
  
AM_MISSING_PROG(HELP2MAN, help2man, $missing_dir)

AC_CHECK_PROGS(SED, sed)
if test "x$ac_cv_prog_SED" = "x"; then
   AC_MSG_ERROR(sed not found)
fi
AC_CHECK_PROGS(HEAD, head)
if test "x$ac_cv_prog_HEAD" = "x"; then
   AC_MSG_ERROR(head not found)
fi
AC_CHECK_PROGS(WHICH, which)
if test "x$ac_cv_prog_WHICH" = "x"; then
    AC_MSG_ERROR(which not found)
fi
AC_CHECK_PROGS(CP, cp)
if test "x$ac_cv_prog_CP" = "x"; then
    AC_MSG_ERROR(cp not found)
fi


# Now find a sendmail or equivalent.

AC_CHECK_PROGS(SENDMAIL, sendmail)
if test "x$ac_cv_prog_SENDMAIL" != "x"; then
   AC_MSG_CHECKING(sendmail path and arguments)
   ac_cv_prog_SENDMAIL="`$ac_cv_prog_WHICH sendmail` -FCronDaemon -odi -oem "
dnl  -or0s"
   AC_MSG_RESULT($ac_cv_prog_SENDMAIL)

else
   AC_CHECK_PROGS(SENDMAIL, mail)
   if test "x$ac_cv_prog_SENDMAIL" != "x"; then
      AC_MSG_CHECKING(mail path)
      ac_cv_prog_SENDMAIL="`$ac_cv_prog_WHICH mail` -d "
      AC_MSG_RESULT($ac_cv_prog_SENDMAIL)
   else
      AC_MSG_RESULT(No mail program found)
   fi
fi
SENDMAIL=$ac_cv_prog_SENDMAIL


# Find out if we are avoiding Vixie.

AC_MSG_CHECKING([whether to avoid clobbering a Vixie installation])
AC_ARG_ENABLE(no-vixie-clobber,
              AC_HELP_STRING([--enable-no-vixie-clobber],
                             [do not install with program names that would override a legacy cron installation]),
              NO_VIXIE_CLOBBER=$enableval,
              NO_VIXIE_CLOBBER=[no])
AC_MSG_RESULT($NO_VIXIE_CLOBBER)
AC_SUBST(NO_VIXIE_CLOBBER)


# Configure the various files that mcron uses at runtime.

AC_MSG_CHECKING([which spool directory to use])
AC_ARG_WITH(spool-dir,
            AC_HELP_STRING([--with-spool-dir],
                           [the crontab spool directory (/var/cron/tabs)]),
              CONFIG_SPOOL_DIR=$withval,
              CONFIG_SPOOL_DIR=[/var/cron/tabs])
AC_MSG_RESULT($CONFIG_SPOOL_DIR)
AC_SUBST(CONFIG_SPOOL_DIR)

AC_MSG_CHECKING([name of socket])
AC_ARG_WITH(socket-file,
            AC_HELP_STRING([--with-socket-file],
                           [unix pathname for cron socket (/var/cron/socket)]),
              CONFIG_SOCKET_FILE=$withval,
              CONFIG_SOCKET_FILE=[/var/cron/socket])
AC_MSG_RESULT($CONFIG_SOCKET_FILE)
AC_SUBST(CONFIG_SOCKET_FILE)

AC_MSG_CHECKING([name of allow file])
AC_ARG_WITH(allow-file,
            AC_HELP_STRING([--with-allow-file],
                           [the file of allowed users (/var/cron/allow)]),
              CONFIG_ALLOW_FILE=$withval,
              CONFIG_ALLOW_FILE=[/var/cron/allow])
AC_MSG_RESULT($CONFIG_ALLOW_FILE)
AC_SUBST(CONFIG_ALLOW_FILE)

AC_MSG_CHECKING([name of deny file])
AC_ARG_WITH(deny-file,
            AC_HELP_STRING([--with-deny-file],
                           [the file of barred users (/var/cron/deny)]),
              CONFIG_DENY_FILE=$withval,
              CONFIG_DENY_FILE=[/var/cron/deny])
AC_MSG_RESULT($CONFIG_DENY_FILE)
AC_SUBST(CONFIG_DENY_FILE)

AC_MSG_CHECKING([name of PID file])
AC_ARG_WITH(pid-file,
            AC_HELP_STRING([--with-pid-file],
                           [the file to record cron's PID (/var/run/cron.pid)]),
              CONFIG_PID_FILE=$withval,
              CONFIG_PID_FILE=[/var/run/cron.pid])
AC_MSG_RESULT($CONFIG_PID_FILE)
AC_SUBST(CONFIG_PID_FILE)

AC_MSG_CHECKING([directory to hold temporary files])
AC_ARG_WITH(tmp-dir,
            AC_HELP_STRING([--with-tmp-dir],
                           [directory to hold temporary files (/tmp)]),
              CONFIG_TMP_DIR=$withval,
              CONFIG_TMP_DIR=[/tmp])
AC_MSG_RESULT($CONFIG_TMP_DIR)
AC_SUBST(CONFIG_TMP_DIR)



        
# This is to support `make DESTDIR=...'
                        
real_program_prefix=`echo $program_prefix | sed s/NONE//`
AC_SUBST(real_program_prefix)

AC_CONFIG_FILES([doc/config.texi
                 makefile
                 scm/mcron/makefile
                 scm/mcron/config.scm])
AC_OUTPUT
