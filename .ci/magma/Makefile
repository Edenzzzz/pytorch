SHELL=/usr/bin/env bash

DOCKER_CMD ?= docker
DESIRED_CUDA ?= 12.8
DESIRED_CUDA_SHORT = $(subst .,,$(DESIRED_CUDA))
PACKAGE_NAME = magma-cuda
CUDA_ARCH_LIST ?= -gencode arch=compute_50,code=sm_50 -gencode arch=compute_60,code=sm_60 -gencode arch=compute_70,code=sm_70 -gencode arch=compute_80,code=sm_80 -gencode arch=compute_86,code=sm_86 -gencode arch=compute_90,code=sm_90

DOCKER_RUN = set -eou pipefail; ${DOCKER_CMD} run --rm -i \
	-v $(shell git rev-parse --show-toplevel)/.ci:/builder \
	-w /builder \
	-e PACKAGE_NAME=${PACKAGE_NAME}${DESIRED_CUDA_SHORT} \
	-e DESIRED_CUDA=${DESIRED_CUDA} \
	-e CUDA_ARCH_LIST="${CUDA_ARCH_LIST}" \
	"pytorch/almalinux-builder:cuda${DESIRED_CUDA}-main" \
	magma/build_magma.sh

.PHONY: all
all: magma-cuda129
all: magma-cuda128
all: magma-cuda126

.PHONY:
clean:
	$(RM) -r magma-*
	$(RM) -r output

.PHONY: magma-cuda129
magma-cuda129: DESIRED_CUDA := 12.9
magma-cuda129: CUDA_ARCH_LIST += -gencode arch=compute_100,code=sm_100 -gencode arch=compute_120,code=sm_120
magma-cuda129:
	$(DOCKER_RUN)

.PHONY: magma-cuda128
magma-cuda128: DESIRED_CUDA := 12.8
magma-cuda128: CUDA_ARCH_LIST += -gencode arch=compute_100,code=sm_100 -gencode arch=compute_120,code=sm_120
magma-cuda128:
	$(DOCKER_RUN)

.PHONY: magma-cuda126
magma-cuda126: DESIRED_CUDA := 12.6
magma-cuda126:
	$(DOCKER_RUN)
