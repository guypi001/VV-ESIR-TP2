# TCC *vs* LCC

Explain under which circumstances *Tight Class Cohesion* (TCC) and *Loose Class Cohesion* (LCC) metrics produce the same value for a given Java class. Build an example of such as class and include the code below or find one example in an open-source project from Github and include the link to the class below. Could LCC be lower than TCC for any given class? Explain.

## Answer
TCC is defined as the percentage of methods pairs, which are directly related. LCC is defined as the percentage of methods pairs, which are either directly or indirectly related
Then TCC is defined as the relative number of directly connected(NDC) public methods TCC = NDC / NP and LCC is defined as the relative number of directly or indirectly
connected (NIC) public methods LCC = NIC / NP.
These two metrics produce the same value when there is no indirectly connected public methods. LCC cannot be lower than TCC because to evaluate LCC we consider both directly and indirectly connected publics methods.
