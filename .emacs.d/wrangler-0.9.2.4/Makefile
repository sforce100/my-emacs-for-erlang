PACKAGE := wrangler
VERSION := 0.9.2.4

prefix      = /usr/local
exec_prefix = ${prefix}
datarootdir = ${prefix}/share
datadir     = ${datarootdir}
erl         = /opt/erl/bin/erl
erlc        = /opt/erl/bin/erlc

ELISP_DIR = ${datadir}/wrangler/elisp
EBIN_DIR = ${datadir}/wrangler/ebin
BIN_DIR = ${datadir}/wrangler/bin
ERL_SRC_DIR = ${datadir}/wrangler/src
WRANGLER_DIR = ${datadir}/wrangler
PLT_DIR = ${datadir}/wrangler/plt
########################################
## Main part

ERL_SRC := $(wildcard src/*.erl)
ERL_OBJ := $(patsubst src/%.erl,ebin/%.beam,${ERL_SRC})

SRC := ${ERL_SRC} 


ELISP_WRANGLER_OBJ := elisp/wrangler.el
INCLUDE_OBJ :=include/wrangler.hrl


OBJECTS := ${ERL_OBJ} ${C_OBJ} ${ELISP_WRANGLER_OBJ} ${INCLUDE_OBJ}

default: prepare c ${ERL_OBJ} ${C_OBJ} 
base: ${ERL_OBJ} ${C_OBJ}
all: prepare base c 

erl: ${ERL_OBJ} 

########################################
## Rules

## Erlang
ebin/%.beam: ${SRC}  
	${erlc} -I include -W -o ebin +debug_info ${SRC} 


## this is ugly code.
prepare:
	@case `uname` in \
          CYGWIN*) \
              sed -e "s;%WRANGLER_DIR%;\"`cygpath -m ${WRANGLER_DIR}`\";" \
                  include/wrangler.hrl.src>include/wrangler.hrl  \
              ;; \
              *) \
              sed -e "s;%WRANGLER_DIR%;\"${WRANGLER_DIR}\";" \
                  include/wrangler.hrl.src>include/wrangler.hrl \
              ;; \
         esac
	@case `uname` in \
          CYGWIN*) \
              sed -e "s;%EBIN_DIR%;\"`cygpath -m ${EBIN_DIR}`\";" \
                  elisp/wrangler.el.src>elisp/wrangler.el  \
              ;; \
              *) \
              sed -e "s;%EBIN_DIR%;\"${EBIN_DIR}\";" \
                  elisp/wrangler.el.src>elisp/wrangler.el \
             ;; \
         esac
c:
	cd ./c_src; make; cd ../..

########################################


Makefile: Makefile.in config.status
	./config.status

clean:
	cd ./c_src; make clean; cd ../..	
	-rm -f ${OBJECTS} 2>/dev/null
	-rm -f ${BIN_DIR}/* 2>/dev/null

distclean: clean
	-rm -f ./c_src/Makefile *~
	-rm -f config.* configure.scan Makefile *~ */*~ 2>/dev/null

install: base
	@echo "* Installing Emacs Lisp Library"
	install -m 775 -d ${ELISP_DIR} 
	install -m 775 elisp/*.el ${ELISP_DIR}
	@echo
	@echo "* Installing Erlang Library"
	install -m 775 -d ${EBIN_DIR} ${ERL_SRC_DIR}
	install -m 775 -d ${BIN_DIR}
	install -m 775 -d ${PLT_DIR}
	install -m 775 ebin/*.beam ${EBIN_DIR}
	install -m 775 src/*.erl ${ERL_SRC_DIR}
	install -m 775 ebin/*.app ${EBIN_DIR}
	install -m 775 c_src/suffixtree/suffixtree* ${BIN_DIR}
	install -m 755 c_src/gsuffixtree/gsuffixtree* ${BIN_DIR}
	install -m 775 include/*.hrl ${ERL_SRC_DIR}
	install -m 775 plt/side_effect_plt ${PLT_DIR}
	install -m 775 plt/dialyzer_plt ${PLT_DIR}
	@echo
	@echo "*** Successfully installed. See README for usage instructions."
	@echo



wc:
	@echo "* Emacs Lisp"
	@wc -l */*.el | sort -nr
	@echo "* Erlang"
	@wc -l */*.erl | sort -nr
	@echo "* C"
	@wc -l */*.c | sort -nr

.PHONY: always
 
