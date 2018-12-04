#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.
AC_PREREQ([2.69])
AC_INIT([vp-kmod], [1.3.2])
AC_CONFIG_SRCDIR([src/main.c])
AM_INIT_AUTOMAKE([foreign])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_HEADERS([config.h])

AC_SUBST(KERNEL)
AC_ARG_WITH([kernel], [AS_HELP_STRING([--with-kernel],
	    [Specify kernel version])],
	    [KERNEL=${with_kernel}],
	    [KERNEL=`uname -r`])
AC_DEFINE_UNQUOTED([KERNEL], ["$KERNEL"], [kernel version])
AC_SUBST([with_kernel_mod], [/lib/modules/$KERNEL/extra])
AC_SUBST([KERNEL_SRC], [/lib/modules/$KERNEL/build])
AC_SUBST([KERNEL_MOD], [$with_kernel_mod])
AC_SUBST(RELEASE)
AC_ARG_WITH([release-id], [AS_HELP_STRING([--with-release-id],
            [Specify release id])],
            [RELEASE=_${with_release_id}],
            [RELEASE=""])
AC_DEFINE_UNQUOTED([RELEASE], ["$RELEASE"], [Release Date])

# Checks for programs.
AC_PROG_CC
AC_PROG_INSTALL
AC_PROG_MKDIR_P
AC_PROG_LN_S
LT_INIT
AC_SUBST([LIBTOOL_DEPS])

# Checks for libraries.
# (none)

# Checks for header files.
AC_CHECK_HEADERS([linux/kernel.h])
AC_C_INLINE

# Checks for typedefs, structures, and compiler characteristics.
# Chardev may or may not use these types, I'm just including them
# for illustrative purposes.

# Checks for library functions.
# (none)

# Final output
AC_CONFIG_FILES([Makefile
		 vp-kmod.spec
		 debian/rules
		 debian/control
		 debian/vp-kmod.install
		 debian/vp-kmod-dev.install])
AC_OUTPUT
