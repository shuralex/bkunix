ETC		= etc/init etc/glob etc/mknod etc/mkfs etc/fsck
BIN		= bin/sh bin/ls bin/echo bin/cal bin/cp bin/date bin/mkdir \
		  bin/sync bin/mv bin/rm bin/rmdir bin/stty bin/od bin/ed \
		  bin/cat bin/ln bin/wc bin/pwd bin/df bin/mount bin/umount \
		  bin/clock bin/halt
SYS		= sys/bkunix sys/boot
STAND		= stand/fdfmt

DEV		= dev/tty0!c0:0 dev/fd0!b0:0 dev/fd1!b0:1
DSK		= root.bkd
FSUTIL		= u6-fsutil -F

all:		$(DSK)
#		$(FSUTIL) -v root.bkd

build:		$(SYS) $(ETC) $(BIN) $(STAND)

clean:
		rm -f *~ $(DSK) bkunix
		$(MAKE) -C bin clean
		$(MAKE) -C etc clean
		$(MAKE) -C sys clean
		$(MAKE) -C libc clean
		$(MAKE) -C libfs clean
		$(MAKE) -C stand clean

root.bkd:	remake
		pdp11-strip $(ETC) $(BIN)
		dd bs=512 count=1600 < /dev/zero > $@
		$(FSUTIL) -n -s753664 -bsys/boot $@
		cp sys/bkunix stand/fdfmt .
		$(FSUTIL) -a $@ bkunix fdfmt usr/ tmp/
		@rm -f bkunix fdfmt
		$(FSUTIL) -a $@ etc/ $(ETC)
		$(FSUTIL) -a $@ bin/ $(BIN)
		$(FSUTIL) -a $@ dev/ $(DEV)

remake:
		$(MAKE) -C sys
		$(MAKE) -C bin
		$(MAKE) -C etc
		$(MAKE) -C stand

run:            $(DSK)
		bk -3 -B $(DSK)

$(SYS):
		$(MAKE) -C sys

$(BIN):
		$(MAKE) -C bin

$(ETC):
		$(MAKE) -C etc

$(STAND):
		$(MAKE) -C stand
