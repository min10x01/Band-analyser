# Bandkit

## What is this software for

Bandkit is a Python package that reads, analyses and plots electronic band structures calculated
with [VASP](https://www.vasp.at/wiki/index.php/The_VASP_Manual). It reads the energies, k-points,
high-symmetry labels and orbital projections from a VASP run, then finds the band edges, computes
effective masses, analyses spin splitting (Rashba, Zeeman, Dresselhaus and exchange) and
reports which atoms and orbitals each state comes from.

This is the code structure:

```
bandkit/
|----- __init__.py
|----- __main__.py              # Lets you run `python -m bandkit`
|----- core.py                  # BandStructure class and the k-path (segments, distances, labels)
|----- plotting.py              # plot_bands() and plot_splitting()
|----- cli.py                   # The `bandkit` command-line tool
|----- io/                      # File readers and load_band_structure()
|      |----- vasprun.py        # vasprun.xml (projections and KPOINTS_OPT included)
|      |----- procar.py         # PROCAR (LORBIT 10/11/12, ISPIN 1/2, non-collinear)
|      |----- eigenval.py       # EIGENVAL
|      |----- outcar.py         # Fermi energy, reciprocal lattice, NELECT, ISPIN
|      |----- kpoints.py        # Line-mode and explicit KPOINTS (labels after "!")
|      |----- poscar.py         # Lattice and element names
|----- analysis/
       |----- edges.py          # VBM, CBM and band gap
       |----- effective_mass.py # Polynomial fits along each path direction
       |----- splitting.py      # Rashba, Zeeman, Dresselhaus and exchange splitting
       |----- orbitals.py       # Projected weights and band character
```

The effective mass comes from the curvature of the band at a band edge:

$$
m^* = \frac{\hbar^2}{\dfrac{d^2E}{dk^2}}
$$

For background on spin splitting, see the [Rashba effect](https://en.wikipedia.org/wiki/Rashba_effect)
and the VASP wiki page on [LORBIT](https://www.vasp.at/wiki/index.php/LORBIT), which controls the
orbital projections Bandkit reads.

* **Read** a band structure from `vasprun.xml`, `PROCAR`, `EIGENVAL`, `OUTCAR`, `KPOINTS` and
  `POSCAR`.
* **Find** the band edges: the valence band maximum (VBM), the conduction band minimum (CBM) and
  the band gap, and whether the gap is direct or indirect.
* **Compute** effective masses along every path direction. Masses are given in units of the
  electron mass and are negative for holes.
* **Analyse** band splitting. For a Rashba pair the code finds the Rashba energy $E_R$, the
  momentum offset $k_0$ and the Rashba parameter $\alpha_R = 2E_R / k_0$. It also classifies a
  splitting as Zeeman-type, Rashba-type or Dresselhaus-type, and gives the exchange splitting in
  spin-polarised runs.
* **Report** the orbital and atomic character of any state, for example `Bi-p 62 %, I-p 21 %`.
* **Plot** the bands with overlays that you switch on as needed: fat bands, projection gradients,
  the dominant orbital, spin texture, effective-mass fits, Rashba markers and splitting arrows.


## How to install

You need Python 3.9 or newer. Bandkit depends on NumPy and Matplotlib.

```bash
git clone https://github.com/min10x01/Bandkit.git
```

```bash
cd Bandkit
```

```bash
pip install -e .
