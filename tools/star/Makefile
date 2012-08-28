RAKUDO_VER  = 2012.08
NQP_VER     = 2012.08
PARROT_VER  = 4.6.0
PARROT_REL  = supported/$(PARROT_VER)

SRC_DIR = src

RAKUDO_TGZ = rakudo-$(RAKUDO_VER).tar.gz
RAKUDO_URL = https://github.com/downloads/rakudo/rakudo/$(RAKUDO_TGZ)
RAKUDO_SRC = $(SRC_DIR)/$(RAKUDO_TGZ)
NQP_TGZ    = nqp-$(NQP_VER).tar.gz
NQP_URL    = https://github.com/downloads/perl6/nqp/$(NQP_TGZ)
NQP_SRC    = $(SRC_DIR)/$(NQP_TGZ)
PARROT_TGZ = parrot-$(PARROT_VER).tar.gz
PARROT_URL = http://ftp.parrot.org/releases/$(PARROT_REL)/$(PARROT_TGZ)
PARROT_SRC = $(SRC_DIR)/$(PARROT_TGZ)

PERL   = perl
PREFIX = $(PERL) $(CURDIR)/tools/star/prefix.pl
WGET   = wget --no-check-certificate

always:

rakudo: nqp $(RAKUDO_SRC)
	tar --xform 's!rakudo-[^/]*!rakudo!' -xvzf $(RAKUDO_SRC)

$(RAKUDO_SRC):
	mkdir -p $(SRC_DIR)
	$(WGET) $(RAKUDO_URL) -O $(RAKUDO_SRC)

nqp: parrot $(NQP_SRC)
	tar --xform 's!nqp-[^/]*!nqp!' -xvzf $(NQP_SRC)

$(NQP_SRC):
	mkdir -p $(SRC_DIR)
	$(WGET) $(NQP_URL) -O $(NQP_SRC)

parrot: $(PARROT_SRC)
	tar --xform 's!parrot-[^/]*!parrot!' -xvzf $(PARROT_SRC)

$(PARROT_SRC):
	mkdir -p $(SRC_DIR)
	$(WGET) $(PARROT_URL) -O $(PARROT_SRC)

MANIFEST: always rakudo/MANIFEST parrot/MANIFEST nqp/MANIFEST
	git ls-files >MANIFEST
	$(PREFIX) rakudo/ rakudo/MANIFEST >>MANIFEST
	$(PREFIX) nqp/ nqp/MANIFEST >>MANIFEST
	cut -d' ' -f1 parrot/MANIFEST | $(PREFIX) parrot/ >>MANIFEST
	git submodule foreach --quiet 'git ls-files | $(PREFIX) $$path/' >>MANIFEST