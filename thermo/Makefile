CXX = g++ -O3 -Wall
FLAGS = -I. -I../src
OBJS = enrichment.o nucleation.o parabola625.o

all: check-nucleation
.PHONY: all

# === Executables ===

check-nucleation: check-nucleation.cpp ../src/globals.h nucleation.c $(OBJS)
	$(CXX) $(FLAGS) $(OBJS) $< -o $@ -lm

ternary-diagram.png: phase-diagram.py parabola625.c parabola625.so
	python $<

# === Generated Sources ===

enrichment.c: enrichment.py
	python $<

parabola625.c: CALPHAD_energies.py constants.py
	python $<

# === Objects ===

enrichment.o: enrichment.c
	$(CXX) -c $< -o $@

nucleation.o: nucleation.c nucleation.h parabola625.c
	$(CXX) $(FLAGS) -c $< -o $@

parabola625.o: parabola625.c
	$(CXX) -c $< -o $@

parabola625.so: parabola625.c
	$(CXX) -shared $< -o $@

# === Extras ===

ternary-landscape.png: ternary-landscape.py tie-lines.npz
	python $<

tie-lines.npz: phase-diagram.py
	python $<

.PHONY: clean
clean:
	rm -f check-enrichment enrichment.c enrichment.o nucleation.o parabola625.*
