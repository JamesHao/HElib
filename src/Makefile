# Copyright (C) 2012-2017 IBM Corp.
#
# This program is Licensed under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance
# with the License. You may obtain a copy of the License at
#   http://www.apache.org/licenses/LICENSE-2.0
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License. See accompanying LICENSE file.
# 
CC = g++
LD = g++
AR = ar
ARFLAGS=rv
GMP=-lgmp
NTL=-lntl
COPT=-g -O2

# INC and LIB changed to when GMP and/or NTL not in default locations
# Put these definitions in the file local-defs, which will be included
# if found.  ====> PUT ONLY *ABSOLUTE PATHS* IN local-defs <====
# You can also overwrite COPT in this local file.
# See local-def.example for an example.
include $(wildcard local-defs)

#
CFLAGS = -std=c++11 -pthread $(COPT) $(INC_NTL) $(INC_GMP) -DFHE_THREADS -fmax-errors=4  

# NOTE: NTL and GMP are distributed under LGPL (v2.1), so you can link
#       against them as dynamic libraries.
LDLIBS = $(LIB_NTL) $(LIB_GMP) $(NTL) $(GMP) -lm


# useful flags:
#   -DFHE_THREADS  tells helib to enable generic multithreading capabilities;
#                  must be used with a thread-enabled NTL and the -pthread
#                  flag should be passed to gcc
#
#   -DFHE_BOOT_THREADS  tells helib to use a multithreading strategy for
#                       bootstrapping; requires -DFHE_THREADS (see above)
#
#  If you get compilation errors, you may need to add -std=c++11

$(info HElib requires NTL version 10.0.0 or higher, see http://shoup.net/ntl)
$(info )

HEADER = EncryptedArray.h FHE.h Ctxt.h CModulus.h FHEContext.h PAlgebra.h DoubleCRT.h NumbTh.h bluestein.h IndexSet.h timing.h IndexMap.h replicate.h hypercube.h matching.h powerful.h permutations.h polyEval.h multicore.h EvalMap.h matmul.h PtrVector.h PtrMatrix.h intraSlot.h recryption.h debugging.h binaryArith.h binaryCompare.h tableLookup.h binio.h sample.h norms.h zzX.h primeChain.h PGFFT.h fhe_stats.h ArgMap.h

SRC = KeySwitching.cpp EncryptedArray.cpp EaCx.cpp FHE.cpp Ctxt.cpp CModulus.cpp FHEContext.cpp PAlgebra.cpp DoubleCRT.cpp NumbTh.cpp bluestein.cpp IndexSet.cpp timing.cpp replicate.cpp hypercube.cpp matching.cpp powerful.cpp BenesNetwork.cpp permutations.cpp PermNetwork.cpp OptimizePermutations.cpp eqtesting.cpp polyEval.cpp extractDigits.cpp EvalMap.cpp recryption.cpp debugging.cpp matmul.cpp intraSlot.cpp binaryArith.cpp binaryCompare.cpp tableLookup.cpp binio.cpp sample.cpp norms.cpp zzX.cpp fft.cpp primeChain.cpp PGFFT.cpp fhe_stats.cpp ArgMap.cpp

OBJ = NumbTh.o timing.o bluestein.o PAlgebra.o  CModulus.o FHEContext.o IndexSet.o DoubleCRT.o FHE.o KeySwitching.o Ctxt.o EncryptedArray.o EaCx.o replicate.o hypercube.o matching.o powerful.o BenesNetwork.o permutations.o PermNetwork.o OptimizePermutations.o eqtesting.o polyEval.o extractDigits.o EvalMap.o recryption.o debugging.o matmul.o intraSlot.o tableLookup.o binio.o sample.o norms.o zzX.o fft.o primeChain.o binaryArith.o binaryCompare.o PGFFT.o fhe_stats.o ArgMap.o

TESTPROGS = Test_General_x Test_PAlgebra_x Test_IO_x Test_Bin_IO_x Test_Replicate_x Test_matmul_x Test_Powerful_x Test_Permutations_x Test_Timing_x Test_PolyEval_x Test_extractDigits_x Test_EvalMap_x Test_ThinEvalMap_x Test_bootstrapping_x Test_ThinBootstrapping_x Test_PtrVector_x Test_intraSlot_x Test_binaryArith_x Test_binaryCompare_x Test_tableLookup_x Test_approxNums_x Test_fatboot_x Test_thinboot_x

all: fhe.a

# if makedep.d exists in the current directory, include it
include $(wildcard makedep.d)

check:
	$(MAKE) check_General
	$(MAKE) check_matmul
	$(MAKE) check_Permutations
	$(MAKE) check_PolyEval
	$(MAKE) check_Replicate
	$(MAKE) check_EvalMap
	$(MAKE) check_extractDigits
	$(MAKE) check_fatboot
	$(MAKE) check_thinboot
	$(MAKE) check_binaryArith
	$(MAKE) check_binaryCompare
	$(MAKE) check_tableLookup
	$(MAKE) check_Bin_IO
	$(MAKE) check_approxNums

check_General: Test_General_x 
	./Test_General_x R=1 k=10 p=2 r=2 noPrint=1
	./Test_General_x R=1 k=10 p=2 d=2 noPrint=1
	./Test_General_x R=2 k=10 p=7 r=2 noPrint=1

check_matmul: Test_matmul_x 
	./Test_matmul_x m=18631 L=300 
	./Test_matmul_x block=1 m=24295 gens="[16386 16427]" ords="[42 16]" L=300

check_Permutations: Test_Permutations_x 
	./Test_Permutations_x noPrint=1

check_PolyEval: Test_PolyEval_x 
	./Test_PolyEval_x p=7 r=2 d=34 noPrint=1

check_Replicate: Test_Replicate_x 
	./Test_Replicate_x m=1247 noPrint=1

check_EvalMap: Test_EvalMap_x 
	./Test_EvalMap_x mvec="[7 3 221]" gens="[3979 3095 3760]" ords="[6 2 -8]" noPrint=1

check_ThinEvalMap: Test_ThinEvalMap_x 
	./Test_ThinEvalMap_x mvec="[7 3 221]" gens="[3979 3095 3760]" ords="[6 2 -8]" noPrint=1

check_extractDigits: Test_extractDigits_x 
	./Test_extractDigits_x m=2047 p=5 noPrint=1

check_fatboot: Test_fatboot_x 
	./Test_fatboot_x mvec="[31 41]" gens="[1026 249]" ords="[30 -2]" noPrint=1
	./Test_fatboot_x  p=17 mvec="[7 5 37]" gens="[556 1037]" ords="[6 4]" noPrint=1

check_thinboot: Test_thinboot_x 
	./Test_thinboot_x mvec="[31 41]" gens="[1026 249]" ords="[30 -2]" noPrint=1
	./Test_thinboot_x  p=17 mvec="[7 5 37]" gens="[556 1037]" ords="[6 4]" noPrint=1

check_binaryArith: Test_binaryArith_x 
	./Test_binaryArith_x

check_binaryCompare: Test_binaryCompare_x 
	./Test_binaryCompare_x

check_tableLookup: Test_tableLookup_x
	./Test_tableLookup_x

check_Bin_IO: Test_Bin_IO_x
	./Test_Bin_IO_x p=2 r=2 m=127
	./Test_Bin_IO_x p=257 m=127

check_approxNums: Test_approxNums_x
	./Test_approxNums_x m=1024 r=8 ep=0.01

# Run all the test programs with tiny parameters,
# just to check that they all compile and run.
test: $(TESTPROGS)
	./Test_PAlgebra_x m=91
	./Test_General_x noPrint=1 m=91
	./Test_matmul_x m=91 gens='[9 3]' ords='[3 -2]'
	./Test_Permutations_x noPrint=1 m=91
	./Test_PolyEval_x noPrint=1 m=91
	./Test_Replicate_x noPrint=1 m=91
	./Test_EvalMap_x noPrint=1 mvec='[3 35]' gens='[71 76]' ords='[2 2]'
	./Test_ThinEvalMap_x noPrint=1 mvec='[3 35]' gens='[71 76]' ords='[2 2]'
	./Test_extractDigits_x noPrint=1 m=91
	./Test_fatboot_x noPrint=1 mvec="[31 41]" gens="[1026 249]" ords="[30 -2]"
	./Test_thinboot_x noPrint=1 mvec="[31 41]" gens="[1026 249]" ords="[30 -2]"
	./Test_binaryArith_x prm=0
	./Test_binaryCompare_x prm=0
	./Test_tableLookup_x prm=0
	./Test_IO_x m=91
	./Test_Bin_IO_x m=91
	./Test_approxNums_x m=128
	./Test_Powerful_x m1=3 m2=5 m3=7
	./Test_intraSlot_x m=91
	./Test_PtrVector_x
	./Test_Timing_x m=91 high=1

obj: $(OBJ)


#extractDigits.o: extractDigits.cpp
#	$(CC) $(CFLAGS) -DDEBUG_PRINTOUT -c extractDigits.cpp
#
#recryption.o: recryption.cpp
#	$(CC) $(CFLAGS) -DDEBUG_PRINTOUT -c recryption.cpp

PGFFT.o: PGFFT.cpp
	$(CC) $(CFLAGS) -march=native -c PGFFT.cpp


%.o: %.cpp
	$(CC) $(CFLAGS) -c $<

fhe.a: $(OBJ)
	$(AR) $(ARFLAGS) fhe.a $(OBJ)
	g++ $(INC_GMP) $(INC_NTL) -std=gnu++11 -MM *.cpp > makedep.d

./%_x: %.cpp fhe.a
	$(CC) $(CFLAGS) -o $@ $< fhe.a $(LDLIBS)


clean:
	rm -f *.d *.o *_x *_x.exe *.a core.*
	rm -rf *.dSYM

# old-style make depend (FIXME: change to modern style?)
makedep:
	g++ $(INC_GMP) $(INC_NTL) -std=gnu++11 -MM *.cpp > makedep.d

info:
	: HElib require NTL version 10.0.0 or higher
	: Compilation flags are 'CFLAGS=$(CFLAGS)'
	: If errors occur, try adding '-std=c++11' in Makefile
	:


