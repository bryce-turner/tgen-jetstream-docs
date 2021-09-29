#######
Phoenix
#######

**Phoenix** is the primary pipeline workflow. Phoenix supports the analysis of human sequencing samples against
the GRCh38 reference genome using ensembl version 98 gene models.  The workflow is designed to support project level analysis
that can include one or multiple types of data. Though not required the expectation is a project contains
data from a single individual thus centralizing all data types in a standardized output structure. The
workflow template supports a diverse array of analysis methods required to analyze DNA, RNA, and single cell
data.  Based on standardized variables it also supports integrated analysis between data types.  For some
processes multiple options are provided that can be individually enabled or disabled by configuration
parameters. Like all `Jetstream <https://github.com/tgen/jetstream>`_ workflows developed at TGen this workflow is designed to
facilitate our dynamic and time sensitive analysis needs while ensuring compute and storage resources are used efficiently. The
primary input is a JSON record from the TGen LIMS but hand created inputs in the form of a JSON or EXCEL worksheet can
also be provided when run manually or by submission to the related `Jetstream Centro <https://github.com/tgen/jetstream_centro>`_
web portal. All required files defined the the ``pipeline.yaml`` can be created using code provided by `Jetstream Resources. <https://github.com/tgen/jetstream_resources>`_


.. _somatic:

*******
Somatic
*******

SNV Calling
===========

.. _lancet:

Lancet
------

.. raw:: html
    :file: workflows/embed/Somatic_Lancet.html

.. _mutect:

Mutect2
-------

.. raw:: html
    :file: workflows/embed/Somatic_Mutect2.html

.. _octopus:

Octopus
-------

.. raw:: html
    :file: workflows/embed/Somatic_Octopus.html

.. _strelka:

Strelka2
--------

.. raw:: html
    :file: workflows/embed/Somatic_Strelka2.html

.. _vardict:

VarDict
-------

.. raw:: html
    :file: workflows/embed/Somatic_Vardict.html
