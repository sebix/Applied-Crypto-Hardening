all: suite_names.md

install: all
	cp suite_names.md ../../src/

.DELETE_ON_ERROR:

suite_names.md: tls-parameters-4.csv openssl-ciphers.txt template.md buildsuites.pl
	perl buildsuites.pl template.md tls-parameters-4.csv openssl-ciphers.txt > $@

tls-parameters-4.csv:
	wget https://www.iana.org/assignments/tls-parameters/tls-parameters-4.csv

openssl-ciphers.txt:
	(openssl ciphers -V 'ALL:COMPLEMENTOFALL'; openssl version) > $@

