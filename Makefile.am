ACLOCAL_AMFLAGS = -I m4

SUBDIRS = src doc

dist-hook: ChangeLog

ChangeLog: configure.ac
	git log --no-merges master | \
		unexpand -a | sed -e 's/\s\s*$$//' > $@

# release with paranoia backup to avoid debauched git repository
rel__tar = ${AMTAR} cof - ".git"
.PHONY: release
release:
	$(rel__tar) | bzip2 -9 -c >.backup-gitmeta-$$(date +%s).tar.bz2 && \
	touch configure.ac && \
	$(MAKE)  $(AM_MAKEFLAGS) \
	  top_distdir="$(top_distdir)" distdir="$(distdir)" \
	  distcheck && \
	git commit -asm 'Prepare release $(PACKAGE_VERSION)' && \
	git branch --no-track release/v$(PACKAGE_VERSION) && \
	git tag -sm 'v$(PACKAGE_VERSION)' v$(PACKAGE_VERSION) && \
	touch configure.ac && \
	$(MAKE)  $(AM_MAKEFLAGS) \
	  top_distdir="$(top_distdir)" distdir="$(distdir)" \
	  distcheck && \
	git commit --amend -aC HEAD
