.. PhaseSpace:

PhaseSpace Class
################

This class implements the phase-space Monte Carlo event generation where N is the number
of particles in the final state. Currently PhaseSpace class supports up-to
N=10 number of particles in the Final state. Most of the PhaseSpace class
methods can work on both ``HOST`` and ``DEVICE``. The number of particles is
associated with suffix with the class name.

This class is the wrapper for the C++ PhaseSpace class. The PhaseSpace class contains one constructor
to instantiate it:

- Constructor with N number of daughter masses.

.. code-block:: python

    import HydraPython as hypy

    p = hypy.PhaseSpace4([3.096916, 0.493677, 0.13957018, 0.0195018])
    # This will construct the PhaseSpace object with the 4 daughter masses in the list.


The PhaseSpace classes provides a method to generate a phase-space decay given an output range
or a phase-space given a range of mother particles and an output range.

.. code-block:: python

    # The below example generates and fills 3 states of 4 particle host events
    vec4 = hypy.Vector4R(5.2795, 0.0, 0.0, 0.0)
    ps = hypy.PhaseSpace4([3.096916, 0.493677, 0.13957018, 0.0195018])
    e_host = hypy.host_events_4(3)
    e_device = hypy.device_events_4(3)
    ps.GenerateOnhost(vec4, e_host)  # Generate particle on host
    ps.GenerateOndevice(vec4, e_device)  # Generate particle on device


.. code-block:: python


    B0_mass = 5.27955
    B0 = hypy.Vector4R(B0_mass, 0.0, 0.0, 0.0)

    mothers = hypy.host_vector_float4(5)
    # Fill mother with some particles
    mothers[0] = (3.326536152819228, -0.7376241292510032, 0.9527533342879685, 0.15239715864543849)
    mothers[1] = (3.3327060111834546, -0.44741166640978447, 1.012640505284964, -0.5390007001803998)
    mothers[2] = (3.4673036097962844, 0.6781637974979919, -1.4020213115136253, -0.0763859825560801)
    mothers[3] = (3.5042443315560945, 1.5383404921780213, -0.1442073504412384, -0.5492280905481964)
    mothers[4] = (3.4406218104833015, -0.16339927010014546, 1.363729549941791, 0.6005257912194031)

    phsp2 = hypy.PhaseSpace2([0.1056583745, 0.1056583745])
    grand_daughter = hypy.host_events_2(5)
    phsp2.GenerateOnhost(mothers, grand_daughter)

    for i in grand_daughter: print(i)


The ``AverageOnhost`` and ``AverageOndevice`` method by PhaseSpace classes calculate the
``mean`` and ``sqrt(variance)`` of a functor over the phase-space with n-samples or
of a functor over the phase-space given a list of mother particles.

.. code-block:: python

    import HydraPython as hypy
    import math
    def foo(*data):
        p1, p2, p3 = data[0], data[1], data[2]
        p = p1 + p2 + p3
        q = p2 + p3
        pd = p * p2
        pq = p * q
        qd = q * p2
        mp2 = p.mass2()
        mq2 = q.mass2()
        md2 = p2.mass2()
        return (pd * mq2 - pq * qd) / math.sqrt((pq * pq - mq2 * mp2) * (qd * qd - mq2 * md2))

    vec4 = hypy.Vector4R(5.2795, 0.0, 0.0, 0.0)
    p = hypy.PhaseSpace4([3.096916, 0.493677, 0.13957018, 0.0195018])
    tup = p.AverageOnhost(vec4, foo, 10)  # Average of host, currently passing functor to device will fail
    print (tup[0])  # Mean
    print (tup[1])  # sqrt of variance


Like generators, the AverageOn method also can accept the list of mother particle instead of one mother particle
and calculate the ``mean`` and ``sqrt(variance)``.

The ``EvaluateOnhost`` and ``EvaluateOndevice`` evaluates a functor over the passed one mother particle or the list
of mother particles.


The complete list of class implementations can be found at [#f1]_ and the complete list of methods supported
can be found at [#f2]_.


.. [#f1] The list of PhaseSpace classe implementations

  - ``PhaseSpace2``  Generate the phase-space with 2 particles. Syntax:

      - p = hypy.PhaseSpace2([2 daughter masses])

  - ``PhaseSpace3``  Generate the phase-space with 3 particles. Syntax:

      - p = hypy.PhaseSpace3([3 daughter masses])

  - ``PhaseSpace4``  Generate the phase-space with 4 particles. Syntax:

      - p = hypy.PhaseSpace4([4 daughter masses])

  - ``PhaseSpace5``  Generate the phase-space with 5 particles. Syntax:

      - p = hypy.PhaseSpace5([5 daughter masses])

  - ``PhaseSpace6``  Generate the phase-space with 6 particles. Syntax:

      - p = hypy.PhaseSpace6([6 daughter masses])

  - ``PhaseSpace7``  Generate the phase-space with 7 particles. Syntax:

      - p = hypy.PhaseSpace7([7 daughter masses])

  - ``PhaseSpace8``  Generate the phase-space with 8 particles. Syntax:

      - p = hypy.PhaseSpace8([8 daughter masses])

  - ``PhaseSpace9``  Generate the phase-space with 9 particles. Syntax:

      - p = hypy.PhaseSpace9([9 daughter masses])

  - ``PhaseSpace10``  Generate the phase-space with 10 particles. Syntax:

      - p = hypy.PhaseSpace10([10 daughter masses])


.. [#f2] The list of methods for the PhaseSpace classes

  - ``GetSeed``  Get the seed. Syntax:

      - p.GetSeed()

  - ``SetSeed``  Set seed. Syntax:

      - p.SetSeed(seed)

  - ``GenerateOnhost``  Generate the phase-space. Syntax:

      - p.GenerateOnhost(vector4R, event)
      - p.GenerateOnhost(hypy.host_vector_float4& mothers, event)

  - ``GenerateOndevice``  Generate the phase-space. Syntax:

      - p.GenerateOndevice(vector4R, event)
      - p.GenerateOndevice(hypy.device_vector_float4& mothers, event)

  - ``AverageOnhost`` Get the mean and sqrt of variance. Syntax:

      - p.AverageOnhost(vector4R, functor, number_of_entires)
      - p.AverageOnhost(hypy.host_vector_float4& mothers, functor)

  - ``AverageOndevice`` Get the mean and sqrt of variance. Syntax:
  - ``AverageOndevice`` Get the mean and sqrt of variance. Syntax:

      - p.AverageOndevice(vector4R, functor, number_of_entires)
      - p.AverageOndevice(hypy.device_vector_float4& mothers, functor)

  - ``EvaluateOnhost`` Evaluate a function over the given particle or list of particles:

      - p.EvaluateOnhost(vector4R, hypy.host_vector_float2& result, functor)
      - p.EvaluateOnhost(hypy.host_vector_float4& mothers, hypy.host_vector_float2& result, functor)

  - ``EvaluateOndevice`` Evaluate a function over the given particle or list of particles:

      - p.EvaluateOndevice(vector4R, hypy.device_vector_float2& result, functor)
      - p.EvaluateOndevice(hypy.device_vector_float4& mothers, hypy.device_vector_float2& result, functor)
