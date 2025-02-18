.. index:: min_style

min_style cg command
====================

min_style hftn command
======================

min_style sd command
====================

min_style quickmin command
==========================

min_style fire command
======================

min_style fire/old command
==========================

min_style abcfire command
=========================

:doc:`min_style spin <min_spin>` command
========================================

:doc:`min_style spin/cg <min_spin>` command
===========================================

:doc:`min_style spin/lbfgs <min_spin>` command
==============================================

Syntax
""""""

.. parsed-literal::

   min_style style

* style = *cg* or *hftn* or *sd* or *quickmin* or *fire* or *fire/old* or *abcfire* or *spin* or *spin/cg* or *spin/lbfgs*

  .. parsed-literal::

       *spin* is discussed briefly here and fully on :doc:`min_style spin <min_spin>` doc page
       *spin/cg* is discussed briefly here and fully on :doc:`min_style spin <min_spin>` doc page
       *spin/lbfgs* is discussed briefly here and fully on :doc:`min_style spin <min_spin>` doc page

Examples
""""""""

.. code-block:: LAMMPS

   min_style cg
   min_style fire
   min_style spin

Description
"""""""""""

Choose a minimization algorithm to use when a :doc:`minimize
<minimize>` command is performed.

Style *cg* is the Polak-Ribiere version of the conjugate gradient (CG)
algorithm.  At each iteration the force gradient is combined with the
previous iteration information to compute a new search direction
perpendicular (conjugate) to the previous search direction.  The PR
variant affects how the direction is chosen and how the CG method is
restarted when it ceases to make progress.  The PR variant is thought
to be the most effective CG choice for most problems.

Style *hftn* is a Hessian-free truncated Newton algorithm.  At each
iteration a quadratic model of the energy potential is solved by a
conjugate gradient inner iteration.  The Hessian (second derivatives)
of the energy is not formed directly, but approximated in each
conjugate search direction by a finite difference directional
derivative.  When close to an energy minimum, the algorithm behaves
like a Newton method and exhibits a quadratic convergence rate to high
accuracy.  In most cases the behavior of *hftn* is similar to *cg*,
but it offers an alternative if *cg* seems to perform poorly.  This
style is not affected by the :doc:`min_modify <min_modify>` command.

Style *sd* is a steepest descent algorithm.  At each iteration, the
search direction is set to the downhill direction corresponding to the
force vector (negative gradient of energy).  Typically, steepest
descent will not converge as quickly as CG, but may be more robust in
some situations.

Style *quickmin* is a damped dynamics method described in
:ref:`(Sheppard) <Sheppard>`, where the damping parameter is related
to the projection of the velocity vector along the current force
vector for each atom.  The velocity of each atom is initialized to 0.0
by this style, at the beginning of a minimization.

Style *fire* is a damped dynamics method described in :ref:`(Bitzek)
<Bitzek>`, which is similar to *quickmin* but adds a variable timestep
and alters the projection operation to maintain components of the
velocity non-parallel to the current force vector.  The velocity of
each atom is initialized to 0.0 by this style, at the beginning of a
minimization. This style correspond to an optimized version described
in :ref:`(Guenole) <Guenole>` that include different time integration
schemes and defaults parameters. The default parameters can be
modified with the command :doc:`min_modify <min_modify>`.

Style *fire/old* is the original implementation of *fire* in Lammps,
conserved for backward compatibility. The main differences regarding
the current version *fire* are: time integration by Explicit Euler
only, different sequence in maintaining velocity components non-parallel
to the current force vector and hard-coded minimization parameters.
A complete description of the differences between *fire/old* and *fire*
can be found in :ref:`(Guenole) <Guenole>` (where the current *fire*
in LAMMPS is called *fire2.0*). By using an appropriate set of
parameters, *fire* can behave similar to *fire/old*, as described
in the :doc:`min_modify <min_modify>` command.

Style *abcfire* (Accelerated Bias-Corrected *fire*) introduces an 
additional factor to *fire* that modifies the bias and the scaling of
the velocities of the atoms during the mixing step. This factor 
accelerates convergence by decreasing the bias towards null velocities 
every time that *fire* freezes the system during a minimization. It 
also works as a scaling parameter that makes the velocities evolve 
faster. More details can be found in :ref:`(Echeverri Restrepo) <EcheverriRestrepo>`.

Style *spin* is a damped spin dynamics with an adaptive timestep.

Style *spin/cg* uses an orthogonal spin optimization (OSO) combined to
a conjugate gradient (CG) approach to minimize spin configurations.

Style *spin/lbfgs* uses an orthogonal spin optimization (OSO) combined
to a limited-memory Broyden-Fletcher-Goldfarb-Shanno (LBFGS) approach
to minimize spin configurations.

See the :doc:`min/spin <min_spin>` page for more information about
the *spin*, *spin/cg* and *spin/lbfgs* styles.

Either the *quickmin*, *fire* and *fire/old* styles are useful in the
context of nudged elastic band (NEB) calculations via the :doc:`neb
<neb>` command.

Either the *spin*, *spin/cg* and *spin/lbfgs* styles are useful in
the context of magnetic geodesic nudged elastic band (GNEB)
calculations via the :doc:`neb/spin <neb_spin>` command.

.. note::

   The damped dynamic minimizers use whatever timestep you have
   defined via the :doc:`timestep <timestep>` command.  Often they
   will converge more quickly if you use a timestep about 10x larger
   than you would normally use for dynamics simulations.
   For *fire*, the default timestep is recommended to be equal to
   the one you would normally use for dynamics simulations.

.. note::

   The *quickmin*, *fire*, *fire/old*, *abcfire*, *hftn*, and *cg/kk* styles do not yet
   support the use of the :doc:`fix box/relax <fix_box_relax>` command
   or minimizations involving the electron radius in :doc:`eFF
   <pair_eff>` models.

----------

.. include:: accel_styles.rst

----------

Restrictions
""""""""""""

The *spin*, *spin/cg*, and *spin/lbfgps* styles are part of the SPIN
package.  They are only enabled if LAMMPS was built with that package.
See the :doc:`Build package <Build_package>` page for more info.

Related commands
""""""""""""""""

:doc:`min_modify <min_modify>`, :doc:`minimize <minimize>`, :doc:`neb <neb>`

Default
"""""""

.. code-block:: LAMMPS

   min_style cg

----------

.. _Sheppard:

**(Sheppard)** Sheppard, Terrell, Henkelman, J Chem Phys, 128, 134106
(2008).  See ref 1 in this paper for original reference to Qmin in
Jonsson, Mills, Jacobsen.

.. _Bitzek:

**(Bitzek)** Bitzek, Koskinen, Gahler, Moseler, Gumbsch, Phys Rev Lett,
97, 170201 (2006).

.. _Guenole:

**(Guenole)** Guenole, Noehring, Vaid, Houlle, Xie, Prakash, Bitzek,
Comput Mater Sci, 175, 109584 (2020).

.. _EcheverriRestrepo:

**(EcheverriRestrepo)** Echeverri Restrepo, Andric, Comput Mater Sci, 
218, 111978 (2023).

