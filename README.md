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

**Add picture** page 16

- Besides the ability to communicate with other nodes, real ECU's need additional software functions: Application, Diagnostics, Network management.

## Bus networking
- Can Bus Networking: Data protection/ CAN Framing/ Bus Access/ Addressing/ Physical Layer/Synchronization.
- Specific requirements of the OSI layers 1 and 2:
1. Serial transmission of digital data
2. Usage of a single medium with low cost
3. Simple and easy connection to the medium
4. Very high system reliability
5. High speed and virtually error free data transmission with high data consistency
6. Sandardized bus system

## Can Standard 
1. 11898-1: CAN Protocol(CAN + CAN FD)
2. 11898-2: High Speed Physical Layer(up to 1 Mbit/s for CAN, typicaly up to 5 Mbit/s for CAN FD)
3. 11898-3: Low Speed Physical Layer(up to 125kbit/s)

- CAN technology comprises the CAN and CAN FD protocols and two different CAN Physical Layers.

## Bit serial signal transmission in the electrical transmission medium singlw wire x two wire 
1. Single wire:
- Vertical flanks are physically not possible
- They are only schematically often drawn vertically
- Depending on the system: Voltage Level, Clocking, sampling point, slop.
- EMC(Electromagnetic Compatibility) interference can distort the defined working voltage to the point of misinterpretation 

2. Two-wire line:
- In systems with two lines, the data is transmitted mirror-inverted on twisted-pair lines.
- The lines are wisted for reasons of symmetry. This ensures that interference affects both lines equally.
- Now the interference can be easily eliminated.
- Twisted Pair: No misinterpretation despite EMC interference.

## Bit transmission 
- In CAN communication, the two possible bit values 0 and 1 are represented by exactly two physical constellations:
1. Transmission Medium: Twisted pair of wires
2. Physical Constellation: Voltage levels

- Bit duration = 1 / Bit rate
- The receiving ECUs measure and interpret the voltage differences between the two CAN-bus wires CAN_H and CAN_L.
- Once the bits has started, the receiver waits a configurable amount of time until it measures whether the bit is a 1 or a 0. At the time of this so-called sampling, the difference of the voltages on the two CAN wires needs to be in one of the two defined ranges. Anything outside of that, has to be interpreted as an error.
- Compared to its transmission time, the reception of a bit is always time delayed. Even the transmitter of a bit can read its own bit on the bus only with a time delay. The futher away receiving ECU are from the sender, the greater these time dalys become.
*** image from page 26 ***

## Structure of CAN-Bus and Electronic Control Unit 
1. Microcontroller:
- Application software
- Communicates with other ECUs via messages
2. CAN-Controller
- Completion of transmit messages
- Check of received messages
- Controls bus access and Bit-timing
3. CAN-Transceiver
-Transmission: Translation of bits to voltages
- Receipt: Voltage levels are sampled and forwarded to controller
*** Image from page 27 ***
- CAN Electronic Control Unit can be viewed as a tripartite system with three functional components:
1. The microcontroller executes the application software of the ECU. Whenever a messages needs to vbe transmitted, the microcontroller prompts the CAN-Controller with a transmission request and provides the data in question.
2. The CAN-Controller then completes the message data to become a full frown CAN-message, controls the bus access and passes the message on the CANols the bus access and passes the message on to the CAN-Transceiver as a sequential bit sream. 
3. The CAN-Transceiver translates the bit stream into a sequence of value specific voltage levels on the bus wires. 

OBS: To avoid signal reflection use 120 ohm resistor
  
## Different types of addressing 
1. Node Addressing
- Peer-to-peer: P2P
- The sender picks the destination 
- E.g: Diagnostics

2. Broadcast Addressing
- Broadcast
- The recipient decides whether to receive or discard the frame 
- Used in standard CAN

- A key characteristic of CAN communication is the fact that all CAN messages are received and checked by all CAN controllers, even those messages which may not pass the Acceptance filter in a certain CAN controller. 
- Rx: Receiver 
- Tx: Transmiter 
- In CAN communication, each message type (one defined ID) ma originate from one CAN node only! A CAN node may send several message types(Different IDs), but each message should have only one source. 

## Bus Access: Priority 
- The bus access is controlled by message priorities
- Message priorities are compared during the arbitration 
- The inverse of the numerical identifier value represents the message priority. High priority 0. Lowest priority: 2047

## Bus Access: Rule
- After recessive level for 11 bit times, the bus is defined to be idle and available for transmission. All nodes with a transmission request will access the bus simultaneously and start transmitting the first message bits 
- If two transmission request occur at different times, the second one finds the bus busy and cannot transmit, no matter what is the message priority. This means that once the bus is busy, the first identifier bit has already started, other nodes have to delay their transmission until the bus is idle again
- With a maximum pay load of eight bytes, CAN messages will block the bus for a relatively short time only
- All CAN messages begin with a logical 0 as start signal to distinguish the transmission start from bus idle which is the logical 1. The start of frame is followed by the first identifier bit, followed by the second id bit and so on. This means that during arbitration message collisions are allowed. The prevailing nodes not realize the other nodes and does not have to suffer any delay of transmission at all. 

*** Image from page 37 ***

## CAN Frame 
- In CAN communication, different message types and formats are used - dependent on use cases and functional needs.
- CAN Data Frame Standard Format: Identifier 11 bit / Data Field 0 to 8 Byte
- CAN Data frame Extended Format: Identifier 29 bit / Data Field 0 to 8 Byte
- CAN Remote Frame Standard Format: Identifier 11 bit/ Data field not available
- CAN Remote Frame Extended Format: Identifier 29 bit/Data field not available

- Use the extended format increases the communication overhead since only the identifier size is extended, not the payload data size.

## Structure of CAN Data Frame in Standard Format 
- SOF: Start of Frame
- RTR: Remote Transmission Request
- IDE: Identifier Extension
- r: Reverse Bit
- DLC: Data Length Code
- CRC: Cyclic Redundancy Check
- DEL: Delimiter
- ACK: Acknowledgement 
- EOF: End Of Frame
- ITM: Intermission
*** Picture from page 40 ***

1. Start of Frame
- Correspond to a status change from bus idle to bus occupied(1->0)
- Indicates the start of a message
- Provides a signal edge used for receivers to synchronize with the sender 
- A frame is always started by changing the current physical status. 
- By receiving the edge between 1(bus idle) and 0 (start of frame), all nodes start their cyclic bit duration timer and are able to recognize all the following bit values as well. 

2. Identifier 
- Transmission: Message priority for arbitration 
- Receiving: Message identification
- During arbitration, the ID is used to determine the message with the highest priority
- A receiving node compares the ID with its acceptance Filter settings to decife if the message is discarded or forwarded to the application software
- The ID informs the application software about contents and structure of the incoming data field 

3. Remote Transmission Request: 
- Dominant: Data Frame -> Transmission of information 
- Recessive: Remote Frame -> Request for data(e.g: Building Services, Automation Engineering)
- A remote frame is a request for a Data Frame. If a node needs the data denoted by a certain identifier, it issues a Remote Frame with this identifier. Since the IDs of a Remote Frame and its corresponding Data Frame are identical, the RTR bit is included into the Arbitration Phase to avoid collisions. 
- In a Data Frame, RTR is dominant in a remote frame, RTR is recessive. In an arbitration of a data frame and its corresponding remote frame, the data frame prevails. The transmission request for the remote frame may be discontinued. 
- Although a Remote Frame does not contain any Data Field, the DLC is set to the value of the requeste data. 
- Remote Frames are used in technical areas which do not share the paradigm of cyclical data transmission. This is especially the case, when CAN buses are very long(KM) and the resulting transmission rate hence does not allow cyclical transmission anymore. 

## Identifier Extension
- Dominant: Frame in standard format
- Recessive: Frame in Extended Format (e.g: utility vehicles)
- Whenever application of CAN buses need more than 2048 identifiers, the identifier extension provides close to 537 million identifiers. The extended format is used for so-called higher CAN protocols like SAE J1939.
- All nodes transmitting during an arbitration compare the bit values they sent with the ones they read back from the bus. This is called bit monitoring. 
*** Picture from page 46 ***
- Arbitration is performed bitwise
- In the case where a standard frame and an extended same has the same identifier, the arbitration shall be resolved such that the standard frame wins. This is guaranteed by using the IDE as part of the arbitration for extended frames.
  

