anglr
=====

.. image:: https://img.shields.io/pypi/dm/anglr.svg
    :target: https://pypi.python.org/pypi/anglr/
    :alt: Downloads

.. image:: https://img.shields.io/pypi/v/anglr.svg
    :target: https://pypi.python.org/pypi/anglr/
    :alt: Latest Version

.. image:: https://img.shields.io/pypi/status/anglr.svg
    :target: https://pypi.python.org/pypi/anglr/
    :alt: Development Status

.. image:: https://img.shields.io/pypi/pyversions/anglr.svg
    :target: https://pypi.python.org/pypi/anglr/
    :alt: Supported Python Versions

.. image:: https://img.shields.io/pypi/l/anglr.svg
    :target: https://pypi.python.org/pypi/anglr/
    :alt: License

Planar angle mathematics library for Python.

This library contains many different functions for converting between units, comparing angles, and doing angle arithmetic.

Links:

-  `PyPI <https://pypi.python.org/pypi/anglr/>`__
-  `GitHub <https://github.com/Uberi/anglr>`__

Quickstart: ``pip3 install anglr``.

Rationale
---------

Consider the following trivial angle comparison code:

.. code:: python

    import math
    heading = get_compass_value() # angle in radians normalized to $[0, 2*pi)$
    if target - math.pi / 4 <= heading <= target + math.pi / 4:
        print("Facing the target")
    else:
        print("Not facing the target")

Angle code is everywhere. The above is totally, utterly **wrong** (consider what happens when ``target`` is 0), yet this could easily be overlooked while writing and during code review.

With anglr, there is a better way:

.. code:: python

    import math
    from anglr import Angle
    heading = Angle(get_compass_value())
    if heading.angle_between(target) <= math.pi / 4:
        print("Facing the target")
    else:
        print("Not facing the target")

Much better - this will now correctly take modular arithmetic into account when comparing angles.

Examples
--------

Angle creation:

.. code:: python

    from math import pi
    from anglr import Angle
    print(Angle())
    print(Angle(87 * pi / 2))
    print(Angle(pi / 2, "radians"))
    print(Angle(Angle(pi / 2, "radians"))) # same as above
    print(Angle(64.2, "degrees"))
    print(Angle(384.9, "gradians"))
    print(Angle(4.5, "hours"))
    print(Angle(203.8, "arcminutes"))
    print(Angle(42352.7, "arcseconds"))
    print(Angle((56, 32), "vector")) # angle in standard position - counterclockwise from positive X-axis

Angle conversion:

.. code:: python

    from anglr import Angle
    x = Angle(58.3)
    print([x], str(x), x.radians, x.degrees, x.gradians, x.hours, x.arcminutes, x.arcseconds, x.vector, x.x, x.y)
    print(complex(x))
    print(float(x))
    print(int(x))
    x.radians = pi / 2
    print(x.dump())
    x.degrees = 64.2
    print(x.dump())
    x.gradians = 384.9
    print(x.dump())
    x.hours = 4.5
    print(x.dump())
    x.arcminutes = 203.8
    print(x.dump())
    x.arcseconds = 42352.7
    print(x.dump())
    x.vector = (56, 32)
    print(x.dump())

Angle arithmetic:

.. code:: python

    from math import pi
    from anglr import Angle
    print(Angle(pi / 6) + Angle(2 * pi / 3))
    print(x * 2 + Angle(3 * pi / 4) / 4 + 5 * Angle(pi / 3))
    print(-abs(+Angle(pi)))
    print(round(Angle(-75.87)))
    print(Angle(-4.3) <= Angle(pi / 4) > Angle(0.118) == Angle(0.118))

    print("0 to 2pi sanity checks")
    print(Angle(0).normalized(0,2*pi)) # 0 as provided
    print(Angle(pi).normalized(0,2*pi)) # pi as provided
    print(Angle(-pi).normalized(0,2*pi)) # wrap to pi
    print(Angle(7*pi).normalized(2*pi,0)) # same as above despite range out of order
    print("\ndefault args and wrapping")  
    print(Angle(-7.5*pi).normalized())
    print(Angle(-7.5*pi).normalized(0)) # same as above
    print(Angle(-7.5*pi).normalized(0,2*pi)) # same as above
    print(Angle(-8.25*pi).normalized(0,2*pi)) # wrap beyond above to cross Tau
    print(Angle(8.25*pi).normalized(0,2*pi)) # cross over other way
    print("\n-pi to pi")
    print(Angle(0).normalized(-pi,pi)) #sanity check
    print(Angle(-7.5*pi).normalized(-pi,pi))
    print(Angle(-8.25*pi).normalized(-pi,pi)) # cross over
    print("\n-pi to 0") # not sure why this normalization range is useful or if results are valid:
    print(Angle(0).normalized(-pi,0))
    print(Angle(-pi/2).normalized(-pi,0))
    print(Angle(pi/2).normalized(-pi,0))
    print("\nbecause we like gradians?")
    print(Angle(-870.3, "gradians").normalized(0, 2 * pi))
    print(Angle(-870.3, "gradians").normalized(-pi, pi))
    print(Angle(-870.3, "gradians").normalized(-pi, 0))
    
    print(Angle(1, "degrees").angle_between_clockwise(Angle(0, "degrees")))
    print(Angle(1, "degrees").angle_between(Angle(0, "degrees")))
    print(Angle(0, "degrees").angle_within(Angle(-45, "degrees"), Angle(45, "degrees")))
    print(Angle(-1, "degrees").angle_within(Angle(-1, "degrees"), Angle(1, "degrees"), strictly_within=True))
    print(Angle(-1, "degrees").angle_to(Angle(180, "degrees")))
    print(Angle(0, "degrees").angle_to(Angle(180, "degrees")))

To run all of the above as tests, simply run ``python3 tests.py`` in the project directory.

Installing
----------

** THIS VERSION IS NOT AVAILABLE IN PIP YET - IGNORE BELOW **
The easiest way to install this is using ``pip3 install anglr``.

Otherwise, download the source distribution from `PyPI <https://pypi.python.org/pypi/anglr/>`__, and extract the archive.

In the folder, run ``python3 setup.py install``.

Requirements
------------

This library requires Python 3.2 or higher to run.

Authors
-------

::

    Uberi <azhang9@gmail.com> (Anthony Zhang)
    Pondersome <karim@compuguru.com> (Karim Virani)

Please report bugs and suggestions at the `issue tracker <https://github.com/Uberi/anglr/issues>`__!

License
-------

Copyright 2014-2015 `Anthony Zhang (Uberi) <https://uberi.github.io>`__.

The source code is available online at `GitHub <https://github.com/Uberi/anglr>`__.

This program is made available under the 3-clause BSD license. See ``LICENSE.txt`` for more information.
