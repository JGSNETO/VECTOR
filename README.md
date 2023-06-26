# CAN(Controller Area Network)

## Introduction
- Starting from the second half of eights more and more eletronics are added into the car.
- Beginning: 3 ECUs(Switchs, Potenciometers connected diretly to indicator lamps and gauges).
- Now: > 70.
- Due the old technology, each signal(analog info) has its own wire. So the cable harnesses grew in thickness and weight and suffered when bent. Problems with the large number of cable connectors plus costs for raw materials. Many components = Much communication = Much effort.

 - Advantages of digital transmission:
1. Logical separation of functions by informing coding(1 bit = 2 states 0/1)
2. Medium is shared in time for several functions and used more efficiently
3. Easier physical troubleshooting(less "cables")

## A/D conversion 
- The classic sensor measurement variables such as oltage, pressure, temperature, require a certain accuracy depending on the application. Due to the digital conversion, however, this is liited/lossy(quantization effect). In pratice, there is again a sufficient accuracy for ach application, which can be achieved accoringly by adding more bits.
- The arger the required value range, the more bits.
- Digital transmission is based on the binary number system. This way, physical values such as the fuel sensor level can be encoded using only ones and seroes.

## Transmission of bits using copper wire
- For the transmission of binary data, different voltages are applied to the wire to represent 0 and 1.
- Boundary conditions need to be met: Compliance with the defined voltage levels. Bit duration/Transmission Speed.
- The signal quality varies with the physical boundary conditions.
- FOr real systems, the decision aabout which strategy to use has to be done when defining the network. In the case of CAN communication, this is defined by OSI 11898-1 such that the first bit sent has the highest significance - in this picture this is the leftmost bit.

## Bus network
- Almost "any" number of signals can be transported on the same path.
- Advantages:
1. Wiring harnesses remain manageable and light
2. Faults can be diagnosed
3. Extensions of the network are easy

## OSI reference model and CAN
7. Application layer: Application Services
6. Presentation layer
5. Session layer
4. Transport layer: Segmentation and assembling, flow control
3. Network layer:Routing, Extended Address, Synchronization
2. Data link layer: Framing, Addressing, Bus access, data protection
1. Physical layer: Transmission, Topologies

- The standardized ISO reference model is a good basis to develop and describe communication protocols. Complex tasks of higher protocols are deconstructed into separate services which then are organized as individual layers and so become manageable and easier to understand.

**Add picture**
