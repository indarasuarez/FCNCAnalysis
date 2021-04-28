ROOTCFLAGS    = $(shell $(ROOTSYS)/bin/root-config --cflags)
ROOTLIBS      = $(shell $(ROOTSYS)/bin/root-config --libs)
ROOTGLIBS     = $(shell $(ROOTSYS)/bin/root-config --glibs)

#######################################
# -- DEFINE ARCH to something sensible!
#  #######################################

#
#CXX           = g++
CXXFLAGS      = -g -Wall -fPIC
LD            = g++
LDFLAGS       = -g
SOFLAGS       = -shared


CXXFLAGS      += $(ROOTCFLAGS)
LIBS           = $(ROOTLIBS)

NGLIBS         = $(ROOTGLIBS)
NGLIBS        += -lMinuit
GLIBS          = $(filter-out -lNew, $(NGLIBS))

COREPATH       = $(HOME)/CORE

CXXFLAGS       += -I$(COREPATH)

CORELIB        = $(COREPATH)/CMS3_CORE.so
STOPVARLIB     = stop_variables/StopVariables.so

event_looper: event_looper.o 
	$(LD) $(LDFLAGS)  -o event_looper event_looper.o $(GLIBS) -lGenVector -lEG

event_looper.o: event_looper.C helpers/fcnc_functions.h helpers/sampleLoader.h helpers/objectClasses.h helpers/weights.h helpers/histogrammingClass.h helpers/flip_weights.h helpers/fake_rates.h
	$(CXX) $(CXXFLAGS) -c event_looper.C

LinkDef_out.so: LinkDef.h
	@rootcling -v -f LinkDef.cc -c -p Math/Vector4D.h LinkDef.h
	$(CXX) $(CXXFLAGS) $(SOFLAGS) LinkDef.cc -o $@

.PHONY: clean
clean:  
	rm -v -f \
	runBabyMaker \
	*.o *.d *.so; echo "Done"
