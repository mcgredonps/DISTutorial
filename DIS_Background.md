#Introduction
## DIS Background

Distributed Interactive Simulation (DIS) is one of the three main standards for exachanging state information in military simulations. Surprisingly, much of the background information about DIS is scattered across several documents, or is the result of experience and lore on the part of practioners. There's no centralized place to learn about the standard, and how to use it. Lore is notoriously difficult to capture--you have to find the right person with the right knowledge, and he has to be available and willing to answer questions. 

This document is intended to be an open source and community maintained introduction to the theory and practice of DIS. 

Often people who find they need to learn something about DIS are programmers who are thrown head-first into the pool. They're given some vague instructions, usually summed up as "Hey, we need to read DIS so we can ingest it into our application. Go make this work." They usually don't have experience with simulation or virutal worlds. They're often working on simulation projects part-time, or expect to be working on a DIS project for a few months before moving on to something else. They're OK with coding, but using DIS involves hidden assumptions about the problem domain. It would seem to be easy to direct an object in a simulation to move one meter north. But what coordinate system is being used? What are the units of measurement in that coordinate system--SI or Imperial? How does one uniquely identify a vehicle in the simulation? 

For all these reasons this document also includes a brief overview of distributed simulations, and some of the problems we face when implementing them. Studying the standard in isolation can be confusing if the reader has no familiarity with the problems DIS is trying to solve. So, as an aid to the reader, some background information on distributed simulations and virtual worlds is also included. If you already know all this, you can safely skip it.

If you just want to sling some code and skip all the throat-clearing, you can start reading the "Protocol Data Units: Exchanging State Information" section. 

DIS standardizes the format of network packets, and has no formal API. What matters is the format of packets on the wire; the implementation of the code that puts them into that format is up to the user. This means that if you compare implementations from two different DIS vendoer libraries the code will look very different. The examples in this document rely primarily on the open-dis project, available at https://github.org/open-dis. The open-dis project has DIS library implementations in Java, C++, Javascript, Python, C#, and Objective-C, along with sample applications. There are other implementations out there, both open source and commercial. There are also many home-grown implementations of the standard.

##History

DIS arose from a Defense Advanced Research Agency (DARPA) project in the 1980's called SIMNET. At the time TCP/IP and high speed networks were getting their legs, computers were becoming powerful enough to do the compuation, and 3D graphics were in their infancy. The idea, quite advanced for the time, was to create a virtual, shared battlefield. Simulators of the era sometimes had displays that replicated a soldier's view of the battlefield, but they weren't networked with each other. Each simulator worked in isolation. An aircraft simulator couldn't see a tank controlled by another simulator. SIMNET's major advance--it was arguably the first large real-time distirubted virtual world--was to create the environment that allowed this to happen. 

A screen capture from an early SIMNET application is shown below:

<img src="images/SimnetDisplay.jpg"/>

Each participant is in a simulator that controls one tank. All the vehicles interact in the same shared enviroment. If one simulator causes a tank to move, the other partipants see the tank move, in real time, in the shared environment. 

We'll be essentially ignoring the graphics portion of this, and focusing on what the network is doing. 

Before the application could be implemented there needed to be a way to exchange *state information* between simulators. State information is data about one participant in the simulation. In the case of a tank, the state information may include it's position, the direction it's facing, how fast it's moving, whether it's on fire, and which way the turret is facing. It's this state information, and the format that it is exchanged in, that DIS standardizes.

DARPA projects are intended to transition out of the incubator research phase and into useful commercial implementations that add value to the military. SIMNET worked out many off the issues involved in implementing a real-time virtual environment, but it needed to be commercialized outside of the purview of DARPA. The group that would eventually become the Simulation Interoperability Standards Group (SISO) took over development of the network protocol portion of the project, renamed to DIS, in a series of workshops held from 1989 to 1996. 

It was in the government's interest to have simulators interoperate with each other, and to avoid vendor lock-in. To be useful the DIS protocol had to be a *standard*. Simply providing a language implementation that did the job without also specifying what was being exchanged was inadequate. Implementations in other languages would have to reverse-engineer from the source code what was really happening, in a bug-for-bug compatible fashion. So SISO refined the SIMNET network protocol and developed a formal description of it, then took it to the IEEE, where it was approved as an international standard (IEEE-1278.1). From that point on anyone could buy the IEEE standard and write their own implementation. 

As is inevitable with these things, the standard has to be maintained and updated in light of experience. The first major release of a standard DIS was DIS version 5, in 1995. It has since been updated with version 6, in 1998, and version 7, in 2012. The text of the version 7 standard clarified a number of ambiguities in interpreting the standard. Some new messages have been added over the years, including those that support directed energy weapons. SISO continues to support and update DIS in working groups.

## References

Simulation Interoperabilty Standards Organization (SISO): http://sisostds.org/
The IEEE DIS Standard, 1998 (Version 6): https://standards.ieee.org/findstds/standard/1278.1a-1998.html
DIS Wikipedia: https://en.wikipedia.org/wiki/Distributed_Interactive_Simulation
DIS Plain and Simple, another document that provides information about DIS: https://www.sisostds.org/DigitalLibrary.aspx?Command=Core_Download&EntryId=29302
SIMNET wikipedia: https://en.wikipedia.org/wiki/SIMNET
SIMNET History: http://www.iitsec.org/about/awardsandrecognition/Documents/2015_FellowPaper_Miller.pdf
Still More SIMNET History: http://www.dtic.mil/dtic/tr/fulltext/u2/a294786.pdf
A whole website devoted to SIMNET history: http://simnet-history.org/