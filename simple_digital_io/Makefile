MMCU=msp430f5529
MSP430_TI=/Applications/ti/ccsv6/ccs_base/msp430
.PHONY: all
all: main.elf

main.ll: main.rs
	rustc -C debuginfo=2 --emit llvm-ir -O -o $@ $^

main.s: main.ll
	llc-3.8 -O=0 -cppgen=program -march=msp430 -debugger-tune=gdb -internalize-public-api-file=main.ll -o $@ $^


main.o: main.s
	msp430-elf-as -g -mmcu=$(MMCU) -mcpu=msp430 -o $@ $^

main.elf: main.o
	msp430-elf-gcc -g -mmcu=$(MMCU) -mcpu=msp430 -L$(MSP430_TI)/include_gcc -I$(MSP430_TI)/include -L$(MSP430_TI)/include -e main -o $@ $^

.PHONY: flash
flash:
	mspdebug rf2500 'prog main.elf'

.PHONY: clean
clean:
	-rm -f main.ll main.s main.o main.elf
