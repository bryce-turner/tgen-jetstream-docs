Phoenix
=======

**Phoenix** is the primary pipeline workflow. Phoenix supports the analysis of human sequencing samples against
the GRCh38 reference genome using ensembl version 98 gene models.  The workflow is designed to support project level analysis
that can include one or multiple types of data. Though not required the expectation is a project contains
data from a single individual thus centralizing all data types in a standardized output structure. The
workflow template supports a diverse array of analysis methods required to analyze DNA, RNA, and single cell
data.  Based on standardized variables it also supports integrated analysis between data types.  For some
processes multiple options are provided that can be individually enabled or disabled by configuration
parameters. Like all JetStream (https://github.com/tgen/jetstream) workflows developed at TGen this workflow is designed to
facilitate our dynamic andtime sensitive analysis needs while ensuring compute and storage resources are used efficiently. The
primary input is a JSON record from the TGen LIMS but hand created inputs in the form of a JSON or EXCEL worksheet can
also be provided when run manually or by submission to the related JetStream Centro (https://github.com/tgen/jetstream_centro)
web portal. All required files defined the the ``pipeline.yaml`` can be created using code provided in the JetStream Resources
repository (https://github.com/tgen/jetstream_resources).


.. _somatic:

Somatic
-------

.. _lancet:

.. raw:: html

   <iframe frameborder="0" style="width:100%;height:625px;" src="https://viewer.diagrams.net/?tags={}&highlight=0000ff&edit=_blank&layers=1&nav=1&title=Somatic%20Lancet#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1mal00GzSs7fPSSKngRkAS7-hT62DsU6A%26export%3Ddownload"></iframe>
