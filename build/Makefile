PYTHON_VERSION = 2.7.10
VIRTUALENV_BIN = $(shell python -c "import sys;print('virtualenv'+('' if sys.version_info.major==2 else '2'))")

SPHINX_FOR_2.7.8 = 1.1.3
SPHINX_FOR_2.7.10 = 1.3.3
SPHINX_VERSION = $(SPHINX_FOR_$(PYTHON_VERSION))
sphinx_bin = sphinx_env_$(SPHINX_VERSION)/bin
sphinx_build = $(sphinx_bin)/sphinx-build

default: python.info

Python-$(PYTHON_VERSION).tgz:
	wget -O $@ http://www.python.org/ftp/python/$(PYTHON_VERSION)/Python-$(PYTHON_VERSION).tgz

Python-$(PYTHON_VERSION): Python-$(PYTHON_VERSION).tgz
	tar -xzf $<
	# based on http://stackoverflow.com/a/3952588
	echo "texinfo_documents = [('contents', 'python', 'Python Documentation', 'Georg Brandl', 'Python', 'The Python Programming Language', 'Documentation tools', 1)]" >> $@/Doc/conf.py

sphinx_env_$(SPHINX_VERSION):
	$(VIRTUALENV_BIN) $@

$(sphinx_build): sphinx_env_$(SPHINX_VERSION)
	$(sphinx_bin)/pip install sphinx==$(SPHINX_VERSION)

python_info/python.texi: Python-$(PYTHON_VERSION) $(sphinx_build)
	$(sphinx_build) -b texinfo Python-$(PYTHON_VERSION)/Doc python_info

python.texi: python_info/python.texi Makefile
	sed -e 's/@dircategory Documentation tools/@dircategory Programming/g' \
		python_info/python.texi > $@

python.info: python.texi
	makeinfo --no-split python.texi

clean:
	rm -f Python-*.tgz
	rm -rf Python-*
	rm -rf python_info
	rm -f python.texi

realclean: clean
	rm -rf sphinx_env_*

install: python.info
	cp python.info /usr/share/info
	install-info --info-dir=/usr/share/info python.info
