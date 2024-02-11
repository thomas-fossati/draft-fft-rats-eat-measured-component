%.cbor: %.diag ; diag2cbor.rb $< > $@
%.cddl: %.cddlc ; cddlc -2tcddl $< > $@

CDDL := measured-component.cddl
DIAG_EXAMPLES := $(wildcard *.diag)
CBOR_EXAMPLES := $(DIAG_EXAMPLES:.diag=.cbor)

all: check-schema check-examples

check-schema: $(CDDL) ; cddl $< g 10
.PHONY: check-schema

clean: ; -rm -f $(CDDL) $(wildcard *.pretty) $(CBOR_EXAMPLES)
.PHONY: clean

check-examples: $(CBOR_EXAMPLES) $(CDDL)
	@for f in $(CBOR_EXAMPLES); do \
		echo ">> validating $$f against $(CDDL)" ; \
		cddl $(CDDL) validate $$f &>/dev/null || exit 1 ; \
		echo ">> saving prettified CBOR to $${f%.cbor}.pretty" ; \
		cbor2pretty.rb $$f > $${f%.cbor}.pretty ; \
	done
.PHONY: check-examples
