% XPath XPertise

[Home](../../index.html)>[Hacking](../index.html)>[XML](index.html)


##  Find missing associations 
![](../assets/images)

To find locations without an associated building:

    /Location[not(@id = /Building/@locationId)]

Two interesting points:

1. The equals operator works on lists. If the right side of the equals is a node set, the xpath engine will automatically look through all the nodes in the set. If any are true, the equality operation will return true; if none are, it will return false
2. The not() operator negates the operation. If we used != instead, it would have returned true if any of the nodes were unequal (not what we want, since obviously some locations and buildings won't match one another). The not operator, in this case, negates the equality, and looks for any location whose id matches none of the building location refs.


##  Find any redheaded people not born in france. 

Note that we are negating an ID cross reference.

    /Person[(hairColor='red' and not(@birthplaceRef=//Country[Name='France']/@id]

##  Search using a list of values 

Are there any Locations in the following states?

    Location[contains(',LA,NE,VT,VA,HI,AK,', concat(',', Addr/StateProvCd, ','))]

Note that we need to concatenate the commas onto the state prov code, to avoid partial matching, and the list needs an additional comma on the beginning and end.


##  Find elements with duplicated attributes 

Find if there are two people with the same surname:

    Person[Name = following-sibling::Person/Name]

Looks for a following-sibling with the same name. 


##  Find aggregates with multiple absences 

Find Homes that have neither a bedroom nor a bathroom

    Location[@type="home" and not(Room[@type="bedroom") and not(Room[@type="bathroom"])


##  Find if any entities in an aggregate vary from one another 

![](../assets/images)

In other words, find if a certain attribute is the same in every member of a collection, or if there is one which is different somehow.

For example, see if any of the washing machine's "running" attribute is different than the others. 


    Washer[@Running != //*/Washer/@Running]

Basically you try to find any one washer with a running state that doesn't match any other washer's.



##  Create a stand - in for matching something absent 

HQ state should have at least one class code

    GL[not(//*/GL/ClassCode[@SubLocationRef=/*/Location[StateProvCd=/*/Policy/HQState]/SubLocation/@id])]

Note the extraneous "GL" at the beginning, to give us something to match with if the sub condition comes up empty.


##  Getting the index of an element that matches 

Get the index of the first bathroom in a locaiton:

    Location/Room[@type="bathroom"]/preceding-sibling::Room

Note: this will return a list of rooms, from 0 - the first one which matched our condition. So we need an external mechanism to count this list. However, this will be the (1 based) index of the Room in question 


[[Category:Hacking]]

