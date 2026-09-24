.. _conan_tools_sbom:

.. include:: ../../common/experimental_warning.inc

conan.tools.sbom
=================

CycloneDX
^^^^^^^^^
The CycloneDX tool is available in the ``conan.tools.sbom`` module.

It provides the ``cyclonedx_1_4`` and ``cyclonedx_1_6`` functions which receive a ``conanfile``
and return a dictionary with the SBOM data in the CycloneDX 1.4/1.6 JSON format.

.. currentmodule:: conan.tools.sbom.cyclonedx
.. autofunction:: cyclonedx_1_4
.. autofunction:: cyclonedx_1_6


Both functions share an interface and are very similar; the main difference is the version of CycloneDX that each of
them supports. The options ``add_build`` and ``add_test`` allow you to include the build and test packages,
respectively, resolved by the graph.

Remember to enable the option if you wish to add any of them to your SBOM!

Customizing components
^^^^^^^^^^^^^^^^^^^^^^

Both functions accept two optional dictionaries to enrich SBOM components. Keys can be
the package name, ``name/version``, the full Conan reference, the purl, or the
``bom-ref``. Matching values are merged in that order.

* ``extra_info``: extra CycloneDX component fields (for example ``description``,
  ``supplier``, ``properties``).
* ``cpes``: CPE 2.3 strings. If omitted, Conan uses
  ``cpe:2.3:a:*:<name>:<version>:*:*:*:*:*:*:*``. A ``*`` version in a custom CPE
  is replaced with the Conan version.

.. code-block:: python

    from conan.tools.sbom import cyclonedx_1_6

    sbom = cyclonedx_1_6(
        conanfile,
        extra_info={
            "zlib": {
                "description": "Compression library",
                "supplier": {"name": "Jean-loup Gailly and Mark Adler"},
            },
        },
        cpes={"zlib": "cpe:2.3:a:gnu:zlib:*:*:*:*:*:*:*:*"},
    )

.. seealso::

    - :ref:`Software Bills of Materials (SBOM) <security_sboms>`.
    - :ref:`Generate SBOMs with the built-in deployers<reference_extensions_deployer_cyclone>`.
