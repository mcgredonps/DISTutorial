## Virutal World Issues in DIS

Implementing a virtual world is a tricky business. There are some basic theoretical problems and some DIS implementation specific issues. We'll discuss some of them here. 

When examining the individual fields of the DIS PDUs, such as the location field, or the dead reckoning field, it's often useful to refer back to this section of the tutorial for more context.

### Location

DIS is mostly about the position of physical entities in the world. This points out an obvious and important basic question: what coordinate system are we using to describe locations?

In the entertainment world games move entities in 3D, but the positions are usually not correlated with the real world. This concept does not work well in LVC applications, where the position of live entities needs to be integrated with simulated entities. For example integrating a live aircraft track with the position of simulated ships requires that they share a coordinate system.

The question is, which coordinate system should be used? DIS has been used in many domains, including sea, subsurface, air, land, and space.  Simulations sometimes cover substantial distances, and the curvature of the earth sometimes cannot be ignored. For reasons of convienence we want to use a coordinate system that makes local physics calculations easy. 

A simulation limted to land operations might choose MGRS. This does not work well for aircraft simulations, where the altitude of an entity is not clamped to the surface of the earth. Naval operations might choose latitude/longitude, but this also does not work well for air operations or space operations. Calculating velocity or computing other physics calculations in either of these coordinate systems is a mess.

DIS chose to use a Cartesian coordinate system with its origin at the center of the earth, and use meters as the unit of measurement for positions put out on the network. The X-axis points out and intersects the surface of the earth at the equator and prime meridian. The Y-axis likewise intersects the earth at the equator, but at 90 degrees east longitude. The Z-axis points up through the north pole.




