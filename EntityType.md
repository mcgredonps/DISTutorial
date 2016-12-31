##Entity Type and Semantics

###Entity Type
When we receive an entity state PDU, how do we know what type of entity is being described? Is it a tank? An aircraft? How should we render it on the screen?

In DIS this problem was solved by using a record that contains a number of numeric fields. The values of the fields are arbitrary, but all simulations need to agree on their meaning. SISO maintains a document, the Enumerated and Bit-Encoded Values (EBV) document that maps these arbitrary numbers to semantic meanings. 

Below is an entity type record that semanticaly describes a US M1A2 tank. Whenever this collection of values is encountered, we know that the entity being referred to is an M1A2 tank.

| Field Name | Value |
|--------|-----------|
| Entity Kind | 1 |
| Entity Domain | 1 |
| Country | 225 |
| Category | 1 |
| Subcategory | 1 |
| Specific | 6 |
| Extra | 0 |

<img src="images/EntityTypeEquivalency.jpg"/>

The entity kind refers to what type of entity this is. This document has been assuming that DIS entities are vehicles. DIS can describe more than vehicles, including munitions, life forms (such as whales, which are a problem in maritime sonar exercises), buildings, and minefields. In this case an M1A2 tank is a vehicle, which as a value of 1.

The entity domain refers to whether the entity is a land, air, surface (naval), subsurface, or space vehicle. Since an M1A2 tank is a land vehicle, and the SISO EBV document specifies this to be a value of 1, that is the value set in the field.

Every country (and most obscure dependencies) has a country code assigned to it. The US is country code 225, Hungary is 97, and Russia is 222. This particular M1A2 tank is a US vehicle, so the value of the field is set to 225.

The category is 1, which the EBV document specifies as a tank for this platform.

The subcategory is 1, an M1 Abrams.

The specific value is 6, an M1A2 tank.

The extra field is unused in this example.

The EBV document has a long listing of all the types of vehicles in all the militaries of the world, at least in theory. SISO actively maintains this list and change requests can be submitted for new equipment.



###Munitions

Just as the EBV document lists all the possible types of vehicles (in principle), it also lists all possible types of munitions (in principle). For example the munition record for a ballistic 155mm M485 illumination round is

| Field Name | Value |
|--------|-----------|
| Entity Kind | 2 |
| Entity Domain | 9 |
| Country | 225 |
| Category | 2 |
| Subcategory | 14 |
| Specific | 4 |
| Extra | 0 |

Other muntions have records with analogous values. When we encounter a munition type to assess the results of combat, we can use damage tables (defined outside of DIS--that's the responsiblity of the simulator) to determine the results.

###EBV Document

SISO maintains the EBV document in an XML format. Programmers can download it, then transform it to create programming language-friendly enumerations.

###Problems

In theory, all simulations respect the values listed in the EBV document. Reality falls short of theory.

No simulation implements all the entity types and munitions listed in the EBV document. It is simply impractical for a simulation to implement the thousands of vehicles and munitions listed, so implementors write code to handle only the weapons they are likely to encounter.

The EBV document has also evolved over time, and military simulations have very long product life cycles. This means that a simulation written in 1998 may interact with a simulation written in 2015. In the intervening years new weapons have been added to the EBV document. The SISO group responsible for the EBV document tries hard for backwards compatibility, but a 1998 simulation handling 2015 weapons that the newer simulation is referring to is a different matter. 

Sometimes simulation authors simply make up the enumerated values for entity and munition types. They expect to work only with their own simulation, and this approach works well enough at the time. A few years down the road it is discovered that they need to work with other simulations, and suddenly the semantic meaning of entity type records is no longer certain. Does the entity type that comes from a simulation with a made-up enumeration values represent an entity from the EBV document, or some private semantic meaning known only to the simulation author?

###Gateways

To address these issues in a practical way "gateways" have become a popular tool. A gateway sits between simulations, reading PDUs from one, examines the entity type records, and changes them on the fly before forwarding them to another simulation. Suppose simulation A is generating M1A1 Abrams tanks, but simulation B was written in such a way that it only handles M1A2 tanks. Can we make the two simulations interoperate?

It depends. If simulation B is only worried about the visual appearance of the tanks it displays, we can insert a gateway between the two simulations. This gateway will read PDUs from simulation A, and examine them for fields that contain references to M1A1 tanks. Whenever it finds such a value, it will change it to an M1A2 tank and forward it on to simulationi B, where it will appear as an M1A2. No changes to the source code of the simulations is needed, and in fact we don't need the source code or need to change any configuration files of the two simulations at all. The gateway acts as a shim between the two simulations.

On the other hand if the simulations depend on some intrinsic value of the tanks--such as the power of the M1A1's gun, or the effectiveness of its armor--we have other problems.

(Pedants will at this point dispute the use of the term "gateway" for this application, and instead claim that what is being described here is a "bridge." Academics insist that gateways translate between protocols, while bridges translate within protocols, and DIS is being used by both simulations. The term "gateway" is embedded in practice, so stop trying to fight that battle in this problem domain.)

Considerable work has been done on gateways. See Lutz & Co.  Some popular gateways include Joint Simulation Bus (JBUS) and AIME from NAVAIR. AIME is only somewhat incidentally a gateway. It's intent is to be a common API that hides the type of protocol being used, be it DIS or HLA. It just happens that it can be repurposed into a gateway.

Further Reading

SISO EBV document:
SISO EBV committee:
JHU Gateway documents:
JBUS:
AIME:

