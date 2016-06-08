NOTICE - This project has moved
===============================

It is now part of Foursquare's open source monorepo [Fsq.io](https://github.com/foursquare/fsqio) and all
future work will be published there.

The project lives on but this Github repo is deprecated.



Spindle Codegen Plugin
=================

A plugin for the Pants build system to use Foursquare's
`Spindle <https://github.com/foursquare/spindle>`_ to Thrift-to-Scala code generator.



Configuration
----------


``pants.ini``
^^^^^^^^^^^^^

The ``spindle-gen`` section of pants.ini must contain ``bootstrap-tools`` and ``scala_record``.

The list ``bootstrap-tools`` should include the address of a ``jar_library`` target (usually defined
in ``BUILD.tools``_) for the spindle codegen jar.

The dictionary ``scala_record`` should include a list ``deps`` of address specs to be added to  the
``dependencies`` of generated targets.

The minimal pants.ini additions to get Spindle working likely looks more or less like::

  [spindle-gen]
  bootstrap-tools: ['//:spindle-codegen']
  scala_record: {
    'target_language': 'scala',
    'template': 'scala/record.ssp',
    'javaTemplate': 'javagen/record.ssp',
    'options': '',
    'deps': [
        '3rdparty:spindle-runtime',
        '3rdparty:mongodb',
        '3rdparty:joda-time',
        '3rdparty:scalaj-collection',
      ]
    }


``BUILD.tools``
^^^^^^^^^^^^^^^
The ``bootstrap-tools`` config in ``pants.ini`` should point to a target that specifies a spindle
codegen jar::
  jar_library(
    name = 'spindle-codegen',
    jars = [
      jar(org = 'com.foursquare', name = 'spindle-codegen-binary_2.10', rev = '3.0.0-M5.1'),
    ],
  )
