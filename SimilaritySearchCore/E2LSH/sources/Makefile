# $Header: Makefile,v 1.3 95/08/17 14:14:17 hmgr Exp $
#This make file build the library libLSH.
#Done by Jun Li.
#Changes:
#add -ltr flag to enable clock_monotic functions to execute. Done By Tere Gonzalez, Feb 20, 2014
#add -ltcmalloc flag to enable memory allocator of tcmalloc to replace the built-in C++ memory allocation. Jun Li. May 29, 2014
#add -DPERF_MONITOR flag to allow monitoring of perf-based tool, Jun Li, July 24, 2014.
GCC=/opt/gcc/bin/g++

##DEFINED_FLAGS   = -g -DREAL_FLOAT -DDEBUG -O3 -fPIC
#DEFINED_FLAGS   = -w -fPIC -DREAL_FLOAT -DPERF_MONITOR -DSPLIT_TIME_MEASURE -O3
#DEFINED_FLAGS   = -w -fPIC -DREAL_FLOAT -DDESERIALIZE_CANDIDATES -O3  -funsafe-math-optimizations
#DEFINED_FLAGS   = -w -fPIC -DREAL_FLOAT -DDESERIALIZE_CANDIDATES -O3
#DEFINED_FLAGS   = -w -fPIC -DREAL_FLOAT -DSERIALIZE_INPUT -O3
DEFINED_FLAGS   = -w -fPIC -DREAL_FLOAT -O3 -funsafe-math-optimizations
##we will need the additional includes later. 
##added for tcmalloc.Jun Li, June 10, 2014.
CXXINC  = -I/usr/local/include
CPLUSPLUS11_OPTIONS=-std=c++11
CXXOPTS = $(CPLUSPLUS11_OPTIONS) $(DEFINED_FLAGS) $(CXXINC)
CXX     = $(GCC)

# for the linking to produce an executable 
##LDOPTS  = -Wall,-w -L$(BOOCHLIB)
LDOPTS  = -fPIC -shared -Wl,-soname,$(SHARED_LIB_TO_BUILD) -ltcmalloc -lrt

SHARED_LIB_TO_BUILD =libLSH.so
OTHERLIBS = -lm -lblaze -lnuma 
CXXLIBS = $(LIB_TO_BUILD) $(OTHERLIBS)

##CFLAGS = $(CXXINC) -Ae -D_REENTRANT -g
SRCS	= BucketHashing.cpp Geometry.cpp GlobalVars.cpp \
	  QueryHashFunctions.cpp NaiveSearch.cpp \
	  HashIndex.cpp IndexBuilder.cpp IndexSearch.cpp \
	  LocalitySensitiveHashing.cpp LSHHashFunction.cpp \
	  LSHManager.cpp MergeResult.cpp \
	  NearNeighbors.cpp Random.cpp SearchCoordinator.cpp \
          SearchCoordinatorTracker.cpp SearchResult.cpp SelfTuning.cpp \
          TimeSeriesBuilder.cpp TimeSeries.cpp Util.cpp LSHtest.cpp 
 
LIB_OBJS    = $(SRCS:.cpp=.o)

.SUFFIXES: .cpp

.cpp.o :
	$(CXX) $(CXXOPTS) -c $<

##NOTE: this needs to be the first rule that is actionable. if we
##want to have "make" to assume "make all"
all : $(SHARED_LIB_TO_BUILD)


$(SHARED_LIB_TO_BUILD) :  $(LIB_OBJS)
	$(CXX) $(LDOPTS) -o $@ $(LIB_OBJS) $(OTHERLIBS)

clean :
	-rm -f core *.o 
	-rm -f $(SHARED_LIB_TO_BUILD)

clobber : clean
	-rm -f $(SHARED_LIB_TO_BUILD)
