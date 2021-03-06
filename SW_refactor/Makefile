# Copyright (C) 2018 Kaan Kara - Systems Group, ETH Zurich

# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU Affero General Public License as published
# by the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.

# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU Affero General Public License for more details.

# You should have received a copy of the GNU Affero General Public License
# along with this program. If not, see <http://www.gnu.org/licenses/>.
#*************************************************************************

CXX			= g++
CPPFLAGS	= -O3 -std=c++11 -march=native -Wall -Wno-unused-variable -Wno-unused-but-set-variable -Wno-unused-function -Wno-reorder -Wno-narrowing #-DAVX2
LDFLAGS		= -lOSAL -lAAS
FPGA_COMMON = ./driver/iFPGA.cpp  ./driver/RuntimeClient.cpp ./src/FPGA_ColumnML.cpp 
COMMON		= ./src/ColumnML.cpp ./src/ColumnStore.cpp 

.PHONY: all clean distclean

all: sgd_sweep # blockwise_tests cpu_tests fpga_tests

sgd_sweep: ./tests/sgd_sweep.cpp $(COMMON)./src/ColumnML.h ./src/ColumnStore.h
	$(CXX) -D AVX2 $(CPPFLAGS) ./tests/sgd_sweep.cpp $(COMMON) -o sgd_sweep -lpthread

cpu_tests: ./tests/cpu_tests.cpp $(COMMON)./src/ColumnML.h ./src/ColumnStore.h
	$(CXX) -D HARPv2 $(CPPFLAGS) ./tests/cpu_tests.cpp $(COMMON) -o cpu_tests -lpthread

fpga_tests: ./tests/fpga_tests.cpp $(COMMON) $(FPGA_COMMON) ./driver/iFPGA.h ./driver/RuntimeClient.h ./src/ColumnML.h ./src/ColumnStore.h ./src/FPGA_ColumnML.h 
	$(CXX) -D HARPv2 -I$(AALSDK)/include $(CPPFLAGS) ./tests/fpga_tests.cpp $(COMMON) $(FPGA_COMMON) -o fpga_tests -L$(AALSDK)/lib -lpthread $(LDFLAGS) -laalrt

blockwise_tests: ./tests/blockwise_tests.cpp $(COMMON)./src/ColumnML.h ./src/ColumnStore.h
	$(CXX) -D HARPv2 $(CPPFLAGS) ./tests/blockwise_tests.cpp $(COMMON) -o blockwise_tests -lpthread

clean:
	rm cpu_tests
	rm fpga_tests
	rm blockwise_tests