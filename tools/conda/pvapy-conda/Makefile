
TOP = ../../..
BUILD_CONF = $(TOP)/configure/BUILD.conf
include $(BUILD_CONF)

ifeq ($(PVA_PY_VERSION),)
    PVA_PY_VERSION=$(shell git rev-parse --short HEAD)
endif
PVA_PY_YAML_TEMPLATE ?= meta.yaml.template

PACKAGE = pvapy
CONDA_ENV_FLAGS += -y

default: package

configure:
	eval "cat ${PVA_PY_YAML_TEMPLATE} | sed 's?BUILD_NUMBER?${BUILD_NUMBER}?g' | sed 's?PVA_PY_VERSION?${PVA_PY_VERSION}?g' | sed 's?PVA_PY_GIT_VERSION?${PVA_PY_GIT_VERSION}?g' | sed 's?EPICS_BASE_VERSION?${EPICS_BASE_VERSION}?g' | sed 's?BOOST_VERSION?${BOOST_VERSION}?g' > meta.yaml"

build: configure
	BUILD_FLAGS=${BUILD_FLAGS} conda build .

package: build

install: build
	conda install $(CONDA_ENV_FLAGS) $(PACKAGE) --use-local

uninstall: 
	conda remove $(CONDA_ENV_FLAGS) $(PACKAGE) || echo "Cannot uninstall $(PACKAGE)"

clean:  
	rm -rf build dist

distclean: clean

