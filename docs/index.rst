.. include Shorthand.rst


=====================================================================
:py:mod:`meep_adjoint`: Adjoint-solver module for :program:`pymeep`
=====================================================================
This is the root of the documentation tree for :py:mod:`meep_adjoint.`
Jump directly to the module-wide :ref:`TableOfContents` below, 
or read on for a quick-start overview.

=========================
What does this module do?
=========================
You'll find a longer answer in the :doc:`Overview <Overview/index>`
(and a more succinct one in the :doc:`API Reference <API/index>`),
but, in a nutshell: It extends the computational capabilities of the
core |pymeep| library in a particular way that facilitates interaction
with `numerical optimization algorithms`_, opening the door to
intelligent design tools that automatically design devices to meet
given performance specifications.



.. |pymeep| raw:: html

   <a href="http://meep.readthedocs.io">
   <b><font style="font-variant: small-caps; font-size: 110%">pymeep</font></b></a>


.. _numerical optimization algorithms: https://en.wikipedia.org/wiki/Category:Optimization_algorithms_and_methods


.. admonition:: Wait, *what* exactly does the module do again?

        If that was a bit vague, we can be more specific about precisely what
        :py:mod:`meep_adjoint` does. Consider a typical design problem
        in which we seek to tune a device geometry to optimize some
        performance metric.
        For example, in the :doc:`cross router <Examples/cross_router>`
        example in the :py:mod:`meep_adjoint` :doc:`example gallery <Examples/index>`,
        the 
        example in the :py:mod:`meep_adjoint` :doc:`example gallery <Examples/index>`,
        the task is to design the central hub region to steer
        incoming power arriving via the **East** input waveguide
        to output 
       
        
        the task is to design the hub region toe steer incident
        power arriving from the 
input is routed 

        to the **North** output, allowing power to leak out through the 
        **West** or **South** outputs.

        
        Thus, :program:pymeep allows our physical design problem
        to be packaged as a black-box numerical function
        :math:`f(\boldsymbol{\beta})` suitable for passage to
        numerical optimizers 
        Automated design optimization, here we come!

        There's just one problem, which perhaps already
        occurred to you if you have any experience with high-dimensional
        optimization:

        and asks for the optimal
        Here :math:`\epsilon(x)`

        .. math::
          \epsilon(\mathbf x)=\sum \beta_d b_d(\mathbf x)

        Now suppose some region of the material geometry is free
        to be tweaked

        you're a device designer tasked with maximizing
        some performance metric $f$ expressed as a function of these
        output quantities---for example, we might put 
        :math:`f=S_2(\omega_3)` to maximize flux through the 2nda
        flux region at the 3rd frequency, or 
        :math:`f = (S_1(\omega_3) - S_2(\omega_3))^2`
        to maximize the *difference* between two fluxes, etc.

        For any given trial design function :math:`\epsilon(\mathbf x)`
        we can use :py:mod:`pymeep` to evaluate 
        we create a ``Simulation``_ with $\epsilon^{\text{trial}}$

        What :py:mod:`meep_adjoint` does is to add a 



.. admonition:: The scope of :program:`meep_adjoint`


         **Q.** Does this mean that the module *only* computes objective-function gradients? That is, it doesn't actually do any optimization?

         **A.** The *primary* mission of :py:mod:`meep_adjoint` is to compute objective-function gradients. This is the task the module guarantees to execute efficiently and accurately, and it's one that's self-contained, unambiguous, and easily *testable.* [#]_
  
         The larger question of how best to *use* gradient information for
         design automation---which involves questions such as which of the
         `myriad available gradient-based optimization algorithms`_ to use,
         and how to configure its tunable parameters---is officially beyond the
         purview of :py:mod:`meep_adjoint`, and indeed is much too broad
         a problem to be treated with anything approaching comprehensiveness
         by any single module. We hope :py:mod:`meep_adjoint` will be
         helpful to :py:mod:`meep` users as they navigate this
         vast domain.


         **With all of that by way of disclaimer,** however, we note that
         :py:mod:`meep_adjoint` does ship with one (rather simple-minded)
         implementation of an optimization algorithm---namely, a 
         :doc:`basic gradient-descent solver.` 
         Per the discussion above, we make
         no claim as to the robustness or efficiency of this solver,
         and we encourage users to consider it a first step in the
         process of optimizing any geometry, to be replaced by more
         sophisticated solvers as necessary; but, having said that,
         we note that the simple built-in optimizer suffices to
         yield good results on all the problems considered in
         the :doc:`example gallery <Examples/index>`.
  

=======================================
What does a typical workflow look like?
=======================================
You'll find a full step-by-step walkthrough in the
:doc:`Tutorial <Tutorial/index>` and additional guided case studies
in the :doc:`Example gallery <Examples/index>`, but here is a 
quick rundown:


1) *Initialization and Problem Definition:* 
You begin by creating an instance of :py:class:`optimization_problem`
This is the top-level python class exported by :py:mod:`meep_adjoint`, 
analogous to the `Simulation <Simulation_>`_
class in the core :py:mod:`meep` module; it stores all data and state
relevant to the progress of a design optimization, and
you will access most :py:mod:`meep_adjoint` functionality
via its class methods.

2) *Interactive single-point calculations*  Before 
launching a full-blown iterative optimization run that
could run for hours or days, you will probably want 
to run some sanity-check calculations involving your
geometry. These include...**(section incomplete)**

3) *Full iterative optimization*: Launch your optimization
run and monitor its progress via graphical or other indicators.

How is the documentation structured?
====================================

.. _TableOfContents:

===================================
Table of Contents
===================================

.. toctree::
   :maxdepth: 2
   :caption: meep_adjoint Table of Contents

   self
   Overview <Overview/index>
   Tutorial <Tutorial/index>
   Example Gallery <Examples/index>
   Visualization Module <Visualization/index>
   Configuration and customization <Config/index>
   Test suite <TestSuite/index>
   Implementation I: Physics and math  <Implementation/MathAndPhysicsOfAdjoints>
   Implementation II: Class hierarchy <Implementation/ClassHierarchy>
   API Reference <API/index>


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`


.. _myriad available gradient-based optimization algorithms: https://en.wikipedia.org/wiki/Category:Optimization_algorithms_and_methods

.. _Simulation:  https://meep.readthedocs.io/en/latest

.. [#] For example, one obvious test of the correctness
       of :py:mod:`meep_adjoint` is to estimate objective-function
       derivatives by numerical finite-differencing and compare to 
       components of the adjoint-method gradient. This is the basis
       of one of the tests in the :py:mod:`meep_adjoint` unit-test suite,
       and also of the :doc:`holey waveguide <Examples/holey_waveguide>`
       example in the :doc:`example gallery.<Examples/index>`
