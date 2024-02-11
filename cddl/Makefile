%.cddl: %.cddlc ; cddlc -2tcddl $< > $@

CDDL := measured-component.cddl

all: check

check: $(CDDL) ; cddl $< g 10
.PHONY: check

clean: ; -rm -f $(CDDL)
