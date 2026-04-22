# InkTime

InkTime este un smartwatch open-source cu consum redus, realizat in jurul microcontroller-ului **nRF52840**. Proiectul include un **display e-paper**, alimentare din **baterie Li-Po**, **incarcare prin USB-C**, **monitorizare baterie**, **IMU** si **feedback haptic**.

Scopul proiectului a fost realizarea unei placi compacte care sa poata fi integrata intr-o carcasa dedicata si sa poata functiona din baterie, cu un consum cat mai mic.

# Diagrama bloc

```text
+-------------+      +----------------+      +------------------+
| USB-C       |----->| ESD Protection |----->| BQ25180 Charger  |
+-------------+      +----------------+      +--------+---------+
                                                        |
                                                        v
                                                +---------------+
                                                | Li-Po Battery |
                                                +---+-------+---+
                                                    |       |
                                                    |       v
                                                    |   +-----------+
                                                    |   | MAX17048  |
                                                    |   | FuelGauge |
                                                    |   +-----------+
                                                    v
                                              +-------------+
                                              | RT6160 DC/DC|
                                              +------+------+ 
                                                     |
                                                     v
                                              +-------------+
                                              | nRF52840    |
                                              | BLE MCU     |
                                              +------+------+-------------------+
                                                     |      |         |         |
                                                     |      |         |         |
                                                     v      v         v         v
                                              +--------+ +------+ +-------+ +--------+
                                              | E-Paper| | IMU  | |Haptic | | Buttons|
                                              +--------+ +------+ +---+---+ +--------+
                                                                        |
                                                                        v
                                                                   +---------+
                                                                   | Shaker  |
                                                                   +---------+


```

## Bill of Materials (BOM)

| Component | Value / Part Number | Package | Qty | Description | Datasheet |
| --------- | ------------------- | ------- | --- | ----------- | --------- |
| MCU | NRF52840_QF | NORDIC_NRF_4_AQFN50P700X700X85_HS-74N | 1 | Main BLE microcontroller | [Datasheet](https://infocenter.nordicsemi.com/pdf/nRF52840_PS_v1.1.pdf) |
| PMIC charger | BQ25180YBGR | BQ25180YBGR_BGA8C40P2X4_100X155X50 | 1 | Li-Po charger | [Datasheet](https://www.ti.com/lit/ds/symlink/bq25180.pdf) |
| DC/DC | RT6160AWSC | RT6160AWSC_BGA15C40P3X5_140X230X60 | 1 | Buck-boost DC/DC converter | [Datasheet](https://www.richtek.com/assets/product_file/RT6160A/DS6160A-05.pdf) |
| Fuel gauge | MAX17048G+T10 | ESP32_C6_LIBRARY_SON50P200X200X80-9N | 1 | Battery fuel gauge | [Datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/max17048-max17049.pdf) |
| IMU | BMA421 | BMA423_BMA423 | 1 | Triaxial accelerometer | [Datasheet](https://www.bosch-sensortec.com/products/motion-sensors/accelerometers/bma421/) |
| Haptic driver | DRV2605YZFR | DRV2605YZFR_BGA9C50P3X3_144X144X62 | 1 | Haptic driver for vibration motor | [Datasheet](https://www.ti.com/lit/ds/symlink/drv2605.pdf) |
| USB ESD | USBLC6-2SC6Y | ESP32_C6_LIBRARY_3_SOT95P280X145-6N | 1 | USB ESD protection | [Datasheet](https://www.st.com/resource/en/datasheet/usblc6-2.pdf) |
| Antenna | 2450AT18B100E | 2450AT18B100E_ANTC3216X140N | 1 | 2.4 GHz chip antenna | [Datasheet](https://www.johansontechnology.com/datasheets/antennas/2450AT18B100.pdf) |
| Display connector | 503480-2400 | 503480-2400_5034802400 | 1 | FPC connector for e-paper display | [Datasheet](https://www.molex.com/en-us/products/part-detail/5034802400) |
| USB-C | KH-TYPE-C-16P | KH-TYPE-C-16P_KINGHELM_KH-TYPE-C-16P | 1 | USB-C connector | - |
| Debug connector | TC2030-IDC | TC2030-IDC_TC2030IDC | 1 | SWD programming / debug connector | [Datasheet](https://www.tag-connect.com/product/tc2030-idc) |
| EPD MOSFET | SI1308EDL-T1-GE3 | ESP32_C6_LIBRARY_6_SOT65P210X110-3N | 1 | MOSFET used in e-paper drive stage | [Datasheet](https://www.vishay.com/docs/68986/si1308edl.pdf) |
| EPD MOSFET | DMG2305UX-7 | ESP32_C6_LIBRARY_4_ESP32_WROVER_SPARKFUN-DISCRETESEMI_MOSFET | 1 | MOSFET used in e-paper drive stage | [Datasheet](https://www.diodes.com/assets/Datasheets/DMG2305UX.pdf) |
| Schottky diode | MBR0530 | ESP32_C6_LIBRARY_5_SOD3716X135N | 3 | Schottky diode used in EPD boost stage | [Datasheet](https://www.onsemi.com/pdf/datasheet/mbr0530t1-d.pdf) |
| Power inductor | FTC252012SR47MBCA | MLP2016SR47MT0S1_INDC2016X100N | 1 | Inductor for RT6160 converter | [JLC Part](https://jlcpcb.com/partdetail/6763488-FTC252012SR47MBCA/C5832368) |
| EPD inductor | 68uH | ESP32_C6_LIBRARY_2_IND_4828-WE-TPC_WRE | 1 | Inductor for e-paper boost stage | - |
| RF inductor | 3.9nH | NORDIC_NRF_2_RESC0402_L | 1 | RF matching network | - |
| RF inductor | 15nH | NORDIC_NRF_2_RESC0402_L | 1 | RF filter / matching | - |
| Power inductor | 10uH | NORDIC_NRF_2_RESC0402_L | 1 | Power stage passive | - |
| HF crystal | 32MHz | NORDIC_NRF_BT-XTAL_2016_N | 1 | Main MCU clock | - |
| LF crystal | 32.768kHz | NORDIC_NRF_1_XTAL_3215_N | 1 | RTC / low-power clock | - |
| Buttons | EVP-AKE31A | 2025-10-22_07-23-44_LIBRARY_SW_EVP-AKE31A_PAN | 3 | Tactile user buttons | - |
| Solder jumper | SJ | ESP32_C6_LIBRARY_7_JUMPER_SJ | 1 | Configuration jumper | - |
| Test pads | HECTOR_WATCH_1_TPTP20R | HECTOR_WATCH_1_TP20R | 14 | Test points for debug / power / signals | - |

BOM-ul complet generat automat se gaseste in folderul Manufacturing.

## Pinii folositi de nRF52840

| Pin nRF52840 | Semnal | Componenta / destinatie | Rol |
| ------------ | ------ | ----------------------- | --- |
| P0.00 / XL1 | XL1 | Cristal 32.768kHz | clock low-power / RTC |
| P0.01 / XL2 | XL2 | Cristal 32.768kHz | clock low-power / RTC |
| P0.02 | SCK | Display e-paper | SPI clock pentru display |
| P0.03 | MOSI | Display e-paper | SPI data pentru display |
| P0.05 | EPD_CS | Display e-paper | chip select pentru display |
| P0.06 | SDA | BMA421 / DRV2605 / MAX17048 / BQ25180 / RT6160 | magistrala I2C comuna |
| P0.07 | SCL | BMA421 / DRV2605 / MAX17048 / BQ25180 / RT6160 | magistrala I2C comuna |
| P0.08 | IMU_INT1 | BMA421 | interrupt de la IMU |
| P0.10 | ALERT | MAX17048 | alerta fuel gauge |
| P0.11 | PMIC_INT | BQ25180YBGR | interrupt de la charger |
| P0.12 | HAPTIC_EN | DRV2605YZFR | enable pentru driverul haptic |
| P0.13 | SW_UP | Buton UP | input utilizator |
| P0.14 | SW_ENT | Buton ENTER | input utilizator |
| P0.15 | EPD_DC | Display e-paper | selectare data / command |
| P0.16 | EPD_RST | Display e-paper | reset display |
| P0.17 | EPD_BUSY | Display e-paper | busy status de la display |
| P0.18 / RESET | RESET | Sistem | reset hardware |
| P1.00 | SWO | Debug | trace output |
| P1.02 | SW_DN | Buton DOWN | input utilizator |
| P1.08 | IMU_INT2 | BMA421 | al doilea interrupt de la IMU |
| SWDIO | SWDIO | TC2030-IDC | programare / debug |
| SWDCLK | SWDCLK | TC2030-IDC | programare / debug |
| ANT | RF | Antena 2.4 GHz | transmisie BLE |
| VBUS | VBUS | USB-C | alimentare 5V |

Am ales pinii in asa fel incat sa separ cat mai clar liniile de comunicatie de semnalele de control. Display-ul foloseste SPI pentru transferul de date, iar semnalele EPD_CS, EPD_DC, EPD_RST si EPD_BUSY sunt separate pentru controlul corect al panoului. IMU-ul, fuel gauge-ul, charger-ul, convertorul DC/DC si driverul haptic folosesc aceeasi magistrala I2C, ceea ce reduce numarul de pini ocupati. Butoanele sunt puse pe GPIO-uri dedicate, iar liniile SWDIO, SWDCLK si SWO sunt folosite pentru programare si debugging.

## Descriere hardware
1. Microcontroller

Microcontroller-ul principal al proiectului este nRF52840 (U1). Acesta este componenta centrala a sistemului si se ocupa de:

comunicatia BLE, controlul display-ului e-paper, comunicatia cu senzorii si circuitele de power prin I2C, citirea butoanelor, controlul driverului haptic, interfata de debug SWD

Pentru clock sunt folosite doua cristale:

X1 = 32MHz
X2 = 32.768kHz

Cristalul de 32MHz este folosit pentru functionarea principala a MCU-ului si pentru partea de radio, iar cristalul de 32.768kHz este folosit pentru RTC si modurile low-power.

2. Alimentare si management energetic

Partea de alimentare am impartit-o in trei blocuri principale:

BQ25180YBGR

IC1 = BQ25180YBGR se ocupa de incarcarea bateriei Li-Po din USB-C. Intrarea de 5V vine din VBUS, iar iesirea merge spre baterie. Circuitul are si semnal de interrupt catre MCU pentru monitorizarea starii de incarcare.

MAX17048

U3 = MAX17048G+T10 monitorizeaza tensiunea bateriei si comunica prin I2C cu nRF52840. Are si semnal de alerta catre MCU pentru praguri critice sau schimbari de stare.

RT6160AWSC

IC9 = RT6160AWSC este convertorul buck-boost care genereaza alimentarea stabila 3V3 / VREG pentru sistem. Acesta este necesar deoarece tensiunea bateriei variaza in functie de starea de incarcare, iar sistemul are nevoie de o alimentare stabila.

3. Bateria

Am folosit o baterie de tip LP502030, 3.7V, aproximativ 250mAh. In proiect, bateria nu este conectata prin conector JST, ci direct la doua test pad-uri:

TP_VBAT, TP_BAT_GND

Am ales varianta asta ca sa economisesc spatiu si pentru ca asta era si cerinta proiectului.

4. Display e-paper

Display-ul este conectat prin conectorul 503480-2400 si este controlat prin:

SCK, MOSI, EPD_CS,EPD_DC, EPD_RST, EPD_BUSY


5. IMU

IMU-ul folosit este BMA421 (IC3) si comunica prin I2C. Are doua semnale de interrupt:

IMU_INT1
,IMU_INT2

Aceste semnale sunt conectate la MCU pentru detectarea rapida a evenimentelor de miscare si pentru trezirea sistemului din moduri low-power.

6. Haptic

Pentru feedback haptic este folosit DRV2605YZFR (IC2), care comanda motorul de vibratie. Comunicarea este facuta pe I2C, iar activarea poate fi controlata si printr-un semnal GPIO dedicat.

7. USB-C si protectie ESD

Conectorul J4 = KH-TYPE-C-16P este folosit pentru alimentare si optional pentru interfata USB. Protectia ESD este facuta cu USBLC6-2SC6Y, pentru a proteja liniile sensibile la descarcari electrostatice.

8. RF si antena

Antena este ANT1 = 2450AT18B100E si este conectata la pinul RF al nRF52840 printr-o retea de matching. Antena este pusa la marginea placii, iar sub ea am lasat zona fara plan de masa si fara trasee, conform regulilor de proiectare RF.


## PCB si layout
Board specifications
- dimensiunea placii: 46 mm x 35 mm
- grosimea PCB-ului: 1 mm
- componentele sunt plasate pe TOP
- rutarea este facuta pe Top si Bottom
- exista poligoane de GND pe Top si Bottom
  
Routing rules

- traseele de semnal au fost rutate cu minimum 0.15 mm
- traseele de putere au fost rutate cu 0.3 mm acolo unde spatiul a permis
- in zonele foarte dense, unele segmente scurte au trebuit ingustate local
  
Ground planes

Am folosit planuri de masa pe placa, iar in zonele importante am pus si via stitching, mai ales in jurul zonei RF si al microcontroller-ului.

Antena si keepout

Antena este pusa la marginea PCB-ului, iar sub ea:

- nu exista plan de masa
- nu exista trasee
- nu exista turnare de cupru

Acest lucru a fost facut pentru a nu afecta performanta RF.
Pe parcursul proiectarii au aparut mai multe probleme, mai ales in zonele cu densitate mare de componente:

- zona din jurul nRF52840 a fost dificil de rutat
- integrarea bateriei, display-ului si shaker-ului in acelasi volum a fost dificila

## Integrare mecanica si 3D

In modelul 3D au fost incluse:
- PCB-ul
- carcasa
- bateria
- display-ul
- shaker-ul

Au fost verificate:
- alinierea USB-C
- alinierea butoanelor
- pozitia display-ului fata de carcasa
- spatiul pentru baterie si shaker
