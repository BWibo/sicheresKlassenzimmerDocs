.. index:: LoRaWAN

###############################################################################
LoRaWAN
###############################################################################


.. index:: LoRaWAN gateway traffic

*******************************************************************************
Abschätzung Gateway Traffic
*******************************************************************************
Die Abschätzung erfolgt auf Basis folgender Eingangsparameter.

Schulen und Sensorknoten
===============================================================================

Pilotphase
-------------------------------------------------------------------------------

* 30 - 40 Schulen |:school:| mit jeweils 4 Klassenzimmern pro Schule und einem
  Sensorknoten pro Klassenzimmer |:arrow_right:|

  * 30 - 40 Gateways
  * 120 - 150 Sensorknoten

Nach der Pilotphase
-------------------------------------------------------------------------------

Es kommen weitere Schulen |:school:| und u.U. Kitas |:baby:| mit dazu.

* 30 - 40 Schulen mit jeweils 4 Klassenzimmern pro Schulen

  * 30 - 40 Gateways
  * 120 - 150 Sensorknoten

* Kitas

Sensordatenströme
===============================================================================

Folgende Tabelle enthält die Datenstrome die vom Sensorknoten zum
Gateway über das LoRa-Netz gesendet werden. Für die Abschätzung der Datengröße
wurde das `CayenneLPP <https://developers.mydevices.com/cayenne/docs/lora/#lora-cayenne
-low-power-payload>`_ Payload-Format verwendet. Hier fallen pro Datenstrom
zusätzlich 2 Bytes für den Datenkanal und den Datentyp an.


.. list-table:: Sensordatenströme - Sensorknoten |:arrow_right:| Gateway
  :header-rows: 1
  :name: lorawan_tab_1

  * - Name
    - Einheit
    - Datentyp
    - Größe (CayenneLPP)
    - Sendefrequenz
  * - Temperatur
    - °C
    - float
    - 4 B
    - 1 / min
  * - rel. Luftfeuchte
    - %H
    - float
    - 4 B
    - 1 / min
  * - CO2-Konzentration
    - ppm
    - u_int16
    - 4 B
    - 1 / min
  * - Beleuchtungsstärke
    - lm
    - u_int16
    - 4 B
    - 1 / min
  * - Bewegung
    - ?
    - ?
    - 4 B (Annahme)
    - 1 / min
  * - Batteriespannung
    - V
    - float
    - 4 B
    - 1 / 15 min
  * - Batterie Ladestatus (optional)
    -
    - u_int8
    - 1 B
    - 1 / 15 min
  * - **Summe**
    -
    -
    - :math:`25\ B + (7 \cdot 2\ B) = 39\ B`
    -
  * - *Worst Case*
    -
    -
    - *50 B*
    -

Zusätzlicher Traffic zwischen Gateway und Netzwerkserver
===============================================================================

* ~ 500 MB / Monat für Statusmeldungen, nach `TTN Forum <https://www.thething
  snetwork.org/forum/t/what-is-the-maximum-data-output-of-a-lorawan-gateway/6525/10>`_

* Overhead pro Datenpaket

.. warning:: Bei der Verwendung der Community-Lösung TTN werden auch Datenpakete
  von anderen Netzwerkteilnehmern empfangen und ggf. geroutet, was zusätzlichen
  Traffic verursacht. Da die Datenmenge aus LoRaWAN-Paketen relativ klein ist
  (vgl. :numref:`lorawan_tab_1`, :numref:`lorawan_tab_2`)

Folgende Datenströme fallen zusätzlich vom Gateway in die Cloud an und werden
via LTE übermittelt.
Hier ist die Abschätzung der Datenmengen abhängig vom verwendeten Format/Protokoll,
das zwischen Gateway und Netzwerkserver eingesetzt wird.

.. list-table:: Sensordatenströme - Gateway |:arrow_right:| Netzwerkserver
  :header-rows: 1
  :name: lorawan_tab_2
  :align: left

  * - Name
    - Einheit
    - Datentyp
    - Größe
    - Sendefrequenz
  * - SNR
    - dB
    - float
    - ?
    - 1 / min
  * - RSSI
    -
    - int16
    - ?
    - 1 / min
  * - CodingRate
    -
    - ?
    - ?
    - 1 / min
  * - Spreading Factor
    -
    - ?
    - ?
    - 1 / min
  * - Counter
    -
    - ?
    - ?
    - 1 / min
  * - Timestamps
    -
    - ?
    - ?
    - 1 / min
  * - weitere Daten
    -
    - ?
    - ?
    - 1 / min
  * - **Summe**
    -
    -
    - **? B**
    -
  * - *Worst Case*
    -
    -
    - *2-5 kB*
    -

Für die folgende Abschätzung des Overhead ist exemplarisch ein Datenpaket,
das von einem TTN-Netzwerkserver empfangen wurde abgebildet.

.. literalinclude:: lora-packet.json
  :name: lorawan_lst1
  :caption: Beispiel Paket LoRaWAN von einem TTN Netwerkserver v3. Größe ~ 1.9 kB.
  :language: json

Abschätzung Traffic
===============================================================================

.. rubric:: Pakete pro Monat pro Gateway

.. math::

  31\ \frac{d}{Monat} \cdot 4\ Nodes \cdot 60 \cdot 24\ \frac{min}{d} = 178.560\ \frac{Pakete}{Monat}

.. rubric:: Datenvolumen pro Paket

.. math::

    50\ B + 2\ kB\ \frac{Overhead}{Paket} = 2098\ \frac{B}{Paket} = 2.05\ \frac{kB}{Paket}

.. rubric:: Datenvolumen Sensordaten pro Monat pro Gateway

.. math::

  2.05\ \frac{kB}{Paket} \cdot 178.560\ \frac{Pakete}{Monat} = 358\ \frac{MB}{Monat}

.. rubric:: Datenvolumen Sensordaten + Overhead pro Monat pro Gateway

.. math::

  358\ \frac{MB}{Monat} + 500\ \frac{MB}{Monat} = 858\ \frac{MB}{Monat}

Zusammenfassung
===============================================================================

Die Abschätzung zeigt, dass das Datenvolumen in erster Linie vom Overhead zwischen
Gateway und Netzwerkserver abhängt. Das Datenvolumen der reinen Sensordaten ist
auch bei einem Datenpaket pro Minute relativ gering (~300 - 400 MB pro Monat und
Gateway). Der Overhead den ein Gateway pro Datenpaket und paketunabhängig erzeugt
ist schwer abzuschätzen und hängt stark von den verwendeten Protokollen, Formaten
und Metadaten ab. Eine Datentarif mit einem max. Volumen von 2 GB kostet aktuell
ca. 4 - 6 € (siehe `hier <check24_lte_>`_).

.. References #################################################################

.. _check24_lte: https://handytarife.check24.de/vergleich/datentarife?context=
  datatariffs&network_tmobile=yes&network_vodafone=yes&network_o2=yes&data_included
  =2000&minutes_included=all&rnp=egal&tariff_type=all&select_contract=-24&data_speed
  =0&maximum_effective_price=egal&provider_restriction=&5g=no&young_tariff
  =no&tariff_activation_date=no&hardware_nature=40&with_data_tariffs
  =all&only_bookable_tariffs=no&fixed_traffic_automatic=egal&sms_included=all
