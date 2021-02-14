###############################################################################
## Makefile for Croatian to Serbian
###############################################################################

LANG1=hbs
LANG1b=hbs_HR
LANG1c=hbs_SR
PREFIX1=$(LANG1b)-$(LANG1c)
PREFIX2=$(LANG1c)-$(LANG1b)
BASENAME=apertium-$(LANG1)

TARGETS_COMMON =		\
	$(LANG1).automorf.bin	\
	$(LANG1).rlx.bin	\
	$(PREFIX1).autobil.bin	\
	$(LANG1b).autogen.bin	\
	$(LANG1b).autopgen.bin	\
	$(PREFIX1).t1x.bin	\
	$(PREFIX2).automorf.bin	\
	$(LANG1).prob		\
	$(PREFIX2).autobil.bin	\
	$(LANG1c).autogen.bin	\
	$(LANG1c).autopgen.bin	\
	$(PREFIX2).t1x.bin	\
	cyr-lat.automorf.bin	\
	lat-cyr.automorf.bin

# This include defines goals for install-modes, .deps/.d, autobil.prefixes and .mode files:
@ap_include@

###############################################################################
## Transducers
###############################################################################

###Croatian and Serbian analysers

$(LANG1).automorf.bin: $(AP_SRC1)/$(LANG1).automorf.bin 
	cp $< $@


## Croatian generator
$(LANG1b).autogen.bin: $(AP_SRC1)/$(LANG1b).autogen.bin
	cp $< $@

## Serbian generator
$(LANG1c).autogen.bin: $(AP_SRC1)/$(LANG1c).autogen.bin
	cp $< $@


###############################################################################
## Tagger models
###############################################################################

$(LANG1).prob: $(AP_SRC1)/$(LANG1).prob
	cp $< $@

###############################################################################
## Disambiguation rules
###############################################################################


$(LANG1).rlx.bin: $(AP_SRC1)/apertium-$(LANG1).$(LANG1).rlx
	cg-comp $< $@


###############################################################################
## Bilingual dictionaries
###############################################################################

$(PREFIX1).autobil.bin: $(BASENAME).$(PREFIX1).dix
	apertium-validate-dictionary $<
	lt-comp lr $< $@

$(PREFIX2).autobil.bin: $(BASENAME).$(PREFIX1).dix
	apertium-validate-dictionary $<
	lt-comp rl $< $@

###############################################################################
## Structural transfer rules
###############################################################################


$(PREFIX1).t1x.bin: $(BASENAME).$(PREFIX1).t1x
	apertium-validate-transfer $<
	apertium-preprocess-transfer $< $@

$(PREFIX2).t1x.bin: $(BASENAME).$(PREFIX2).t1x
	apertium-validate-transfer $<
	apertium-preprocess-transfer $< $@

###############################################################################
## Postgeneration dictionaries
###############################################################################

$(LANG1b).autopgen.bin: $(AP_SRC1)/$(LANG1b).autopgen.bin
	cp $< $@

$(LANG1c).autopgen.bin: $(AP_SRC1)/$(LANG1c).autopgen.bin
	cp $< $@

###############################################################################
## Alphabet transliteration 
###############################################################################

lat-cyr.autogen.bin: $(AP_SRC1)/$(BASENAME).Cyrl-Latn.dix
	if [ ! -d .deps ]; then mkdir .deps; fi
	apertium-validate-dictionary $<
	lt-comp rl $< $@

cyr-lat.automorf.bin: $(AP_SRC1)/$(BASENAME).Cyrl-Latn.dix
	if [ ! -d .deps ]; then mkdir .deps; fi
	apertium-validate-dictionary $<
	lt-comp lr $< $@

$(LANG1b)-$(LANG1c)cyr.mode:
	apertium-gen-modes modes.xml
	cp *.mode modes/


$(LANG1c)cyr-$(LANG1b).mode:
	apertium-gen-modes modes.xml
	cp *.mode modes/

###############################################################################
## Distribution
###############################################################################

EXTRA_DIST=					\
		$(BASENAME).$(PREFIX1).dix	\
		$(BASENAME).$(PREFIX1).t1x	\
		$(BASENAME).$(PREFIX2).t1x	\
		expand-to-dix.py		\
		gen-bidix.sh			\
		alt.xsl				\
		modes.xml

###############################################################################
## Installation
###############################################################################

apertium_hbs_HR_hbs_SRdir=$(prefix)/share/apertium/$(BASENAME)/

# Files which should be installed to $(prefix)/share/apertium/$(BASENAME)/:
apertium_hbs_DATA=	\
	$(TARGETS_COMMON)				\
	$(BASENAME).$(PREFIX1).t1x			\
	$(BASENAME).$(PREFIX2).t1x


# All modes are created by the same goal, listing several will just give problems with make -j2
noinst_DATA=modes/$(PREFIX1).mode \
	.deps/$(LANG1).automorf.bin

install-data-local: install-modes

###############################################################################
## Cleanup
###############################################################################

CLEANFILES = $(TARGETS_COMMON)
clean-local:
	-rm -rf .deps modes

###############################################################################
## Test
###############################################################################

test: all
	echo "house" | apertium -d . -f line hbs_HR-hbs_SR | tee .test-mt
	@echo "house" > .test-ref
	@diff .test-ref .test-mt
	echo "house" | apertium -d . -f line hbs_SR-hbs_HR | tee .test-mt
	@echo "house" > .test-ref
	@diff .test-ref .test-mt
	@rm .test-ref .test-mt
