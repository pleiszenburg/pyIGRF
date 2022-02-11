# pyIGRF (forked)

**This is a cleaned-up and modernized fork of ``pyIGRF``. Be aware that there are a number of small function and module name differences to the original ``pyIGRF`` package. The fork's main goals are verified results, tests, speed and ease of maintainability. This is work in progress.**

- [x] package structure cleanup
- [x] type annotations
- [x] doc strings completed and prepared for Sphinx autodoc
- [x] debug mode via environment variable `PYIGRF_DEBUG=1`
- [x] pure Python 3 implementation without dependency to `numpy`
- [x] JIT-compiled implementation depending on `numba` and `numpy`, installation target `jited`
- [x] array implementation depending on `numba` and `numpy`, installation target `array`
- [x] unit tests via `test` makefile target
- [x] verification of field's values against original Fortran implementation
- [ ] verification of field's annual/seasonal variation -> huge differences to original Fortran implementation
- [x] benchmark

![benchmark](benchmark/plot.png?raw=true "benchmark")

## What is pyIGRF?

`pyIGRF` is a Python package offering the IGRF-13 ([International Geomagnetic Reference Field](https://en.wikipedia.org/wiki/International_Geomagnetic_Reference_Field)) model. You can use it to calculate the magnetic field's intensity and to transform coordinates between GeoGraphical and GeoMagnetic. The package offers different implementations, pure Python and Python JIT-compiled via `numba`.

## How to Install?

Use pip to install the latest development version from Github:

```bash
pip install git+https://github.com/pleiszenburg/pyIGRF.git@master
```

## How to Use it?

First import the package, either as pure Python 3 or JIT-compiled via `numba` and `numpy`:

```python
from pyIGRF.pure import get_value, get_variation # pure Python 3
# or
from pyIGRF.jited import get_value, get_variation # JIT-compiled via `numba` and `numpy`
# or
from pyIGRF.array import get_value, get_variation # array implementation via `numba` and `numpy`
```

You can calculate the magnetic field's intensity:

```python
get_value(lat, lon, alt, year)
```

You can calculate the annual variation of the magnetic field's intensity:

```python
get_variation(lat, lon, alt, year)
```

The return value is a tuple (or array) of seven floating point numbers representing the local magnetic field:

- D: declination (+ve east) [degree]
- I: inclination (+ve down) [degree]
- H: horizontal intensity [nT]
- X: north component [nT]
- Y: east component [nT]
- Z: vertical component (+ve down) [nT]
- F: total intensity [nT]

If you want to use the IGRF-13 model in a more flexible manner, you can use the functions `geodetic2geocentric` and `get_syn`. They are somewhat closer to the original Fortran implementation.

Another function, `get_coeffs`, can be used to get `g[m][n]` or `h[m][n]` corresponding to the IGRF's formula.

## Alternatives

This project was originally forked from `pyIGRF`:

- [Original pyIGRF](https://github.com/zzyztyy/pyIGRF)

The official IGRF implementations by IAGA are available via the NOAA website:

- [IAGA IGRF, Fortran](https://www.ngdc.noaa.gov/IAGA/vmod/igrf13.f)
- [IAGA IGRF, Python](https://www.ngdc.noaa.gov/IAGA/vmod/pyIGRF.zip)

Another Python implementation is part of the [navtools package](https://github.com/slott56/navtools):

- [navtools IGRF, Python](https://github.com/slott56/navtools/blob/master/navtools/igrf.py)

Another Python wrapper around a cleaned-up version of IAGA's Fortran code is maintained as part of the [space physics project](https://github.com/space-physics):

- [space-physics IGRF, Python & Fortran](https://github.com/space-physics/igrf)

The IGRF is not to be confused with the [World Magnetic Model](https://en.wikipedia.org/wiki/World_Magnetic_Model) (WMM). Recent implementations of the WMM accessible via Python are maintained as part of the [space physics project](https://github.com/space-physics):

- [space-physics WMM2015, Python & C](https://github.com/space-physics/WMM2015)
- [space-physics WMM2020, Python & C](https://github.com/space-physics/wmm2020)

## References

- [Model introduction and IGRF-13 coefficients file download at NOAA](https://www.ngdc.noaa.gov/IAGA/vmod/igrf.html)
