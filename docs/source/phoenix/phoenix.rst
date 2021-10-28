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


**********
Benchmarks
**********

We are consistently working towards bringing the most efficient workflow to the table. Here are some benchmarks comparing different workflow results
utilizing NA12878.


Benchmarking DNA Alignment
==========================

We currently allow for **five** different alignment workflows. All supporting whether or not we run base recalibration. We will also compare results
with and without base recalibration.


Benchmarking Duplicate Marking
------------------------------

Our prep of NA12878 is PCRfree, meaning marked duplicates should be platform duplicates. Additionally, we've noticed some weird behavior with
Samtools duplicate marking in which these take much longer to run than expected, so we will also compare the "--no-multi-dup" option results,
represented with :sup:`nmd`.

.. table:: Markdup metrics from different tools

  +------------------------+-----------+-------------+-------------+------------+--------------------+-------------+-----------------+
  |                        | Ashion    | Broad       | Parabricks  | PCRfree    | PCRfree :sup:`nmd` | TGen        | TGen :sup:`nmd` |
  +========================+===========+=============+=============+============+====================+=============+=================+
  | PAIRED                 | NR        | 1170551473  | 1170551472  | 2341102946 | 2341102946         | 2341102946  | 2341102946      |
  +------------------------+-----------+-------------+-------------+------------+--------------------+-------------+-----------------+
  | SINGLE                 | NR        | 3774168     | 3774180     | 3774168    | 3774168            | 3774168     | 3774168         |
  +------------------------+-----------+-------------+-------------+------------+--------------------+-------------+-----------------+
  | DUP PAIR               | NR        | 141988421   | 141991258   | 273739194  | 273739194          | 283862614   | 283862614       |
  +------------------------+-----------+-------------+-------------+------------+--------------------+-------------+-----------------+
  | DUP SINGLE             | NR        | 1659995     | 1659760     | 1217722    | 1217722            | 1659995     | 1659995         |
  +------------------------+-----------+-------------+-------------+------------+--------------------+-------------+-----------------+
  | DUP PAIR OPTICAL       | NR        | 87434015    | 88510953    | 199633180  | 163991918          | 199548282   | 163566541       |
  +------------------------+-----------+-------------+-------------+------------+--------------------+-------------+-----------------+
  | DUP SINGLE OPTICAL     | NR        | NR          | NR          | 301529     | 263213             | 280617      | 239921          |
  +------------------------+-----------+-------------+-------------+------------+--------------------+-------------+-----------------+
  | DUP NONPRIMARY         | NR        | NR          | NR          | 10753612   | 10753612           | 11580472    | 11580472        |
  +------------------------+-----------+-------------+-------------+------------+--------------------+-------------+-----------------+
  | DUP NONPRIMARY OPTICAL | NR        | NR          | NR          | 5704286    | 1415147            | 5681792     | 1375825         |
  +------------------------+-----------+-------------+-------------+------------+--------------------+-------------+-----------------+
  | DUP PRIMARY TOTAL      | NR        | NR          | NR          | 274956916  | 274956916          | 285522609   | 285522609       |
  +------------------------+-----------+-------------+-------------+------------+--------------------+-------------+-----------------+
  | FLAGSTAT DUP TOTAL     | 294331202 | 297256491   | 285642276   | 285710528  | 285710528          | 297103081   | 297103081       |
  +------------------------+-----------+-------------+-------------+------------+--------------------+-------------+-----------------+
  | ESTIMATED_LIBRARY_SIZE | NR        | 10387896027 | 10582462610 | 7763705647 | 5310088035         | 13239367277 | 9487788958      |
  +------------------------+-----------+-------------+-------------+------------+--------------------+-------------+-----------------+

NR Signifies that the metric was not reported in the stats file. Also worth noting that the Ashion workflow uses samblaster, which does not output
a stats file.

Benchmarking Constitutional SNV Calling
=======================================

We have a handful of callers at our disposal. Here we make some recommendations along with benchmarks behind those recommendations. In most cases,
deepvariant is the best caller, and it's important to note how these results could change depending on the alignment style used:

.. table:: hap.py variant analysis for deepvariant

  +----------------+------------+-------------+-------------+----------+--------------------+--------------+-----------------+
  |                | Ashion     | Broad       | Parabricks  | PCRfree  | PCRfree :sup:`nmd` | TGen         | TGen :sup:`nmd` |
  +================+============+=============+=============+==========+====================+==============+=================+
  | Indel_F1_Score | 0.993792   | 0.993846    | 0.993844    | 0.993808 | 0.993808           | **0.993847** | 0.993847        |
  +----------------+------------+-------------+-------------+----------+--------------------+--------------+-----------------+
  | SNP_F1_Score   | 0.999390   | 0.999391    | 0.999390    | 0.999389 | 0.999389           | **0.999391** | 0.999391        |
  +----------------+------------+-------------+-------------+----------+--------------------+--------------+-----------------+


.. table:: hap.py variant analysis for haplotypecaller

  +----------------+--------------+-------------+-------------+--------------+--------------------+----------+-----------------+
  |                | Ashion       | Broad       | Parabricks  | PCRfree      | PCRfree :sup:`nmd` | TGen     | TGen :sup:`nmd` |
  +================+==============+=============+=============+==============+====================+==========+=================+
  | Indel_F1_Score | 0.993397     | 0.993367    | 0.993373    | **0.993401** | 0.993401           | 0.993376 | 0.993376        |
  +----------------+--------------+-------------+-------------+--------------+--------------------+----------+-----------------+
  | SNP_F1_Score   | **0.998473** | 0.998465    | 0.998468    | 0.998471     | 0.998471           | 0.998461 | 0.998461        |
  +----------------+--------------+-------------+-------------+--------------+--------------------+----------+-----------------+


.. table:: hap.py variant analysis for strelka2

  +----------------+------------+-------------+-------------+--------------+--------------------+----------+-----------------+
  |                | Ashion     | Broad       | Parabricks  | PCRfree      | PCRfree :sup:`nmd` | TGen     | TGen :sup:`nmd` |
  +================+============+=============+=============+==============+====================+==========+=================+
  | Indel_F1_Score | 0.992681   | 0.992754    | 0.992750    | **0.992760** | 0.992760           | 0.992754 | 0.992754        |
  +----------------+------------+-------------+-------------+--------------+--------------------+----------+-----------------+
  | SNP_F1_Score   | 0.998903   | 0.998905    | 0.998904    | **0.998905** | 0.998905           | 0.998905 | 0.998905        |
  +----------------+------------+-------------+-------------+--------------+--------------------+----------+-----------------+


.. table:: hap.py variant analysis for freebayes

  +----------------+------------+--------------+-------------+--------------+--------------------+----------+-----------------+
  |                | Ashion     | Broad        | Parabricks  | PCRfree      | PCRfree :sup:`nmd` | TGen     | TGen :sup:`nmd` |
  +================+============+==============+=============+==============+====================+==========+=================+
  | Indel_F1_Score | 0.976822   | **0.977327** | 0.977313    | 0.977320     | 0.977320           | 0.977325 | 0.977325        |
  +----------------+------------+--------------+-------------+--------------+--------------------+----------+-----------------+
  | SNP_F1_Score   | 0.996370   | 0.996368     | 0.996356    | **0.996376** | 0.996376           | 0.996365 | 0.996365        |
  +----------------+------------+--------------+-------------+--------------+--------------------+----------+-----------------+


We do not recommend using the gatk base recalibration workflow, but if it is required it is important to note how this may
impact the results. Here are same tables but using alignments that have base recalibration applied.


.. table:: hap.py variant analysis for deepvariant

  +----------------+------------+-------------+--------------+----------+--------------------+----------+-----------------+
  |                | Ashion     | Broad       | Parabricks   | PCRfree  | PCRfree :sup:`nmd` | TGen     | TGen :sup:`nmd` |
  +================+============+=============+==============+==========+====================+==========+=================+
  | Indel_F1_Score | 0.993246   | 0.993259    | 0.994620     | 0.993692 | **0.993692**       | 0.993272 | 0.993272        |
  +----------------+------------+-------------+--------------+----------+--------------------+----------+-----------------+
  | SNP_F1_Score   | 0.999382   | 0.999375    | **0.999397** | 0.999386 | 0.999386           | 0.999376 | 0.999376        |
  +----------------+------------+-------------+--------------+----------+--------------------+----------+-----------------+


.. table:: hap.py variant analysis for haplotypecaller

  +----------------+------------+-------------+--------------+----------+--------------------+----------+-----------------+
  |                | Ashion     | Broad       | Parabricks   | PCRfree  | PCRfree :sup:`nmd` | TGen     | TGen :sup:`nmd` |
  +================+============+=============+==============+==========+====================+==========+=================+
  | Indel_F1_Score | 0.993279   | 0.993283    | **0.993336** | 0.993272 | 0.993272           | 0.993275 | 0.993275        |
  +----------------+------------+-------------+--------------+----------+--------------------+----------+-----------------+
  | SNP_F1_Score   | 0.998484   | 0.998480    | **0.998524** | 0.998483 | 0.998483           | 0.998483 | 0.998483        |
  +----------------+------------+-------------+--------------+----------+--------------------+----------+-----------------+


.. table:: hap.py variant analysis for strelka2

  +----------------+------------+--------------+-------------+--------------+--------------------+----------+-----------------+
  |                | Ashion     | Broad        | Parabricks  | PCRfree      | PCRfree :sup:`nmd` | TGen     | TGen :sup:`nmd` |
  +================+============+==============+=============+==============+====================+==========+=================+
  | Indel_F1_Score | 0.992726   | 0.992811     | 0.992803    | **0.992812** | 0.992812           | 0.992807 | 0.992807        |
  +----------------+------------+--------------+-------------+--------------+--------------------+----------+-----------------+
  | SNP_F1_Score   | 0.998875   | **0.998884** | 0.998861    | 0.998882     | 0.998882           | 0.998883 | 0.998883        |
  +----------------+------------+--------------+-------------+--------------+--------------------+----------+-----------------+


.. table:: hap.py variant analysis for freebayes

  +----------------+------------+-------------+--------------+--------------+--------------------+----------+-----------------+
  |                | Ashion     | Broad       | Parabricks   | PCRfree      | PCRfree :sup:`nmd` | TGen     | TGen :sup:`nmd` |
  +================+============+=============+==============+==============+====================+==========+=================+
  | Indel_F1_Score | 0.976451   | 0.976952    | 0.976546     | **0.977073** | 0.977073           | 0.976934 | 0.976934        |
  +----------------+------------+-------------+--------------+--------------+--------------------+----------+-----------------+
  | SNP_F1_Score   | 0.996629   | 0.996611    | **0.996706** | 0.996664     | 0.996664           | 0.996616 | 0.996616        |
  +----------------+------------+-------------+--------------+--------------+--------------------+----------+-----------------+


*********
Workflows
*********

.. toctree::
   :maxdepth: 1

   workflows/alignment/alignment
   workflows/rna/rna
   workflows/qc/qc
   workflows/somatic/somatic
   workflows/tumor_only/tumor_only
   workflows/constitutional/constitutional
   workflows/annotation/annotation
   workflows/metrics/metrics
