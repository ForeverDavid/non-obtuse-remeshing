SHELL = /bin/sh

OOQP=../../..
OOQPINCLUDEDIR=$(OOQP)/include
OOQPLIBDIR=$(OOQP)/lib

CC       = cc
CXX      = c++

CFLAGS   =-O 
CXXFLAGS =-O 
CPPFLAGS =-I$(OOQPINCLUDEDIR)
LDFLAGS  =-L$(OOQPLIBDIR)
BLAS     = $(OOQP)/extras/blas/libblas.a
MA27LIB  = $(OOQP)/extras/MA27/libMA27.a
FLIBS    = -lg2c
SPARSE_LIBS     = -looqpgensparse -looqpsparse  -looqpgondzio -looqpbase \
                $(BLAS) $(MA27LIB) $(FLIBS)
DENSE_LIBS     = -looqpgendense -looqpdense  -looqpgondzio -looqpbase \
                $(BLAS) $(FLIBS)

all: mps.exe example.exe dense_example.exe

mps.exe : mps.o
	$(CXX) $(CPPFLAGS) $(CFLAGS) $(LDFLAGS) -o $@ $^ $(SPARSE_LIBS)

example.exe : example.o
	$(CXX) $(CPPFLAGS) $(CFLAGS) $(LDFLAGS) -o $@ $^ $(SPARSE_LIBS)

dense_example.exe : dense_example.o
	$(CXX) $(CPPFLAGS) $(CFLAGS) $(LDFLAGS) -o $@ $^ $(DENSE_LIBS)

clean:
	rm -f *.o

veryclean: clean
	rm -f *.exe

distclean: veryclean

.c.o:
	$(CC) -c $(CPPFLAGS) $(CFLAGS) -o $@ $<
