# Copyright 2020 ETH Zurich and University of Bologna.
# Solderpad Hardware License, Version 0.51, see LICENSE for details.
# SPDX-License-Identifier: SHL-0.51
#
# Nils Wistoff <nwistoff@iis.ee.ethz.ch>

MKFILE_PATH := $(abspath $(lastword $(MAKEFILE_LIST)))
MKFILE_DIR  := $(dir $(MKFILE_PATH))
ROOT        := ${MKFILE_DIR}../../..
DEBUG       ?= 0

VIVADO ?= vivado
BENDER ?= bender

BENDER_TARGETS += -t cv64a6_imafdc_sv39 -t occamy -t snax_cluster

ifeq ($(EXT_JTAG), 0)
	BENDER_TARGETS += -t bscane
endif

ifdef SNAX_XIAOLING
	BENDER_TARGETS += -t snax_streamer_gemm -t snax_streamer_simd -t snax_data_reshuffler -t snax_xiaoling
endif

ifdef SNAX_MINIMAL
	BENDER_TARGETS += -t snax_minimal
endif

include $(ROOT)/util/Makefrag

occamy_xilinx: define-sources.tcl
	# $(MAKE) -C ../../ update-source
	# $(MAKE) -C ../../sim rtl

	${VIVADO} -mode batch -source occamy_xilinx.tcl -tclargs $(DEBUG) $(EXT_JTAG)

define-sources.tcl:
	${BENDER} script vivado $(BENDER_TARGETS) > $@

clean:
	rm -rf .Xil occamy_xilinx vivado* define-sources.tcl component.xml xgui *.jou *.log
