Newsletter #03
========================

**10 October 2024.** English version, version française `ici <newsletter_03.html>`_.

Dear Meso-NH users,

This is the 3rd newsletter from our Meso-NH community. You'll find an interview with a member of the Meso-NH support team, the latest news from the support team and a list of the latest publications using Meso-NH.

Interview with `Juan Escobar <mailto:juan.escobar-munoz@cnrs.fr>`_ (LAERO, CNRS)
************************************************************************************

.. image:: photo_je.jpg
  :width: 450

Juan, you've ported Meso-NH 5-5-1 to a GPU with Philippe. Could you describe what this is all about, and the applications it opens up?
  GPUs (*Graphics Processing Units*) have become the successors to the parallel and vector supercomputers of the 2000s (like CRAY or NEC). They have dozens of processors capable of executing thousands of instructions in parallel, with much greater memory bandwidth than CPUs, while being much more economical and less power-hungry.

  Back in the 2010s, I started porting Meso-NH to GPUs using Fortran ERP compilers, using directive-based programming (DBC), which has evolved into the OpenACC standard. Today, around 90% of the most computationally-intensive parts (advection, turbulence, single-moment microphysics and pressure solver) are ported to GPUs, enabling Meso-NH to be used for scientific simulations. This version was successfully tested during the Adastra GPU Grand Challenge, simulating extreme events on around 1/3 of Adastra's GPU partition (128 compute nodes with a total of 1024 GPUs).

What's the advantage of running simulations on GPU-equipped computers rather than CPU-only ones?
  The main advantage of GPUs is their superior computing power. For example, on Adastra, 338 GPU nodes deliver 61 Petaflops, compared with just 13 Petaflops [#flop1]_ for 556 CPU nodes. What's more, GPUs are far more energy-efficient: at the Adastra Grand Challenge, code ran 5 times faster on GPUs while consuming 2 times less energy. This gap is set to widen. For example, the future European supercomputer in Jülich will reach 1 Exaflop [#flop2]_ with its GPU partition, while the CPU partition will deliver just 50 Petaflops, or 0.5% of total power.

.. [#flop1] Petaflops = millions of billions of computing operations per second. 
.. [#flop2] 1 Exaflop = 1000 Petaflops.

Are there any situations that lend themselves particularly well to using Meso-NH 5-5-1 on GPUs?
  GPUs are well suited to massively parallel simulations, such as Giga-LES, where grids contain billions of points. For example, during the Adastra GPU Grand Challenge, we used grids of 2.14 billion points (4096x4096x128) for simulations at 100-meter resolution.

  GPUs also make it possible to run a large number of simultaneous simulations on smaller configurations. For example, on Adastra, we simulated 350 days of weather in Corsica with a grid of 256x256x70 points at 1 km resolution, using 1 GPU node per simulation. Thanks to this approach, all simulations were completed in just 3 days.

What recommendations would you make to users wishing to simulate on a GPU?
  You should get in touch with Meso-NH Support to get your foot in the door on the GPU. In terms of code usage, there's no fundamental difference for someone who has already run Meso-NH on a CPU-based supercomputer. The current GPU version is based on MNH-5-5-1, so you'll need to use this version first on the CPU to fine-tune and calibrate your simulations, and then switch to the GPU.

  Of course, it is preferable to use the parameterizations already ported to the GPU, to gain in performance. However, this is not mandatory at first, as the code can be compiled to be bit-reproducible while running partly on CPU and partly on GPU.

What are the current limitations of this Meso-NH port, and what are the prospects?
  At present, only part of the code has been ported to the GPU, and that's in version MNH-5-5-1, so we have to run simulations with this version of the code.

  With the introduction of PHYEX, in versions MNH-5-6-X and MNH-5-7-X, some of the GPU porting work carried out has been lost (notably turbulence, ICE3 and sub-mesh condensation). However, Quentin Rodier and Sebastien Riette at CNRM are in charge of porting PHYEX to the GPU using the DSL (*Domain Specific Language*) methodology chosen by Météo-France. To this end, they have developed a Python pre-compiler, `pyft_tool <https://github.com/UMR-CNRM/pyft>`, enabling semi-automatic transformation of code for porting to GPUs. The library transforms the code on the fly before compiling it on GPUs, resulting in a slightly leaner source code.

  Phillippe Wautelet and I are collaborating with them, contributing our GPU and OpenACC expertise to add the necessary functionalities to this pyft tool to transform PHYEX code in MNH-5-6-X and adapt it to GPUs. These features include “hand-made tools” for porting Meso-NH to GPUs, such as the automatic addition of calls to bit-reproducible mathematical functions, the automatic addition of calls to MPPDB_CHECK routines for “run-time” verification of the identity of calculations on GPU and CPU, optimized memory management for arrays, expansion of *array syntax* in nested loops, etc.

  The MNH-5-6 + PHYEX version on GPU is well underway, and should be integrated “quickly” (before the end of the year ;-) ) in the latest MNH-5-7-X release. Other parts of the code will then be ported, such as shallow convection and LIMA.


.. note::

   A longer version of the beginning of the interview, with more details, is available `here <https://mesonh-beta-test-guide.readthedocs.io/en/latest/community/newsletters/newsletter_03_extended.html>`_.

   An article under discussion in *Geosci. Model Dev.* relates the work of porting Meso-NH to GPU, it is available `online here <https://doi.org/10.5194/egusphere-2024-2879>`_ and in pdf: `Escobar et al. (2024) <https://egusphere.copernicus.org/preprints/2024/egusphere-2024-2879/egusphere-2024-2879.pdf>`_.

   If you too would like to explain a development you've implemented in Meso-NH, or an analysis method you share with the community, please don't hesitate to let me know by `mail <mailto:thibaut.dauhut@univ-tlse3.fr>`_.

