Edges should be treated as queues, the functionality for transforming the data that goes through it may also be handled by the edges, but this may be deferred to the nodes for simplicity.

Originally the idea was edges would keep track of the bandwidth it consumed (averaged over a recent time period). This seems like a bad idea because 1) the period will be variabled and 2) tells us nothing of if the current physical link is sufficient. Instead this information should be tracked at the level of the physical connection where the bandwidth between two points can be calculated, and it can be determined if a link is maxed out or not at any given time. 

Edges instead would record the current backlog (in bytes as well as messages, but bytes is more important). There are two backlogs represented on an remote edge, the incoming and outgoing. On local edges there is only the outgoing backlog (as there is no transport from input to output. This discreprency is best represented by splitting remote edges into two seperate edges (one in each remote graph), thereby allowing all edges to have just a single backlog.

Each edge will have a maximum backlog allowed, at this point the edge will begin "blocking" in some sense. This should have the side effect of backlogs occuring on the input edges until eventually the backlog will trickle back to the input nodes int eh graph.

Similarly nodes have backlogs. The backlog of a node indicates how many operations are waiting to be executed at any time. there are no garuntees of how many (if any) messages from source edges will be consumed each time a node executes an operation.  Therefore nodes can have infinite backlogs (or undefined?) this would be the case for example in a bias neuron where the output pulse is the same regardless of any inputs.

Nodes may have several different types of operations it can perform, each operations can consume different messages from edges or produce them. Which edges are consumed from or output to is defined for each operation seperatly. As such its impossible to predict how nodes will consume/produce messages on each operation executed.

Each node has a different backlog for each operation it can perform.

Scratch the whole idea of nodes having backlogs. Instead the signal processing nodes connect to eachother through their own edges, the backlogs on these edges indicate how backlogged the processor is (assuming its all on the same processor). Similarly the edges which connect the signal processing nodes to the actual algorithms nodes do not have a queue/backlog and are blocking. This ensures the signal processing nodes maintain proper order of execution.

If a processor isnt overloaded then the graph on it will have edges with no backlog. Once backlogs occur on local edges then the destination 


