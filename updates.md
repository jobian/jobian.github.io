# Research Updates
### Running log of advisor meeting notes, planned activities, experiment successes and failures!

// 12-5-2020 - PROGRESS UPDATE
* Modified Attacker (non-DRL) Agent standalone demo is working (with minor tweaks to the network graph updates)
  * Now on to the Defender DRL agent updates!
  * RESEARCH need to meld dynamic network code into an OpenAI Gym

// 12-3-2020 - NOTES from meeting with Prof. Cho
* A WORKING (!) DDPG implementation is now running in place of the MADDPG algorithm
  * Currently modifying a TensorFlow2 DDPG algorithm developed by Phil Tabor

* Iteratively integrating/testing my agent & network models with the working DDPG code
  * Replacing the Pendulum OpenAI Gym with my own Gym environment based on a dynamic network topology with 100 geographically-distributed nodes of varying vulnerabilities
  * Updating dynamic network graphs (Networkx) and end-or-run plots (PyPlot)
  * Updated attacker agent to act as an independent agent that seeks to maximize utility based on easiest path for a given attack goal (LC, LI, or LA)

* Early observations:
  * 250 runs with the original DDPG algorithm took 2.5 - 3 hours to complete
  * Clarity of DDPG implementation → confirmed continuation of DRL approach

* Next Steps:
  * Exclude topology control (TC) -- only consider hiding edges or adding fake edges
  * Only the defender uses DRL; the attacker uses a simple rule-based attack
  * Use the following for the defender's DRL process:
    * state, S(t) = (SS(t), PS(t))
      * System Security (SS): (# of active nodes / total # of nodes initially given) -- range [0, 1]
      * Performance State (PS): fraction of a size of the giant component -- range [0, 1]

    * action, a(t) = (SS(t), PS(t))
      * Select # of nodes, a budget 'b', that will hide one edge and add a fake edge
      * Pick b number of nodes and hide an edge with an adjacent node with the highest exploitability and add an edge with a decoy node
      * Available actions
        1. Increase b
        2. Decrease b
        3. Stay as same b

    * reward, r(t)
      * r(t) = \delta * (S(t+1) - S(t)) * C(t) where S(t) = SS(t) + PS(t), C(t) is e^{-\lambda b}, and \delta is a constant

// 11-19-2020 - NOTES from meeting with Prof. Cho
* I reported that progress has been impeded for several months as I have struggled to find a policy-based MADRL solution to the two-player, non-cooperative security game as proposed in my draft paper
* Other studies have documented similar issues with the concurrent training of multiple agents on a common environment
  * Example: [Xia2019] trains two agents using different algorithms (DDPG, DQN) and rewards decisions without applying selected actions to the environment
* The scale and complexity induced by the high number of network nodes, attack types, and defense techniques – in addition to a dynamic network topology – result in stochastic agent policies and an expansive set of potential actions and states
  * MADRL algorithm effectiveness is hampered by *observation→action→reward* inconsistency

* Next Steps:
  * Explore use of DDPG algorithm for single agent (defender) and implement a utility function to drive the attacker's decisions (rebrand the attacker as 'primitive' or unsophisticated)
  * If discernible progress is not achieved in next few weeks, replace DRL approach with a game theoretic model -- a GT approach will impact the 'imperfect information' and 'uncertainty' aspects of the DRL approach.
  * Resume biweekly meetings and post meeting notes -- next meeting is scheduled for 12/3
