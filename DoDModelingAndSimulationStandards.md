#Department of Defense Modeling and Simlation Standards

There are three major network standards in DoD modeling and simulation: Distributed Interactive Simulation (DIS), High Level Architecture (HLA) and Test and Training Enabling Architecture (TENA). Each has evolved to serve different niches in M&S. While this document focuses on explaining DIS, the reader should also understand the place of DIS in  the DOD modeling and simulation world.

Distributed simulations are, ultimately, exchanging state information, usually about *entities*--vehicles, persons, ships, or aircraft. If a simulation wants to display a tank from another simulation it needs to know something about that tank--its position, which direction it's facing, how fast it's traveling, and other information. The data needs to be sent across the network from one host to any other hosts that may need to know about the tank. DIS, HLA, and TENA have solved this problem in different ways.


## DIS

DIS was the original standard for modeling and simulation. It consists of 

* A set of standardized messages for exchanging state information
* Agreements about semantics. This includes what coordinate system to use, units of measurement, and enumerated values that provide a compact way to describe agreed-upon meaning
* Standard agreements for handling information, entity discovery, and other procedures

There are dozens of DIS messages, called Protocol Data Units (PDUs). Each PDU exactly describes the position and format of data in the message. For example, one PDU, called the entity state PDU, transmits the position of an entity, along with other information. The entity's position is 48 bytes from the start of the PDU, and consists of three double precision floating point numbers for the X, Y, and Z values. A simulation that wants to transmit the position of an entity needs to construct the entity state PDU message in exactly the format specified by the standard. 

What's implicit in this message is that both the sender and receiver are using the same coordinate system, and the same units of measurement. These agreements about the semantics of those three floating point numbers are specified by the standard as well.

Not all message exchanges consist of simple state updates. For example simulating shooting requires that multiple messages be exchanged between the shooter and target. The sequence in which the messages must be exchanged to accomplish this are also specified by the DIS standard.

##HLA

DIS was intended to address a particular problem domain: real-time virtual environments. But the DoD has broader simulation problems to solve that DIS can't address. As a result, starting in 1995, the DoD started the process of creating a new standard for modeling and simulation, which eventually evolved into HLA.

HLA encompasses a broader set of tools that can be brought to M&S applications. DIS works in real time, AKA wall clock time. This works well for DIS simulations, where it is usually expected that there will be humans in the simulation loop, and a simulation should move no faster or slower than what a human would experience in real time. But some simulations may need to run faster or slower than real time. HLA may optionally make use of a simulation clock that can diverge from real time. Also, the DIS PDUs define a collection of state information. State information not included in the DIS standard is more difficult to define. HLA allows more  


While DIS specifies the format of data on the wire, HLA is an API standard. The standard specifies a set of function calls that simulations can use. 


## TENA

## Comparision of Standards

##