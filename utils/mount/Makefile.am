## Process this file with automake to produce Makefile.in

# These binaries go in /sbin (not /usr/sbin), unless CONFIG_SBIN_OVERRIDE
# is disabled as may be appropriate when /sbin is a symlink.
# Note that we don't use "if CONFIG_SBIN_OVERRIDE" as that
# causes autotools to notice the override and disable it.
@CONFIG_SBIN_OVERRIDE_TRUE@sbindir = /sbin

man8_MANS	= mount.nfs.man umount.nfs.man
man5_MANS	= nfs.man

sbin_PROGRAMS	= mount.nfs
EXTRA_DIST = nfsmount.conf $(man8_MANS) $(man5_MANS)
mount_common = error.c network.c token.c \
		    parse_opt.c parse_dev.c \
		    nfsmount.c nfs4mount.c stropts.c\
		    mount_constants.h error.h network.h token.h \
		    parse_opt.h parse_dev.h \
		    nfs4_mount.h stropts.h version.h \
		    mount_config.h utils.c utils.h \
		    nfs_mount.h

if MOUNT_CONFIG
mount_common += configfile.c
man5_MANS += nfsmount.conf.man
else
EXTRA_DIST += nfsmount.conf.man
endif

mount_nfs_LDADD = ../../support/nfs/libnfs.la \
		  ../../support/export/libexport.a \
		  ../../support/reexport/libreexport.a \
		  ../../support/misc/libmisc.a \
		   $(LIBTIRPC) $(LIBPTHREAD)

mount_nfs_SOURCES = $(mount_common)

if CONFIG_LIBMOUNT
mount_nfs_SOURCES += mount_libmount.c
mount_nfs_LDADD += $(LIBMOUNT)
else
mount_nfs_SOURCES += mount.c fstab.c nfsumount.c fstab.h

endif

MAINTAINERCLEANFILES = Makefile.in

install-exec-hook:
	(cd $(DESTDIR)$(sbindir) && \
	  ln -sf mount.nfs mount.nfs4 && \
	  ln -sf mount.nfs umount.nfs && \
	  ln -sf mount.nfs umount.nfs4 && \
	  chmod 4711 mount.nfs )
uninstall-hook:
	(cd $(DESTDIR)$(sbindir) && \
	    rm -f mount.nfs4 umount.nfs umount.nfs4)


install-man-links:
	(cd $(DESTDIR)$(man8dir) && \
	  for m in $(man8_MANS) $(dist_man8_MANS) $(nodist_man8_MANS); do \
	    inst=`echo $$m | sed -e 's/man$$/8/'`; \
	    rm -f $$inst ; \
	  done)
	(cd $(DESTDIR)$(man5dir) && \
	  for m in $(man5_MANS) $(dist_man5_MANS) $(nodist_man5_MANS); do \
	    inst=`echo $$m | sed -e 's/man$$/5/'`; \
	    rm -f $$inst ; \
	  done)

uninstall-man-links:
	(cd $(DESTDIR)$(man8dir) && \
	  for m in $(man8_MANS) $(dist_man8_MANS) $(nodist_man8_MANS); do \
	    inst=`echo $$m | sed -e 's/man$$/8/'`; \
	    rm -f $$inst ; \
	  done)
	(cd $(DESTDIR)$(man5dir) && \
	  for m in $(man5_MANS) $(dist_man5_MANS) $(nodist_man5_MANS); do \
	    inst=`echo $$m | sed -e 's/man$$/5/'`; \
	    rm -f $$inst ; \
	  done)

