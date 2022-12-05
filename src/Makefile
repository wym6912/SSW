CC = gcc
CXX = g++
DEBUG = 0
ifeq ($(DEBUG), 0)
CFLAGS += -Wall -pipe -O2
else
CFLAGS += -Wall -pipe -g -fsanitize=address	# for debug
endif
CXXFLAGS := $(CFLAGS)
LOBJS = ssw.o
LCPPOBJS = ssw_cpp.o
PROG = ssw_test
LIB = libssw.so
EXAMPLE = example_c
EXAMPLE_CPP = example_cpp

.PHONY: all default java clean

default: $(PROG) $(EXAMPLE) $(EXAMPLE_CPP) $(LIB) 
core: $(PROG)
java: java_home $(JAVA_JAR) $(JAVA_LIB)
all: default java

$(LIB): ssw.c ssw.h
	$(CC) $(CFLAGS) -fPIC -shared -rdynamic -o $@ $<

$(PROG): main.c kseq.h

$(EXAMPLE): example.c

ifdef __arm__ # (M1)
$(PROG) $(EXAMPLE): $(LOBJS)
	$(CC) -o $@ $(filter-out %.h,$^) $(CFLAGS) $(LDFLAGS) -lm -lz -march=armv8-a+fp+simd+crypto+crc
else # x86(Intel)
$(PROG) $(EXAMPLE): $(LOBJS)
	$(CC) -o $@ $(filter-out %.h,$^) $(CFLAGS) $(LDFLAGS) -lm -lz
endif

$(EXAMPLE_CPP): example.cpp $(LOBJS) $(LCPPOBJS)
	$(CXX) -o $@ $^ $(CXXFLAGS) $(LDFLAGS) -lm -lz

ssw.o: ssw.c ssw.h
	$(CC) -c -o $@ $< $(CFLAGS)

ssw_cpp.o: ssw_cpp.cpp ssw_cpp.hpp ssw.h
	$(CXX) -c -o $@ $< $(CXXFLAGS)

clean:
	-rm -f $(LOBJS) $(LCPPOBJS) $(PROG) $(LIB) $(EXAMPLE) $(EXAMPLE_CPP) *~
