# Humanoid Robot CAN & Power Controller

Custom electronics platform developed for a humanoid robotic system: multi-channel CAN/CAN-FD communication, USB-C integration with a central Linux computer, 48 V power distribution, regulated auxiliary power rails, hardware protection, remote debugging, and validation on real robot hardware.

This project was developed by **GEC Engineering**.  
**Lead Engineer: Oleg Gridin, BEng**

---

## Project Overview

The client required a compact, integrated electronics solution for a mobile humanoid robot powered from a 48 V battery and controlled by an NVIDIA Jetson Orin Nano.

The robot architecture includes multiple motorized limbs, RobStride actuators, servo-driven joints, auxiliary mechanisms and end-effectors. Instead of using multiple independent communication adapters and separate power modules, the goal was to consolidate the communication and power-distribution functions into a dedicated custom electronics platform.

The development therefore focused on two main subsystems:

1. **Multi-channel CAN / CAN-FD communication board**
2. **48 V Power Distribution Board (PDB)**

The communication subsystem provides several independent CAN/CAN-FD channels and connects them to the robot's central Linux computer through USB-C.

The power subsystem accepts the main 48 V battery supply, distributes high-current power to the robot limbs, and generates regulated auxiliary voltage rails for logic, sensors and other onboard electronics.

The resulting electronics were manufactured and assembled by the client, integrated into the physical robot hardware, remotely debugged together with GEC Engineering, modified after the first hardware tests, and successfully brought to full intended functionality.

---

## Client Requirements

The original technical requirements defined a humanoid mobile robot with:

- 48 V main battery system
- NVIDIA Jetson Orin Nano as the central computer
- multiple motorized limbs
- RobStride motors
- CAN / CAN-FD communication
- several independent communication channels
- USB-C connection to the central computer
- 48 V power distribution to the limbs
- regulated 24 V, 12 V and 5 V rails
- individual protection of power outputs
- compact integration inside the robot body
- compatibility with Linux SocketCAN
- hardware suitable for real robot operation and service

The communication architecture was intended to support up to **6 independent CAN channels** operating simultaneously.

The power architecture was intended to support:

- 4 main robot limbs
- 2 reserve channels
- separate protected high-current power paths
- auxiliary regulated power for electronics and sensors

---

## System Architecture

The final electronics concept combines communication and power-management functions required by the robot.

### Central Computer

The robot uses an **NVIDIA Jetson Orin Nano** as the high-level controller.

Its responsibilities include:

- high-level robot control
- actuator communication
- Linux-based software
- SocketCAN interface
- sensor and peripheral integration
- robot behavior and computation

### Communication Layer

The custom communication board connects multiple independent CAN/CAN-FD buses to the Jetson through a common USB interface.

Conceptually:

```text
Robot Limb / Actuator Bus #1 --+
Robot Limb / Actuator Bus #2 --+
Robot Limb / Actuator Bus #3 --+
Robot Limb / Actuator Bus #4 --+--> Custom Multi-CAN Board --> USB-C --> Jetson Orin Nano
CAN Bus #5 (reserve) -----------+
CAN Bus #6 (reserve) -----------+
```

### Power Layer

The main robot power system is based on a nominal **48 V battery**.

Conceptually:

```text
48 V Battery
     |
     v
Custom Power Distribution Board
     |
     +--> 48 V Limb Power Bus #1
     +--> 48 V Limb Power Bus #2
     +--> 48 V Limb Power Bus #3
     +--> 48 V Limb Power Bus #4
     +--> Reserve Power Output #1
     +--> Reserve Power Output #2
     |
     +--> Regulated 24 V
     +--> Regulated 12 V
     +--> Regulated 5 V
```

---

## Board 1 — Multi-CAN / USB-C Communication System

The first part of the development is the communication board.

Its purpose is to replace multiple separate USB-CAN adapters with a compact integrated solution.

### Main Functions

- up to 6 independent CAN / CAN-FD channels
- simultaneous communication on all channels
- USB-C interface to the central Linux computer
- Linux SocketCAN compatibility
- independent CAN transceiver for each channel
- CAN-line protection
- termination support
- compact robot-integrated PCB format
- onboard status indication

### CAN / CAN-FD Requirements

The development requirements included support for:

- Classical CAN 2.0B
- CAN FD / FDCAN
- arbitration rate up to approximately 1 Mbit/s
- higher CAN-FD data rate where required by connected devices
- simultaneous independent operation of the channels

The board was intended to communicate with robot actuators and other CAN-based devices used in the humanoid platform.

---

## candleLight / SocketCAN Architecture

For the USB-CAN communication architecture, the project used the open-source **candleLight firmware ecosystem** as an important compatibility reference.

Reference project:

https://github.com/candle-usb/candleLight_fw

The goal was not to use a collection of external adapters inside the robot, but to integrate the required USB-CAN functionality into the custom electronics architecture.

The communication concept was designed around:

- Linux SocketCAN compatibility
- standard USB-CAN operation
- integration with Jetson Orin Nano
- multiple independent CAN channels
- compact custom PCB implementation

This approach allowed the robot software to interact with the communication subsystem through a familiar Linux CAN interface.

---

## Board 2 — 48 V Power Distribution Board

The second major part of the electronics development is the high-power distribution section.

The PDB receives the main robot battery supply and distributes it between the robot's limbs and auxiliary systems.

### Input

- nominal voltage: **48 V DC**
- operating range: approximately **40–60 V DC**

### Main 48 V Outputs

The architecture provides multiple high-current outputs:

- Arm 1
- Arm 2
- Leg 1
- Leg 2
- Reserve output 1
- Reserve output 2

Each limb is treated as an independent power bus.

### Regulated Auxiliary Power

The board also provides regulated rails for auxiliary electronics:

- **24 V**
- **12 V**
- **5 V**

These rails are intended for:

- logic electronics
- sensors
- actuators
- communication devices
- auxiliary mechanisms
- end-effector electronics

### Power Protection

The design requirements included:

- individual output protection
- fuse / eFuse concepts
- transient protection
- reverse-polarity protection
- filtering
- thermal management
- high-current PCB routing
- stable low-voltage supply for sensitive electronics

---

## Robot Load Analysis

Before the PCB design was finalized, the expected actuator loads were analyzed.

The robot uses multiple high-power motors per limb.

The design documentation considered:

- multiple RobStride motors per arm
- multiple RobStride motors per leg
- additional wheel-drive load
- simultaneous and peak-current conditions
- voltage-drop risks
- current limiting
- distribution of power between independent limb buses

The theoretical maximum system load was significantly higher than the expected normal operating load, so the power-distribution architecture had to consider current management, protection and load balancing.

This load analysis directly influenced:

- connector selection
- copper and power-path design
- protection components
- DC/DC converter selection
- thermal design
- power-bus separation

---

## PCB Design

The PCB was designed as a custom robot-specific electronics platform rather than as a generic development board.

The design work included:

- system architecture
- schematic design
- PCB layout
- power-path routing
- communication-channel routing
- connector positioning
- component selection
- protection circuitry
- regulated power sections
- CAN interfaces
- USB interface
- status indication
- manufacturing preparation

The development requirements were oriented toward practical PCB manufacturing and integration into the robot.

---

## 3D PCB Design

The PCB was modeled and reviewed in 3D before manufacturing.

The 3D design stage was used to verify:

- connector positions
- component clearances
- board proportions
- integration constraints
- large power components
- access to robot wiring
- mechanical compatibility

The project photographs include 3D PCB renders showing both the power-conversion section and the multi-channel communication section.

---

## Manufacturing and Hardware Assembly

After the electronics design was completed, the client manufactured and assembled the physical hardware.

This allowed the project to move from schematic and PCB design into real-world validation.

The assembled board was then connected to:

- the robot power system
- multiple motors
- communication lines
- test equipment
- the mechanical robot platform

---

## Remote Hardware Debugging

After the first hardware assembly, GEC Engineering worked with the client remotely to perform system bring-up and debugging.

The debugging process included:

- voltage measurements
- checking power rails
- checking PCB nodes
- communication verification
- identifying hardware behavior
- comparing real measurements with the design
- guiding the client through measurement and modification procedures

The project photographs show this stage with the client performing measurements directly on the physical PCB using a multimeter and test probes.

---

## Hardware Modification After Initial Testing

During the first real-hardware debugging stage, a small PCB modification was identified as necessary.

GEC Engineering provided the client with instructions for the modification.

The client then performed the hardware correction directly on the manufactured PCB according to these instructions.

After this modification:

- the board was tested again
- the communication and power sections were verified
- the system reached the intended functional state

This step is an important part of the project because it demonstrates not only PCB design, but also practical hardware bring-up, fault analysis and engineering support after manufacturing.

---

## Robot Integration

After debugging, the board was installed into the physical robot assembly.

The photographs show the electronics mounted on the robot structure with multiple motors connected.

This confirms that the project progressed beyond PCB design and was integrated into a real electromechanical system.

The integrated hardware includes:

- custom PCB
- robot structural components
- multiple connected motors
- power wiring
- communication wiring
- active electronics

---

## Final Functional Validation

After the PCB modification and repeated testing, the system achieved full intended functionality.

The final validation photographs show:

- the powered PCB
- active status LEDs
- multiple communication channels
- connected motor wiring
- power distribution operating
- the electronics installed in the robot system

The successful bring-up demonstrates the complete engineering cycle:

```text
Client Requirements
        ↓
System Architecture
        ↓
Electrical Design
        ↓
PCB Design
        ↓
Manufacturing
        ↓
Hardware Assembly
        ↓
Remote Debugging
        ↓
PCB Modification
        ↓
Robot Integration
        ↓
Final Functional Validation
```

---

## Main Engineering Work Performed by GEC Engineering

GEC Engineering was responsible for the electronics-development work based on the client's technical requirements.

The engineering scope included:

- analysis of the humanoid robot architecture
- power-budget analysis
- CAN / CAN-FD communication architecture
- USB-CAN integration concept
- SocketCAN compatibility planning
- custom PCB architecture
- schematic design
- PCB layout
- power-distribution design
- regulated power-rail design
- connector and interface planning
- component selection
- protection circuitry
- manufacturing preparation
- hardware bring-up support
- remote debugging
- hardware modification instructions
- system validation support

**Lead Engineer: Oleg Gridin, BEng**

---

## Related BMS Development

GEC Engineering has also developed dedicated battery-management monitoring and diagnostic systems for high-power robotic platforms.

This includes multi-BMS monitoring, RS-485 communication, diagnostics and battery-system integration.

Detailed BMS development is documented separately:

https://github.com/gridinwork/JK-BMS-PB2A16S-20P

---

## Project Files

This repository is intended to include the available engineering materials related to this development, including where applicable:

- test firmware
- schematic files
- PCB design files
- manufacturing files
- technical documentation
- hardware photographs
- debugging photographs
- robot integration photographs
- short validation video

Some project materials may be omitted or simplified where required by client confidentiality.

---

## Project Media

A short hardware validation video will be added to this repository.

The video demonstrates the physical electronics operating in the assembled robotic system.

---

# Русское описание

## Электроника гуманоидного робота — Multi-CAN управление и распределение питания

Компания **GEC Engineering** разработала специализированную электронную систему для мобильного гуманоидного робота по техническому заданию клиента.

**Главный инженер проекта — Oleg Gridin, BEng.**

Основной задачей было создать компактную интегрированную электронику, которая одновременно решает две большие задачи:

1. обеспечивает связь центрального компьютера робота с несколькими независимыми CAN/CAN-FD шинами;
2. распределяет основное питание 48 В между конечностями и формирует дополнительные стабилизированные напряжения для электроники.

---

## Архитектура робота

Центральным вычислителем системы является **NVIDIA Jetson Orin Nano**.

В составе робота используются:

- моторизированные руки и ноги;
- приводы RobStride;
- дополнительные сервоприводы и исполнительные механизмы;
- CAN / CAN-FD;
- питание 48 В;
- вспомогательные линии 24 В, 12 В и 5 В;
- различные датчики и конечные исполнительные устройства.

Для такой архитектуры требовалось отказаться от набора отдельных адаптеров и создать собственную компактную электронную платформу.

---

## Плата связи Multi-CAN / USB-C

Первая часть разработки — многоканальная коммуникационная система.

Она предназначена для одновременной работы нескольких независимых CAN/CAN-FD каналов и подключения их к Jetson Orin Nano через USB-C.

Основные функции:

- до 6 независимых CAN/CAN-FD каналов;
- одновременная работа всех каналов;
- USB-C подключение к Linux-компьютеру;
- совместимость с SocketCAN;
- отдельный CAN-трансивер на каждый канал;
- защита CAN-линий;
- терминаторы;
- индикация состояния;
- компактная интеграция в корпус робота.

---

## candleLight / SocketCAN

При разработке USB-CAN части использовалась архитектура open-source проекта **candleLight** как основа совместимости с Linux SocketCAN.

Исходный проект:

https://github.com/candle-usb/candleLight_fw

Задачей было не просто установить несколько готовых адаптеров, а интегрировать необходимую функциональность в собственную PCB-систему робота.

---

## Плата распределения питания 48 В

Вторая основная часть разработки — силовая система.

Плата принимает питание от основного аккумулятора робота:

- номинально 48 В;
- рабочий диапазон примерно 40–60 В.

Основное питание распределяется по независимым силовым шинам:

- левая/правая рука;
- левая/правая нога;
- два резервных канала.

Также формируются стабилизированные линии:

- 24 В;
- 12 В;
- 5 В.

Они используются для питания логики, сенсоров, дополнительных приводов и вспомогательных узлов.

---

## Анализ нагрузки

Перед разработкой силовой части был выполнен расчёт нагрузки приводов.

В системе используется большое количество высокомощных двигателей RobStride и дополнительные моторы.

При проектировании учитывались:

- рабочие токи;
- пиковые токи;
- одновременная работа нескольких приводов;
- просадки напряжения;
- защита отдельных линий;
- распределение нагрузки;
- тепловые режимы;
- выбор разъёмов и силовой части PCB.

---

## Разработка PCB

Проект включал:

- анализ технического задания;
- разработку архитектуры;
- схемотехнику;
- PCB layout;
- силовую разводку;
- разводку CAN;
- USB-интерфейс;
- DC/DC преобразователи;
- защиту;
- подбор компонентов;
- расположение разъёмов;
- подготовку к производству.

Перед изготовлением плата была проверена в 3D.

---

## Производство и сборка

После завершения проектирования клиент изготовил реальную печатную плату и собрал оборудование.

Плата была подключена к реальным приводам и механической системе робота.

---

## Удалённая отладка

После сборки GEC Engineering совместно с клиентом выполнила удалённую отладку аппаратуры.

Клиент по нашим инструкциям выполнял измерения непосредственно на плате.

Проверялись:

- напряжения;
- силовые линии;
- отдельные узлы PCB;
- работа коммуникационных каналов;
- фактическое поведение оборудования.

---

## Модификация PCB после первых испытаний

В процессе первой аппаратной отладки была определена необходимость небольшой модификации уже изготовленной платы.

GEC Engineering подготовила инструкции.

Клиент самостоятельно выполнил модификацию по нашим указаниям.

После этого система была повторно проверена и достигла полного запланированного функционала.

---

## Интеграция в реального робота

После отладки плата была установлена в реальную роботизированную систему.

На фотографиях проекта видно:

- установленную PCB;
- механическую часть робота;
- подключённые двигатели;
- силовую проводку;
- работающую электронику.

Таким образом, проект не ограничился разработкой схемы и PCB — оборудование было физически изготовлено, подключено к реальной роботизированной системе и проверено клиентом.

---

## Финальная проверка

После внесения модификации и повторной отладки система работала в полном объёме.

Фотографии финального теста показывают:

- активное питание;
- работающую LED-индикацию;
- активные коммуникационные каналы;
- подключённые двигатели;
- работоспособность платы в реальной системе.

---

## Работы GEC Engineering

В рамках проекта GEC Engineering выполнила:

- анализ архитектуры гуманоидного робота;
- расчёт силовой нагрузки;
- архитектуру CAN/CAN-FD;
- концепцию USB-CAN;
- интеграцию SocketCAN;
- разработку собственной PCB;
- схемотехнику;
- PCB layout;
- проектирование силовой части;
- DC/DC питание 24/12/5 В;
- защиту силовых и сигнальных линий;
- подбор компонентов;
- подготовку к производству;
- техническую поддержку при сборке;
- удалённую аппаратную отладку;
- разработку инструкции по модификации PCB;
- поддержку финальной проверки оборудования.

**Главный инженер проекта — Oleg Gridin, BEng.**

---

## Связанный проект BMS

GEC Engineering также разработала отдельную систему мониторинга и диагностики BMS для мощных роботизированных платформ.

Подробное описание BMS-разработки:

https://github.com/gridinwork/JK-BMS-PB2A16S-20P

---

## Материалы проекта

В данный репозиторий будут добавлены доступные материалы проекта:

- тестовая прошивка;
- схемы;
- PCB-файлы;
- производственные файлы;
- техническая документация;
- фотографии разработки;
- фотографии удалённой отладки;
- фотографии модификации PCB;
- фотографии интеграции платы в робота;
- короткое видео проверки оборудования.

Часть материалов может быть сокращена или не опубликована из-за конфиденциальности клиента.

---

## Developer

**GEC Engineering**  
**Lead Engineer — Oleg Gridin, BEng**

Website: https://gec-engineering.tech/  
YouTube: https://www.youtube.com/@GEC_Company  
GitHub: https://github.com/gridinwork
