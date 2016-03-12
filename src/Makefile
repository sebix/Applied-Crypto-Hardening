filelist=acknowledgements.md abstract.md introduction.md practical_settings.md practical_settings/webserver.md practical_settings/mailserver.md practical_settings/vpn.md practical_settings/GPG.md practical_settings/ipmi.md practical_settings/im.md practical_settings/DBs.md practical_settings/proxy_solutions.md practical_settings/kerberos.md theory/overview.md theory/cipher_suites.md theory/cipher_suites/architecture.md theory/cipher_suites/forward.md theory/cipher_suites/recommended.md theory/cipher_suites/compatibility.md theory/RNGs.md theory/keylengths.md theory/ECC.md theory/SHA.md theory/DH.md theory/PKIs.md theory/TLS.md theory/tls/HSTS.md theory/tls/HPKP.md tools.md links.md suggested_reading.md cipher_suite_names.md suite_names.md further_research.md footnotes.md
pandoc ?= pandoc
pandocopts = -f markdown_mmd+citations+yaml_metadata_block+header_attributes+implicit_header_references --table-of-contents -N --bibliography=applied-crypto-hardening.yaml --filter $(pandoc)-citeproc
# --csl=din-1505-2-alphanumeric.csl
tmpfile = applied-crypto-hardening/applied-crypto-hardening.md
texfile = applied-crypto-hardening.tex

TEXMFHOME ?={./texmf,$(shell kpsewhich -var-value TEXMFHOME)}
LATEX = TEXMFHOME=${TEXMFHOME} pdflatex -synctex=1

all:	pdf html

applied-crypto-hardening.yaml: all-rfcs.bib  applied-crypto-hardening.bib  iacr_cryptodb.bib  security.bib
	$(pandoc)-citeproc --bib2yaml applied-crypto-hardening.bib security.bib > applied-crypto-hardening.yaml

metadata:
	./update-metadata-for-gitinfo

concatenate:
	mkdir -p applied-crypto-hardening
	cat $(filelist) > $(tmpfile)

pdf: concatenate applied-crypto-hardening.yaml metadata
	sed -i 's/logo.svg/logo.pdf/' $(tmpfile)
	$(pandoc) $(tmpfile) metadata.yaml $(pandocopts) --template=templates/default.latex --listings -o $(texfile)
	sed -i 's/\\begin{itemize}/\\begin{itemize*}/' $(texfile)
	sed -i 's/\\end{itemize}/\\end{itemize*}/' $(texfile)
	sed -i '/\\tightlist/d' $(texfile)
	sed -i 's/≥/$$\\ge$$/' $(texfile)
	${LATEX} applied-crypto-hardening
	bibtex applied-crypto-hardening
	makeglossaries applied-crypto-hardening
	${LATEX} applied-crypto-hardening
	# re-do for refs
	${LATEX} applied-crypto-hardening
	while grep -s "Rerun to get cross-references right" \
		applied-crypto-hardening.log ; do \
		${LATEX} applied-crypto-hardening ; \
	done

html: metadata applied-crypto-hardening.yaml
	$(pandoc) $(filelist) metadata.yaml $(pandocopts) --template=templates/default.html5 -s -t html5 -o applied-crypto-hardening.html

txt:
	echo "Not supported yet"

epub:
	echo "Not supported yet"

upload:
	rsync -avz --progress --no-p --no-g --no-t applied-crypto-hardening.pdf www.bettercrypto.org:/var/www/bettercrypto.org/static
	rsync -avz --progress --no-p --no-g --no-t configuration www.bettercrypto.org:/var/www/bettercrypto.org/static

www:	pdf upload

clean:
	rm -rf gitHeadInfo.gin
