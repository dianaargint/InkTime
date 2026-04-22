# InkTime

InkTime este un smartwatch open-source cu consum redus, realizat in jurul microcontroller-ului **nRF52840**. Proiectul include un **display e-paper**, alimentare din **baterie Li-Po**, **incarcare prin USB-C**, **monitorizare baterie**, **IMU** si **feedback haptic**.

Scopul proiectului a fost realizarea unei placi compacte care sa poata fi integrata intr-o carcasa dedicata si sa poata functiona din baterie, cu un consum cat mai mic.

# Diagrama bloc

```text
                                +----------------------+
                                |       USB-C          |
                                |   alimentare / USB   |
                                +----------+-----------+
                                           |
                                           v
                                +----------------------+
                                |   ESD Protection     |
                                |    USBLC6-2SC6Y      |
                                +----------+-----------+
                                           |
                                           v
                                +----------------------+
                                |  Li-Po Charger       |
                                |    BQ25180YBGR       |
                                +----+------------+----+
                                     |            |
                                     |            +--------------------+
                                     |                                 |
                                     v                                 v
                           +-------------------+             +-------------------+
                           |    Li-Po Battery  |             |   Fuel Gauge      |
                           |     LP502030      |<-- I2C ---> |    MAX17048       |
                           +---------+---------+             +-------------------+
                                     |
                                     v
                           +-------------------+
                           |  Buck-Boost DC/DC |
                           |    RT6160AWSC     |
                           +---------+---------+
                                     |
                                   3V3 / VREG
                                     |
        +----------------------------+------------------------------+
        |                            |                              |
        v                            v                              v
+---------------+          +-------------------+          +-------------------+
|   nRF52840    |<--I2C--->|      BMA421       |<--I2C--->|     DRV2605       |
| BLE MCU       |          |       IMU         |          |   Haptic Driver   |
+-------+-------+          +-------------------+          +---------+---------+
        |                                                              |
        | SPI + GPIO                                                    v
        v                                                      +----------------+
+-------------------+                                          | Vibration Motor |
| 1.54" E-Paper     |                                          |     shaker      |
| via FPC connector |                                          +----------------+
+-------------------+
        |
        +--> etaj dedicat pentru EPD:
             MOSFET-uri + diode Schottky + inductoare + condensatori
