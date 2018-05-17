Rotary Encoder Sensor
=====================

The ``rotary_encoder`` sensor platform allows you to use any continuous-rotation
rotary encoders with esphomeyaml. These devices usually have two pins with which
they encode the rotation. Every time the knob of the rotary encoder is turned, the
signals of the two pins go HIGH and LOW in turn. See
`this Arduino article <https://playground.arduino.cc/Main/RotaryEncoders>`__ to gain
a better understanding of these sensors.

.. figure:: /esphomeyaml/components/sensor/images/rotary_encoder-full.jpg
    :align: center
    :width: 75.0%

    Example of a continuous rotary encoder. Pin ``+`` is connected to ``3.3V``,
    ``GND`` is connected to ``GND``, and ``CLK`` & ``DT`` are A & B.

.. figure:: /esphomeyaml/components/sensor/images/rotary_encoder-ui.png
    :align: center
    :width: 75.0%

To use rotary encoders in esphomeyaml, first identify the two pins encoding th step value.
These are often called ``CLK`` and ``DT`` as in above image. Note if the values this sensor
outputs go in the wrong direction, you can just swap these two pins.

.. code:: yaml

    # Example configuration entry
    sensor:
      - platform: rotary_encoder
        name: "Rotary Encoder"
        pin_a: D1
        pin_b: D2

Configuration variables:
~~~~~~~~~~~~~~~~~~~~~~~~

-  **pin_a** (**Required**, `Pin Schema </esphomeyaml/configuration-types.html#pin-schema>`__):
   The first pin for determining the step value. Must not be a pin from an external I/O expander.
-  **pin_b** (**Required**, `Pin Schema </esphomeyaml/configuration-types.html#pin-schema>`__):
   The second pin for determining the step value. Must not be a pin from an external I/O expander.
-  **name** (**Required**, string): The name of the rotary encoder sensor.
-  **pin_reset** (*Optional*, `Pin Schema </esphomeyaml/configuration-types.html#pin-schema>`__):
   An optional pin that resets the step value. This is useful with rotary encoders that have have a
   third pin. Defaults to no reset pin.
-  **id** (*Optional*,
   `id </esphomeyaml/configuration-types.html#id>`__): Manually specify
   the ID used for code generation.
-  All other options from
   `Sensor </esphomeyaml/components/sensor/index.html#base-sensor-configuration>`__
   and `MQTT
   Component </esphomeyaml/components/mqtt.html#mqtt-component-base-configuration>`__.

Throttling Output
~~~~~~~~~~~~~~~~~

This sensor can output a lot of values in a short period of time when turning the knob.
In order to not put too much stress on your network connection, you can leverage esphomelib's
sensor filters. The following will only send out values once every half second max:

.. code:: yaml

    # Example configuration entry
    sensor:
      - platform: rotary_encoder
        name: "Rotary Encoder"
        pin_a: D1
        pin_b: D2
        filters:
          - throttle: 0.5s