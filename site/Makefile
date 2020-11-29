STAGE_DIR = $(abspath .)/stage

PAGES = $(wildcard *.md)
STAGE_PAGES = $(patsubst %.md,$(STAGE_DIR)/%.html,$(PAGES))

INCLUDE=$(wildcard include/*)
STAGE_INCLUDE=$(patsubst include/%,$(STAGE_DIR)/%,$(INCLUDE))

TARGET_DIR = /cse/web/homes/vegaluis
PUBS_DIR = $(HOME)/z/pubs
CV_FILE = $(HOME)/z/luisvega-cv.pdf

default: stage sync

.PHONY: sync
sync:
	rsync \
		--verbose \
		--recursive \
		--copy-dirlinks \
		--delete \
		--force \
		stage/ recycle:$(TARGET_DIR)

.PHONY: stage
stage: $(STAGE_DIR) $(STAGE_PAGES) $(STAGE_INCLUDE) $(STAGE_DIR)/luisvega-cv.pdf $(STAGE_DIR)/pubs

$(STAGE_DIR)/%.html: %.md TEMPLATE.html
	@pandoc \
		--from=markdown \
		--to=html5 \
		--template=TEMPLATE.html \
		--standalone \
		--fail-if-warnings \
		$< REFLINKS --output=$@

$(STAGE_DIR)/%: include/%
	cp -r $< $@

$(STAGE_DIR)/pubs: $(PUBS_DIR)
	cp -r $< $@

$(STAGE_DIR)/luisvega-cv.pdf: $(CV_FILE)
	$(MAKE) --directory ../cv luisvega-cv.pdf
	cp ../cv/luisvega-cv.pdf $@

$(STAGE_DIR):
	mkdir -p $@

.PHONY: check
check: stage
	html5validator \
	  --root stage/ \
	  --also-check-css \
	  --show-warnings \
	  --format text 2>&1 \
	| tr -d "'" \
	| awk 'BEGIN {RS=","; ORS="\n"} /Error/ || /Warning/ {print "\n"} {print $$0}'

clean:
	-rm -rf $(STAGE_DIR)
