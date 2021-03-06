#######################################################################
# Makefile for Delphi test-suite
#######################################################################

LANGUAGE     = delphi
SCRIPTSUFFIX = _runme.dpr
DELPHIPATHSEPARATOR = "/"
DELPHICYGPATH_W     = echo
srcdir       = .
top_srcdir   = ../../../..
top_builddir = ../../../..

CPP_TEST_CASES = 

CUSTOM_TEST_CASES = 
#CUSTOM_TEST_CASES = intermediary_classname


include $(srcdir)/../common.mk

# Overridden variables here
SWIGOPT += -xmlout tree.xml -runtime_const $(SWIGOPTSPECIAL)

# Rules for the different types of tests
%.cpptest: 
	$(setup)
	+(cd $* && $(swig_and_compile_cpp))
	+$(run_testcase)

%.ctest:
	$(setup)
	+(cd $* && $(swig_and_compile_c))
	+$(run_testcase)

%.multicpptest: 
	$(setup)
	+(cd $* && $(swig_and_compile_multi_cpp))
	+$(run_testcase)

# Rules for custom tests
#intermediary_classname.customtest: 
#	$(MAKE) intermediary_classname.cpptest SWIGOPTSPECIAL="

# Makes a directory for the testcase if it does not exist
setup = \
	if [ -f $(srcdir)/$(SCRIPTPREFIX)$*$(SCRIPTSUFFIX) ]; then	  \
	  echo "Checking testcase $* (with run test) under $(LANGUAGE)" ; \
	else								  \
	  echo "Checking testcase $* under $(LANGUAGE)" ;		  \
	fi;								  \
	if [ ! -d $* ]; then						  \
		mkdir $*;						  \
	fi;

# Compiles Delphi files then runs the testcase. A testcase is only run if
# a file is found which has _runme.cs appended after the testcase name.
# Note Delphi(FPC) uses LD_LIBRARY_PATH under Unix, PATH under Cygwin/Windows and SHLIB_PATH on HPUX.
run_testcase = \
	if [ -f $(srcdir)/$(SCRIPTPREFIX)$*$(SCRIPTSUFFIX) ]; then (	  \
	  $(MAKE) -f $*/$(top_builddir)/$(EXAMPLES)/Makefile \
	  DELPHIFLAGS='' \
	  DELPHISRCS='`$(DELPHICYGPATH_W) $(srcdir)/$(SCRIPTPREFIX)$*$(SCRIPTSUFFIX)` \
	  $*$(DELPHIPATHSEPARATOR)*.pas' delphi_compile && \
	  env LD_LIBRARY_PATH="$*:$$LD_LIBRARY_PATH" PATH="$*:$$PATH" SHLIB_PATH="$*:$$SHLIB_PATH" $(INTERPRETER) $*_runme.exe; ) \
	else ( \
	  cd $* && \
	  $(MAKE) -f $(top_builddir)/$(EXAMPLES)/Makefile \
	  DELPHIFLAGS='' \
	  DELPHISRCS='*.pas' delphi_compile; ); \
	fi;

# Clean: remove testcase directories
%.clean:
	@if [ -d $* ]; then \
		rm -rf $*; \
	fi;

clean:
	@rm -f *.exe *.exe.mdb *.dcu *.a *.ppu *.pp
