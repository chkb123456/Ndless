CFLAGS := -mcpu=arm926ej-s -std=c11 -nostdlib -O3 -fPIE -mlong-calls -Wall -Werror -I ../include/ -I ../thirdparty/nspire-io/include/ -D_TINSPIRE -ffunction-sections -fdata-sections
CXXFLAGS := -mcpu=arm926ej-s -std=c++11 -nostdlib -O3 -fPIE -fno-exceptions -fno-rtti -mlong-calls -Wall -Werror -I ../include/ -I ../thirdparty/nspire-io/include/ -D_TINSPIRE -ffunction-sections -fdata-sections

AR_FILE := ../lib/libsyscalls.a
AR_FILE_NSPIREIO := ../lib/libsyscalls_nspireio.a
O_FILES := stubs.o osvar.o realpath.o
H_FILE := ../include/syscall-decls.h

all: $(AR_FILE) $(AR_FILE_NSPIREIO)

$(AR_FILE): $(O_FILES) stdlib.o
	arm-none-eabi-ar rcs $@ $^

$(AR_FILE_NSPIREIO): $(O_FILES) stdlib_nspireio.o
	arm-none-eabi-ar rcs $@ $^
	
%.o: %.cpp
	arm-none-eabi-g++ $(CXXFLAGS) -c $^ -o $@

%.o: %.c
	arm-none-eabi-gcc $(CFLAGS) -c $^ -o $@

stdlib_nspireio.o: stdlib.cpp
	arm-none-eabi-g++ $(CXXFLAGS) -D USE_NSPIREIO -c $^ -o $@
	
stubs.cpp: ../include/syscall-list.h mkStubs.php
	php mkStubs.php > $(H_FILE)
	
clean:
	rm -f $(AR_FILE) $(AR_FILE_NSPIREIO) $(O_FILES) stdlib*.o
