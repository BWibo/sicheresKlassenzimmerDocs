###############################################################################
SensorThingsAPI Example entries
###############################################################################

*******************************************************************************
Thing
*******************************************************************************

.. code-block:: json
  :caption: Example *Thing*, ``@iotID = 1``

  {
    "name": "SikLa Test Thing",
    "description": "I am for testing and create random data.",
    "properties": {
      "frequency": "30 s",
      "DevEUI": "XXXq235454XXXXX"
    },
    "Locations": [
      {
        "name": "TUM, Arcisstr. 21 - Testlocation",
        "description": "Test location",
        "encodingType": "application/vnd.geo+json",
        "location": {
          "type": "Point",
          "coordinates": [
            11.568654,
            48.148653
          ]
        }
      }
    ]
  }

*******************************************************************************
Sensor
*******************************************************************************

.. code-block:: json
  :caption: Example *Sensor*, ``@iotID = 1``

  {
    "name": "Example Sensor",
    "description": "I am a example sensor generating random data between 0 and 40.",
    "encodingType": "text/html",
    "metadata": "https://de.wikipedia.org/wiki/Zufallszahlengenerator"
  }

*******************************************************************************
ObservedProperty
*******************************************************************************

.. code-block:: json
  :caption: Example *ObservedProperty*, ``@iotID = 1``

  {
    "name": "Example physical property",
    "definition": "https://en.wikipedia.org/wiki/Nothing",
    "description": "I'm not real."
  }


*******************************************************************************
Datastream
*******************************************************************************

.. code-block:: json
  :caption: Example *Datastream*, ``@iotID = 1``

  {
    "name": "Example Datastream",
    "description": "I am an example datastream.",
    "observationType": "http://www.opengis.net/def/observationType/OGC-OM/2.0/OM_Measurement",
    "unitOfMeasurement": {
      "name": "Centigrade",
      "symbol": "C",
      "definition": "http://www.qudt.org/qudt/owl/1.0.0/unit/Instances.html#DegreeCentigrade"
    },
    "Thing": {
      "@iot.id": 1
    },
    "ObservedProperty": {
      "@iot.id": 1
    },
    "Sensor": {
      "@iot.id": 1
    }
  }
