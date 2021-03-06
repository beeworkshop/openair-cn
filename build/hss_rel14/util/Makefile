CC := g++ # This is the main compiler

OPENAIRCN_DIR :=../../..
OAI_HSS_DIR := $(OPENAIRCN_DIR)/src/hss_rel14
OAI_HSS_UTIL_DIR := $(OPENAIRCN_DIR)/src/hss_rel14/util
OAI_MODULES_DIR := $(OPENAIRCN_DIR)/build/hss_rel14
OAI_BUILD_DIR := $(OPENAIRCN_DIR)/build/hss_rel14/util

SRCDIR := $(OAI_HSS_UTIL_DIR)/src
BUILDDIR := $(OAI_BUILD_DIR)/build
TARGETDIR := $(OAI_BUILD_DIR)/lib
LIBNAME := libc3po.a
TARGET := $(TARGETDIR)/$(LIBNAME)
INSTDIRINC := /usr/local/include
INSTDIRLIB := /usr/local/lib
INSTLIB := $(INSTDIRLIB)/$(LIBNAME)
 
SRCEXT := cpp
SOURCES := $(shell find $(SRCDIR) -type f -name *.$(SRCEXT))
OBJECTS := $(patsubst $(SRCDIR)/%,$(BUILDDIR)/%,$(SOURCES:.$(SRCEXT)=.o))
DEPENDS := $(OBJECTS:%.o=%.d)
CFLAGS := -std=c++11 -Wreturn-type -g -pthread -lrt # -Wall
INC := -I $(OAI_HSS_UTIL_DIR)/include \
 -I $(OAI_MODULES_DIR)/../git_submodules/rapidjson/include \
 -I $(OAI_MODULES_DIR)/../git_submodules/freeDiameter/include \
 -I $(OAI_MODULES_DIR)/../git_submodules/c-ares

$(TARGET): $(OBJECTS)
	@echo " Linking..."
	@mkdir -p $(TARGETDIR)
	@echo " ar rcs $(TARGET) $(OBJECTS)"; ar rcs $(TARGET) $(OBJECTS)

$(BUILDDIR)/%.o: $(SRCDIR)/%.$(SRCEXT) Makefile
	@mkdir -p $(BUILDDIR)
	@echo " $(CC) $(CFLAGS) $(INC) -MMD -c -o $@ $<"; $(CC) $(CFLAGS) $(INC) -MMD -c -o $@ $<

clean:
	@echo " Cleaning..."; 
	@echo " $(RM) -r $(BUILDDIR) $(TARGETDIR)"; $(RM) -r $(BUILDDIR) $(TARGETDIR)

install: $(TARGET)
	@echo " Installing..."
	@echo "  creating/verifying $(INSTDIRINC)"
	@mkdir -p $(INSTDIRINC)
	@echo "  copying headers to $(INSTDIRINC)"
	@cp -a include/* $(INSTDIRINC)
	@echo "  creating/verifying $(INSTDIRLIB)"
	@mkdir -p $(INSTDIRLIB)
	@echo "  copying $(TARGET) to $(INSTLIB)"
	@cp -a $(TARGET) $(INSTLIB)
	@echo " Installation complete"
	
-include $(DEPENDS)

.PHONY: clean
