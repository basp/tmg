# Big Design Until Fail
- Bas Pennings
- meticulous
- 2013/10/07
- Programming
- published

Big design up front is a software fail of epic propertions. What still amazes me is seeing young professional software developers fresh out of education still doing it. This probably means that the people who educated them are members of the GIP too (worrying). Another reason might be that they don't trust their coding (or sometimes even typing) skills so they create these diagram filled documents as a sort of safety net: if the actual development doesn't work out there is at least something to show for their effort.

BDUF fails because it is part of some ritual dance that is performed to make one or more people that have absolutely nothing to contribute to actual development feel happy and involved. It's usually also a huge waste of time because no matter how cool those boxes and arrows might look on paper, once the actual development starts the big design is more than often useless.

What most people don't realize is that almost always the code and related artifacts **is** the design. It's the design that is used by the compiler to generate the actual product: the deliverables. 

In software development it's generally useless to spend any resources on design because the cost of producing the product pales in comparison to the cost of designing it. Contrast this to a physical object such as a piece of electronics, a machine or a house. The cost of producing these is usually high so it *pays off* to spend resources on design. In other words - if you build if wrong it will be expensive to fix.

In software it is completely the opposite. If you build it wrong, you just change the design, compile and deploy it (simplified).